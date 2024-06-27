# Functions-and-Errors---ETH-AVAX-assesment :
## Online Marketplace Smart Contract 
It is a simple smart contract used to implement a Marketplace wherein the owner can add products ,sell them and  remove them as soon as the product is sold.
This contract is used to demonstrate the functionality of require,revert and assert functions.

## Description
- The Marketplace contract represents a basic marketplace for buying and selling products.
- Key features:
   ->It allows the owner (deployer) to add products with names and prices.

   ->Users can buy products by sending the correct amount of Ether.

  ->Unsold products can be removed by the owner.

-Purpose:
  
      ->This contract demonstrates more complex functionality, including state management, ownership control, and transaction handling.
   
      ->It can serve as a foundation for building more sophisticated decentralized applications (DApps) related to e-commerce or marketplaces.

## Program Code :
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Marketplace {
    address public owner;
    struct Product {
        string name;
        uint256 price;
        address payable seller;
        bool isSold;
    }

    mapping(uint256 => Product)public products;
    uint256 public productCount;

    constructor() {
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Caller is not the owner"); 
        _;
    }

    function addProduct(string memory _name, uint256 _price) public {
        require(_price > 0, "Price must be greater than zero"); // Product only added if price greater than 0
        productCount += 1;
        products[productCount] = Product(_name, _price, payable(msg.sender), false);
    }

    function buyProduct(uint256 _productId) public payable {
        Product storage product = products[_productId];
        require(product.price == msg.value, "Incorrect value sent"); // check the price marked correctly for buying
        require(!product.isSold, "Product already sold");
        
        product.isSold = true;
        product.seller.transfer(msg.value);
        
        assert(product.isSold == true); //verify that the product is indeed marked as sold.
    }

    function removeProduct(uint256 _productId) public onlyOwner {
        if (products[_productId].isSold) {
            revert("Cannot remove sold product"); // to check sold products are  not removed 
        }
        delete products[_productId];
    }
}

1. Require 
//require(_price > 0, "Price must be greater than zero");

This line ensures that the product price (_price) is greater than zero before adding a new product to the marketplace.

If the condition is not met, the transaction reverts with the specified error message.

2. Revert
  // if (_value == 0) {
    revert("Value cannot be zero");
}

This snippet checks whether _value is zero.

If it is, the transaction reverts with the custom error message “Value cannot be zero.”

3. Assert
   //assert(product.isSold == true);

This line verifies that the isSold flag for a product is indeed set to true.

If it’s not (which should never happen), the transaction reverts due to an internal error.


## AUTHOR 
PARTH MEHTA 
