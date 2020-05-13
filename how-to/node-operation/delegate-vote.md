# Delegate / Vote

## Introduction of delegation and vote

All holders can vote in 2 types. One is 'delegation' \(vote to validator\), and the another one is 'vote' \(vote to dApp\)

You may check these articles:

* \*\*\*\*[**Proof of Profession, Better Alternative to Proof of Stake**](https://medium.com/hdac/proof-of-profession-better-alternative-to-proof-of-stake-b9460e47928a)\*\*\*\*
* \*\*\*\*[**The New Economic Ecosystem — Voting System for dApps**](https://medium.com/hdac/the-new-economic-ecosystem-voting-system-for-dapps-844c1e1f3b1d)\*\*\*\*
* \*\*\*\*[**From token economy to codes**](https://medium.com/hdac/from-token-economy-to-codes-24e9133b1732)\*\*\*\*

## How to delegate

### Self-delegation for being validator

Both delegation and [bond](play-with-hdac-token.md#bond) command work for it. You may check [bond documentation](play-with-hdac-token.md#bond) for learning how. [Unbond](play-with-hdac-token.md#unbond) means the opposite meaning of bond.

### Delegation to other validators

```bash
clif hdac delegate <delegatee_address> <amount> <fee> --from <delegator_wallet_alias>
clif hdac delegate friday135260rslw2gkhpph2gjclm5zvvaeaf9qw7tknm 1 0.002 --from enna
```

Holders can stake and delegate to validators for making from the validator candidate to the actual validator. Then, top 100 validators will get commissions from block producing reimbursement, and holders will get some rewards.

### Undelegation / Redelegation

```bash
clif hdac undelegate <delegatee_address> <amount> <fee> --from <delegator_wallet_alias>
clif hdac undelegate friday135260rslw2gkhpph2gjclm5zvvaeaf9qw7tknm 1 0.002 --from enna
```

```bash
clif hdac redelegate <from_delegatee> <to_delegatee> <amount> <fee> --from <delegator_wallet_alias>
clif hdac redelegate friday135260rslw2gkhpph2gjclm5zvvaeaf9qw7tknm friday1yvmwx4j67qfryn8k2m0xxv8vvcn87cy4wz8wlg 1 0.002 --from enna
```

If holders want to unstake and liquify their tokens, they execute undelegation. If they want to change your delegation power from one to others, redelegation is the answer.

## How to vote

### Introduction

Vote feature is a kind of supporting method dApps, that reduces execution cost of transactions.

$$
\sum (delegation_i) = available\;vote\;power
$$

Holders can vote up to the whole amount they delegated. The fee will be deducted for dApps and holders got reward the amount of the deducted fee. You may learn that from [this article](https://medium.com/hdac/from-token-economy-to-codes-24e9133b1732).

### Vote

```bash
clif hdac vote <contract_hash> <amount> <fee> --from <wallet_alias>
clif hdac vote 0000000000000000000000000000000000000000000000000000000000000000 1 0.002 --from fridayxxxxxxxxxxxxx
```

### Unvote

```bash
clif hdac unvote <contract_hash> <amount> <fee> --from <wallet_alias>
clif hdac unvote 0000000000000000000000000000000000000000000000000000000000000000 1 0.002 --from fridayxxxxxxxxxxxxx
```

## Querying the status

### Delegation

#### Check the given validator

```bash
clif hdac delegator <validator_address>
clif hdac delegator $(clif keys show anna -a)
```

#### Check the given delegator \(holder\)

```bash
clif hdac delegator --from <delegator_address>
clif hdac delegator --from $(clif keys show anna -a)
```

#### Check the specific validator from the given delegator \(holer\)

```bash
clif hdac delegator <delegatee_address> --from <delegator_address>
clif hdac delegator $(clif keys show elsa -a) --from $(clif keys show anna -a)
```

#### Output

```bash
[
  {
    "address": "friday1jk2zrqqa98pwax7cq0xgkqw67qk2p8nhcpup8k",
    "amount": "1000000000000000000"
  }
]
```

### Vote

#### Check status of specific dApp

```bash
clif hdac voter <contract_hash>
clif hdac voter 0000000000000000000000000000000000000000000000000000000000000000
```

#### Check status of the voter \(holder\)

```bash
clif hdac voter --from <holder_address>
clif hdac voter --from fridayxxxxxxxxxxxxx
```

#### Check status of the specific dApp from the voter \(holder\)

```bash
clif hdac voter <contract_hash> --from <holder_address>
clif hdac voter 0000000000000000000000000000000000000000000000000000000000000000 --from fridayxxxxxxxxxxxxx
```

