# Token

## Token manipulation

### Get balance

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

```bash
clif hdac transfer-to <recipient_address_or_nickname> <amont> <fee> --from <wallet|nickname|address>
```

In `recipient_address_or_nickname`, CLI automatically check and parse for easy use.

```bash
clif hdac transfer-to friday15evpva2u57vv6l5czehyk69s0wnq9hrkqulwfz 10 0.01 --from walletelsa
clif hdac transfer-to friday15evpva2u57vv6l5czehyk69s0wnq9hrkqulwfz 10 0.01 --from princesselsa
clif hdac transfer-to sisteranna 10 0.01 --from friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv

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

## Ecosystem participation

### Bond Hdac token \(Self delegation\)

```
clif hdac bond --from <wallet|nickname|address> <amount> <fee>
```

```bash
clif hdac bond --from walletelsa 10 0.01
clif hdac bond --from princesselsa 10 0.01
clif hdac bond --from friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv 10 0.01

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

### Unbond Hdac token \(Undelegation of self-delegated\)

```bash
clif hdac unbond --from <wallet|nickname|address> <amount> <fee>
```

```bash
clif hdac unbond --from walletelsa 10 0.01
clif hdac unbond --from princesselsa 10 0.01
clif hdac unbond --from friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv 10 0.01

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

### Delegation

```bash
clif hdac delegate <delegatee_address> <amount> <fee> --from <delegator_wallet_alias>
```

```bash
clif hdac delegate friday135260rslw2gkhpph2gjclm5zvvaeaf9qw7tknm 1 0.002 --from enna

{
  "height": "0",
  "txhash": "2FAF79E92B5414683D5F33DAF6D7784AE9B626BF84AEBF3AF8B204CD774E0D57",
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

### Undelegation

```bash
clif hdac undelegate <delegatee_address> <amount> <fee> --from <delegator_wallet_alias>
```

```bash
clif hdac undelegate friday135260rslw2gkhpph2gjclm5zvvaeaf9qw7tknm 1 0.002 --from enna

{
  "height": "0",
  "txhash": "2FAF79E92B5414683D5F33DAF6D7784AE9B626BF84AEBF3AF8B204CD774E0D57",
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

### Redelegation

```bash
clif hdac redelegate <from_delegatee> <to_delegatee> <amount> <fee> --from <delegator_wallet_alias>
```

```bash
clif hdac redelegate friday135260rslw2gkhpph2gjclm5zvvaeaf9qw7tknm friday1yvmwx4j67qfryn8k2m0xxv8vvcn87cy4wz8wlg 1 0.002 30000000 --from enna

{
  "height": "0",
  "txhash": "2FAF79E92B5414683D5F33DAF6D7784AE9B626BF84AEBF3AF8B204CD774E0D57",
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

## Vote

### Vote

```bash
clif hdac vote <contract_hash> <amount> <fee> --from <wallet_alias>
```

```bash
clif hdac vote 0000000000000000000000000000000000000000000000000000000000000000 1 0.002 30000000 --from fridayxxxxxxxxxxxxx
```

### Unvote

```bash
clif hdac unvote <contract_hash> <amount> <fee> --from <wallet_alias>
```

```bash
clif hdac unvote 0000000000000000000000000000000000000000000000000000000000000000 1 0.002 30000000 --from fridayxxxxxxxxxxxxx
```

### Query vote status

```bash
clif hdac voter <contract_hash> [--from <holders_address>]
```

```bash
clif hdac voter AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=

[
  {
    "address": "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=",
    "amount": "1000000000000000000"
  }
]
```

## Claim

### Claim commission \(validators\)

```bash
clif hdac claim commission <fee> --from <wallet_alias>
clif hdac claim commission 0.001 --from alsa
```

### Claim reward \(holders\)

```bash
clif hdac claim reward <fee> --from <wallet_alias>
clif hdac claim reward 0.001 --from alsa
```

### Querying unclaimed reward/commission status

```bash
clif hdac getreward --from <wallet_alias>
clif hdac getcommission --from <wallet_alias>
```

```bash
clif hdac getreward --from alsa
10840.472696777918278845

clif hdac getcommission --from alsa
10842.137486639570739678
```

