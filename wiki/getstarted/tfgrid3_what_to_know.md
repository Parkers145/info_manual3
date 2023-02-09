# TFGrid 3.0 Whats There To Know

- [Storage Concepts](tfgrid3_storage_concepts)
- [Network Concepts](tfgrid3_network_concepts)

___
<details><summary>

## Workload Networking Overview 
___

</summary>

<h2>Types Of Networks On The Grid</h2>

### Znet Networks (Wiregaurd Based)

For a project that needs a private network, we need a network that can span across multiple nodes, this can be achieved by the network workload reservation [Network](@threefold:tfgrid_network)

### Planetary Network (Yggdrasil Based)

For a project that want their workloads directly connected to the planetary network we will need the option planetary enabled when deploying a VM or kubernetes. Check [Planetary network](@threefold:planetary_network) for more info about 

### Public IPs (Classic IPV4 and IPV6)
When you want to have a public IP assigned to your workload, you need to reserve the number of IPs along with your contract and then you can attach it to the VM workload

## Exposing the workloads to the public

When deploying with a Public IP there is no special configuration needed to configure your workload to be exposed to the internet, for workloads that are deployed without public I.P. addresses,  Threefold also provides [Web Gateway technology](https://library.threefold.me/info/threefold#/technology/threefold__webgw?id=webgw-20) a very cost-efficient technology to help with exposing your workloads

<details><summary>

### How Web Gateways Work
</summary>

Along side your VM workload a  `domain reservation` workload can be created that can be 
- `prefix` based e.g `mywebsite` that will internally translate to `mywebsite.ghent01.devnet.grid.tf` 

or

- `full domain` based e.g `mysuperwebsite.com`  You would set your DNS records to direct traffic to the gateways nodes public IPV4/IPV6 Address. 

And then you need to specify the yggrassil IP of your backend service, so the gateway knows where to redirect the traffic

#### TLS With Your Gateway Workload 
As a user, you have two options
- let the gateway terminate the TLS traffic for you and communicate with your workloads directly 
- let the gateway forward the traffic to your backend and you do the termination yourself (the recommended way if you are doing any sensitive business)
    </details>
</details>

___
<details><summary>

## Compute Workloads Overiew
___
</summary>


The simplest of Compute deployments is VM workload, This can either be A Full Fledged Linux Virtual Machine Or a [flist-based](@threefold:zos_fs) container

### How can I create an flist?

The easiest way is by converting existing docker images using [the hub](https://hub.grid.tf/docker-convert)

In order to create a full vm flist See the Documentation Available on [The Forums](https://forum.threefold.io/t/manipulating-cloud-images-for-the-grid/3380)


### How flist-based container run in a VM?

ZOS injects its own generic kernel while booting the container based on the content of the filesystem

### kubernetes 

We leverage the VM primitive to allow provisioning kubernetes clusters across multiple nodes based on k3os flist.

</details>

## Exploring the capacity

You can easily check using [explorer-ui](@explorer_home) , also to plan your deployment you can use these [example queries](explorer_graphql_examples)

## Getting started

For more detailed information before you get started deploying check out

- [How Threefold Grid Components Interact](@grid3_components)
- [Definitions On The Threefold Grid](@grid3_definitions)
- [ZOS Primitives](threefold:tfgrid_primitives)
- [Getting started](https://library.threefold.me/info/manual/#/getstarted/manual__tfgrid3_getstarted)
- [Proof of Utilization](@proof_of_utilization_manual)