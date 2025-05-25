```mermaid
sequenceDiagram
    actor Owner as "Contract Owner (Admin)"
    participant CLProcess as "Story CL / Off-chain Process"
    participant UBIPool as "UBIPool.sol"
    actor Validator

    autonumber

    box "Pre-flow: Funding and Distribution Setup"
        CLProcess->>+UBIPool: Transfers UBI_TOKEN_AMOUNT (e.g., via direct transfer or minting)
        Note over UBIPool: Contract's UBI_TOKEN balance increases
        UBIPool--)-CLProcess: 

        Owner->>+UBIPool: Calls setUBIPercentage(newPercentage)
        Note over UBIPool: (Optional step, may signal CL or be for internal logic)
        UBIPool-->>Owner: Emits UBIPercentageSet(newPercentage)
        UBIPool--)-Owner: Returns success

        Owner->>+UBIPool: Calls setUBIDistribution(distributionId, totalAmountToDistribute)
        Note over UBIPool: Verifies parameters (e.g., owner permission, nonReentrant)
        Note over UBIPool: Calculates individual validator shares based on IPTokenStaking data (not shown explicitly)
        Note over UBIPool: Updates distributions mapping and validatorUBIAmounts
        UBIPool-->>Owner: Emits UBIDistributionSet(distributionId, totalAmountToDistribute)
        UBIPool--)-Owner: Returns success
    end

    box "Validator Claiming UBI"
        Validator->>+UBIPool: Calls claimUBI(distributionId, validatorCmpPubkey)
        Note over UBIPool: Verifies validatorCmpPubkey against IPTokenStaking
        Note over UBIPool: Checks if distributionId is valid and not already claimed by validator
        Note over UBIPool: Checks if validator has a claimable amount for this distributionId
        UBIPool->>Validator: Transfers UBI_TOKEN (claimableAmount)
        Note over UBIPool: Updates distributions[distributionId] (marks as claimed for validator)
        Note over UBIPool: Decrements validatorUBIAmounts[validatorAddress]
        Note over UBIPool: Decrements totalPendingClaims
        UBIPool-->>Validator: Emits UBIClaimed(validatorAddress, claimableAmount, distributionId)
        UBIPool--)-Validator: Returns success
    end
```
