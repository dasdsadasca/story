```mermaid
graph TD
    subgraph "User Interaction"
        User
    end

    subgraph "Story Execution Client (story-geth, EVM)"
        ExecutionClient[Story Execution Client]
        EngineAPI[Engine API (JSON-RPC)]
        ExecutionClient -- EVM Blocks --> EngineAPI
    end

    subgraph "Story Consensus Client (Go, Cosmos SDK, CometBFT)"
        ConsensusClient[Story Consensus Client]
        EvmEngine[evmengine module]
        EvmStaking[evmstaking module]
        MintModule[mint module]

        ConsensusClient --> EvmEngine
        ConsensusClient --> EvmStaking
        ConsensusClient --> MintModule
    end

    subgraph "Smart Contracts (Solidity)"
        SmartContracts[Smart Contracts]
        IPTokenStaking[IPTokenStaking]
        UBIPool[UBIPool]
        UpgradeEntrypoint[UpgradeEntrypoint]

        SmartContracts --> IPTokenStaking
        SmartContracts --> UBIPool
        SmartContracts --> UpgradeEntrypoint
    end

    User -- Interacts via transactions --> ExecutionClient
    ExecutionClient -- Relays EVM blocks --> EngineAPI
    EngineAPI -- EVM Blocks --> ConsensusClient
    ConsensusClient -- Manages EVM state --> EvmEngine
    ConsensusClient -- Manages staking --> EvmStaking
    ConsensusClient -- Manages minting --> MintModule
    ExecutionClient -- Interacts with --> SmartContracts

    %% Styling
    classDef client fill:#f9f,stroke:#333,stroke-width:2px;
    classDef module fill:#bbf,stroke:#333,stroke-width:2px;
    classDef contract fill:#dfd,stroke:#333,stroke-width:2px;
    classDef api fill:#ff9,stroke:#333,stroke-width:2px;

    class User,ExecutionClient,ConsensusClient client;
    class EvmEngine,EvmStaking,MintModule module;
    class IPTokenStaking,UBIPool,UpgradeEntrypoint,SmartContracts contract;
    class EngineAPI api;
```
