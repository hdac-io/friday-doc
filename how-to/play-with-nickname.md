# Play with nickname

Every CLI interface has `--nickname` flag. Set your nickname and achieve your convenience.

## Get balance

`clif hdac getbalance --wallet|--nickname|--address` 

```bash
clif hdac getbalance --wallet walletelsa
clif hdac getbalance --nickname princesselsa
clif hdac getbalance --address friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv

# Response:
# {
#  "value": "5000000000000"
# }
```

## Transfer

`clif hdac transfer-to <recipient_address_or_nickname> <amont> <fee> <gas-price> --address|--wallet|--nickname <from>`

```bash
clif hdac transfer-to sisteranna 1000000 100000000 20000000 --wallet walletelsa
clif hdac transfer-to sisteranna 1000000 100000000 20000000 --nickname princesselsa
clif hdac transfer-to sisteranna 1000000 100000000 20000000 --address friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv
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

## Bond

`clif hdac bond --wallet|--address|--nickname <owner> <amount> <fee> <gas-price>`

```bash
clif hdac bond --wallet walletelsa 1000000 100000000 30000000
clif hdac bond --nickname princesselsa 1000000 100000000 30000000
clif hdac bond --address friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv 1000000 100000000 30000000
```

## Unbond

`clif hdac unbond --wallet|--address|--nickname <owner> <amount> <fee> <gas-price>`

```bash
clif hdac unbond --wallet walletelsa 1000000 100000000 30000000
clif hdac unbond --nickname princesselsa 1000000 100000000 30000000
clif hdac unbond --address friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv 1000000 100000000 30000000
```

