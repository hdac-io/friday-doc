# Work with RPC server

## Run RPC server

```text
clif rest-server
```

## How to send Tx via RPC server

### Execute appropriate API endpoint

#### Call appropriate endpoint

Note: The example is made from `transfer-to`

**POST** `http://<remote_address>:<port>/hdac/transfer`

Request header: `Content-type: applicatoin/json`

Request body:

```javascript
{
	"base_req": {
		"chain_id": "testnet",
		"gas": "20000000",
		"memo": "",
		"from": "friday19sw6aqmy2qdwg54lnkzl7qjxcds8pnx3dclu8n"
	},
	"recipient_address_or_nickname":"friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv",
	"amount":"1000000",
	"fee": "100000000"
}
```

#### Take a response which is the unsigned tx

```javascript
{
    "type": "friday/StdTx",
    "value": {
        "msg": [
            {
                "type": "executionengine/Transfer",
                "value": {
                    "contract_address": "",
                    "from_address": "friday19sw6aqmy2qdwg54lnkzl7qjxcds8pnx3dclu8n",
                    "to_address": "friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv",
                    "amount": "1000000",
                    "fee": "100000000",
                    "gas_price": "20000000"
                }
            }
        ],
        "fee": {
            "amount": [],
            "gas": "20000000"
        },
        "signatures": null,
        "memo": ""
    }
}
```

You may see that `signatures` is null. Now, we should fill this part in local.

