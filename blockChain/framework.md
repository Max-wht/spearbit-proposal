## [Foundry](https://www.getfoundry.sh/introduction/getting-started)

### FORGE

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

2. **Testing:**





