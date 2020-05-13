# Claim your reward

## Why claim is needed

About the process holders and validators get commissions & rewards, you may check the article below:

* \*\*\*\*[**Proof of Profession, Better Alternative to Proof of Stake**](https://medium.com/hdac/proof-of-profession-better-alternative-to-proof-of-stake-b9460e47928a)\*\*\*\*
* \*\*\*\*[**The New Economic Ecosystem — Voting System for dApps**](https://medium.com/hdac/the-new-economic-ecosystem-voting-system-for-dapps-844c1e1f3b1d)\*\*\*\*
* \*\*\*\*[**From token economy to codes**](https://medium.com/hdac/from-token-economy-to-codes-24e9133b1732)\*\*\*\*

With this process and formula, holders and validators should get reward for every blocks. But if they got reimbursed for every blocks, it will be suffered from thousands of transfer wasm execution. In that point, claim feature is written and you can get financial benefit by each request.

## Claim commission \(validator\)

```bash
clif hdac claim commission <fee> --from <wallet_alias>
clif hdac claim commission 0.001 --from alsa
```

## Claim reward \(holder\)

```bash
clif hdac claim reward <fee> --from <wallet_alias>
clif hdac claim reward 0.001 --from alsa
```

## Querying unclaimed reward/commission status

```bash
clif hdac getreward --from <wallet_alias>
clif hdac getcommission --from <wallet_alias>

clif hdac getreward --from alsa
clif hdac getcommission --from alsa
```

