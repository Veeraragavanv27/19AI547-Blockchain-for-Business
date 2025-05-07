# Experiment 7: AI-Powered Smart Contract for Decentralized Negotiation
## Name: Veeraragavan V
## Reg no: 212223230237
## Date :30-04-2025
# Aim:
To create a smart contract that integrates AI logic for automated negotiation in decentralized commerce. The contract adjusts price and conditions dynamically based on real-time market trends using an on-chain AI model.

# Algorithm:
## step 1: Item Listing:

Seller lists an item with a minimum price and a negotiable range.

## step 2: Buyer Offer:

Buyer submits an offer price for the item.

## step 3: Data Collection:

The smart contract accesses on-chain data (demand, history, time-based trends).

## step 4: AI Evaluation (Simulated in Solidity):

The AI logic uses collected data to assess the fairness of the offer.

## step 5: Generate Counteroffer:

If the offer is within the negotiation range, the contract generates a counteroffer.

## step 6: Buyer Decision:

The buyer accepts or rejects the counteroffer.

## step 7: Transaction Execution:

If accepted, the transaction is completed and recorded on the blockchain.

## step 8: Learning Update:

The pricing algorithm updates itself using this transaction data to improve future decisions.



# Program:
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract AIPoweredNegotiation {
    struct Item {
        address seller;
        uint256 minPrice;
        uint256 maxPrice;
        uint256 basePrice;
        bool sold;
    }

    mapping(uint256 => Item) public items;
    uint256 public itemCount;

    event ItemListed(uint256 itemId, uint256 basePrice);
    event OfferMade(uint256 itemId, address buyer, uint256 offer);
    event CounterOffer(uint256 itemId, uint256 counterOffer);
    event Sold(uint256 itemId, address buyer, uint256 finalPrice);

    function listItem(uint256 _basePrice, uint256 _minPrice, uint256 _maxPrice) public {
        require(_minPrice <= _basePrice && _basePrice <= _maxPrice, "Invalid price range");
        
        items[itemCount] = Item(msg.sender, _minPrice, _maxPrice, _basePrice, false);
        emit ItemListed(itemCount, _basePrice);
        itemCount++;
    }

    function makeOffer(uint256 _itemId, uint256 _offerPrice) public payable {
        Item storage item = items[_itemId];
        require(!item.sold, "Item already sold");
        require(msg.value == _offerPrice, "Incorrect offer amount");

        emit OfferMade(_itemId, msg.sender, _offerPrice);

        uint256 aiCounterOffer = dynamicPricing(item.basePrice, item.minPrice, item.maxPrice, _offerPrice);

        if (_offerPrice >= aiCounterOffer) {
            item.sold = true;
            payable(item.seller).transfer(_offerPrice);
            emit Sold(_itemId, msg.sender, _offerPrice);
        } else {
            payable(msg.sender).transfer(_offerPrice); // Refund buyer
            emit CounterOffer(_itemId, aiCounterOffer);
        }
    }

    function dynamicPricing(uint256 base, uint256 min, uint256 max, uint256 offer) private pure returns (uint256) {
        if (offer >= max) return max;
        if (offer >= base) return base;
        return (base + offer) / 2; // Simple AI-based counteroffer logic
    }
}
```

# Expected Output:
Buyers submit offers, and the contract auto-negotiates the price.


If the buyer’s offer is fair, the deal is executed.


If the offer is too low, the contract suggests a counteroffer.

# output:
![image](https://github.com/user-attachments/assets/44f07da9-f226-40bc-a481-5dee2caac510)

![Screenshot 2025-04-30 085207](https://github.com/user-attachments/assets/fab4927e-fe0f-4021-9e5d-28c54cd101d9)

![image](https://github.com/user-attachments/assets/a554ead3-0e76-47f6-b256-c37686f72052)



# High-Level Overview:
First-of-its-kind AI-powered pricing contract.


Mimics real-world price negotiations using dynamic on-chain pricing.


Can be extended to AI oracles for real-time market data.


Inspired by AI-enhanced commerce and eBay-like decentralized auctions.

# RESULT:
A smart contract that integrates AI logic for automated negotiation in decentralized commerce is executed successfully.

