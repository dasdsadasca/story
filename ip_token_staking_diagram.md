```mermaid
graph TD
    subgraph "State Variables"
        MinStakeAmount["minStakeAmount (uint256)"]
        MinCommissionRate["minCommissionRate (uint256)"]
        Fee["fee (uint256)"]
        DelegationIdCounter["_delegationIdCounter (uint256)"]
        Owner["owner (address)"]
        Operator["operator (address)"]
        TotalStaked["totalStaked (uint256)"]
        Validators["validators (mapping)"]
        Delegations["delegations (mapping)"]
    end

    subgraph "Events"
        MinStakeAmountSet["MinStakeAmountSet"]
        MinCommissionRateSet["MinCommissionRateSet"]
        FeeSet["FeeSet"]
        SetOperatorEvent["SetOperator"]
        CreateValidatorEvent["CreateValidator"]
        UpdateValidatorEvent["UpdateValidator"]
        DepositEvent["Deposit"]
        WithdrawEvent["Withdraw"]
        UnjailEvent["Unjail"]
        TokensBurnedEvent["TokensBurned (External Interaction)"]
        ConsensusLayerInteraction["Consensus Layer Interaction (via Events)"]
    end

    subgraph "Modifiers"
        OnlyOwner["onlyOwner"]
        ChargesFee["chargesFee"]
        NonReentrant["nonReentrant"]
        VerifyCmpPubkey["verifyCmpPubkeyWithExpectedAddress"]
    end

    subgraph "Admin Functions"
        SetMinStakeAmount["setMinStakeAmount()"]
        SetMinCommissionRate["setMinCommissionRate()"]
        SetFee["setFee()"]
        WithdrawFee["withdrawFee()"]
        SetOperator["setOperator()"]

        SetMinStakeAmount -- onlyOwner --> SetMinStakeAmount
        SetMinStakeAmount --> MinStakeAmountSet

        SetMinCommissionRate -- onlyOwner --> SetMinCommissionRate
        SetMinCommissionRate --> MinCommissionRateSet

        SetFee -- onlyOwner --> SetFee
        SetFee --> FeeSet

        WithdrawFee -- onlyOwner --> WithdrawFee

        SetOperator -- onlyOwner --> SetOperator
        SetOperator --> SetOperatorEvent
    end

    subgraph "Operator Functions"
        Pause["pause()"]
        Unpause["unpause()"]

        Pause -- "onlyOperator" --> Pause
        Unpause -- "onlyOperator" --> Unpause
    end

    subgraph "Staking Configuration (Read-only)"
        GetMinStakeAmount["getMinStakeAmount()"]
        GetMinCommissionRate["getMinCommissionRate()"]
        GetFee["getFee()"]
        GetOperator["getOperator()"]
    end

    subgraph "Validator Creation & Configuration"
        CreateValidator["createValidator()"]
        UpdateValidator["updateValidator()"]

        CreateValidator -- chargesFee --> CreateValidator
        CreateValidator -- nonReentrant --> CreateValidator
        CreateValidator -- VerifyCmpPubkey --> CreateValidator
        CreateValidator --> CreateValidatorEvent
        CreateValidator --> ConsensusLayerInteraction

        UpdateValidator -- "onlyValidatorOwner" --> UpdateValidator
        UpdateValidator -- nonReentrant --> UpdateValidator
        UpdateValidator --> UpdateValidatorEvent
        UpdateValidator --> ConsensusLayerInteraction
    end

    subgraph "Token Staking"
        Deposit["deposit()"]
        DepositToValidator["depositToValidator()"]

        Deposit -- chargesFee --> Deposit
        Deposit -- nonReentrant --> Deposit
        Deposit --> DepositEvent
        Deposit --> ConsensusLayerInteraction

        DepositToValidator -- chargesFee --> DepositToValidator
        DepositToValidator -- nonReentrant --> DepositToValidator
        DepositToValidator --> DepositEvent
        DepositToValidator --> ConsensusLayerInteraction
    end

    subgraph "Unstake"
        Withdraw["withdraw()"]
        WithdrawFromValidator["withdrawFromValidator()"]

        Withdraw -- nonReentrant --> Withdraw
        Withdraw --> WithdrawEvent
        Withdraw --> ConsensusLayerInteraction
        Withdraw --> TokensBurnedEvent

        WithdrawFromValidator -- nonReentrant --> WithdrawFromValidator
        WithdrawFromValidator --> WithdrawEvent
        WithdrawFromValidator --> ConsensusLayerInteraction
        WithdrawFromValidator --> TokensBurnedEvent
    end

    subgraph "Unjail"
        Unjail["unjail()"]

        Unjail -- "onlyValidatorOwner" --> Unjail
        Unjail -- nonReentrant --> Unjail
        Unjail --> UnjailEvent
        Unjail --> ConsensusLayerInteraction
    end

    %% Styling
    classDef stateVar fill:#lightgrey,stroke:#333,stroke-width:1px;
    classDef event fill:#lightblue,stroke:#333,stroke-width:1px;
    classDef modifier fill:#lightgreen,stroke:#333,stroke-width:1px;
    classDef func fill:#orange,stroke:#333,stroke-width:2px;
    classDef external fill:#pink,stroke:#333,stroke-width:1px;

    class MinStakeAmount,MinCommissionRate,Fee,DelegationIdCounter,Owner,Operator,TotalStaked,Validators,Delegations stateVar;
    class MinStakeAmountSet,MinCommissionRateSet,FeeSet,SetOperatorEvent,CreateValidatorEvent,UpdateValidatorEvent,DepositEvent,WithdrawEvent,UnjailEvent event;
    class OnlyOwner,ChargesFee,NonReentrant,VerifyCmpPubkey modifier;
    class SetMinStakeAmount,SetMinCommissionRate,SetFee,WithdrawFee,SetOperator,Pause,Unpause,GetMinStakeAmount,GetMinCommissionRate,GetFee,GetOperator,CreateValidator,UpdateValidator,Deposit,DepositToValidator,Withdraw,WithdrawFromValidator,Unjail func;
    class TokensBurnedEvent,ConsensusLayerInteraction external;
```
