# Web3.js
Web3.js is a collection of JavaScript libraries that allow you to interact with a local or remote [[Ethereum]] node using HTTP, IPC or WebSocket.

**Web 3.0** applications that run on blockchain. Basically a decentralized web.


## Metamask
Is a browser extension for Chrome and Firefox that lest users securely manage their Ethereum accounts and private keys, and interact with websites that are using Web3.js

With this your browser will be Web3 enabled.

### Boilerplate code to require users to have Metamask 
```javascript
window.addEventListener('load', function() {

  // Checking if Web3 has been injected by the browser (Mist/MetaMask)
  if (typeof web3 !== 'undefined') {
    // Use Mist/MetaMask's provider
    web3js = new Web3(web3.currentProvider);
  } else {
    // Handle the case where the user doesn't have web3. Probably
    // show them a message telling them to install Metamask in
    // order to use our app.
  }

  // Now you can start your app & access web3js freely:
  startApp()

})
```


## [[Smart Contract]]
Web3.js will need 2 things to talk to your contract: its **address** and **[[ABI]]**

### Functions
Web3.js has two methods we will use to call functions on our contract: **call** and **send**.

>**call** is used for view and pure functions. It only runs on the local node, and won't create a transaction on the blockchain.
>**send** will create a transaction and change data on the blockchain. On any function that isn't view or pure. (will pop up Metamask to sign a transaction)

This is **asynchreonous**, like and API call to an external server. Web3 returns a promise here. (Ya i have no idea... some javascript bullshit)