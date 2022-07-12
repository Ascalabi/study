# Ethereum

The Ethereum [[Blockchain]] is made up of **accounts**. An account has a balance of **Ether**. 

**Address** is a unique identifier. It can be used either by a specific user or a smart contract.


## Nodes
The Ethereum network is made up of nodes, with each containing a copy of the blockchain. When you want to call a function on a [[Smart Contract]], you need to 
 1. The address of the smart contract
 2. The function you want to call
 3. The variables you want to pass to that function

Ethereum nodes only speak a language called JSON-RPC.

```bash
// Using NPM
npm install web3
```

You could host your own Ethereum node as a provider. But you can use [[Infura]]  a service that maintains a set of Ethereum nodes with a caching layer for fast reads, which you can access for free through their API.


