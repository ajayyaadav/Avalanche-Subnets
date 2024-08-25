#Avalance-Subnets
#Avalance-subnets

Overview
This Solidity smart contract implements a basic ERC20 token. The contract includes fundamental functionalities such as token transfer, approval, minting, and burning. Additionally, the contract is owned by a single address, which has special permissions to mint and burn tokens.

Key Features:
Token Name: Solidity by Example (SOLBYEX)
Decimals: 18 (standard for ERC20 tokens)
Ownership: The contract has an owner who can mint and burn tokens.
Contract Details
Variables
totalSupply: Tracks the total supply of the token.
balanceOf: A mapping to store the balance of each address.
allowance: A mapping that keeps track of allowances for delegated transfers.
name: The name of the token ("Solidity by Example").
symbol: The symbol of the token ("SOLBYEX").
decimals: The number of decimals the token uses (18).
owner: The address that deploys the contract and has special permissions.
Events
Transfer: Emitted when tokens are transferred from one address to another.
Approval: Emitted when an allowance is set by the token holder.
Modifiers
onlyOwner: Restricts certain functions to be callable only by the owner.
Constructor
constructor: Sets the contract deployer as the owner.
Functions
transfer(address recipient, uint amount): Allows a token holder to transfer tokens to another address.
approve(address spender, uint amount): Approves a spender to spend a specific amount of tokens on behalf of the token holder.
transferFrom(address sender, address recipient, uint amount): Allows a spender to transfer tokens from a sender's account to another address, given that the spender has been approved to do so.
mint(uint amount): Allows the owner to create new tokens and add them to their balance. This increases the total supply of the token.
burn(uint amount): Allows the owner to destroy a certain amount of tokens from their balance. This decreases the total supply of the token.
Usage
Transfer Tokens:

Call the transfer function to move tokens from your address to another.
Approve Spending:

Use approve to authorize another address to spend tokens on your behalf.
Transfer from Approved Allowance:

Use transferFrom to execute a transfer from an approved address (typically used in exchanges).
Mint Tokens:

The owner can mint new tokens using the mint function.
Burn Tokens:

The owner can burn tokens from their balance using the burn function.
Security Considerations
Ownership: The contract owner has powerful privileges, including the ability to mint and burn tokens. It’s crucial to secure the owner's private key.
Allowance Risks: Be cautious with the approve function as it can lead to security issues if misused.
Conclusion
This ERC20 token implementation provides the basic functionality needed for a fungible token. It's a useful starting point for creating custom tokens on the Ethereum blockchain.

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract ERC20 {
    uint public totalSupply;
    mapping(address => uint) public balanceOf;
    mapping(address => mapping(address => uint)) public allowance;
    string public name = "Solidity by Example";
    string public symbol = "SOLBYEX";
    uint8 public decimals = 18;

		event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);

    function transfer(address recipient, uint amount) external returns (bool) {
        balanceOf[msg.sender] -= amount;
        balanceOf[recipient] += amount;
        emit Transfer(msg.sender, recipient, amount);
        return true;
    }

    function approve(address spender, uint amount) external returns (bool) {
        allowance[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(
        address sender,
        address recipient,
        uint amount
    ) external returns (bool) {
        allowance[sender][msg.sender] -= amount;
        balanceOf[sender] -= amount;
        balanceOf[recipient] += amount;
        emit Transfer(sender, recipient, amount);
        return true;
    }

    function mint(uint amount) external {
        balanceOf[msg.sender] += amount;
        totalSupply += amount;
        emit Transfer(address(0), msg.sender, amount);
    }

    function burn(uint amount) external {
        balanceOf[msg.sender] -= amount;
        totalSupply -= amount;
        emit Transfer(msg.sender, address(0), amount);
    }
}
