# Gas
Gas is a unit of computational measure. The more computation a transaction uses the more gas you have to pay for. (e.g. writing to stroage is much more expensive than adding two integers)

**Gas limit:** Max amount of has in a transaction (usually in gwei)

The more people want to make transactions, the higher the gas price, and therefore the higher the transaction fees. Basically depends of the "demand".

So **gas** is **gas price** * **gas limit**

Ethereum is like a big, slow, but extremely secure computer. When you execute a function, every single node on the network needs to run that same function to verify its output — thousands of nodes verifying every function execution is what makes Ethereum decentralized, and its data immutable and censorship-resistant.


Using uint8 instead of uint256 won't save you any gas, because solidity reserves 256 bits of storage regardless. Exception is inside a **struct**
