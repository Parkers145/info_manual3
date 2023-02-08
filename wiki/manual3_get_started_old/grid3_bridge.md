# Transferring TFT between Stellar and TF-Chain

!!!include dashboard_portal_home


> NOTE: We are working hard to have the runtime environments running in a fully stable manner. However, as we're still in test phase, it is recommended not to send too many tokens to TF-Chain for testing, until a further notice.

This document will explain how you can transfer TFT from TF-Chain to Stellar and back.

## Option 1 : Use the Bridge UI

A Web User interface is available to transfer tokens from TF-Chain to the Stellar network. 

- On [Testnet](https://bridge.test.grid.tf/), works with Stellar mainnet TFT
- On [Devnet](https://bridge.dev.grid.tf/), works with Stellar testnet TFT

### Transfer TFT from Stellar network to TF-Chain

Deposit TFTs from the Stellar network to TF-Chain needs to be done from your Stellar wallet, however, instructions are available in the [Stellar-TF-Chain Bridge](https://bridge.test.grid.tf/). To get the instructions, click on the `DEPOSIT FROM STELLAR` button. 

![](img/grid3_bridge_stellar_to_tfchain.png ':size=400')

Tokens need to be sent to the bridge address (on Testnet `GA2CWNBUHX7NZ3B5GR4I23FMU7VY5RPA77IUJTIXTTTGKYSKDSV6LUA4`), with a memo indicating the destination on TF-Chain. In most cases, you will send your tokens to the twin you have created on TF-Chain. 
So in case you have a twin defined which has the twin_ID = 123, add the memo text `twin_123` in your transfer.  

### Transfer TFT from TF-Chain to Stellar Network

In order to make the bridge work, you need to trust the Web UI that accesses the account you have created on the TF-Chain. 

![](img/grid3_bridge_trust_ui.png ':size=400')

Click on `Yes, allow this application access`.

Once done, you get the transfer screen. 

![](img/grid3_bridge_tfchain_to_stellar.png ':size=600')

Fill in the Stellar Address and the amount to transfer to your Stellar wallet, and click `WITHDRAW`. 
