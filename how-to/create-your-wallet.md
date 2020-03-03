# Create your wallet

{% hint style="info" %}
Before create your wallet, make sure you have [install Friday](../first-step/installation.md)
{% endhint %}

## Create Your Wallet

First of all, you need your own wallet. It is needed when you execute action and send your own message to Tx. This section describes what is wallet, what you can get, and how to execute with your wallet.

{% hint style="info" %}
If you passed the tutorial "[Deploy Your Own Friday Testnet](../first-step/deploy-your-own-friday-testnet.md)", you already created your wallet. In there, the step proceeded without any detail information. If you're already done it, please just read the below and learn what the each step means.
{% endhint %}

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

If you lost your wallet data, you can restore it if you knew the mnemonic words.

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

## 

{% hint style="info" %}
If you want to learn about wallet more, please check [What is wallet](../mechanism-and-features-description/what-is-wallet.md).
{% endhint %}

