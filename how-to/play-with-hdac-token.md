# Play with Hdac Token

## Introduction

In this chapter, you can make a fun with Hdac token. You may learn what and how to do with the token.

Currently, the testnet supports:

* Get balance
* Transfer
* Bond / Unbond

## Create your wallet

First of all, you need your own wallet. It is needed when you execute action and send your own message to Tx. This section describes what is wallet, what you can get, and how to execute with your wallet.

\[Note\] You already created your wallet at [Run node from genesis](../first-step/deploy-your-own-friday-testnet.md). In there, the step proceeded without any detail information. If you're already done it, please just read the below and learn what the each step means.

### Create new wallet

`clif keys add <wallet_alias>`

The command above creates your wallet in the local. The information of the wallet is stored with encrypted by your passphrase. After created, please backup the mnemonic words in safe place. It is a kind of your private key.

```bash
clif keys add walletelsa
Enter a passphrase to encrypt your key to disk:
Repeat the passphrase:
{
  "name": "walletelsa",
  "type": "local",
  "address": "friday1rkf9xxxxxxxxxxxxxxxxxxxx0vfmwp",
  "pubkey": "fridaypub1addwnpexxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxg0t5p",
  "mnemonic": "skin bracket grief xxxx xxxx xxxx xxxx xxxx"
}
```

### Recover your wallet from mnemonic words

If you lost your wallet data, but it can be restored if you know the mnemonic words.

`clif keys add <wallet_alias> --recover`

```bash
clif keys add walletelsa --recover
Enter a passphrase to encrypt your key to disk:
Repeat the passphrase:
> Enter your bip39 mnemonic
skin bracket grief xxxx xxxx xxxx xxxx xxxx

{
  "name": "walletelsa",
  "type": "local",
  "address": "friday1rkf9xxxxxxxxxxxxxxxxxxxx0vfmwp",
  "pubkey": "fridaypub1addwnpexxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxg0t5p",
  "mnemonic": "skin bracket grief xxxx xxxx xxxx xxxx xxxx"
}
```

## Check & transfer token

Now, you can manage your token with your own wallet. Input command below and experience the wallet by yourself.

### Get balance

`clif hdac getbalance --wallet|--nickname|--address` 

```bash
clif hdac getbalance --wallet walletelsa
clif hdac getbalance --address friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv

# Response:
# {
#  "value": "5000000000000"
# }
```

### Transfer

`clif hdac transfer-to --address|--wallet|--nickname <from> <recipient_address_or_nickname> <amont> <fee> <gas-price>`

```bash
clif hdac transfer-to --wallet walletelsa friday15evpva2u57vv6l5czehyk69s0wnq9hrkqulwfz 1000000 100000000 22000000
clif hdac transfer-to --address friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv friday15evpva2u57vv6l5czehyk69s0wnq9hrkqulwfz 1000000 100000000 22000000
```

```bash
// ... confirm transaction before signing and broadcasting [y/N]: y
// Password to sign with 'elsa': # 
// input your password 
{
  "height": "0",
  "txhash": "141F12A891659F52B055EF7F701B1D406E5F1721CE929630CC5CE3CE0C4C8718",
  "raw_log": "[{\"msg_index\":0,\"success\":true,\"log\":\"\",\"events\":[{\"type\":\"message\",\"attributes\":[{\"key\":\"action\",\"value\":\"executionengine\"}]}]}]",
  "logs": [
    {
      "msg_index": 0,
      "success": true,
      "log": "",
      "events": [
        {
          "type": "message",
          "attributes": [
            {
              "key": "action",
              "value": "executionengine"
            } 
          ] 
        } 
      ]
    }
  ]
}
```

### Bond

Bond is a kind of time deposit. When you 'bond' your balance, you cannot transfer or withdraw tokens from your account. Instead, you can participate Hdac ecosystem, and get interests as a reward.

`clif hdac bond --wallet|--address|--nickname <owner> <amount> <fee> <gas-price>`

```bash
clif hdac bond --wallet walletelsa 1000000 100000000 30000000
clif hdac bond --nickname princesselsa 1000000 100000000 30000000
clif hdac bond --address friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv 1000000 100000000 30000000
```

### Unbond

When you 'unbond' your balance, you can transfer or withdraw tokens from your account. Your token cannot be liquidated right after 'unbond' execution, needed some days after.

`clif hdac unbond --wallet|--address|--nickname <owner> <amount> <fee> <gas-price>`

```bash
clif hdac unbond --wallet walletelsa 1000000 100000000 30000000
clif hdac unbond --nickname princesselsa 1000000 100000000 30000000
clif hdac unbond --address friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv 1000000 100000000 30000000
```

{% hint style="info" %}
If you want to learn about wallet more, please check [What is wallet](../mechanism-and-features-description/what-is-wallet.md).
{% endhint %}

