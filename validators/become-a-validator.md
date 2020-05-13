# Become a Validator

{% hint style="info" %}
Before setting up your validator node, make sure you have already gone through the [Join a Network](../how-to/node-operation/join-a-network.md) guide.
{% endhint %}

## What is a Validator?

[Validators](https://hub.cosmos.network/master/validators/overview.html) are responsible for committing new blocks to the blockchain through voting. A validator's stake is slashed if they become unavailable or sign blocks at the same height. Please read about [Sentry Node Architecture](security.md#sentry-nodes-ddos-protection) to protect your node from DDOS attacks and to ensure high-availability.

{% hint style="warning" %}
Users looking to operate a Friday validator should study up on the correct [security model](security.md), study [robust network topologies](become-a-validator.md), and familiarize themselves with the [Friday Consensus Mechanism](become-a-validator.md).
{% endhint %}

## Run your own node.

* Please follow the instruction of [Join a Network](../how-to/node-operation/join-a-network.md)

{% hint style="info" %}
Be sure that `nodef` and `casperlabs-engine-grpc-server` should RUN AS DAEMONS. You should daemonize both of them by your own method. If not, you are going to [face the problem](../troubleshooting.md#node-dies-with-wrong-apphash-error).
{% endhint %}

## Create Your Own Validator

* Prepare local wallet
  * [Create your wallet](../how-to/node-operation/create-your-wallet.md)
* Create validator

  ```bash
  clif hdac create-validator <fee> \
  --from <wallet alias> \
  --pubkey $(nodef tendermint show-validator) \
  --moniker <validator moniker name> \
  --chain-id <chain-id>
  ```

* Bond amount

  ```bash
  clif hdac bond \
  --from <wallet alias> 10 0.01 \
  --chain-id <chain-id>
  ```

{% hint style="danger" %}
If you want to disconnect your validator node, please beware of [unbonding ](../cli/token.md#unbond-hdac-token)it first!
{% endhint %}

{% hint style="warning" %}
You should check the status of every transaction you executed! There is a [guide of how to check the status!](../how-to/node-operation/play-with-hdac-token.md#check-your-transaction)
{% endhint %}

## Check that you are in the validator pool

* Check your validator consensus address

```bash
nodef tendermint show-address
# DO NOT USE [ clif key shows <wallet_alias> --bech cons ]!!!
# fridayvalcons1kgkzwcuvn6f4cuvcwnh5y3sn4yx9ejle3qwwe9
```

* Get the list of validators and lookup your address

```bash
clif query tendermint-validator-set

{
  "block_height": "222225",
  "validators": [
    {
      "address": "fridayvalcons1x3e8ket8zzp38zka45ycavkfa08hc09dqe0uyg",
      "pub_key": "fridayvalconspub16jrl8jvqq9y8v2mdg44hqmn4vanxkv66fde5yvmsfc6kjnetxfj8v3m289x4g4msg4gkgct4x4s5cm3ntfpxue2eg4g9wvjdgaey653tfayxys2pgexxsjtgv3mkxsmv0phnw3tcxd2hze3kvvcys4tsf5mhgsjv2pyxu5ntffu5yjf3vfhx76nggacyywtzw39ry5zx23f8vs30g9vzkjgnlu7np",
      "proposer_priority": "105",
      "voting_power": "100"
    },
    {
      "address": "fridayvalcons1xjx2z5x0tpl5w3ygzlvxgp94mqxc2kdsldytqz",
      "pub_key": "fridayvalconspub16jrl8jvqq9m9gwtd0qeywsnpt9u57djffcmr2m62xfghz5e5gsmx7s64va5kgwpngauy2nmtfa3nz7jnwe59gc20w4g4semnfgcny5e0derhq523d3u8vstsffykz52dd9arxmj9vch4gkttddfhg3jz9veng56g8qhhjsmd0pknwnmrxycrv7zkfp3yxv23da5yzwpj2y6xy7zkddf5wksmfay65",
      "proposer_priority": "-27",
      "voting_power": "20"
    },
    {
      "address": "fridayvalcons18v4kv363jey6d5zv665kyw0v23atnrqxx04nsq",
      "pub_key": "fridayvalconspub16jrl8jvqq9k92n6ff9ryzd3edu68s6mtva5x2vtkv95hyc6hw96j7vnf9dehyn6rdfd9qum52324w2m22dg9vuze2ah5xn28f9zhvkrf2ephx52tddar2djsvymkx5n8ffp9zkjxxf9zkdm2vfuk6ezvxdgxv635gvm95wtt29t5catx0qhkx4txgvcy2wzt9akkksnpddj457ph2yhkkkswcz7aa",
      "proposer_priority": "-339",
      "voting_power": "100"
    },
    {
      "address": "fridayvalcons1299vl3py6aztuqm66cwrgthysvqzlpj3rs9sw7",
      "pub_key": "fridayvalconspub16jrl8jvqq9gyjc2sg329g5pexerxvarr2agyjsjrd3f9w4352p5yut6ntpxxk5mr9v68x430w35xxunc2az56ej2w4nx65rp25hhwerffeyzks250yckj728tpn456zg2pshxvzexdx9zk239dy8vujxg4dz7m2y2emyzdf529456vjrd3yyx7nrd455s3t2x9ykckn2d4rycm2pty68j3qrn7m4s",
      "proposer_priority": "257",
      "voting_power": "60"
    },
    {
      "address": "fridayvalcons15dujpvgskwdrvncq9acdze0e6nslu00agrry50",
      "pub_key": "fridayvalconspub16jrl8jvqq9h82cj0vyexjdnvdfp8yepsfden2jj2f3chguzx9vuy5ancxyh5vaztdftrv3mzdse4y32yf9p52nmk2359get6de6jkvr0we4xka6s9auxkurkxa89j66tverrqdtzd4e5j4net9qhs3zh0ychvvtdge6rx4nnxy44g3tstpj5cun2tfghsn2sxa5xk4e523v923t9wffjkjqr6f2td",
      "proposer_priority": "152",
      "voting_power": "20"
    }
}
```

