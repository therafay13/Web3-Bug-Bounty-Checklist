# Smart Contract Auditing Checklist

## 1. Reentrancy
- [ ] Are state changes happening before external calls? (Checks-Effects-Interactions pattern)
- [ ] Are `transfer` or `send` used instead of `call` where appropriate? (Though `call` is recommended now, it requires reentrancy guards)
- [ ] Is `nonReentrant` modifier used on functions making external calls?
- [ ] Are cross-function reentrancies mitigated?
- [ ] Are cross-contract reentrancies mitigated?

## 2. Arithmetic Issues (Integer Overflow/Underflow)
- [ ] Is the contract using Solidity >= 0.8.0? (Built-in overflow checks)
- [ ] If < 0.8.0, is SafeMath used for all arithmetic operations?
- [ ] Are there unchecked blocks that could be vulnerable?

## 3. Access Control & Authorization
- [ ] Do sensitive functions have appropriate modifiers (e.g., `onlyOwner`)?
- [ ] Are roles correctly assigned and checked?
- [ ] Can the contract be unexpectedly self-destructed (`selfdestruct`)?
- [ ] Are initialization functions protected from being called multiple times?
- [ ] Can critical parameters (like fees or addresses) be changed without timelocks?

## 4. Front-Running (MEV)
- [ ] Are transactions susceptible to transaction order dependence?
- [ ] Are slippage parameters implemented and enforced in DEX swaps?
- [ ] Are commits/reveals used for sensitive actions to prevent front-running?

## 5. Denial of Service (DoS)
- [ ] Are there loops over unbounded arrays?
- [ ] Can an external call revert and block execution for others? (e.g., refunding ETH to a contract that rejects it)

## 6. Logic Flaws & Flash Loans
- [ ] Is the price oracle decentralized and manipulation-resistant (e.g., Chainlink)?
- [ ] Can an attacker use a flash loan to manipulate the pool balance and exploit the logic?
- [ ] Are rounding errors favoring the protocol rather than the user?

## 7. Randomness
- [ ] Is `block.timestamp` or `blockhash` used as a source of randomness? (Vulnerable to miner manipulation)
- [ ] Is a secure randomness oracle (like Chainlink VRF) used?
