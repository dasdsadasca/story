```markdown
# Story L1 Blockchain Protocol Glossary

| Term                       | Definition                                                                                                                                                              |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Core Architecture**      |                                                                                                                                                                         |
| EVM                        | Ethereum Virtual Machine. The runtime environment for smart contracts on Story L1, inherited from Ethereum.                                                               |
| Execution Layer (EL)       | The layer responsible for executing transactions and smart contracts. In Story L1, this is represented by `story-geth`, an EVM-compatible execution client.              |
| `story-geth`               | A modified version of `go-ethereum` (Geth) that serves as the Execution Layer client for the Story L1 blockchain.                                                        |
| Consensus Layer (CL)       | The layer responsible for block production, consensus, and overall network agreement. Built using the Cosmos SDK and CometBFT.                                            |
| CometBFT                   | A Byzantine Fault Tolerant consensus engine used by the Story L1 Consensus Layer. Successor to Tendermint Core.                                                          |
| Cosmos SDK                 | A framework for building application-specific blockchains. Used to build the Story L1 Consensus Layer.                                                                    |
| Engine API                 | A JSON-RPC interface that connects the Execution Layer (`story-geth`) to the Consensus Layer, allowing them to communicate and pass block information.                    |
| ABCI++                     | Application Blockchain Interface Plus Plus. An enhanced version of ABCI that allows for more complex interactions between the consensus engine (CometBFT) and the application (Cosmos SDK app). |
| Genesis File               | A file that defines the initial state of the blockchain, including initial account balances, validator sets, and other parameters.                                      |
| Seed Node                  | A node in the blockchain network that new nodes can connect to in order to discover other peers and join the network.                                                 |
| Gas                        | A unit that measures the amount of computational effort required to execute operations on the EVM. Transactions require gas, paid in the native currency.                 |
| **Staking & Validation**   |                                                                                                                                                                         |
| `$IP` Token                | The native utility and governance token of the Story Protocol, used for staking, paying transaction fees, and participating in UBI.                                     |
| Staking                    | The process of locking up `$IP` tokens to participate in network consensus, secure the network, and earn rewards.                                                       |
| Validator                  | A node operator who participates in consensus by proposing and validating blocks. Validators must stake `$IP` tokens.                                                      |
| `validatorCmpPubkey`       | Compressed Public Key of a validator. Used in smart contracts like `IPTokenStaking.sol` and `UBIPool.sol` to verify validator identity for actions like claiming UBI.      |
| Moniker                    | A human-readable name or alias for a validator, making it easier to identify them.                                                                                      |
| Commission Rate            | The percentage of staking rewards that a validator keeps as a fee for their services before distributing the rest to their delegators.                                  |
| Self-delegation            | When a validator stakes their own `$IP` tokens to their validator node. Often a minimum amount is required.                                                               |
| Delegation                 | The act of an `$IP` token holder assigning their tokens to a validator to stake on their behalf, thereby earning a share of the validator's rewards.                     |
| `delegationId`             | A unique identifier for a specific delegation made by a user to a validator or directly into a staking pool, managed by `IPTokenStaking.sol`.                           |
| Redelegation               | The process of moving staked `$IP` tokens from one validator to another without an unbonding period. (Assumed, common in Cosmos SDK systems)                              |
| Unstaking (Unbonding)      | The process of withdrawing staked `$IP` tokens. This typically involves an unbonding period during which the tokens are locked before they become liquid.                   |
| Slashing                   | A penalty mechanism where a validator loses a portion of their staked `$IP` tokens (including delegators' tokens) for malicious behavior or significant downtime.       |
| Unjailing                  | The process for a previously jailed (and slashed) validator to rejoin the active validator set after a certain period and potentially meeting specific conditions.         |
| `IPTokenStaking.sol`       | The smart contract responsible for managing validator registration, `$IP` token staking, delegation, and related operations on the Execution Layer.                      |
| Staking Period             | The duration for which tokens are staked. While not explicitly detailed with "Flexible, Medium, Long" in provided contracts, this is a common concept in staking systems. |
| **UBI System**             |                                                                                                                                                                         |
| UBI (Universal Basic Income) | A system designed to distribute `$IP` tokens to participants, potentially validators or users, managed by `UBIPool.sol`.                                                |
| `UBIPool.sol`              | The smart contract that manages the collection and distribution of UBI rewards to eligible validators/participants.                                                      |
| Distribution ID (UBI)      | A unique identifier for a specific UBI distribution period or batch, managed by `UBIPool.sol`.                                                                          |
| **Upgrades**               |                                                                                                                                                                         |
| `x/upgrade` module         | A Cosmos SDK module within the Consensus Layer responsible for managing on-chain software upgrades.                                                                       |
| Software Upgrade Plan      | A proposal detailing a software upgrade, including its name, information, and the block height at which it should be executed. Managed via `UpgradeEntrypoint.sol`.     |
| `UpgradeEntrypoint.sol`    | A smart contract on the Execution Layer that allows authorized accounts (e.g., owner) to schedule and cancel software upgrades by emitting events to the Consensus Layer. |
| **Smart Contract General** |                                                                                                                                                                         |
| `msg.value`                | A property in Solidity smart contracts representing the amount of the native currency (e.g., `$IP` tokens) sent with a function call.                                   |
| `evmengine` module         | A Cosmos SDK module in the Story Consensus Client that likely handles interactions with the EVM, possibly processing EVM-related state or transactions.                 |
| `evmstaking` module        | A Cosmos SDK module in the Story Consensus Client that likely bridges staking information or logic between the EVM-based `IPTokenStaking.sol` contract and the CL.      |
| `mint` module              | A Cosmos SDK module in the Story Consensus Client responsible for minting new `$IP` tokens, potentially including those allocated for UBI.                               |
```