{% hint style="info" %}
If you want to know why the signature step cannot be proceeded in remote and only can be in local, please read [this article](../mechanism-and-features-description/what-is-wallet.md#the-auth-data-will-not-be-and-doesnt-have-to-be-stored-in-any-of-node).
{% endhint %}

### Sign the Tx

#### Get user information

**GET** `http://<remote_address>:<port>/auth/accounts/<address>`

Response:

```javascript
{
  "height": "187",
  "result": {
    "type": "friday/Account",
    "value": {
      "address": "friday19sw6aqmy2qdwg54lnkzl7qjxcds8pnx3dclu8n",
      "coins": [
        {
          "denom": "dummy",
          "amount": "5000000000000"
        }
      ],
      "public_key": {
        "type": "tendermint/PubKeySecp256k1",
        "value": "A+UmciOYYvW4k9hy/yshq5i+XOK7bv6gIWWfLnInah7o"
      },
      "account_number": "0",
      "sequence": "1"
    }
  }
}
```

You should keep the value of `account_number` and `sequence`.

**\[Note\]** Don't have to +1 of your sequence. If you get "1", just use "1", not "2".

#### Prepare sign message

This is the basic form of sign message. The key of JSON should be sorted.

```javascript
{
    "chain_id": "<chain_id>",
    "account_number": "<account_number>",
    "fee": {
        "gas": "<gas_price>",
        "amount": [],
    },
    "memo": "<memo>",
    "sequence": "<sequence>",
    "msgs": "<message_array>",
}
```

So, the sign message of the case above would be like this:

```javascript
{
    "chain_id": "testnet",
    "account_number": "0",
    "fee": {
        "gas": "20000000",
        "amount": [],
    },
    "memo": "",
    "sequence": "1",
    "msgs": [
        {
            "type": "executionengine/Transfer",
            "value": {
                "contract_address": "",
                "from_address": "friday19sw6aqmy2qdwg54lnkzl7qjxcds8pnx3dclu8n",
                "to_address": "friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv",
                "amount": "1000000",
                "fee": "100000000",
                "gas_price": "20000000"
            }
        }
    ],
}
```

#### Make signature

1. Sort the prepared sign message by the key
2. Encode into byte by UTF-8
3. Prepare your private key \(Secp256k1\)
4. Sign the message using the prepared message and signing function of cryptographic library of your language. \(Hash function: SHA256\)
5. Encode the signature into Base64, and take it

#### Encode Secp256k1 public key to Base64

Take 33 byted-Secp256k1 public key, and encode into Base64

#### Prepare pushable Tx

This is the basic form of pushable Tx.

```javascript
{
    "tx": {
        "msg": "<msg_array>",
        "fee": {
            "gas": "<gas_price>",
            "amount": [],
        },
        "memo": "<memo>",
        "signatures": [
            {
                "signature": "<signature>",
                "pub_key": {
                    "type": "tendermint/PubKeySecp256k1",
                    "value": "<secp256k1_public_key>"
                },
                "account_number": "<account_num>",
                "sequence": "<seq_number>",
            }
        ],
    },
    "mode": "<sync|async>",
}
```

In the case, the pushable message would like this:

```javascript
{
    "tx": {
        "msg": [
            {
                "type": "executionengine/Transfer",
                "value": {
                    "contract_address": "",
                    "from_address": "friday19sw6aqmy2qdwg54lnkzl7qjxcds8pnx3dclu8n",
                    "to_address": "friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv",
                    "amount": "1000000",
                    "fee": "100000000",
                    "gas_price": "20000000"
                }
            }
        ],
        "fee": {
            "gas": "20000000",
            "amount": [],
        },
        "memo": "",
        "signatures": [
            {
                "signature": "hJdZJcMfVDccOKaUh8wPdl5ks2ACbCImI4CE5KB5Z7xTZcxbH/Q69H94TDHMw0dXbv0inS6hAYQrQixRWTlOIA==",
                "pub_key": {
                    "type": "tendermint/PubKeySecp256k1",
                    "value": "A+UmciOYYvW4k9hy/yshq5i+XOK7bv6gIWWfLnInah7o"
                },
                "account_number": "0",
                "sequence": "1",
            }
        ],
    },
    "mode": "sync",
}
```

### Send Tx

#### Call send tx API

**POST** `http://<remote_address>:<port>/txs`

Request header: `Content-type: applicatoin/json`

Request body: entire JSON of the pushable tx above  
In this case, it would be as like below:

```javascript
{
    "tx": {
        "msg": [
            {
                "type": "executionengine/Transfer",
                "value": {
                    "contract_address": "",
                    "from_address": "friday19sw6aqmy2qdwg54lnkzl7qjxcds8pnx3dclu8n",
                    "to_address": "friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv",
                    "amount": "1000000",
                    "fee": "100000000",
                    "gas_price": "20000000"
                }
            }
        ],
        "fee": {
            "gas": "20000000",
            "amount": [],
        },
        "memo": "",
        "signatures": [
            {
                "signature": "hJdZJcMfVDccOKaUh8wPdl5ks2ACbCImI4CE5KB5Z7xTZcxbH/Q69H94TDHMw0dXbv0inS6hAYQrQixRWTlOIA==",
                "pub_key": {
                    "type": "tendermint/PubKeySecp256k1",
                    "value": "A+UmciOYYvW4k9hy/yshq5i+XOK7bv6gIWWfLnInah7o"
                },
                "account_number": "0",
                "sequence": "1",
            }
        ],
    },
    "mode": "sync",
}
```

Response:

```javascript
{
  "height": "0",
  "txhash": "00218B2D3627DC3838730188A1DD06DCA6C58B563DC6688FF51E96035DFD3DDB",
  "raw_log": "[{\"msg_index\":0,\"success\":true,\"log\":\"\",\"events\":[{\"type\":\"message\",\"attributes\":[{\"key\":\"action\",\"value\"
:\"executionengine\"}]}]}]",
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

TADA~~ finally, you can send Tx via JSON RPC!!

But it is not the end of that.

1. Check `success` whether it is true or not
2. It can be `false` in broadcasting phase although it appears `true`

### Check the sent Tx is really finalized or not

Take `txhash` and check the `success` keyword.

**GET** `http://<remote_address>:<port>/txs/<tx_hash>`

```javascript
{
  "height": "502",
  "txhash": "00218B2D3627DC3838730188A1DD06DCA6C58B563DC6688FF51E96035DFD3DDB",
  "code": 0,
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
  "gas_wanted": "22000000",
  "gas_used": "5499449",
  "tx": {
    "type": "friday/StdTx",
    "value": {
      "msg": [
        {
          "type": "executionengine/Transfer",
          "value": {
            "contract_address": "dummyAddress",
            "from_address": "friday19sw6aqmy2qdwg54lnkzl7qjxcds8pnx3dclu8n",
            "to_address": "friday15evpva2u57vv6l5czehyk69s0wnq9hrkqulwfz",
            "amount": "1000000000",
            "fee": "100000000",
            "gas_price": "22000000"
          }
        }
      ],
      "fee": {
        "amount": [],
        "gas": "22000000"
      },
      "signatures": [
        {
          "pub_key": {
            "type": "tendermint/PubKeySecp256k1",
            "value": "A+UmciOYYvW4k9hy/yshq5i+XOK7bv6gIWWfLnInah7o"
          },
          "signature": "hJdZJcMfVDccOKaUh8wPdl5ks2ACbCImI4CE5KB5Z7xTZcxbH/Q69H94TDHMw0dXbv0inS6hAYQrQixRWTlOIA=="
        }
      ],
      "memo": ""
    }
  },
  "timestamp": "2020-02-18T08:51:40Z",
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

You can see `success: true`!  
Real TADA~~! 

## Complicated?

You may use python SDK \(Unofficial\). It is in [https://github.com/psy2848048/hdacpy](https://github.com/psy2848048/hdacpy)  
In Javascript, you may contribute from forking with [https://github.com/cosmostation/cosmosjs](https://github.com/cosmostation/cosmosjs)

