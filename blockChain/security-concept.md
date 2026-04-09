# Overview

**Description:** each part of the docs represent a vulnerablitiy aspect. I will walk through each aspect and explain it in detial.

- [Dos Attack](#dos-attack)

# Dos Attack

**Description:** `Dos` stands for `denial of service`, which simply represents making some of service unavailable. What ever service you can think of --- users can not access it anymore.

the dos attack in blockchain is to stop smart contrack from working, break the function/contract logic for temporarily or permanently.

there are few aspects:

  ## 1 Depend on an owner

  ownership is a privilege. The owner can have special privileges within the contract. These privileges can also originate from other contract.
  - ERC20: USDC has a blackList to denail transfer

  ## 2 Depend on External call

  This function relies on an external call. If the external call reverts, the entire transaction will revert. Therefore, the external contract must be trusted.

  Some contract will naively think that some of the address is EOA. Every call should be checked.

  **In Transaction Scenario**, their is a patern call `pull-base`
  ```js
  ownerFees[baseAsset] += feeAmount;
  token.safeTransfer(msg.sender, order.strike - feeAmount);
  ```
  one function one transfer. In this scenario, what is owed to the owner is recorded first, and the other party can collect it later. Thus the two transaction will no interrupt each other.

  If the contract transfer an NFT to another contract. OnERCReceive() matters.

  ## 3 Depend on External Data 

  the external data is not under the contract's control(eg. Oracle, another contract), attack can make sure that the data will cause the call always revert.
  - ERC20.balanceOf(address(this))

  ## 4 Gas Manipulations

  Every block has gas limit. If tx reaches the limit => revert  
  the most common scenario is arrary's length

  **In Order Recording Scenario**, which refers to a case where the contract needs to record each transaction in an order queue and process the orders later. A minimum transaction amount should be enforced; otherwise, an attacker can insert zero-value or dust orders into the queue, leading to gas manipulation.

`Denial Of Service` is a huge aspect. Any For-Loop, externalcall, requirement can cause it.


# Flash Loan Attack

  **Description:** Price oracle attack is usually a part of  Flash Loan Attack. Leveraging Flash Loan to attack protocol(eg. Pools/Vault, Yield farming, Goverance, Oracle/PriceFeed)

  ## 1 Governance Manipulation

  ## 2 Price Manipulate

  If protocols based on on-chain AMM price, the price can be operated maliciously.
  It relies on a single liquidity pool or oracle pricing mechanism and lacks TWAP protection and maxSharePriceChangeRate(IMPORTANT)

  ## 3 Flash Loan callback

  the best practice is define the interface to callbacka, instead of call the external contract directly. Never trust the external contract. Developer should use defined interface to constrain external contracts behaviors. What's more, the contract better to use the `pull-base` pattern. 
  ```js
  function flashloan(..) external{
    ...
    // NO : (address)to.call(data)
    // YES: (address)to.flashLoanCallback(data)
    ...
    // NO: transfer token in callback
    // YES: token.transferFrom(to, address(this), amount)
    ... 
  }
  ```

  # Consistency

  Most Vulnerabilities are rooted in consistency issue within the system's logic.
  There are some things SR have to check:

  ## 1 another contract data structure and function
  - constraints are inconsistent
  - key fields check
  

