# Block/Tx

## Check block

```text
clif query block [<height>]
```

```bash
clif query block 9

{
  "block_meta": {
    "block_id": {
      "hash": "7D5093C80E0437F9B055CDCE22C997E7EF3199D29C2E5514217C35E8B9C52B83",
      "parts": {
        "total": "1",
        "hash": "EFB9E66F90A01699F9427D51CD0553452D32FFC62B2627600D1FA4D1A3D5E1AA"
      }
    },
    "header": {
      "version": {
        "block": "10",
        "app": "0"
      },
      "chain_id": "testnet",
      "height": "18442",
      "time": "2020-04-29T02:20:21.321092Z",
      "num_txs": "0",
      "total_txs": "0",
      "last_block_id": {
        "hash": "25FD45134927C68D6F6B8CF6EAAFE1364CB04C007B5BE694A789C7DBCB35BB38",
        "parts": {
          "total": "1",
          "hash": "2C84C9C23A5D5D4B829A04F90BBD9B3F25AE4BF95B2C7FB14A433A7E95E1F783"
        }
      },
  ...
```

If you not provide `height`, it queries the latest block.

## Check transaction status

```bash
clif query tx <tx_hash>
```

```bash
clif query tx C2C79032C284AF93E35BACC3588DB81B5469A214A6F16664C7DAF4FB3A783F81

{
  "height": "18504",
  "txhash": "C2C79032C284AF93E35BACC3588DB81B5469A214A6F16664C7DAF4FB3A783F81",
  "raw_log": "[{\"msg_index\":0,\"success\":true,\"log\":\"\",\"events\":[{\"type\":\"message\",\"attributes\":[{\"key\":\"action\",
\"value\":\"executionengine\"}]}]}]",
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
  ],
  ...
  "fee": {
        "amount": [],
        "gas": "30000000"
      },
      "signatures": [
        {
          "pub_key": {
            "type": "tendermint/PubKeySecp256k1",
            "value": "AjR7cbcSEpAHRGtijMPfaCCjLm7VVSeXpbROTnzkQigI"
          },
          "signature": "xtYM73M6r1tTIEYx+DAFLGlSRXitXDn8jlMtjJrUZrICtx6+vWvn2lqhHNhlMQ1ckOIn5jZaOz5IbWwvK3Swqg=="
        }
      ],
      "memo": ""
    }
  },
  "timestamp": "2020-04-29T02:25:35Z",
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
```

