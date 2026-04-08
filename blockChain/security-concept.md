# Overview

**Description:** each part of the docs represent a vulnerablitiy aspect. I will walk through each aspect and explain it in detial.

- [Dos Attack]()
- [Logic]()

# Dos Attack

**Description:** `Dos` stands for `denial of service`, which simply represents making some of service unavailable. What ever service you can think of --- users can not access it anymore.

the dos attack in blockchain is to stop smart contrack from working, break the function/contract logic for temporarily or permanently.

there are few aspects:

  ## 1. Depend on an owner
  
  ownership is a privilege. The owner can have special privileges within the contract. These privileges can also originate from other contract.
  - ERC20: USDC has a blackList to denail transfer

  ## 2. Depend on External call

  This function relies on an external call. If the external call reverts, the entire transaction will revert. Therefore, the external contract must be trusted.
  
  Some contract will naively think that some of the address is EOA. Every call should be checked.

  ## 3. Depend on External Data 

  ## 4. Gas Manipulations


