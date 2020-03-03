# Hdac specific

### Get balance

Check the value of owner's balance

```bash
clif hdac getbalance --from <wallet|nickname|address>
```

```bash
clif hdac getbalance --from walletelsa
clif hdac getbalance --from princesselsa
clif hdac getbalance --from friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv

{
   "value": "5000000000000"
}
```

### Transfer

Transfer Hdac token to another account

```bash
clif hdac transfer-to <recipient_address_or_nickname> <amont> <fee> <gas-price> --from <wallet|nickname|address>
```

In `recipient_address_or_nickname`, CLI automatically check and parse for easy use.

```bash
clif hdac transfer-to friday15evpva2u57vv6l5czehyk69s0wnq9hrkqulwfz 10 0.01 30000000 --from walletelsa
clif hdac transfer-to friday15evpva2u57vv6l5czehyk69s0wnq9hrkqulwfz 10 0.01 30000000 --from princesselsa
clif hdac transfer-to sisteranna 10 0.01 30000000 --from friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv

...
<some texts of tx>
...
confirm transaction before signing and broadcasting [y/N]: y
Password to sign with 'walletelsa': # input your password

...
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

### Create validator

Create and join as a validator with an address for reward and BLS-based validator consensus key

```bash
clif hdac create-validator --from <wallet_alias> --pubkey <BLS_valconspub_key> [--moniker <moniker>] [--identity <identity>] [--website <website>] [--details <details>]
```

```bash
clif hdac create-validator \
--from princesselsa \
--pubkey $(nodef tendermint show-validator) \
--moniker frozen
```

### Bond Hdac token

Bond Hdac token for doing useful activity of Hdac ecosystem

```bash
clif hdac bond --from <wallet|nickname|address> <amount> <fee> <gas-price>
```

```bash
clif hdac bond --from walletelsa 10 0.01 30000000
clif hdac bond --from princesselsa 10 0.01 30000000
clif hdac bond --from friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv 10 0.01 30000000

confirm transaction before signing and broadcasting [y/N]: y
Password to sign with 'walletelsa':
{
  "height": "0",
  "txhash": "22DF1E0D8D9EB8BE2B5F50995C6FC0AB20E34715875A9F9856A9466A8C406807",
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

### Unbond Hdac token

Unbond Hdac token for liquidating

```bash
clif hdac unbond --from <wallet|nickname|address> <amount> <fee> <gas-price>
```

```bash
clif hdac unbond --from walletelsa 10 0.01 30000000
clif hdac unbond --from princesselsa 10 0.01 30000000
clif hdac unbond --from friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv 10 0.01 30000000

confirm transaction before signing and broadcasting [y/N]: y
Password to sign with 'walletelsa':
{
  "height": "0",
  "txhash": "69C51D25E3E5DB4F2D4ACE832C775DC8EE993E9CDB7560A3AF470FF07CC7FFC9",
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

