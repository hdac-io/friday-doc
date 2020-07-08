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

  * create-validator uses 1000HDAC for create fee

* Bond amount

  ```bash
  clif hdac bond \
  --from <wallet alias> <amount> <fee> \
  --chain-id <chain-id>
  ```

* Self delegate bonded amount

  ```bash
  clif hdac delegate \
  $(clif keys show <wallet alias> -a)
  --from <wallet alias> <amount> <fee> \
  --chain-id <chain-id>
  ```

{% hint style="danger" %}
If you want to disconnect your validator node, please beware of [unbonding ](../cli/token.md#unbond-hdac-token)it first!
{% endhint %}

{% hint style="warning" %}
You should check the status of every transaction you executed! There is a [guide of how to check the status!](../how-to/node-operation/play-with-hdac-token.md#check-your-transaction)
{% endhint %}

## Check that you are in the validator pool

### Via execution engine state

* Check your wallet address

```bash
clif keys show <wallet alias> -a
```

* Get the list of validators and lookup your address

```bash
$ clif hdac validator
[
  {
    "address": "friday1plcrykeqtmgcq4ev3xkm286uyd8h470r9zqkuyj06yj1k2lvyuws8zg1xr",
    "consensus_pubkey": "fridayvalconspub16jrl81vqq9k4sum8gg68vs2n0ph4jjjsv92hje6c1aehzurt89fhy7ndvd9kvs69gaekx3ng256xgjmpxdg8yarcxeknzdetgdeywcjjdquhzu6nt9f4wvrdgdyyj3mdve6424t0vdfysv6ewvhkcjmsxfp5gnf3gfrxcvj9gdtygar909m8z6z6293k5jzfvfkjkje0de5kznmnwd9kxkq6rmj7y",
    "description": {
      "moniker": "uno",
      "identity": "",
      "website": "",
      "details": ""
    },
    "stake": "10000000000000000000"
  },
  {
    "address": "friday1t8mlkkcrfyznqjudfhnufw7gr564ku1fx07zhgyxrqx3fd497j2sh1z8cj",
    "consensus_pubkey": "fridayvalconspub16jrl81vqq9y4jv2tv36ku4tkvdgy7jfswp955mmd13c4q26hxffy75zvdpdy2vmwd4py57z42eyhg5e0wenh5sn42pp9v369f9py5722dd9xv5252ff52369deq57smr2uu4yvms9d38q7j2we9ry3tpxqmhz7jd25ujka6ff3vk7jnwdeyx6kt6veghveecw99z7n2f2pyjk3thv5uy7ss7jzlsw",
    "description": {
      "moniker": "dos",
      "identity": "",
      "website": "",
      "details": ""
    },
    "stake": "10000000000000000000"
  },
  {
    "address": "friday1hmm59jnjfaj13v62utxrp98lx7yx0z33qln55hde0kuh60azd6vq7al7q1",
    "consensus_pubkey": "fridayvalconspub16jrl81vqq988sjpe9d28wc2d9a985nrktyeyk6j11fvhxnrhx9nxsnmex9z5k26k0gc4v6thdfp4zkn2w4uz7snyw56856ft89crz4m9g5enqu6ggftzk6rv2a2xw7rhxaeyynrtg42kwktgxecxw56wgexzka2g2enyyjtwg9exwvnexemkknp424thgcmev4c4wnrxdecxvdj0xfek64qj6jf0n",
    "description": {
      "moniker": "tres",
      "identity": "",
      "website": "",
      "details": ""
    },
    "stake": "10000000000000000000"
  }
]
```

### Via tendermint state

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

