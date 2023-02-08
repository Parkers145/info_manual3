## Grace Period

### What is the Grace Period.

When the owner of a contract runs out funds on his wallet to pay for his deployment, the contract goes in to a Grace Period state. The deployment, whatever that might be, will be inaccessible during this period to the user. When the wallet is funded with TFT again, the contract goes back to a normal operating state. If the grace period runs out (by default 2 weeks) the user's deployment and data will be deleted from the node.

### How it works

- When a twin runs out of funds, all of the contracts linked to this twin will go into a `Grace Period` when the next billing cycle is reached.
- The `Grace Period` is 14 days by default. During this period the user will not be able to use any of the deployments that are linked to this twin.
- The user will also not be able to delete contracts while in `Grace Period` whether it is a node, name or rent contract.
- The deployments will be usable again if the user funds the twin with the appropriate amount of TFTs.
- If the user did not fund the twin with TFTs during the `Grace Period` then the Contracts will be deleted after this period.

### When does the Grace Period Kick In.

The Grace Period Start or kicks in when the twin balance doesn't satisfy the balance required for the deployments/workloads.

### How to resume your workloads

The only way to resume workloads that are in `Grace Period` is to fund your twin with TFTs.

### Where can you check the contract state from

There are multiple ways to check the state of the contracts from.

- Grid Weblets

   Probably the easiest way to check the state of your contracts is to navigate to the `Contracts` tab in the grid weblets where you can retrieve all of the details for the desired contract including its `State` & `Expiration` date if the node is in grace period.
   
   ![](img/grace_period_weblets.png)
    

- TFgrid Proxy

  The Contracts that are in grace period can be checked through this endpoint `https://gridproxy.grid.tf/contracts?state=GracePeriod&twin_id=<YOUR_TWIN_ID>`
  
  ![](img/grace_period_gridproxy.png)


- TFchain Graphql

  The Contract State can also be checked through [graphql](https://graphql.grid.tf/graphql) using these queries

   - Node Contract
   
      ![](img/grace_period_graphql_node.png)

   
   - Rent Contract
    
      ![](img/grace_period_graphql_rent.png)


- PolkadotJS UI

    The Contract state can also be checked from the [PolkadotJS UI](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Ftfchain.grid.tf#/chainstate) through `chainstate` -> `SmartContractModule` -> `Contracts(ID_OF_CONTRACT)`
    
    ![](img/grace_period_polkadot_ui.png) 


