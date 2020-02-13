# Set Your Nickname

## Set Nickname - Address Mapping

You may register by address, and the address can be access from two ways: wallet alias and address \(e.g. friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv\)

* Usage:
  * By address: `clif nickname set princesselsa --address friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv`
  * By local wallet alias: `clif nickname set princesselsa --wallet walletelsa`
* Example

```bash
clif nickname set princesselsa --address friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv
clif nickname set princesselsa --wallet walletelsa
```

```bash
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

## Check Nickname - Address Mapping Status

* Usage: `clif nickname get-address princesselsa`
* Example

```bash
clif nickname get-address princesselsa

{
  "name": "princesselsa",
  "address": "friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv"
}
```

## Change Public Key of Nickname

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



