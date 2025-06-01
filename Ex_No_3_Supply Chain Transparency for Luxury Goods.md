# EX.NO.3: SUPPLY CHAIN TRANSPARENCY FOR LUXURY GOODS
# Name: VIDHYADHARAN R
# Reg No: 212222110053

# Aim:
To develop a smart contract that tracks the supply chain of luxury goods, ensuring authenticity.
# Algorithm:
The manufacturer records product creation details on-chain.


The product moves through different supply chain checkpoints.


The ownership of the product can be transferred securely.


Buyers can verify the product’s authenticity.


# Program:
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract LuxurySupplyChain {
    struct Product {
        string name;
        address currentOwner;
        bool verified;
    }

    mapping(uint256 => Product) public products;

    event ProductRegistered(uint256 productId, string name);
    event OwnershipTransferred(uint256 productId, address newOwner);

    function registerProduct(uint256 productId, string memory name) public {
        require(products[productId].currentOwner == address(0), "Product already registered");
        products[productId] = Product(name, msg.sender, true);
        emit ProductRegistered(productId, name);
    }

    function transferOwnership(uint256 productId, address newOwner) public {
        require(products[productId].currentOwner == msg.sender, "Not the owner");
        products[productId].currentOwner = newOwner;
        emit OwnershipTransferred(productId, newOwner);
    }

    function verifyProduct(uint256 productId) public view returns (string memory, address, bool) {
        Product memory p = products[productId];
        return (p.name, p.currentOwner, p.verified);
    }
}
```
# Expected Output:
![Screenshot 2025-04-28 115016](https://github.com/user-attachments/assets/80bb61d0-2ab7-4cd6-bf91-d1e34b12a82f)

![Screenshot 2025-04-28 115027](https://github.com/user-attachments/assets/2680bdf7-3dd2-4e9d-ad4a-8f16ddab6fd5)

![Screenshot 2025-04-28 115036](https://github.com/user-attachments/assets/dcefb1ae-ea88-45c1-bff2-16775de6634c)

A luxury good (e.g., a Rolex watch) is registered on-chain.


Ownership is transferred at every checkpoint.


Buyers can check the authenticity before purchasing.


# High-Level Overview:
Helps prevent counterfeit luxury goods.


Teaches real-world supply chain use cases.

# RESULT : 

Thus ,the exection of the supply chain Transparency for Luxury Goods has executed Successfully.
