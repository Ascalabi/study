# Solidity
Solidity is a contract-oriented programming language designed for developing [[Smart Contract]] that run on [[Blockchain]] platforms, most notably [[Ethereum]].

After you deploy a contract to Ethereum, it's **immutable** which means that it can never be modified or updated again.

Instead of hard coding contract addresses into out DApp, that let us change addresses in the future in case something happens to the contract.

Used with a DApp JavaScript front-end as [[web3.js]].


## Types
### Contract
Class bruh

### Struct
Types used to represent a record. Using smaller-sized uint when possible will pack these variables together to take up less storage. (if we decleare an int outside of struct will be reserved 256 storage regardless of the uint size). Clustering together identical data types will minimize storage space

You can pass them as an argument to a **private** or **internal** function.
```solidity
contract SandwichFactory {
	struct Sandwich {
	string name;
	string status;
  }
  SandwichFactory factory;
  
  function setFactory() public {
  	factory = SandwichFactory("Name", "Status");
  }
  function getFactoryStatus() public view {
  	return factory.status;
  }
}  

  
```

### Function
You can have multiple definitions for the same function name in the same scope.(basically same name different kind or number of attributes its called function overloading)

If a function is not marked payable and you try to send Ether, the function will reject your transaction.	

Solidity provides inbuilt mathematical as well as cryptographic functions. (keccak256,...)
``` solidity
function getResult() public view returns(uint){
      uint a = 1; // local variable
      uint b = 2;
      uint result = a + b;
      return result;
   }
```

##### State modifiers
>**view** functions ensure that they will not modify the state
>**pure** functions ensure that they not read or modify the state
>**fallback** has no name or arguments. It is called when a non-existent function is called on the contract, can not return anything, not marked payable
>**payable** type of function that can receive Ether.

### Modifiers
Are kind of half-functions that are used to modify other functions. If condition of the modifier is satisfied when calling a function than is executed, otherwise, an exception is thrown. (if we use a require... its not only limited to require tho)
```solidity
modifier onlyOwner() {
    require(isOwner());
    _;
  }
 //The function body is inserted where the __ is
 
 function renounceOwnership() public onlyOwner {
    emit OwnershipTransferred(_owner, address(0));
    _owner = address(0);
  }
```

Can also take arguments:
```solidity
modifier olderThan(uint _age, uint _userId) {
  require(age[_userId] >= _age);
  _;
}
function driveCar(uint _userId) public olderThan(16, _userId) {
  // Some function logic
}
```
### Mapping 
Are another way of storing organized data, basically a key-value store.
```solidity
// For a financial app, storing a uint that holds the user's account balance:
mapping (address => uint) public accountBalance;
// Or could be used to store / lookup usernames based on userId
mapping (uint => string) userIdToName;

accountBalance[msg.sender] = newNumber;
```

### Require
Will throw an error and stop executing if some condition is not true
```solidity
require(ownerZombieCount[msg.sender] == 0);
```

### Interface
Idk kinda weird

### Constructor
Has the same name as the contract and will be executed when the contract is first created.
bruh constructor man
```solidity
constructor() internal {
    _owner = msg.sender;
    emit OwnershipTransferred(address(0), _owner);
  }
```

### Events
Are inheritable members of a contract. Are emitted, it stores arguments passed in transaction logs
```solidity
//Declare an Event
event Deposit(address indexed _from, bytes32 indexed _id, uint _value);

//Emit an event
emit Deposit(msg.sender, _id, msg.value);
```

### library
Similar to **contracts** but with a few differences. Libraries allow us to use the using keyword, which automatically tacks on all of the library's methods to another data type.

```solidity
using SafeMath for uint;
// now we can use these methods on any uint
uint test = 2;
test = test.mul(3); // test now equals 6
test = test.add(5); // test now equals 11
```

The code behind add in SafeMath:
```function add(uint256 a, uint256 b) internal pure returns (uint256) {
  uint256 c = a + b;
  assert(c >= a);
  return c;
}
```
**assert** is similar to require, where it will throw an error if false. The difference is also that **require** will refund the user the rest of their gas when a function fails, whereas **assert** will not. So most of the time you want to use require in you code; **assert** is typically used when something has gone horribly wrong with the code (like a uint overflow)

