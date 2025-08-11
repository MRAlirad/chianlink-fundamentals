# ChainLink

## Introduction

-   [chainlink](https://chain.link)
-   [chainlink discourd](https://discord.com/invite/chainlink)
-   [Ethereum Stack Exchange](https://ethereum.stackexchange.com/)

### Major Chainlink Services

-   Chainlink Data and Price Feeds
-   Chainlink Automation
-   Chainlink CCIP
-   Chainlink Functions
-   Chainlink VRF
-   Chainlink Data Feeds
-   Chainlink Proof of Reserve

## Smart Contract and Solidity Fundammentals

### Solidity Programming: A Guide for Blockchain Enthusiasts

[Solidity](https://soliditylang.org) is a high-level, object-oriented programming language for writing smart contracts. It is a curly-bracket language similar to C++, Python, and JavaScript. Solidity is specifically built to run on the Ethereum Virtual Machine (EVM), making it the primary language for developing decentralized applications (dApps) and blockchain-based protocols.

Solidity provides features such as:

-   **Static typing**: Variable types must be defined in advance and are checked at compile time, reducing errors.
-   **Inheritance**: Contracts can inherit from other contracts, promoting code reuse and modularity.
-   **User-defined types**: You can create custom data structures using structs, mappings, and enums.
-   **Library support**: Reusable code modules can be imported to extend functionality.
-   **Smart contract interaction**: Contracts can easily communicate with other contracts on the blockchain.
-   **Low-level EVM access**: Advanced developers can use inline assembly for optimized performance.

#### Solidity Code Structure

-   **License Identifier** : specify how others can use your code with an SPDX license identifier:
    ```solidity
    // SPDX-License-Identifier: MIT
    ```
-   **Version Pragma** : declare which version of Solidity your code is compatible with using that _pragma_ directive:
    ```solidity
    pragma solidity ^0.8.19;
    ```
-   **Contract Declaration** : The main code is contained within a contract declaration:
    ```solidity
    contract MyContract {
        // Contract code goes here
    }
    ```

#### Programming Fundamentals for Solidity

##### Variables: Storing Information

Variables are named containers that hold data values. In Solidity, each variable must have a specific type that defines what kind of data it can store, and its location (storage, memory, or calldata) determines where the data is stored.

**State Variables** : These are permanently stored on the blockchain in storage:

```solidity
contract StorageExample {
    uint256 public myNumber = 42;    // A number
    string public myText = "Hello";  // Text
    bool public isActive = true;     // A true/false value
    address public owner;            // An Ethereum address
    uint256 private secretNumber;    // A private number
}
```

When you see a variable defined at the contract level like this, it's a state variable that will be stored on the blockchain permenantly. Sometimes, for clarity, we label variables as stored in storage using the s\_ prefix e.g. s_balance or s_owner.

State Variable Visibility :

-   **public**: Anyone can read this variable, and Solidity automatically creates a getter function.

    ```solidity
    uint256 public counter = 0; // Creates a counter() function that returns the value
    ```

-   **private**: Only accessible within the current contract (not truly private on the blockchain since they can be accessed via the contract storage).

    ```solidity
    uint256 private password = 123456; // Not accessible from other contracts
    ```

-   **internal**: Accessible within the current contract and contracts that inherit from it

    ```solidity
    uint256 internal sharedSecret = 42; // Visible to this contract and child contracts
    ```

The default visibility is _internal_ if not specified.

##### Constant and Immutable variables

Solidity provides two special types of state variables that can significantly reduce gas costs and improve security:

**Constant Variables**: Variables marked as constant must be assigned a value at declaration and cannot be changed afterwards:

```solidity
contract TokenContract {
    // Must be assigned at declaration
    uint256 public constant DECIMAL_PLACES = 18;
    string public constant TOKEN_NAME = "My Token";
    address public constant DEAD_ADDRESS = 0x000000000000000000000000000000000000dEaD;
}
```

Key characteristics of _constant_ variables:

-   Must be assigned a value when declared.
-   Value is set at compile time.
-   Cannot be changed after contract deployment.
-   Do not take up storage slots (saving gas).
-   Only value types and strings can be constants.
-   Usually, constant variables are capitalized.

**Immutable Variables**: Variables marked as immutable can be assigned only once, but this assignment can happen in the constructor:

```solidity
contract TokenContract {
    // Declared but not assigned yet
    address public immutable deployer;
    uint256 public immutable deploymentTime;

    constructor() {
        // Assigned once in the constructor
        deployer = msg.sender;
        deploymentTime = block.timestamp;
    }
}
```

Key characteristics of _immutable_ variables:

-   Can be assigned in the constructor or at declaration
-   Cannot be changed after construction
-   More gas efficient than regular state variables
-   Less gas efficient than constants
-   Only value types can be immutable (not strings or reference types)

When to Use Each

-   Use constant for values known at compile time (e.g., mathematical constants, configuration values)
-   Use immutable for values that depend on deployment conditions but won't change after that (e.g., deployer address, configuration based on constructor arguments)
-   Use regular state variables for values that need to change during the contract's lifetime

Use _constant_ and _immutable_ whenever possible to:

-   Reduces gas costs
-   Makes contract intentions clearer
-   Improves security by preventing accidental state changes

##### Data Types: Different Kinds of Information

Solidity has two main categories of data types: value types and reference types. Understanding this distinction is important for how data is stored and managed in your contracts.

**Value Types** : Value types store their data directly. When you assign a value type to another variable, you get a copy of the value.

```solidity
// Let's say we create a variable with a value:
uint a = 5;
​
// When we assign it to another variable:
uint b = a;
​
// Now b has its own copy of the value 5
​
// If we change b:
b = 10;
​
// The value of a still remains 5
// The value of b is now 10
```

This happens because value types (like integers, booleans, addresses, etc.) store their actual data directly in the variable. Each variable has its own separate copy of the data.

This differs from how some other types (called reference types) work, where instead of getting a new copy, you might just get directions pointing to the original data (like when someone gives you an address to a house rather than building you a new identical house). We will go through this shortly.

The Solidity value types include:

-   **uint (Unsigned Integer)**: Positive whole numbers (no decimals, no negatives)

    ```solidity
    uint256 public score = 100;
    ```

-   **int (Signed Integer)**: Whole numbers that can be negative (no decimals)

    ```solidity
    int256 public temperature = -5;
    ```

    The number after uint/int (like 256) represents how many bits are used to store the number. More bits mean larger possible values. The maximum value of e.g. a uint256 is 2^256-1 and a int256 2^255-1 since it has to be able to store the sign too.

-   **bool (Boolean)**: _true_ or _false_ values

    ```solidity
    bool public isComplete = false;
    ```

-   **address**: An Ethereum account (wallet) address

    ```solidity
    address public contractCreator = 0x123...;
    ```

-   **bytes (Fixed Size)**: Fixed-size byte arrays (bytes1 to bytes32)
    ```solidity
    bytes32 public dataHash = 0xabcd...;
    ```

**Reference Types** : Reference types, unlike value types don't store their data directly in the variable but instead store a **"pointer"** or reference to where the data is located. When you assign a reference type to another variable, both variables point to the same data. We will be going through pointers and what they are shortly. Think of reference types like having a TV remote control. The remote doesn't contain the actual TV shows, but it points to the TV where you can watch them.

The Solidity reference types include:

-   **string**: Text values (dynamic length bytes)

    ```solidity
    string public message = "Welcome!";
    ```

-   **arrays**: Ordered lists of items of the same type

    ```solidity
    uint256[] public scores = [85, 90, 95];
    ```

-   **mapping**: Key-value pairs (like a dictionary or lookup table)

    ```solidity
    mapping(address => uint256) public balances;
    // This creates a relationship where each address has an associated uint256 value.
    ```

-   **struct**: Custom groupings of related data.

    ```solidity
    struct Person {
        string name;
        uint256 age;
        address walletAddress;
    }
    ```

-   **bytes (Dynamic Size)**: Variable length byte array

    ```solidity
    bytes public dynamicData;
    ```

##### Understanding Pointers and Data Location

A **pointer** is a variable that stores the memory address/location for another piece of data rather than the data itself. Think of it as a signpost pointing to where the actual data lives rather than directly containing it.

In Solidity, reference types (arrays, structs, strings, and mappings) are stored as pointers. When you pass these types around, Solidity doesn't copy all the data; it just references where the data is stored.

This differs from value types (like uint, int, and bool), which directly store their data and create copies when assigned to new variables.

To explain, the concept of a pointer, here is a pseudocode example:

```solidity
// Conceptual example in Solidity-like pseudocode
​
// Declare and initialize an array in storage (blockchain permanent memory)
array storageArray = [1, 2, 3]; // original array in storage
​
// Create a pointer to the storage array
// This doesn't copy the data, it just creates a reference to the same storage location
array storagePointer = storageArray; // points to the same data
​
// Modify the array through the pointer
storagePointer[0] = 100; // changes the actual storage array
​
// At this point:
// storageArray is [100, 2, 3]  (it was changed!)
​
// Create a memory copy of the storage array
// This copies the entire array to a new location in temporary memory
array memoryCopy = copy of storageArray; // memoryCopy is [100, 2, 3]
​
// Modify the memory copy
memoryCopy[1] = 200; // only changes the copy, not the original
​
// Final result:
// storageArray is [100, 2, 3]  (unchanged by memory modifications)
// memoryCopy is [100, 200, 3]  (modified locally)
```

##### Storage Locations

Understanding how Solidity handles data storage is especially important when working with reference types. These keywords specify where data is stored:

-   **storage**: Permanent storage on the blockchain (expensive)

    -   Used for state variables.
    -   Data persists between function calls and transactions.
    -   Most expensive in terms of gas costs.

-   **memory**: Temporary storage during function execution (cheaper)

    -   Only exists during function execution.
    -   Cheaper than storage.
    -   Used for function parameters, return values, and local variables.

-   **calldata**: Read-only temporary storage for function parameters (most efficient)

    -   Similar to memory but read-only.
    -   Can't be modified.
    -   Most gas-efficient.
    -   Used primarily for external function parameters.

```solidity
// State variable - stored in storage
uint256[] permanentArray;
​
function processArray(uint256[] calldata inputValues) external {
    // 'inputValues' exists in calldata - can't be modified

    // Local variable in memory - temporary copy
    uint256[] memory tempArray = new uint256[](inputValues.length);
    for (uint i = 0; i < inputValues.length; i++) {
        tempArray[i] = inputValues[i] * 2;
    }

    // Reference to storage - changes will persist
    uint256[] storage myStorageArray = permanentArray;
    myStorageArray.push(tempArray[0]); // This updates the blockchain state
}
```

When working with reference types like strings, arrays, and structs:

-   Use calldata for external function parameters when possible (most efficient).
-   Use memory for function parameters that need to be updated.
-   Use storage when you need to modify state variables.

##### Functions: Making Things Happen

Functions are blocks of code that perform specific actions. They're how your contract actually does things:

```solidity
contract Counter {
    uint256 public count = 0;

    // This function increases the count by 1
    function increment() public {
        count = count + 1;  // You can also write: count += 1;
    }

    // This function decreases the count by 1
    function decrement() public {
        count = count - 1;  // You can also write: count -= 1;
    }
}
```

Function Parts:

-   **Name**: What you call the function (like increment).
-   **Parameters**: Input values the function needs .
-   **Visibility**: Who can call this function.
-   **Returns**: What output the function provides.
-   **Function Body**: The code inside the curly braces {}.

```solidity
function add(uint256 a, uint256 b) public pure returns (uint256) {
    return a + b;
}
```

this function:

-   Is named _add_.
-   Takes two parameters: _a_ and _b_, both of type _uint256_.
-   Is _public_ so anyone can call it.
-   Is _pure_ which means it doesn't read or modify state.
-   Returns a _uint256_ value.
-   Adds the two input values and returns the result.

Function Visibility

-   **public**: Anyone can call this function
-   **private**: Only this contract can call this function
-   **internal**: Only this contract and contracts that inherit from it can call this function-
-   **external**: Only calls from outside the contract are allowed (more efficient for certain use cases)

Special Function Types:

-   **view**: Can read but not modify state

    ```solidity
    function getCount() public view returns (uint256) {
        return count;
    }
    ```

-   **pure**: Cannot read or modify state

    ```solidity
    function addNumbers(uint256 a, uint256 b) public pure returns (uint256) {
        return a + b;
    }
    ```

-   **constructor**: Runs only once when the contract is deployed

    ```solidity
    constructor() {
        owner = msg.sender;  // Sets the contract creator as the owner
    }
    ```

-   **payable**: means that the function can be sent ether.

    ```solidity
    mapping(address => uint256) balances;
    ​
    function sendMeMoney() public payable {
        balances[user] += msg.value; // more on this "msg.value" coming in a second!
    }
    ```

##### Transaction Context and Global Variables

Solidity provides access to transaction information and blockchain data through special built-in variables. These are crucial for building secure and functional smart contracts.

-   **msg.sender**: The address that called the current function:

    -   A wallet address or another contract
    -   Commonly used for access control and tracking user activity

    ```solidity
    contract OwnerExample {
        address public owner;

        constructor() {
            owner = msg.sender; // The address that deploys the contract becomes the owner
        }
    }
    ```

-   **msg.value**: The amount of ETH (in wei) sent with the function call:

    -   Only available if the function is marked as payable
    -   Used to receive payments or deposits

    ```solidity
    contract PaymentExample {
        mapping(address => uint256) public payments;

        // Function that can receive ETH
        function makePayment() public payable {
            require(msg.value > 0, "Must send some ETH");
            payments[msg.sender] += msg.value;
        }

        // Function that checks if minimum payment was made
        function verifyMinimumPayment(uint256 minimumAmount) public view returns (bool) {
            return payments[msg.sender] >= minimumAmount;
        }
    }
    ```

-   **msg.data**: The complete calldata (input data) of the transaction:

    -   Contains the function signature and arguments
    -   Used in advanced use cases and proxies

    ```solidity
    contract DataExample {
        bytes public lastCallData;

        // Store the raw calldata of the latest transaction
        function recordCallData() public {
            lastCallData = msg.data;
        }

        // View the size of the calldata
        function getCallDataSize() public view returns (uint256) {
            return lastCallData.length;
        }
    }
    ```

##### Block Information Variables

-   **block.timestamp**: The current block's timestamp (seconds since Unix epoch):

    -   Can be used for time-based logic
    -   Don't rely on it for precise timing (miners have some flexibility)

    ```solidity
    contract TimestampExample {
        uint256 public contractCreationTime;

        constructor() {
            contractCreationTime = block.timestamp;
        }

        // Check if a specified duration has passed since contract creation
        function hasDurationPassed(uint256 durationInSeconds) public view returns (bool) {
            return (block.timestamp >= contractCreationTime + durationInSeconds);
        }

        // Create a simple time lock that releases after a specified date
        function isTimeLockExpired(uint256 releaseTime) public view returns (bool) {
            return block.timestamp >= releaseTime;
        }
    }
    ```

-   **block.number**: The current block number:

    -   Useful for counting blocks or implementing block-based logic

    ```solidity
    contract BlockNumberExample {
        uint256 public deploymentBlockNumber;

        constructor() {
            deploymentBlockNumber = block.number;
        }

        // Calculate how many blocks have been mined since deployment
        function getBlocksPassed() public view returns (uint256) {
            return block.number - deploymentBlockNumber;
        }

        // Check if enough blocks have passed for a specific action
        function hasReachedBlockThreshold(uint256 blockThreshold) public view returns (bool) {
            return getBlocksPassed() >= blockThreshold;
        }
    }
    ```

Combining Context Variables in a Contract:

```solidity
contract TimeLockedWallet {
    address public owner;
    uint256 public unlockTime;

    event Deposit(address indexed sender, uint256 amount, uint256 timestamp);
    event Withdrawal(uint256 amount, uint256 timestamp);

    constructor(uint256 _unlockDuration) {
        owner = msg.sender;
        unlockTime = block.timestamp + _unlockDuration;
    }

    // Accept deposits from anyone
    function deposit() public payable {
        require(msg.value > 0, "Must deposit some ETH");
        emit Deposit(msg.sender, msg.value, block.timestamp);
    }

    // Only allow the owner to withdraw after the unlock time
    function withdraw() public {
        require(msg.sender == owner, "You are not the owner");
        require(block.timestamp >= unlockTime, "Funds are still locked");
        require(address(this).balance > 0, "No funds to withdraw");

        uint256 balance = address(this).balance;
        payable(owner).transfer(balance);

        emit Withdrawal(balance, block.timestamp);
    }

    // Check if withdrawal is possible yet
    function withdrawalStatus() public view returns (bool canWithdraw, uint256 remainingTime) {
        if (block.timestamp >= unlockTime) {
            return (true, 0);
        } else {
            return (false, unlockTime - block.timestamp);
        }
    }
}
```

##### Control Structures: Making Decisions

-   **Conditionals (if/else)**: Conditionals let your code make decisions:

    ```solidity
    function checkValue(uint256 value) public pure returns (string memory) {
        if (value > 100) {
            return "Value is greater than 100";
        } else if (value == 100) {
            return "Value is exactly 100";
        } else {
            return "Value is less than 100";
        }
    }
    ```

-   **Loops**: Loops repeat code until a condition is met:

    ```solidity
    function sumArray(uint256[] memory numbers) public pure returns (uint256) {
    uint256 total = 0;

        for (uint i = 0; i < numbers.length; i++) {
            total += numbers[i];
        }

        return total;

    }
    ```

> Note:
>
> Be careful with loops in Solidity because each operation costs gas, and loops with too many iterations can exceed block gas limits. This is known as a denial of service (DoS).

##### Error Handling and Requirements

-   **Require Statements**: require checks a condition and reverts the transaction if it fails. The second parameter is an error message that helps users understand what went wrong.

    ```solidity
    function withdraw(uint256 amount) public {
        require(balances[msg.sender] >= amount, "Insufficient balance");
        balances[msg.sender] -= amount;
        payable(msg.sender).transfer(amount);
    }
    ```

-   **Custom Errors**: For gas efficiency, you can define custom errors. This is what you should do fpr errors, rather than using the _require_ keyword:

    ```solidity
    error InsufficientBalance(address user, uint256 balance, uint256 withdrawAmount);

    function withdraw(uint256 amount) public {
        if (balances[msg.sender] < amount) {
            revert InsufficientBalance(msg.sender, balances[msg.sender], amount);
        }
        balances[msg.sender] -= amount;
        payable(msg.sender).transfer(amount);
    }
    ```

##### Events: Communicating with the Outside World

Events in Solidity are like announcements that your contract makes when something important happens. Events should be emitted when the contract state is updated:

```solidity
contract Token {
    event Transfer(address indexed from, address indexed to, uint256 amount);

    mapping(address => uint256) public balances;

    function transfer(address to, uint256 amount) public {
        require(balances[msg.sender] >= amount, "Insufficient balance");

        balances[msg.sender] -= amount;
        balances[to] += amount;

        emit Transfer(msg.sender, to, amount);
    }
}
```

> Note:
>
> The indexed keyword makes it easier to search for specific events later.

##### Modifiers: Reusable Function Conditions

Modifiers are a way to create reusable logic for your functions:

```solidity
contract Owned {
    address public owner;

    constructor() {
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Not the owner");
        _; // This placeholder is replaced with the function code
    }

    function setOwner(address newOwner) public onlyOwner {
        owner = newOwner;
    }
}
```

The _ in the modifier represents where the function code will be executed. For example, if the _ is before the modifier logic, the function will be executed before the modifier logic.

##### Interfaces

Interfaces in Solidity act as blueprints that define what functions a contract must implement without specifying how those functions work. Interfaces cannot contain function implementations, state variables, constructors, or inheritance from other contracts. They can only declare function signatures (name and input). When a contract implements an interface using the "is" keyword, it must include all the functions defined in that interface with matching signatures.

For example, imagine we have an interface called IPayable:

```solidity
interface IPayable {
    function pay(address recipient, uint256 amount) external returns (bool);
    function getBalance(address account) external view returns (uint256);
}
```

Any contract that implements this interface must include these exact functions:

```solidity
contract PaymentProcessor is IPayable {
    mapping(address => uint256) private balances;

    function pay(address recipient, uint256 amount) external override returns (bool) {
        require(balances[msg.sender] >= amount, "Insufficient balance");
        balances[msg.sender] -= amount;
        balances[recipient] += amount;
        return true;
    }

    function getBalance(address account) external view override returns (uint256) {
        return balances[account];
    }
}
```

This standardization makes interfaces particularly useful for interacting with unknown contracts—you only need to know what functions you can call, not how they're implemented, enabling different contracts to communicate consistently and predictably. It tells calling contracts what functions are available on another contract and how to call those functions.

#### Programming Best Practices for Solidity

1. Keep it Simple: Complex code is harder to secure
2. Check conditions before changing state: Validate all inputs with require or custom errors
3. Use descriptive variable and function names: Make your code easy to understand
4. Comment your code: Explain why you're doing something, not just what
5. Follow naming conventions:
    - Contracts: PascalCase (like SimpleToken)
    - Functions/variables: camelCase (like balanceOf)
    - Private/internal state variables: prefix with \_ (like \_owner)
6. Be aware of gas costs: Every operation costs money in the form of gas

#### What Is ABI (Application Binary Interface)?

The ABI is like a smart contract's instruction manual that tells applications exactly how to talk to your contract on the blockchain.

It describes, using structured data, exactly what functions and data types are available for use in the contract and how to “call” or use them.

##### Why do we need ABIs?

Imagine you have a smart contract deployed on Ethereum. Your contract might have functions like transfer, approve, or getBalance. But how does a website or another application know:

-   What functions exist in your contract?
-   What parameters each function needs?
-   What data types to expect in return?

This is where the ABI comes in! It bridges the gap between your human-readable smart contract code and the binary data that the blockchain understands.

##### Purpose of the ABI

The ABI serves as a standardized way to:

-   Call functions in a smart contract
-   Encode function arguments
-   Decode return data
-   Interact with contracts from outside the blockchain (like through web applications)

Think of the ABI as a contract's "API documentation" but in a machine-readable format.

##### ABI Format

The ABI is a JSON array that describes everything publically visible on a contract.

Let's take a simple example. Take a simple smart contract Simple Math that has one function add that adds two numbers together:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
​
contract SimpleMath {
    // Adds two numbers and returns the result
    function add(uint256 a, uint256 b) public pure returns (uint256) {
        return a + b;
    }
}
```

When you compile this contract, you will get the following ABI in JSON format:

```json
[
	{
		"inputs": [
			{ "internalType": "uint256", "name": "a", "type": "uint256" },
			{ "internalType": "uint256", "name": "b", "type": "uint256" }
		],
		"name": "add",
		"outputs": [{ "internalType": "uint256", "name": "", "type": "uint256" }],
		"stateMutability": "pure",
		"type": "function"
	}
]
```

The ABI tells you that there is a function named _add_ which takes two numbers (of type _uint256_) and returns a number.

##### Using ABI in Practice

When you deploy a contract, you'll need its ABI to interact with it later.

Frontend applications use the ABI to format calls to your contract correctly. The following JavaScript example demonstratess how you would use the ABI to interact with a smart contract (using the ethers.js library):

```js
// JavaScript example using ethers.js
const contract = new ethers.Contract(contractAddress, contractABI, provider); // You need to pass the ABI!
await contract.deposit(100); // Calls the deposit function
```

### Smart Contract Libraries and Inheritance

Libraries and Inheritance concepts allow you to reuse code and use external dependencies, making your contracts cleaner, more secure, and cheaper to deploy.

When to Use Each

| Feature     | Best For                         | Key Benefit                        | Limitation                                  |
| ----------- | -------------------------------- | ---------------------------------- | ------------------------------------------- |
| Libraries   | Utility functions and operations | Deploy once, use in many contracts | Cannot store state variables                |
| Inheritance | Extending existing contracts     | Build on top of established code   | Can create complexity with multiple parents |

#### Smart Contract Libraries

Libraries are reusable pieces of code that you can share across multiple contracts. Think of them as toolboxes containing helpful functions that your contracts can use.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;
​
library MathUtils {
    // Find the smaller of two numbers
    function min(uint256 a, uint256 b) internal pure returns (uint256) {
        return a < b ? a : b;
    }
​
    // Find the larger of two numbers
    function max(uint256 a, uint256 b) internal pure returns (uint256) {
        return a > b ? a : b;
    }
}
```

There are two ways to use library functions:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;
​
import "./MathUtils.sol"; // Import the library
​
contract Calculator {
    // Attach library functions to uint256
    using MathUtils for uint256;
​
    // Method 1: Call as a library function
    function getMinimum(uint256 a, uint256 b) public pure returns (uint256) {
        return MathUtils.min(a, b);
    }
​
    // Method 2: Call as an attached function
    function getMaximum(uint256 a, uint256 b) public pure returns (uint256) {
        return a.max(b); // Same as MathUtils.max(a, b)
    }
}
```

##### Benefits of Libraries

-   Write Once, Use Everywhere: Define code in one place and use it in multiple contracts
-   Gas Savings: Libraries with internal functions get embedded in your contract's bytecode, while external library functions are deployed separately and can be reused by many contracts
-   No State Variables: Libraries cannot have state variables, making them perfect for pure utility functions

##### Types of Libraries

1. Embedded Libraries: Use internal functions that get copied into your contract's code
2. Linked Libraries: Use external and public functions. These functions don't get copied into your contract's bytecode - instead, your contract makes calls to the deployed library.

#### Contract Inheritance

Inheritance lets one contract build upon another. Think of it like building with blocks—you start with a foundation and add more features.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;
​
// Base contract with core functionality
contract BaseToken {
    string public name;
    uint256 public totalSupply;
​
    constructor(string memory _name) {
        name = _name;
        totalSupply = 1000000;
    }
​
    function getInfo() public virtual view returns (string memory) {
        return string.concat("Token: ", name);
    }
}
​
// Enhanced contract that inherits and extends BaseToken
contract GoldToken is BaseToken {
    constructor() BaseToken("Gold Token") {}
​
    // Add new functionality
    function getSymbol() public pure returns (string memory) {
        return "GLD";
    }
}
```

In this example, GoldToken inherits all features from BaseToken and adds a new function.

#### Function Overriding

Sometimes, you want to modify behavior from a parent contract. This is where function overriding comes in.

To override a function:

1. Mark the parent function with virtual (meaning "can be changed")
2. Mark the child function with override (meaning "I'm changing this")

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;
​
contract BaseToken {
    // The virtual keyword allows this function to be overridden
    function getTokenName() public virtual pure returns (string memory) {
        return "BaseToken";
    }
}
​
contract CustomToken is BaseToken {
    // The override keyword shows we're replacing the parent's function
    function getTokenName() public override pure returns (string memory) {
        return "CustomToken";
    }
}
```

##### Using _super_ to Call Parent Functions

Sometimes, you want to extend a function rather than completely replace it:

```solidity
contract ExtendedToken is BaseToken {
    function getTokenName() public override pure returns (string memory) {
        // Call the parent function and add to it using the super keyword
        return string.concat(super.getTokenName(), " Plus");
        // Returns "BaseToken Plus"
    }
}
```

##### Multiple Inheritance

Solidity allows a contract to inherit from multiple parents, but this requires careful handling.

```solidity
contract Mintable {
    function canMint() public virtual pure returns (bool) {
        return true;
    }
}
​
contract Burnable {
    function canBurn() public virtual pure returns (bool) {
        return true;
    }
}
​
// Inherit from both contracts
contract Token is Mintable, Burnable {
    // This contract now has both canMint() and canBurn() functions
}
```

When multiple parents have functions with the same name, you must specify which ones you're overriding:

```solidity
contract BaseA {
    function getValue() public virtual pure returns (string memory) {
        return "A";
    }
}
​
contract BaseB {
    function getValue() public virtual pure returns (string memory) {
        return "B";
    }
}
​
// Multiple inheritance with function name conflict
contract Combined is BaseB, BaseA {
    // Must specify all contracts being overridden
    function getValue() public override(BaseB, BaseA) pure returns (string memory) {
        return "Combined";
    }
}
```

The order in which you list parent contracts is important:

```solidity
// BaseB comes first in the inheritance list
contract TokenX is BaseB, BaseA {
    function getValue() public override(BaseB, BaseA) pure returns (string memory) {
        // This calls BaseB's implementation first
        return super.getValue(); // Returns "B"
    }
}
​
// BaseA comes first in the inheritance list
contract TokenY is BaseA, BaseB {
    function getValue() public override(BaseA, BaseB) pure returns (string memory) {
        // This calls BaseA's implementation first
        return super.getValue(); // Returns "A"
    }
}
```

#### Using OpenZeppelin Contracts

One of the most common uses of inheritance is extending standardized contracts from dependencies like [OpenZeppelin](https://www.openzeppelin.com/):

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;
​
import { ERC20 } from "@openzeppelin/contracts/token/ERC20/ERC20.sol";
​
// Create a custom token by inheriting from ERC20
contract MyToken is ERC20 {
    constructor() ERC20("My Token", "MTK") {
        // Mint 1 million tokens to the deployer
        _mint(msg.sender, 1000000 * 10**18);
    }
​
    // Add custom features
    function burn(uint256 amount) public {
        _burn(msg.sender, amount);
    }
}
```

Inheritance lets you customize standard behavior:

```solidity
contract FeeToken is ERC20 {
    address public feeCollector;
​
    constructor(address _feeCollector) ERC20("Fee Token", "FEE") {
        feeCollector = _feeCollector;
        _mint(msg.sender, 1000000 * 10**18);
    }
​
    // Override the transfer function to add a 1% fee
    function transfer(address to, uint256 amount) public override returns (bool) {
        uint256 fee = amount / 100; // 1% fee
        uint256 netAmount = amount - fee;
​
        // Send fee to collector
        super.transfer(feeCollector, fee);
​
        // Send remaining amount to recipient
        return super.transfer(to, netAmount);
    }
}
```

##### OpenZeppelin Contracts Wizard

The [OpenZeppelin Contract Wizard](https://wizard.openzeppelin.com/) is a helpful tool that generates customized smart contracts with just a few clicks. It's perfect for:

-   Creating token contracts (ERC20, ERC721, ERC1155)
-   Adding security features
-   Implementing access control
-   Setting up governance

The wizard generates production-ready code that you can customize further.

#### You can import code from various sources:

```solidity
// Import from npm package
import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
​
// Import specific contracts
import {ERC721, IERC721} from "@openzeppelin/contracts/token/ERC721/ERC721.sol";
​
// Import from local file
import "./MyContract.sol";
```

#### Key Takeaways

-   Libraries are great for utility functions and can be used across many contracts.
-   Inheritance lets you build on existing contracts and extend their functionality
    -   Use _virtual_ in parent contracts and _override_ in child contracts when overriding functions
    -   The _super_ keyword lets you call parent implementations
    -   With multiple inheritance, the order of parent contracts matters
    -   OpenZeppelin provides battle-tested contracts you can extend

#### Best Practices

-   Keep it Simple: Avoid deep inheritance chains that are hard to follow
-   Document Function Overrides: Clearly comment what you're changing and why
-   Be Careful with Multiple Inheritance: It can create unexpected behavior if not managed properly
-   Reuse Trusted Code: Whenever possible, build on well-audited contracts like those from OpenZeppelin

### Testnet Funds

We will use Chainlink's services in smart contracts. We will deploy and interact with these contracts on testnets. Usually, this will be Sepolia, but in some instances, we will also use Base Sepolia.

Let's walk through how to get some testnet LINK, how to add LINK to MetaMask and how to send your tokens to another address

#### Adding LINK to Metamask

-   Head to the [Chainlink documentation](https://docs.chain.link/resources/link-token-contracts) and scroll down to the chain you want to use LINK on. E.g. [Sepolia Testnet](https://docs.chain.link/resources/link-token-contracts#sepolia-testnet)
-   Click the **Add to wallet** button to import the LINK token to NetaMask. Note that this will only add the token for that secific network. This will need to be repeated for all networks you intend to use LINK tokens.

    <img src='./images/smart-contract-solidity-fundamentals/testnet-funds/add-to-wallet.png' alt='add-to-wallet' />

-   Click Add token to add the token to your MetaMask

    <img src='./images/smart-contract-solidity-fundamentals/testnet-funds/add-token.png' alt='add-token' />

#### Using a LINK faucet

To obtain testnet LINK, head to the [Chainlink Faucet](https://faucets.chain.link/) page. Here, you will find a list of all supported networks.

Click the Link button at the top right, select the network(s) you want to get LINK on and click \*\*Continue. "

<img src='./images/smart-contract-solidity-fundamentals/testnet-funds/select-networks-link.png' alt='select-networks-link' />

-   Click **Get tokens** and click **Confirm** in MetaMask to sign the transaction.
-   Once the transaction has confirmed, you can see the **LINK** balance in your wallet.

    <img src='./images/smart-contract-solidity-fundamentals/testnet-funds/link-sent.png' alt='link-sent' />

    <img src='./images/smart-contract-solidity-fundamentals/testnet-funds/link-metamask.png' alt='link-metamask' />

#### Adding a token to MetaMask

LINK was easy to add to MetaMask since Chainlink included that cheeky **Add to wallet** button. But how do we add other tokens, such as USDC to MetaMask?

-   Find the token address. For USDC, for example, we can head to the [Circle documentation](https://developers.circle.com/stablecoins/usdc-on-test-networks) to find the address of the USDC contract on different chains. Copy the address for the USDC token on the chain you are working on.

-   Open MetaMask and check you are on the same chain as the token address you just copied by clicking on the network button on the top left corner in MetMask

    <img src='./images/smart-contract-solidity-fundamentals/testnet-funds/select-a-network.png' alt='select-a-network' />

-   Click the **Tokens** tab, click the vertical three dots button, and then **Import tokens**

    <img src='./images/smart-contract-solidity-fundamentals/testnet-funds/token-tab.png' alt='token-tab' />

-   Enter the token contract address. MetaMask will automatically detect your token and its related information if it follows the ERC-20 standard.

-   Check the correct information (Address, Token Symbol, and Decimals).

    <img src='./images/smart-contract-solidity-fundamentals/testnet-funds/import-usdc.png' alt='import-usdc' />

-   Click **Next** to confirm the information. Click Import to import your token to MetaMask. This lets you view your balance and send tokens to others using the MetaMask UI.

#### Sending tokens to another address

-   Click on the **Tokens** tab, then click the token you want to send (you need to have imported it already), e.g., the LINK token.

    <img src='./images/smart-contract-solidity-fundamentals/testnet-funds/mm-link-token.png' alt='mm-link-token' />

-   Click the Send button.

    <img src='./images/smart-contract-solidity-fundamentals/testnet-funds/send-link.png' alt='send-link' />

-   Paste the address you want to send the tokens to.

    <img src='./images/smart-contract-solidity-fundamentals/testnet-funds/send-to.png' alt='send-to' />

-   Enter the amount and then click Next

    <img src='./images/smart-contract-solidity-fundamentals/testnet-funds/send-amount.png' alt='send-amount' />

-   To sign the transaction, click Confirm. This transaction will now be sent and the tokens will be transferred to the address you specified.

    <img src='./images/smart-contract-solidity-fundamentals/testnet-funds/confirm-transaction.png' alt='confirm-transaction' />

### Introduction To Remix

[Remix](https://remix.ethereum.org/) is an online Integrated Development Environment (IDE) specifically designed for smart contract development. It provides an easy-to-use interface for writing, compiling, deploying, and debugging Solidity smart contracts. Remix is widely used because of its simplicity, built-in tools, and browser-based functionality, allowing you to get started quickly without installing additional software.

#### Key Features of Remix:

-   File Explorer: Manage your contract files and projects.
-   Solidity compiler: Compile and check smart contracts for errors.
-   Deploy & run transactions: Deploy contracts to local or public EVM networks and interact with them.
-   Debugger: Step through transactions to debug execution.
-   Plugin System: Extend Remix’s functionality with additional tools.

#### Navigating Remix

-   File Explorer (left panel): Manage your workspaces, create new files, and organize contracts.
-   Editor (center panel): Write and modify Solidity smart contracts.
-   Terminal (bottom panel): View logs, compilation messages, and transaction details.
-   Solidity Compiler (sidebar): Compile smart contracts with different Solidity versions.
-   Deploy & Run Transactions (sidebar): Deploy contracts to a blockchain and interact with them.

<img src='./images/smart-contract-solidity-fundamentals/remix/remix.png' alt='remix' />

#### Creating Workspaces, Folders, and Files

Remix allows you to create different workspaces to organize your projects. This allows you to keep related files grouped together. A bit like a GitHub repository or a project folder on your comoputer.

1. Create a new Workspace in Remix:

    - At the top of the **File Explorer** panel, click on the Workspaces actions button (the burger icon)
    - Select **Create blank** to create a new workspace.
    - Give a name for your workspace. In this example, we will use the name "CLF".
    - Hit **Ok**

    <img src='./images/smart-contract-solidity-fundamentals/remix/create-blank-workspace.png' alt='create-blank-workspace' />

2. Create a new folder:

    - Right-click in the file explorer sidebar and click New Folder
    - Name the folder _contracts_

3. Create a new file:

    - Right-click on the contr_acts folder and click New file
    - Name the file _MyERC20.sol_

    <img src='./images/smart-contract-solidity-fundamentals/remix/file-explorer.png' alt='file-explorer' />

4. Write the code in your new file!

#### Compiling Smart Contracts

Before deploying a smart contract, you must compile it to check for errors and generate the necessary bytecode.

1. Open the Solidity Compiler panel in Remix.
2. Select the correct Solidity version (matching your contract).
3. Ensure your contract file is still open in the main window and click **Compile** [YourContract.sol].
4. If there are errors, they will appear in the terminal.

 <img src='./images/smart-contract-solidity-fundamentals/remix/solidity-compiler-tab.png' alt='solidity-compiler-tab' />

5. You can also compile by hitting _Cmd + S_ on your Mac keyboard or _Ctrl + S_ on Windows.

    - If there are no errors, the contract will compile successfully, and you’ll see a green checkmark.

6. When we compile our contract, the compiler generates the ABI! To get the ABI of a contract (e.g., for verification or a front-end application) scroll down in the Solidity Compiler tab and blick the ABI button:

 <img src='./images/smart-contract-solidity-fundamentals/remix/remix-abi.png' alt='remix-abi' />

#### Connecting to MetaMask

To deploy a contract on a real blockchain (e.g., Ethereum Mainnet, Sepolia, or Polygon), you need to connect Remix to MetaMask.

1. Open MetaMask and make sure you are on the desired network.
2. Go to the Deploy & Run Transactions panel in Remix.
3. Select Injected Provider - MetaMask as the environment.
4. MetaMask will prompt you to connect your wallet and approve the connection.

<img src='./images/smart-contract-solidity-fundamentals/remix/metamask-environment.png' alt='metamask-environment' />

#### Deploying Smart Contracts

Once compiled, a smart contract can be deployed on a local blockchain (e.g., Remix VM) or a real network (e.g., Ethereum).

1. Open the Deploy & Run Transactions panel.
2. Select a deployment environment (e.g., Remix VM for testing or Injected Provider - MetaMask for live networks).

 <img src='./images/smart-contract-solidity-fundamentals/remix/metamask-environment.png' alt='metamask-environment' />

3. Choose the correct contract from the dropdown.
4. Click Deploy and confirm the transaction in MetaMask to deploy your contract.

 <img src='./images/smart-contract-solidity-fundamentals/remix/deploy.png' alt='deploy' />

#### Pinning Deployed Smart Contracts

After deployment, Remix allows you to pin a deployed contract to your workspace so you can continue interacting with it even after refreshing the page. Click the pin icon to pin the deployed contract.

<img src='./images/smart-contract-solidity-fundamentals/remix/pin-contract.png' alt='pin-contract' />

#### The contract address

1. In the **Deploy & Run Transactions** panel, find the deployed contract under Deployed Contracts.
2. Click the **copy icon** to save the contract address.
3. If needed, use the **At Address** button to reattach the contract after a page refresh.

#### Interacting with Smart Contracts

Once deployed, you can interact directly with your smart contract from Remix.

1. Find the deployed contract in the Deploy & Run Transactions panel.
2. Click the dropdown to see available functions.
3. Enter any required parameters and click the transact button.
4. Confirm transactions in MetaMask (for state-changing functions).
5. View results in the Remix terminal.

<img src='./images/smart-contract-solidity-fundamentals/remix/remix-interact.png' alt='remix-interact' />

### Writing an ERC20 Token Contract

[ERC-20](https://eips.ethereum.org/EIPS/eip-20) is a technical standard for fungible tokens on the Ethereum blockchain. "ERC" stands for "Ethereum Request for Comment," the naming convention for Ethereum improvement proposals. The "20" refers to the proposal number that created this standard. An ERC-20 token is a cryptocurrency that follows specific rules defined in this standard, including functions for transferring tokens, checking balances, and approving spending by third parties. These standardized functions ensure that ERC-20 tokens work consistently across the Ethereum ecosystem, allowing them to be easily exchanged, stored in wallets, and traded on decentralized exchanges. Popular examples include USDT (Tether), USDC, DAI, and many other cryptocurrencies that operate on Ethereum.

#### Writing a Simple ERC20 Token

1.  Create a new Workspace in Remix:

    -   At the top of the File Explorer panel, click on the Workspaces actions button (the burger icon)
    -   Select Create blank to create a new workspace.
    -   Give a name for your workspace. In this example, we will use the name "CLF".
    -   Hit Ok

    <img src='./images/smart-contract-solidity-fundamentals/erc-20/create-blank-workspace.png' alt='create-blank-workspace' />

2.  Create a new folder:

    -   Name the folder contracts
    -   Right-click in the file explorer sidebar and click New Folder

3.  Create a new file:

    -   Right-click on the contracts folder and click New file
    -   Name the file MyERC20.sol

    <img src='./images/smart-contract-solidity-fundamentals/erc-20/file-explorer.png' alt='file-explorer' />

4.  Write the code!

    -   Copy and Paste the following token contract code: You could also create your token using the Openzeppelin Wizard.

        ```solidity
        // SPDX-License-Identifier: MIT
        pragma solidity ^0.8.26;

        import { ERC20 } from "@openzeppelin/contracts@5.2.0/token/ERC20/ERC20.sol";

        contract MyERC20 is ERC20 {
            constructor() ERC20("My Cyfrin CLF Token", "CLF") {}

            function mint(address to, uint256 amount) public {
                _mint(to, amount);
            }
        }
        ```

    -   Here, we have inherited the _ERC20_ smart contract from OpenZeppelin by importing it and then inheriting it using the is keyword.

    -   Then, in the _MyERC20_ constructor, we have invoked the parent _ERC20_ constructor:

        ```solidity
        constructor() ERC20("My Cyfrin CLF Token", "CLF") {}
        ```

    -   Finally, we created a way to create a supply of tokens by adding a public _mint_ function:

        ```solidity
        function mint(address to, uint256 amount) public {
            _mint(to, amount);
        }
        ```

#### Adding Access Control

We can add access control to smart contracts using **modifiers**. We also can use dependencies, including OpenZeppelin.

We have written a simple token smart contract called _MyERC20_. This contract can create new tokens via the _mint_ function. However, currently, anyone can call this function and mint tokens (for free!). Let's change this.

Let's add the ability to give roles to certain addresses to give them permission to mint tokens.

#### Adding Access Control Using OpenZeppelin

To add access control to our ERC-20 token contract, we will use OpenZeppelin's AccessControl smart contract. We need to import this contract and then inherit from it.

Then, we can use the state variables, functions, and modifiers declared inside the AccessControl smart contract from within our ERC-20 contract.

OpenZeppelin's AccessControl smart contract provides the ability to create [Roles](https://docs.openzeppelin.com/contracts/2.x/api/access#Roles).

-   Each new role created needs a role identifier used to grant, revoke, and check if an account has a role.
-   We will create a single role that enables the addresses with this role to mint tokens.
-   This _role identifier_ is a _bytes32 constant_ variable. By convention, this is usually a hashed string of the role name (with the string in capitals since this is a constant):

    ```solidity
    bytes32 public constant MINTER_ROLE = keccak256("MINTER_ROLE");
    ```

-   This role identitifier can now be passed to the functions in `AccessControl` to grant roles with the `AccessControl::grantRole` function, check whether an address has a role with the `AccessControl::hasRole` function, and revoke a role using the `AccessControl::revokeRole` function by passing in the role identifier e.g. `MINTER_ROLE` and the required address to grant/check/revoke the role for.

#### Adding a Minter Role to a Token Contract

1.  Inherit the AccessControl contract:

    ```solidity
    // SPDX-License-Identifier: MIT
    pragma solidity ^0.8.19;

    import { ERC20 } from "@openzeppelin/contracts@4.6.0/token/ERC20/ERC20.sol";
    import { AccessControl } from "@openzeppelin/contracts@4.6.0/access/AccessControl.sol";

    contract MyERC20 is ERC20, AccessControl {
        constructor() ERC20("My Cyfrin CLF Token", "CLF") {}

        function mint(address to, uint256 amount) public {
        _mint(to, amount);
        }
    }
    ```

2.  Create a bytes32 constant state variable containing the hashed name of the role–the role identifier:

    ```solidity
    // SPDX-License-Identifier: MIT
    pragma solidity ^0.8.19;

    import { ERC20 } from "@openzeppelin/contracts@4.6.0/token/ERC20/ERC20.sol";
    import { AccessControl } from "@openzeppelin/contracts@4.6.0/access/AccessControl.sol";

    contract MyERC20 is ERC20, AccessControl {
        bytes32 public constant MINTER_ROLE = keccak256("MINTER_ROLE");

        constructor() ERC20("My Cyfrin CLF Token", "CLF") {}

        function mint(address to, uint256 amount) public {
            _mint(to, amount);
        }
    }
    ```

3.  Now, we can add the onlyRole modifier from the AccessControl contract to our mint function (since our token contract is inheriting this contract) to add a check that the msg.sender has the role that we pass to the modifier.

    ```solidity
    // SPDX-License-Identifier: MIT
    pragma solidity ^0.8.19;

    import { ERC20 } from "@openzeppelin/contracts@4.6.0/token/ERC20/ERC20.sol";
    import { AccessControl } from "@openzeppelin/contracts@4.6.0/access/AccessControl.sol";

    contract MyERC20 is ERC20, AccessControl {
        bytes32 public constant MINTER_ROLE = keccak256("MINTER_ROLE");

        constructor() ERC20("My Cyfrin CLF Token", "CLF") {}

        function mint(address to, uint256 amount) public onlyRole(MINTER_ROLE) {
            _mint(to, amount);
        }
    }
    ```

Only addresses with the `MINTER_ROLE` can call the mint function.

However, we do not have a way to give addresses this `MINTER_ROLE`. To do this, we need another role that can grant roles to other addresses. OpenZeppelin's `AccessControl` contract has a role that does this already, called the `DEFAULT_ADMIN` role.

Let's grant this role to the deployer in the constructor. When an address deploys a smart contract, it automatically calls the constructor. Therefore, the deployer is the msg.sender in the context of the constructor.

Let's grant them the `DEFAULT_ADMIN` role and while we are at it, also the `MINTER_ROLE` so they can mint tokens. We can do this by calling the `_grantRole` function, which takes the role and the address to grant the role to:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;
​
import { ERC20 } from "@openzeppelin/contracts@4.6.0/token/ERC20/ERC20.sol";
import { AccessControl } from "@openzeppelin/contracts@4.6.0/access/AccessControl.sol";
​
contract MyERC20 is ERC20, AccessControl {
   bytes32 public constant MINTER_ROLE = keccak256("MINTER_ROLE");
​
    constructor() ERC20("My Cyfrin CLF Token", "CLF") {}
​
    function mint(address to, uint256 amount) public onlyRole(MINTER_ROLE) {
        _mint(to, amount);
    }
}
```

### Deploying an ERC-20 Contract

Now we have written our permissioned ERC-20 token contract, let's deploy it to a live testnet and then add that token to our MetaMask so we can see our balance and use the UI to perform token transfers.

#### Compile & Deploy

1.  Compile your smart contract

    -   Make sure you still have the `MyERC20.sol` file open.
    -   Head to the **Solidity compiler** tab on the left sidebar.
    -   Select the compiler version. Here, we are using version `0.8.19`. Ensure the compiler version is set to `0.8.19` or later, as indicated in the pragma directive (`pragma solidity ^0.8.19`;). The `^` symbol tells the compiler that any version above `0.8.19` can be used. Without the symbol, only version `0.8.19` would be allowed.
    -   Hit **Compile MyERC20.sol** to compile the contract

        <img src='./images/smart-contract-solidity-fundamentals/erc-20/solidity-compiler-tab.png' alt='solidity-compiler-tab' />

    -   You can also compile by hitting `Cmd + S` on your keyboard on Mac or `Ctrl + S` on windows.
    -   If there are no errors, the contract will compile successfully, and you’ll see a green checkmark.

2.  Deploy your smart contract

    -   Head to the **Deploy and run transactions** tab in the left-hand sidebar.
    -   We want to deploy our contract with MetaMask. To connect your wallet to Remix, click **Select Environment** dropdown menu and select **Injected Provider - MetaMask**.
    -   This will open MetaMask and ask to connect to Remix. Click Connect.

        <img src='./images/smart-contract-solidity-fundamentals/erc-20/metamask-environment.png' alt='metamask-environment' />

    -   In the Contract dropdown, select MYERC20.sol.

        <img src='./images/smart-contract-solidity-fundamentals/erc-20/deploy.png' alt='deploy' />

    -   Click **Deploy**.

    -   Sign the transaction in Metamask to deploy your smart contract!

You will know if your contract has been successfully deployed if:

-   The Remix terminal window (the block at the bottom of your screen) shows a green tick next to the transaction information:

    <img src='./images/smart-contract-solidity-fundamentals/erc-20/remix-terminal.png' alt='remix-terminal' />

-   Your MetaMask shows a successful contract deployment transaction:

    <img src='./images/smart-contract-solidity-fundamentals/erc-20/contract-deployment.png' alt='contract-deployment' />

-   And your contract is in the Deployed contracts section on Remix:

    <img src='./images/smart-contract-solidity-fundamentals/erc-20/deployed-contract.png' alt='deployed-contract' />

> Note: We are going to be using this ERC-20 contract we deployed in the next section so double check you have pinned the contract so you can use it again later. It may also be worth copying the address and keeping a note of it somewhere.

<img src='./images/smart-contract-solidity-fundamentals/erc-20/copy-address.png' alt='copy-address' />

#### Check Total Token Supply and Balances

1.  Once your contract is deployed, you can view it in the Deployed Contracts section. Here, you will find all the functions available within the contract, allowing you to interact with them directly.

2.  You can pin your contract by clicking on the pin icon. This will save your contracts if you want to close Remix and continue another time. Note that pinning is workspace-specific, so make sure you open the same Remix workspace when you need to access this token contract.

    <img src='./images/smart-contract-solidity-fundamentals/erc-20/pin-contract.png' alt='pin-contract' />

3.  To check the total supply of your token, call the totalSupply function.

    If you want to view the balance of a specific wallet, use the balanceOf function and pass the wallet address as a parameter. Currently, your token will not have any tokens minted, so the return value will be 0 for both function calls.

    <img src='./images/smart-contract-solidity-fundamentals/erc-20/0-total-supply.png' alt='0-total-supply' />

#### Adding Your Token to MetaMask

If you deployed your contract on a testnet, you can add your token to MetaMask by following these steps:

1. Copy the token contract address. In Remix, you can find this address in the Deployed Contracts section.
   <img src='./images/smart-contract-solidity-fundamentals/erc-20/copy-address.png' alt='copy-address' />

2. Open your MetaMask wallet and click on the Tokens Tab. Hit the three vertical dots icon on the right hand side and click Import tokens.
   <img src='./images/smart-contract-solidity-fundamentals/erc-20/token-tab.png' alt='token-tab' />

3. Enter the token contract address. MetaMask will automatically detect your token and its related information since it follows the ERC-20 standard.
4. Check if the information is correct (Address, Token Symbol, and Decimals).
   <img src='./images/smart-contract-solidity-fundamentals/erc-20/import-token.png' alt='import-token' />

5. Click Next to confirm the information. Click Import to import your token to MetaMask. This allows you to view your balance and send tokens to others using the MetaMask UI.

### Interacting with a Contract - Minting Tokens

We have not yet minted any tokens, which means currently the total available supply of our token is `0` - no tokens currently exist! To increase the supply of tokens, we have to call the function `mint` from our contract:

-   Go to the **Deploy & Run Transactions** tab.
-   Find your token and expand the tab to see the contract's functions.

    <img src='./images/smart-contract-solidity-fundamentals/mont-token/functions-on-contract.png' alt='functions-on-contract' />

-   Find the function `mint` and expand it to check the parameters.

    -   `to`: The address receiving the minted tokens. We will create `100` tokens by setting the amount value as `100000000000000000000`. This is because our Token has `18` decimals.
    -   amount: The amount to mint.

        <img src='./images/smart-contract-solidity-fundamentals/mont-token/transact.png' alt='transact' />

-   To send the transaction, click on the transact button and confirm the transaction in your Metamask wallet.
-   Now, you can check to see if your balance has increased by calling balanceOf and passing the address you minted the tokens to.
-   You can also check your balance in MetaMask by heading to the Tokens tab since we added our token in the previous lesson.

#### Allowance and Token Approvals

Token approvals enable another address to spend another address's tokens. This feature is commonly used in DeFi applications that need to transfer ERC20 tokens from your wallet to another wallet or contract via another intermediary smart contract.

To use this feature, you can call the `approve` function, specifying the address that will spend your tokens and the amount they are allowed to spend.

<img src='./images/smart-contract-solidity-fundamentals/mont-token/approve.png' alt='approve' />

You can always verify if a contract has permission to spend your tokens by calling the allowance function.

<img src='./images/smart-contract-solidity-fundamentals/mont-token/allowance.png' alt='approve' />
