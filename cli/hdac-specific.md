# Hdac specific

### Transfer

Transfer Hdac token to another account

```text
clif hdac transfer-to <recipient_address_or_nickname> <amount> <fee> <gas-price> --address|--wallet|--nickname <from>
```

In `recipient_address_or_nickname`, CLI automatically check and parse for easy use.

```text
clif hdac transfer-to sisteranna 1000000 100000000 20000000 --wallet walletelsa
clif hdac transfer-to sisteranna 1000000 100000000 20000000 --nickname princesselsa
clif hdac transfer-to sisteranna 1000000 100000000 20000000 --address friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv

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

### Get balance

Check the value of owner's balance

```text
clif hdac getbalance --nickname|--wallet|--address <owner>
```

```text
clif hdac getbalance --wallet walletelsa
clif hdac getbalance --nickname princesselsa
clif hdac getbalance --address friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv

{
   "value": "5000000000000"
}
```

### Bond Hdac token

Bond Hdac token for doing useful activity of Hdac ecosystem

```text
clif hdac bond --wallet|--address|--nickname <owner> <amount> <fee> <gas-price>
```

```text
clif hdac bond --wallet walletelsa 1000000 100000000 30000000
clif hdac bond --nickname princesselsa 1000000 100000000 30000000
clif hdac bond --address friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv 1000000 100000000 30000000

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

```text
clif hdac unbond --wallet|--address|--nickname <owner> <amount> <fee> <gas-price>
```

```text
clif hdac unbond --wallet walletelsa 1000000 100000000 30000000
clif hdac unbond --nickname princesselsa 1000000 100000000 30000000
clif hdac unbond --address friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv 1000000 100000000 30000000

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

### Create validator

Create and join as a validator with an address for reward and BLS-based validator consensus key

```text
clif hdac create-validator --from <wallet_alias> --pubkey <BLS_valconspub_key> [--moniker <moniker>] [--identity <identity>] [--website <website>] [--details <details>]
```

```text
nodef tendermint show-validator
# fridayvalconspub16jrl8jvqq929y3r2dem455nptpd9g3mn0929q5eswaay6365vdtrx6j42dkrxtek24n5ycmpfax9s4mp9apkgkpe2vux64e0xe3xz5f09ucrje6e25cxwe3tf3kxjc6gfesnyv308p382ujc24snqn2kwfq45j60gc6nqs6wvfp8xen3d3ersnjnxfmrv6jv8pjxsmjtv3kxcapc09y5w5sa9v92q

 clif hdac create-validator \
--from bryan \
--pubkey fridayvalconspub16jrl8jvqq929y3r2dem455nptpd9g3mn0929q5eswaay6365vdtrx6j42dkrxtek24n5ycmpfax9s4mp9apkgkpe2vux64e0xe3xz5f09ucrje6e25cxwe3tf3kxjc6gfesnyv308p382ujc24snqn2kwfq45j60gc6nqs6wvfp8xen3d3ersnjnxfmrv6jv8pjxsmjtv3kxcapc09y5w5sa9v92q \
--moniker valiator-bryan

# or --pubkey $(nodef tendermint show-validator)
```

