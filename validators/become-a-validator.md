# Become a Validator

{% hint style="info" %}
Before setting up your validator node, make sure you have already gone through the [Join a Network](../how-to/join-a-network.md) guide.
{% endhint %}

## What is a Validator?

[Validators](https://hub.cosmos.network/master/validators/overview.html) are responsible for committing new blocks to the blockchain through voting. A validator's stake is slashed if they become unavailable or sign blocks at the same height. Please read about [Sentry Node Architecture](security.md#sentry-nodes-ddos-protection) to protect your node from DDOS attacks and to ensure high-availability.

{% hint style="warning" %}
Users looking to operate a Friday validator should study up on the correct [security model](security.md), study [robust network topologies](become-a-validator.md), and familiarize themselves with the [Friday Consensus Mechanism](become-a-validator.md).
{% endhint %}

## Create Your Own Validator

* Prepare local wallet
  * [Create your wallet](../how-to/create-your-wallet.md)
* Create validator

  ```bash
  clif hdac create-validator \
  --from <wallet alias> \
  --pubkey $(nodef tendermint show-validator) \
  --moniker <validator moniker name> \
  --chain-id <chain-id>
  ```

* Bond amount

  ```bash
  clif hdac bond \
  --from <wallet alias> 10 0.01 30000000 \
  --chain-id <chain-id>
  ```

{% hint style="danger" %}
If you want to disconnect your validator node, please beware of [unbonding ](../cli/hdac-specific.md#unbond-hdac-token)it first!
{% endhint %}

 

