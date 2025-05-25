```mermaid
sequenceDiagram
    actor Owner as "Contract Owner (Admin)"
    participant UpgradeEntrypoint as "UpgradeEntrypoint.sol"
    participant ConsensusLayer as "Story Consensus Layer (x/upgrade module)"
    participant Validators as "Validators/Node Operators"

    autonumber

    box "Scenario 1: Proposing and Executing a Software Upgrade"
        Owner->>+UpgradeEntrypoint: Calls scheduleUpgrade(planName, planInfo, planHeight)
        Note over UpgradeEntrypoint: Verifies owner, checks if not paused
        UpgradeEntrypoint-->>ConsensusLayer: Emits SoftwareUpgrade(planName, planInfo, planHeight) event
        UpgradeEntrypoint--)-Owner: Returns success
        
        ConsensusLayer->>ConsensusLayer: Processes SoftwareUpgrade event
        Note over ConsensusLayer: Stores upgrade plan, sets timer for planHeight
        ConsensusLayer-->>Validators: Notifies of scheduled upgrade (e.g., via event subscription, querying state)
        
        Validators->>Validators: Observe scheduled upgrade plan
        Validators->>Validators: Prepare new software binary
        Validators->>Validators: Update node software before planHeight
        
        ConsensusLayer->>ConsensusLayer: Reaches planHeight
        Note over ConsensusLayer: Chain halts or automatically switches to new binary based on x/upgrade logic
        ConsensusLayer-->>Validators: Chain undergoes upgrade
        Validators->>Validators: Nodes restart with new software (if applicable)
    end

    box "Scenario 2: Cancelling a Software Upgrade"
        Owner->>+UpgradeEntrypoint: Calls cancelUpgrade(planHeight)
        Note over UpgradeEntrypoint: Verifies owner, checks if not paused
        UpgradeEntrypoint-->>ConsensusLayer: Emits CancelUpgrade(planHeight) event
        UpgradeEntrypoint--)-Owner: Returns success
        
        ConsensusLayer->>ConsensusLayer: Processes CancelUpgrade event
        Note over ConsensusLayer: Clears the previously scheduled upgrade plan for planHeight
        ConsensusLayer-->>Validators: Notifies of upgrade cancellation
        
        Validators->>Validators: Observe upgrade cancellation
        Note over Validators: No software update action needed if not already performed
    end
```
