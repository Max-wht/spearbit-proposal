# [Foundry](https://www.getfoundry.sh/introduction/getting-started)

## FORGE

Forge compiles, tests, and deploys Solidity smart contracts. It's the core development tool in the Foundry suite.

- Build Project: `forge biuld`
- Run Test: `forge test`
- Deploy via script: `forge script <file> --broadcast --rpc-url <url>`

1. **Building:**

- Pin Specific version

```toml
[profile.default]
solc_version = "0.8.28"
// or
[profile.default]
solc = ">=0.8.0 <0.9.0"
```

- Enable the optimizer for production deployments with via-IR for production

```toml
[profile.default]
optimizer = false
 
[profile.production]
optimizer = true
optimizer_runs = 200
via_ir = true
```



# [@openzeppelin](https://docs.openzeppelin.com/)



## *1. Token*

### {Safe ERC20 Upgradeable}

<u>--Descrpition:</u>  an OpenZeppelin utility library that wraps standard ERC20 operations to handle non-compliant tokens that don't return a bool.

<u>--What problem does it solve:</u> The ERC20 spec says transfer / transferFrom / approve return a bool. But some widely-used tokens (e.g., USDT) return void instead — calling them via the standard IERC20 interface reverts. Other tokens return false without reverting, causing silent failures.

<u>--Function:</u> 

```js
safeTransfer(token, to, value)          // works with both bool-returning and void tokens
safeTransferFrom(token, from, to, value)
safeApprove(token, spender, value)      // also prevents the allowance race condition
safeIncreaseAllowance(token, spender, value)
safeDecreaseAllowance(token, spender, value)
```

### {ERC20 Burnable Upgradeable}

<u>--Description:</u> Adds a public burn function to an ERC20 token.

<u>--What problem does it solve:</u>  Allows token holders or approved operators to permanently destroy tokens, reducing the total supply. Commonly used for deflationary mechanics, redemption systems, cross-chain bridges, and governance-controlled supply reduction.

<u>--Functions:</u>

```js
burn(amount)          // destroy caller's own tokens
burnFrom(account, amount)  // destroy someone else's tokens (needs allowance)

```

### {IERC20 Upgradeable}

<u>--Description:</u> The standard ERC20 interface definition.

<u>--What problem does it solve:</u> Lets the Bridge talk to any ERC20 token generically — balanceOf, transfer, transferFrom, approve — without importing a full
token implementation.

<u>--Functions:</u>

```js
transfer(to, amount) → bool
transferFrom(from, to, amount) → bool
balanceOf(account) → uint256
approve(spender, amount) → bool
allowance(owner, spender) → uint256
totalSupply() → uint256
```





---

## *2. Access*

### {Access Control Upgradeable}

<u>--Description:</u> OpenZeppelin's role-based permissions contract. Unlike Ownable (one owner), it lets you define multiple roles (bytes32 hashes), each with independent permissions.
                                                                                                                                                               --<u>--What problem does it solve:</u> 

<u>--Functions:</u>

```js
hasRole(role, account)          // check if an address holds a role
grantRole(role, account)        // assign a role (only 		DEFAULT_ADMIN_ROLE)
revokeRole(role, account)       // remove a role (only DEFAULT_ADMIN_ROLE)
renounceRole(role, caller)      // voluntarily give up your own role
```

---

## *3. Proxy*

### {Initializable}

Initializable

<u>--Description:</u> Replaces the constructor for upgradeable contracts. Since proxy deployments don't run the implementation's constructor, initializer functions
  handle setup.

<u>--What problem does it solve:</u> In a UUPS proxy, the implementation contract's constructor never executes on the proxy. Without Initializable, you can't set initial state (roles, addresses). The initializer modifier also prevents the function from being called twice.

  <u>--Functions:</u>

```js
// modifier applied to initialize() — ensures it can only run once
modifier initializer()
```

### {UUPS Upgradeable}

<u>--Description:</u> The UUPS (Universal Upgradeable Proxy Standard) implementation. Unlike transparent proxies, the upgrade logic lives in the implementation
  contract itself.

<u>--What problem does it solve</u>: Allows the contract's code to be replaced without changing its address or losing storage. Cheaper gas than transparent proxies
  (upgrade logic in implementation, not proxy).

<u>--Functions:</u>

```js
upgradeToAndCall(newImpl, data)   // upgrade the proxy to a new   implementation
_authorizeUpgrade(newImpl)        // virtual — override this to add access control (Bridge uses onlyRole(MULTISIG_ROLE))
```



## 4. Security

### {Reentrancy Guard Upgradeable}

<u>--Description:</u> A simple mutex that prevents reentrant calls — a function can't call itself again within the same transaction.

<u>--What problem does it solve:</u> Without it, an attacker could write a malicious token that calls back into bridgeTokens() during transferFrom, draining funds before the first call finishes. The nonReentrant modifier blocks this.

<u>--Functions:</u>

```js
// modifier — put on any function that makes external calls after state changes
modifier nonReentrant()
```



## 5. utils

### {ERC165Checker}

<u>--Description:</u> A utility to check whether a contract implements a given interface (via ERC-165).

<u>--What problem does it solve:</u> Before trusting the Mapper address, the Bridge verifies it actually supports IMapper. Without this, a wrong address could be
  set as Mapper and every call to mapInfo() would silently return garbage data.

<u>--Functions:</u>

```js
supportsInterface(account, interfaceId) → bool   // does this contract implement this interface?
```

