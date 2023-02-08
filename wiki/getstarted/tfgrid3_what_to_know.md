# TFGrid 3.0 Whats There To Know

- [Storage Concepts](tfgrid3_storage_concepts)
- [Network Concepts]

## networking

### Private network (ZNET)

For a project that needs a private network, we need a network that can span across multiple nodes, this can be achieved by the network workload reservation [Network](@threefold:tfgrid_network)

### Planetary network

For a project that want their workloads directly connected to the planetary network we will need the option planetary enabled when deploying a VM or kubernetes. Check [Planetary network](@threefold:planetary_network) for more info about 

### Public IPs
When you want to have a public IP assigned to your workload, you need to reserve the number of IPs along with your contract and then you can attach it to the VM workload

## Exposing the workloads to the public

Typically, if you reserved a public IP you can do that directly and create a domain referencing you public IP. Threefold provides also [Webgateway technology](https://library.threefold.me/info/threefold#/technology/threefold__webgw?id=webgw-20) a very cost-efficient technology to help with exposing your workloads

### how it works
Basically you create a `domain reservation` that can be 
- `prefix` based e.g `mywebsite` that will internally translate to `mywebsite.ghent01.devnet.grid.tf` 
- `full domain` e.g `mysuperwebsite.com`  (this needs to point to the gateway IP)

And then you need to specify the yggrassil IP of your backend service, so the gateway knows where to redirect the traffic

#### TLS
As a user, you have two options
- let the gateway terminate the TLS traffic for you and communicate with your workloads directly 
- let the gateway forward the traffic to your backend and you do the termination yourself (the recommended way if you are doing any sensitive business)


## Compute

VM workload is the only workload that you will need to run a full blown VM or an [flist-based](@threefold:zos_fs) container

### How can I create an flist?

The easiest way is by converting existing docker images using [the hub](https://hub.grid.tf/docker-convert)


### How flist-based container run in a VM?
ZOS injects its own generic kernel while booting the container based on the content of the filesystem

### kubernetes 
We leverage the VM primitive to allow provisioning kubernetes clusters across multiple nodes based on k3os flist.


## Exploring the capacity
You can easily check using [explorer-ui](@explorer_home) , also to plan your deployment you can use these [example queries](explorer_graphql_examples)

## Getting started

Please check [Getting started](https://library.threefold.me/info/manual/#/getstarted/manual__tfgrid3_getstarted) to get the necessary software / configurations


For detailed information check

- [Components Interaction](@grid3_components)
- [Definitions](@grid3_definitions)
- [ZOS Primitives](threefold:tfgrid_primitives)
- [Getting started](https://library.threefold.me/info/manual/#/getstarted/manual__tfgrid3_getstarted)
- [Proof of Utilization](@proof_of_utilization_manual)