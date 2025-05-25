```mermaid
sequenceDiagram
    actor User
    participant IPTokenStaking as "IPTokenStaking.sol"
    participant BurnAddress as "address(0)"
    participant ConsensusLayer as "Story Consensus Layer"

    autonumber

    box "Scenario 1: New Validator Creation"
    User->>+IPTokenStaking: Calls createValidator(validatorAddress, pubkey, commissionRate, description, msg.value)
    Note over IPTokenStaking: Verifies parameters (e.g., min stake, commission rate, pubkey format)
    Note over IPTokenStaking: Charges fee if applicable (modifier: chargesFee)
    IPTokenStaking->>+BurnAddress: transfer(msg.value - fee)
    Note over IPTokenStaking,BurnAddress: Burns $IP tokens (stake amount)
    BurnAddress--)-IPTokenStaking: 
    IPTokenStaking-->>ConsensusLayer: Emits CreateValidator(validatorAddress, pubkey, msg.value, commissionRate, description) event
    IPTokenStaking--)-User: Returns success/failure
    ConsensusLayer->>ConsensusLayer: Processes CreateValidator event
    Note over ConsensusLayer: Creates new validator state, adds to validator set if eligible
    end

    box "Scenario 2: Delegator Staking (Directly or to a Validator)"
    User->>+IPTokenStaking: Calls deposit(delegationId, msg.value) OR depositToValidator(validatorAddress, msg.value)
    Note over IPTokenStaking: Verifies parameters (e.g., delegationId exists or validator exists)
    Note over IPTokenStaking: Charges fee if applicable (modifier: chargesFee)
    IPTokenStaking->>+BurnAddress: transfer(msg.value - fee)
    Note over IPTokenStaking,BurnAddress: Burns $IP tokens (stake amount)
    BurnAddress--)-IPTokenStaking: 
    IPTokenStaking-->>ConsensusLayer: Emits Deposit(delegationId/validatorAddress, msg.sender, msg.value) event
    IPTokenStaking--)-User: Returns success/failure
    ConsensusLayer->>ConsensusLayer: Processes Deposit event
    Note over ConsensusLayer: Updates delegation state for the user or validator
    end
```
