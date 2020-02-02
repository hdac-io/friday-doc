# Set & use nickname

## What & why nickname?

In transfer, currently we should execute as like that, in address form:  
`friday15evpva2u57vv6l5czehyk69s0wnq9hrkqulwfz` -&gt;  `friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv`

It is very hard to check and remember this kind of complex way. It needs to implement our kind-of-ID in readable way.

Nickname feature is kind of alias of the address that helps users remembering and reducing risks against mistakes.

#### Different point of wallet alias

* Wallet alias: Local mapping. Other users cannot use it in transfer, or some other function.
* Nickname: Global mapping. All users can use nickname instead of address

## Nickname handling

Hdac main supports readable nickname for better usage. You may organize up to **20 letters** with 0-9, a-z, '-', '.', and '\_' . With this feature, you don't have to memo recipient's complex hashed address. Just remember easy address and send token!  
Of course, you can also use previous hashed address system. This is optional for your availability.

### Set nickname - address mapping

You may register by address, and the address can be access from two ways: wallet alias and address \(e.g. friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv\)

* Usage:
  * By address: `clif nickname set princesselsa --address friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv`
  * By local wallet alias: `clif nickname set princesselsa --wallet walletelsa`
* Example

```bash
clif nickname set princesselsa --address friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv
clif nickname set princesselsa --wallet walletelsa
```

```javascript
{
  "chain_id": "testnet",
  "account_number": "1",
  "sequence": "2",
  "fee": {
    "amount": [],
    "gas": "200000"
  },
  "msgs": [
    {
      "type": "readablename/SetName",
      "value": {
        "name": {
          "H": "1183736206936",
          "L": "0"
        },
        "address": "friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv",
        "pubkey": "AiJmIrS9ZPdCWmzQ92BZUxzJ49eGdF0aTCPw60a+Ft/2"
      }
    }
  ],
  "memo": ""
}
```

After confirmation, you may use `princesselsa` as a nickname instead of `friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv` . Of course, `friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv` is still valid to use.  
\(NOTE: `elsa` is an alias of your local wallet, **not nickname**. The name of the local wallet alias is not stored in mainnet.\)

### Check nickname - address mapping status

* Usage: `clif nickname get-address princesselsa`
* Example

```bash
clif nickname get-address princesselsa

{
  "name": "princesselsa",
  "address": "friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv"
}
```

### Change public key of readable ID

* Usage
  * By wallet alias: `clif nickname change-to princesselsa friday15evpva2u57vv6l5czehyk69s0wnq9hrkqulwfz --wallet walletanna`
  * By address directly: `clif nickname change-to princesselsa friday15evpva2u57vv6l5czehyk69s0wnq9hrkqulwfz --address friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv`

```bash
clif nickname change-to princesselsa friday15evpva2u57vv6l5czehyk69s0wnq9hrkqulwfz --wallet walletanna
clif nickname change-to princesselsa friday15evpva2u57vv6l5czehyk69s0wnq9hrkqulwfz --address friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv

friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv  ->  friday15evpva2u57vv6l5czehyk69s0wnq9hrkqulwfz

{
  "chain_id": "testnet",
  "account_number": "6",
  "sequence": "0",
  "fee": {
    "amount": [],
    "gas": "200000"
  },
  "msgs": [
    {
      "type": "readablename/ChangeKey",
      "value": {
        "ID": "bryan",
        "old_address": "friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv",
        "new_address": "friday15evpva2u57vv6l5czehyk69s0wnq9hrkqulwfz",
        "old_pubkey": "A3CcPTCO6Jr9Q0HSlgOK4unk+atk4n71AK2J+sQqkKy1",
        "new_pubkey": "A7TB68o82IU3jpP0oiiGVtN9glfWtyWiBE+k4gFWMq3Q"
      }
    }
  ],
  "memo": ""
}
```

## Operation with nickname

Every CLI interface has `--nickname` flag. Set your nickname and achieve your convenience.

* Get balance: `clif hdac getbalance --wallet|--nickname|--address` 

```bash
clif hdac getbalance --wallet walletelsa
clif hdac getbalance --nickname princesselsa
clif hdac getbalance --address friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv

# Response:
# {
#  "value": "5000000000000"
# }
```

* Transfer: 
  * `clif hdac transfer-to <recipient_address_or_nickname> <amont> <fee> <gas-price> --address|--wallet|--nickname <from>`

```bash
clif hdac transfer-to sisteranna 1000000 100000000 20000000 --wallet walletelsa
clif hdac transfer-to sisteranna 1000000 100000000 20000000 --nickname princesselsa
clif hdac transfer-to sisteranna 1000000 100000000 20000000 --address friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv
```

```javascript
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

* Bond
  * `clif hdac bond --wallet|--address|--nickname <owner> <amount> <fee> <gas-price>`

```bash
clif hdac bond --wallet walletelsa 1000000 100000000 30000000
clif hdac bond --nickname princesselsa 1000000 100000000 30000000
clif hdac bond --address friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv 1000000 100000000 30000000
```

* Unbond
  * `clif hdac unbond --wallet|--address|--nickname <owner> <amount> <fee> <gas-price>`

```bash
clif hdac unbond --wallet walletelsa 1000000 100000000 30000000
clif hdac unbond --nickname princesselsa 1000000 100000000 30000000
clif hdac unbond --address friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv 1000000 100000000 30000000
```

