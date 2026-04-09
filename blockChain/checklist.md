# ROOT
The directory should be divided into categories based on "vulnerability causes/failure mechanisms".

  ## Unchecked Data
  Controllable inputs are not validated for range, format, or existence before entering critical logic.

  <details>
  <summary>Cases Of Unchecked Data</summary>

  **1_Chainlink Abstract Payment V2:** This system use COW to swap token. `isValidSignature` reduces signature verification to a hash consistency check of externally `controllable order data`, without enforcing it as an authorization boundary on `appData`. <u>Business-related parameters were ignored.</u>

  >   [report in c4](https://code4rena.com/audits/2026-03-chainlink-payment-abstraction-v2/submissions/F-54)

  </details>

  ## Broken Access Control
  High-risk function may have missing incorrect, or circumventable caller constraints.

  ## Invalid State Transition
  Critical operations lacks pre and post state verification, allowing the state machine to skip transitions.

  ## Identity/Auth Assumption Failure
  Treating erroneous entities as trusted identities

  ## Unsafe External Interaction
  There is a excessive trust in external contract, and call boundaries are not isolated.

  <details>
  <summary>Cases Of Unsafe External Interaction</summary>

  **1_Push Base Transfer:** Two external transaction in one function.In the withdrawal critical path, the contract performs an external call `safeTransfer(owner(), feeAmount)` upfront and assumes its success. However, if the owner is set to a non-compliant or malicious address, or if the baseAsset (e.g., an ERC777 token with revertible hooks) introduces unexpected behavior, the external call may fail and revert the entire withdrawal flow.

  >   [report in solodit](https://solodit.cyfrin.io/issues/m-06-denial-of-service-contract-owner-could-block-users-from-withdrawing-their-strike-code4rena-putty-putty-contest-git)
  > DOS

  </details>



  ## Reentrancy-Safe Ordering Failure
  An error in order of external interaction and internal accounting allows reentrancy to exploited.

  ## Invariant Enforcement Failure
  Invariants such as total amount, debt, share, and collateral ratio have not been continuously and strongly validated.

  ## Oracle/Pricing Assumption Failure
  Price sources are manipulated, expired, or the single point or update mechanism is unreliable.

  ## Arithmetic/Precision/Unit Error
  Overflow, truncation, rounding, precision or unit conversion can cause numerical errors.

  ## Signature Verification Failure
  The signer, message, algorithm, or recovery process verification is incomplete.

  ## Replay/Nonce/Domain Separation Failure
  Authorization can be reused across time/chain/contract, but lacks nonce/domain isolation.

  ## Initialization/Upgrade Misconfiguration
  Initialization is repeatable, the contract is unlocked, and the upgrade entry control is ineffective.

  ## Privilege/Governance Guardrail Failure
  The privileged operation lacks safeguards such as delay, multiple signatures, limit, and circuit breaker.

  ## Dependency Trust Boundary Failure
  The return value of the third-party component is treated as a strong trusted input.

  ## Ordering/MEV Assumption Failure
  The logic depends on the order of transactions, making it susceptible to being preempted, caught in a trap, or experiencing a reversal.

  ## Resource Bound/Gas Control Failure
  Loops, queues, and batch processing lack upper bounds or gas degradation protection.

  ## Error Handling/Recovery Failure
  Failure paths, including errors, partial execution, and lack of compensation, can lead to a corrupted system state.

  ## Token Semantics Assumption Failure
  The assumption is incorrect. ERC20 behavior is consistent (taxes, blacklists, callbacks, non-standard returns).

  ## Storage/ABI/Encoding/Layout Mismatch
  The storage layout, ABI, encoding/decoding, or proxy slots are inconsistent.

  ## Randomness Source Integrity Failure
  Randomness comes from predictable/influenceable on-chain variables.

