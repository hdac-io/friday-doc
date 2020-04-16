# Play with Hdac Token

## Introduction

In this chapter, you can make a fun with Hdac token. You may learn what and how to do with the token.

Currently, the testnet supports:

* Get balance
* Transfer
* Bond / Unbond

## Check and Transfer Token

Now, you can manage your token with your own wallet. Input command below and experience the wallet by yourself.

### Get balance

`clif hdac getbalance --from <wallet|nickname|address>` 

```bash
clif hdac getbalance --from walletelsa
clif hdac getbalance --from princesselsa
clif hdac getbalance --from friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv

# Response:
# {
#  "value": "5000000000000"
# }
```

### Transfer

`clif hdac transfer-to <recipient_address_or_nickname> <amont> <fee> <gas-price> --from <wallet|nickname|address>`

```bash
clif hdac transfer-to friday15evpva2u57vv6l5czehyk69s0wnq9hrkqulwfz 10 0.01 30000000 --from walletelsa
clif hdac transfer-to friday15evpva2u57vv6l5czehyk69s0wnq9hrkqulwfz 10 0.01 30000000 --from princesselsa
clif hdac transfer-to friday15evpva2u57vv6l5czehyk69s0wnq9hrkqulwfz 10 0.01 30000000 --from friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv
```

```bash
// ... confirm transaction before signing and broadcasting [y/N]: y
// Password to sign with 'wallwtelsa': # 
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

## Bond and Unbond Token

### Bond

Bond is a kind of time deposit. When you 'bond' your balance, you cannot transfer or withdraw tokens from your account. Instead, you can participate Hdac ecosystem, and get interests as a reward.

`clif hdac bond --from <wallet|nickname|address> <amount> <fee> <gas-price>`

```bash
clif hdac bond --from walletelsa 10 0.01 30000000
clif hdac bond --from princesselsa 10 0.01 30000000
clif hdac bond --from friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv 10 0.01 30000000
```

### Unbond

When you 'unbond' your balance, you can transfer or withdraw tokens from your account. Your token cannot be liquidated right after 'unbond' execution, needed some days after.

`clif hdac unbond --from <wallet|nickname|address> <amount> <fee> <gas-price>`

```bash
clif hdac unbond --from walletelsa 10 0.01 30000000
clif hdac unbond --from princesselsa 10 0.01 30000000
clif hdac unbond --from friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv 10 0.01 30000000
```



