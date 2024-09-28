# Financial System Contract 
This project includes a Solidity smart contract called Assessment and a React front-end application to interact with the contract. Below is a detailed description of the code

## Overview 
The Financial System contract offers an easy-to-use way to handle money on the Ethereum network. This contract, which is intended for a single owner, makes it easier to do basic financial functions like depositing and withdrawing money. It also provides functions to view the current block number on the blockchain, retrieve the gas price of the most recent transaction, and check the balance.

### Key Features
##### 1. Owner Only Access:
Because the contract is intended to be managed by a single owner, access to its features is safe and regulated.
##### 2. Deposit Event: 
This event records the amount deposited and is emitted when a deposit is made.
##### 3. Withdraw Event: 
This event records the amount deposited and is emitted when a deposit is made.
##### 4. Balance Inquiry: 
At any moment, the owner has access to the contract's current balance.
##### 5. Gas Price: 
Retrieve the gas price of the current transaction to help monitor and manage transaction costs.
##### 6. Current Block Number:
Get the current block number on the Ethereum blockchain, providing context for the timing of transactions. 

## Get Started
### Executing Program 
To run this program, you can use Remix, an online Solidity IDE. To get started, go to the Remix website at Remix Ethereum.


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
Name: Gurnoor Oberoi

## License
This project is licensed under the MIT License - see the LICENSE.md file for details



