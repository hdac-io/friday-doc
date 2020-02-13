# Become a Validator

{% hint style="info" %}
Before setting up your validator node, make sure you have already gone through the [Full Node Setup](../first-step/join-a-network.md) guide.
{% endhint %}

## What is a Validator?

[Validators](https://hub.cosmos.network/master/validators/overview.html) are responsible for committing new blocks to the blockchain through voting. A validator's stake is slashed if they become unavailable or sign blocks at the same height. Please read about [Sentry Node Architecture](security.md#sentry-nodes-ddos-protection) to protect your node from DDOS attacks and to ensure high-availability.

{% hint style="warning" %}
Users looking to operate a Friday validator should study up on the correct [security model](security.md), study [robust network topologies](become-a-validator.md), and familiarize themselves with the [Friday Consensus Mechanism](become-a-validator.md).
{% endhint %}

## Create Your Own Validator

* Prepare local wallet
  * [Create new wallet](../first-step/play-with-hdac-token.md#create-new-wallet) or [recover wallet](../first-step/play-with-hdac-token.md#recover-your-wallet-from-mnemonic-words)
* [Create validator](../cli/hdac-specific.md#create-validator)

```bash
clif hdac create-validator \
--from welcomeval \
--pubkey $(nodef tendermint show-validator) \
--moniker valiator-bryan
```

* Bonding amount

```bash
clif hdac bond --wallet welcomeval 1000000 100000000 30000000
```

{% hint style="danger" %}
If you want to disconnect your validating node, please execute [unbond](../cli/hdac-specific.md#unbond-hdac-token) first!
{% endhint %}

 