## Storage vs Memory
>**storage** refers to variables stored permanently on the blockchain

>**memory** variables are temporary, and are erased between external functions to your contract. memory cannot be used at the contract level, only in methods.

>**calldata** similar to memory, nbut it's only available to **external** functions

Think of it like Hard disk vs RAM on a computer.

Solidity handles them by default. **State variables** (variables declared outside of of function) are by default **storage**, while variables declared inside functions are stored as **memory** and will disapear when the function call ends.

You can declare variables in memory:
```solidity
uint[] memory values = new uint[](3);
```
- Note: memory array must be created with a length argument. Cannot be resized like storage arrays can with array.push() 

## State variables
-   **Public** − Public state variables can be accessed internally as well as via messages. For a public state variable, an automatic getter function is generated.

    
-   **Internal** − Internal state variables can be accessed only internally from the current contract or contract deriving from it without using this (inheritance).
    
-   **Private** − Private state variables can be accessed only internally from the current contract they are defined not in the derived contract from it.
-   **External** - These functions can ONLY be called outside the contract - they can't be called by other functions inside that contract.

## Global Variables
These are special variables which exist in global workspace and provide inf about the blockchain and transaction properties.

>**msg.sender** (address payable) refers to the address of the person (or smart contract) who called the function.
> 
>Using **msg.sender** gives your the securtiy of the [[Ethereum]] [[blockchain]].

>**msg.value** how much Ether was sent to the contract

>**gasleft()** returns **uint** remaining gas

>**tx.origin** (address payable)  sender of the transaction

>**now** returns current unix timestamp of the lastes block


## Features

**inheritance** class inheritance. None of the contracts that inherit can access private functions. Inharitance works on the whole hierarchy.
Inheritance can can work from multiple contracts

**import** import .sol

>**Multiple retured values**
>```solidity
>function multipleReturns() internal returns(uint a, uint b, uint c) {
  		return (1, 2, 3);
}
>function processMultipleReturns() external {
 		uint a;
   		uint b;
   uint c;
   // This is how you do multiple assignment:
   (a, b, c) = multipleReturns();
}
>function getLastReturnValue() external {
  uint c;
  // We can just leave the other fields blank:
  (,,c) = multipleReturns();
}
>```

### Random generator
We use the keccak 256 hash function
**NOT A SAFE METHOD**... can be exploited if you run a note and don't publish transactions where you lose... idk not sure.
**BUT** in practice unless our random function has a lot of money on the line, the users won't have enought resources to attack it.

Ya creating random numbers on a blockchain where the entire contents are visible to all participants, is a difficult problem.

```solidity
// Random number from 0-99
uint random = uint(keccak256(abi.encodePcked(now, msg.sender)));
```
>**Note**: abi.encodePacked() kinda packs inputs together

## Libraries
**OpenZeppelin** solidity library of secure and community-vetted smart contracts. (SafeMath,...)

```solidity
using SafeMath for uint256;

uint256 a = 5;
uint256 b = a.add(3); // 5 + 3 = 8
uint256 c = a.mul(2); // 5 * 2 = 10
```



## Optimization

Normally there's no benefit to using int sub-types because Solidity reserves 256 bits of storage regardless of the uint size.
**Exception**: inside structs

You'll also want to cluster identical data types together (i.e. put them next to each other in the struct) so that Solidity can minimize the required storage space. For example, a struct with fields uint c; uint32 a; uint32 b; will cost less gas than a struct with fields uint32 a; uint c; uint32 b; because the uint32 fields are clustered together.

Use **view** functions. But if a **view** function is called internally from another function in the same contract that is not a view function, it will still cost gas. So view functions are only free when they're called externally.

One of the more expensive operations in Solidity is using storage - particularly writes. In order to keep costs down, you want to avoid writing data to storage except when absolutely necessary. Sometimes this involves seemingly inefficient programming logic - like rebuilding an array in memory every time a function is called instead of simply saving that array in a variable for quick lookyps









