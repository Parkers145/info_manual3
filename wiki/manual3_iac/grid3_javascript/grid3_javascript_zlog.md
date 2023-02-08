!!!include:grid3_javascript_loadclient
# Zlogs

Zlogs is a utility that allows you to stream VM logs to a remote location. You can find the full description [here](https://github.com/threefoldtech/zos/tree/main/docs/manual/zlogs)

## Using Zlogs

In grid3_ts, a vm has a `zlogsOutput`, this field should contain a string of target URL to stream logs to.

Valid protocols are: `ws`, `wss`, and `redis`.

For example, to deploy two VMs named "vm1" and "vm2", with one vm1 streaming logs to vm2. so the [multiple_vms.ts](https://github.com/threefoldtech/grid3_client_ts/blob/development/scripts/multiple_vms.ts) will be :

````js
import { DiskModel, FilterOptions, MachineModel, MachinesModel, NetworkModel } from "../src";
import { config, getClient } from "./client_loader";
import { log } from "./utils";

async function main() {
    const grid3 = await getClient();

    // create network Object
    const n = new NetworkModel();
    n.name = "monNetwork";
    n.ip_range = "10.238.0.0/16";

    // create disk Object
    const disk1 = new DiskModel();
    disk1.name = "newDisk1";
    disk1.size = 10;
    disk1.mountpoint = "/newDisk1";

    const vmQueryOptions: FilterOptions = {
        cru: 1,
        mru: 2, // GB
        sru: 10,
        farmId: 1,
    };

    // create vm node Object
    const vm1 = new MachineModel();
    vm1.name = "testvm1";
    vm1.node_id = +(await grid3.capacity.filterNodes(vmQueryOptions))[0].nodeId;
    vm1.disks = [disk1];
    vm1.public_ip = false;
    vm1.planetary = true;
    vm1.cpu = 1;
    vm1.memory = 1024 * 2;
    vm1.rootfs_size = 0;
    vm1.flist = "https://hub.grid.tf/tf-official-apps/base:latest.flist";
    vm1.entrypoint = "/sbin/zinit init";
    vm1.env = {
        SSH_KEY: config.ssh_key,
    };

    //  Add the the target redis URL
    vm1.zlogsOutput = "redis://10.238.3.5:6379/zlog";

    // create disk Object
    const disk2 = new DiskModel();
    disk2.name = "newDisk2";
    disk2.size = 10;
    disk2.mountpoint = "/newDisk2";

    // create  vm2 node Object
    const vm2 = new MachineModel();
    vm2.name = "testvm2";
    vm2.node_id = +(await grid3.capacity.filterNodes(vmQueryOptions))[1].nodeId;
    vm2.disks = [disk2];
    vm2.public_ip = false;
    vm2.planetary = true;
    vm2.cpu = 1;
    vm2.memory = 1024 * 2;
    vm2.rootfs_size = 0;
    vm2.flist = "https://hub.grid.tf/tf-official-apps/base:latest.flist";
    vm2.entrypoint = "/sbin/zinit init";
    vm2.env = {
        SSH_KEY: config.ssh_key,
    };
    vm2.ip = "10.238.3.5";

    // create VMs Object
    const vms = new MachinesModel();
    vms.name = "monVMS";
    vms.network = n;
    vms.machines = [vm1, vm2];
    vms.metadata = "{'testVMs': true}";
    vms.description = "test deploying VMs via ts grid3 client";

    // deploy vms
    const res = await grid3.machines.deploy(vms);
    log(res);

    // get the deployment
    const l = await grid3.machines.getObj(vms.name);
    log(l);

    await grid3.disconnect();
}

main();

````

At this point, two VMs are deployed, and vm1 is ready to stream logs to vm2. But what is missing here is that vm1 is not actually producing any logs, and vm2 is not listening for incoming messages.

### Use Redis

on vm2, we need to enable redis server on port `6379` and subscribe `zlog` channel, as we sat `vm1.zlogsOutput=redis://10.238.3.5:6379/zlog` 
> Note : `10.238.3.5` in `vm1.zlogsOutput` is vm2's ip.

the python script will be :

````python
import os
import redis
import gzip

redis_conn = redis.Redis(charset="utf-8", decode_responses=False)
from multiprocessing import Process

def sub(name: str):

    pubsub = redis_conn.pubsub()
    pubsub.subscribe("zlog")
    print("[+] Subscribed....")
    for message in pubsub.listen():
         if message.get("type") == "message":
            data = message.get("data")
            decompressedData =gzip.decompress(data).decode('utf-8')
            f = open("/output.txt", "a")
            f.write(decompressedData)
            f.close()

Process(target=sub, args=("reader",)).start()

````

- Note that incoming messages are decompressed since zlogs compresses any messages using gzip.
- After a message is decompressed, it is then appended to output.txt.
  
### Streaming logs

- Zlogs streams anything written to stdout of the zinit process on a vm.
- So, simply running ```echo "to be streamed" 1>/proc/1/fd/1``` on vm1 should successfully stream this message to the vm2 and we should be able to see it in `output.txt`.
- Also, if we want to stream a service's logs, a service definition file should be created in ```/etc/zinit/``` directroy as `yaml` file, on vm1 and should look like this:
  
```yaml
exec: sh -c "echo 'to be streamed'"
log: stdout
```
> NOTE: 
> - You must set `log: stdout` to see the logs
> - You can replace `sh -c "echo 'to be streamed'"` with any command that you need to be executed here, the mentioned command is for testing only


checkout more about [zinit](https://github.com/threefoldtech/zinit).
