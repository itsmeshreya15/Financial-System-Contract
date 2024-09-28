### Financial System Contract Overview

The **Financial System Contract** is a Solidity-based smart contract designed for managing financial transactions on the Ethereum blockchain. It is intended for use by a single owner and includes features like depositing, withdrawing, balance inquiry, and retrieving gas prices and block numbers. The contract offers secure financial management with restricted access for enhanced safety. Additionally, the project provides a React-based front-end application for easy interaction with the contract.

---

### Key Features

1. **Owner-Only Access**  
   The contract is designed for a single owner. All key functions related to financial transactions are restricted to the owner, ensuring secure management of the funds.

2. **Deposit Event**  
   A `Deposit` event is triggered whenever the owner deposits ether into the contract. The event logs the deposited amount and the address from which it was sent.

3. **Withdraw Event**  
   A `Withdraw` event is emitted when the owner withdraws ether. This event records the amount withdrawn, providing transparency for all transactions.

4. **Balance Inquiry**  
   The owner can check the contract’s current balance at any time. This feature is crucial for monitoring the financial status of the contract.

5. **Gas Price Retrieval**  
   The contract provides a function to retrieve the gas price of the most recent transaction. This helps the owner monitor and manage transaction costs on the Ethereum network.

6. **Current Block Number**  
   The contract also offers a function to fetch the current block number on the Ethereum blockchain. This provides context for the timing of transactions, which can be useful for record-keeping or time-sensitive operations.

---

### Contract Functions

- **Deposit Function**:  
  Allows the owner to deposit ether into the contract. The function is restricted to the owner and triggers the `Deposit` event upon successful deposit.

- **Withdraw Function**:  
  Enables the owner to withdraw a specified amount of ether. The function checks if the contract has sufficient balance before executing the withdrawal and emits a `Withdraw` event upon completion.

- **Get Balance Function**:  
  Provides the current balance of ether in the contract, accessible only by the owner.

- **Get Gas Price Function**:  
  Retrieves the gas price for the current transaction, allowing the owner to monitor transaction fees.

- **Get Current Block Number Function**:  
  Fetches the current block number on the Ethereum network, useful for tracking the timing of transactions.

---

### Deployment and Interaction

To deploy and interact with this contract, the following steps are recommended:

1. **Open Remix IDE**  
   Navigate to Remix IDE, an online Solidity development environment, to work with the smart contract.

2. **Create and Compile the Contract**  
   Create a new Solidity file, paste the contract code, and compile it using Remix's built-in compiler.

3. **Deploy the Contract**  
   Deploy the contract using Remix, connected to a local blockchain or Ethereum test network (e.g., Rinkeby or Goerli) with MetaMask. Ensure the MetaMask wallet has test ether for transaction fees.

4. **Interact with the Contract**  
   Once deployed, the owner can use Remix or the provided React front-end application to interact with the contract. Functions like `deposit`, `withdraw`, `getBalance`, `getGasPrice`, and `getCurrentBlockNumber` can be executed directly from the UI.

---

### Front-End Application

The project includes a **React** front-end application that integrates with the contract using Web3.js or Ethers.js. This interface allows the owner to:

- Deposit ether into the contract.
- Withdraw ether.
- Check the balance, gas price, and current block number.

The front-end simplifies the interaction with the contract by providing a user-friendly interface for executing financial operations without directly interacting with the Solidity code.

---

### Conclusion

The Financial System contract offers a secure and efficient way to manage funds on the Ethereum blockchain. Its owner-restricted functions and event-driven architecture ensure that all transactions are recorded transparently. The integration of gas price monitoring and block number retrieval further enhances its functionality, making it an ideal contract for basic financial operations in a decentralized environment.


``` Solidity
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.9;

//import "hardhat/console.sol";

contract Assessment {
    address payable public owner;
    uint256 public balance;

    event Deposit(uint256 amount);
    event Withdraw(uint256 amount);


    constructor(uint initBalance) payable {
        owner = payable(msg.sender);
        balance = initBalance;
    }

    function getBalance() public view returns(uint256){
        return balance;
    }

    function deposit(uint256 _amount) public payable {
        uint _previousBalance = balance;

        // make sure this is the owner
        require(msg.sender == owner, "You are not the owner of this account");

        // perform transaction
        balance += _amount;

        // assert transaction completed successfully
        assert(balance == _previousBalance + _amount);

        // emit the event
        emit Deposit(_amount);
    }

    // custom error
    error InsufficientBalance(uint256 balance, uint256 withdrawAmount);

    function withdraw(uint256 _withdrawAmount) public {
        require(msg.sender == owner, "You are not the owner of this account");
        uint _previousBalance = balance;
        if (balance < _withdrawAmount) {
            revert InsufficientBalance({
                balance: balance,
                withdrawAmount: _withdrawAmount
            });
        }

        // withdraw the given amount
        balance -= _withdrawAmount;

        // assert the balance is correct
        assert(balance == (_previousBalance - _withdrawAmount));

        // emit the event
        emit Withdraw(_withdrawAmount);
    }

   function getgasPriceTransaction() public view returns(uint){
      return tx.gasprice;
    }

    function getCurrentBlock() public view returns(uint){
      return block.number;
    }
}
```

After cloning the github, you will want to do the following to get the code running on your computer.

1. Inside the project directory, in the terminal type: npm i
2. Open two additional terminals in your VS code
3. In the second terminal type: npx hardhat node
4. In the third terminal, type: npx hardhat run --network localhost scripts/deploy.js
5. Back in the first terminal, type npm run dev to launch the front-end.

After this, the project will be running on your localhost. 
Typically at http://localhost:3000/

## Author
Name: Shreya Kandpal
## Video Explanation
https://www.loom.com/share/e01b4df0a1ea439db2d580e6e9d3f850?sid=31f76152-8358-4b1b-ac9b-cc1e46e415b3

## License
This project is licensed under the MIT License - see the LICENSE.md file for details



