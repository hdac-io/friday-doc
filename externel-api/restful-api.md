---
description: RESTFul API endpoint description
---

# RESTFul API

### Step to send Tx

* Run rest server

```text
clif rest-server
```

* Execute appropriate API endpoint
  * If sending tx purpose, proceed to sign tx
  * If query purpose, use the response
* Get account number & sequence from \`Get account info\` API
* Sign tx in local, and take the result
  * The response is unsigned tx
  * The API endpoint itself do not send the tx
* Send the response above with \`Send Tx\` API

Example python SDK \(Unofficial\) is in [https://github.com/psy2848048/hdacpy](https://github.com/psy2848048/hdacpy)  
In Javascript, you may contribute from forking with [https://github.com/cosmostation/cosmosjs](https://github.com/cosmostation/cosmosjs)

{% api-method method="get" host="https://localhost:1317" path="/blocks/<height>" %}
{% api-method-summary %}
Check block
{% endapi-method-summary %}

{% api-method-description %}
Get block infor of given height
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="height" type="integer" required=true %}
Height of block
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    block_meta: {
        block_id: {
            hash: "D3CDC7F5AE4FD4DA6089E763B2C65396F087D267CDBB3A32721402322DAB91D5",
            parts: {
                total: "23",
                hash: "0A2199FFAF6044260E69EF7BED9B00B72C6FC25D24E4B78E8686CCE090B5081A"
            }
        },
        header: {
            version: {
                block: "10",
                app: "0"
            },
            chain_id: "friday-devnet",
            height: "6",
            time: "2019-12-21T12:55:31.817734Z",
            num_txs: "1",
            total_txs: "1",
            last_block_id: {
                hash: "FB82EAC2527DF97F1B6D602DDD666BC33A93ED89F4C6767E1001E26FD5DF1156",
                parts: {
                    total: "1",
                    hash: "A34081FFB9D23E43CDFC54A025C8F22FD8D59EB04B02689080A57A52565419F1"
                }
            },
            last_commit_hash: "83CE78D03180C1B4C2F52FE826C48E72B4077C512100B71D0DA966DC52DD9255",
            data_hash: "C1F3F67A513A8493A544DEB22D3488397AA203F13527E7A78402C4D973886EAE",
            validators_hash: "FBEA4619EF72C75E9FEFBDE103D4863ECA7CF98CB55CC6F84FAF3295623FCFF2",
            next_validators_hash: "FBEA4619EF72C75E9FEFBDE103D4863ECA7CF98CB55CC6F84FAF3295623FCFF2",
            consensus_hash: "048091BC7DDC283F77BFBF91D73C44DA58C3DF8A9CBC867405D8B7F3DAADA22F",
            app_hash: "6D1B04A1BFA534D78C3181D481133A2AF1CD0DE94B2F2EFB6FA9BCA37FC5DDDA",
            last_results_hash: "",
            evidence_hash: "",
            proposer_address: "724FCF87E77AE1EB4CB730A4EC30BF69BC94FE8B"
        }
    },
    block: {
        header: {
            version: {
                block: "10",
                app: "0"
            },
            chain_id: "friday-devnet",
            height: "6",
            time: "2019-12-21T12:55:31.817734Z",
            num_txs: "1",
            total_txs: "1",
            last_block_id: {
                hash: "FB82EAC2527DF97F1B6D602DDD666BC33A93ED89F4C6767E1001E26FD5DF1156",
                parts: {
                    total: "1",
                    hash: "A34081FFB9D23E43CDFC54A025C8F22FD8D59EB04B02689080A57A52565419F1"
                }
            },
            last_commit_hash: "83CE78D03180C1B4C2F52FE826C48E72B4077C512100B71D0DA966DC52DD9255",
            data_hash: "C1F3F67A513A8493A544DEB22D3488397AA203F13527E7A78402C4D973886EAE",
            validators_hash: "FBEA4619EF72C75E9FEFBDE103D4863ECA7CF98CB55CC6F84FAF3295623FCFF2",
            next_validators_hash: "FBEA4619EF72C75E9FEFBDE103D4863ECA7CF98CB55CC6F84FAF3295623FCFF2",
            consensus_hash: "048091BC7DDC283F77BFBF91D73C44DA58C3DF8A9CBC867405D8B7F3DAADA22F",
            app_hash: "6D1B04A1BFA534D78C3181D481133A2AF1CD0DE94B2F2EFB6FA9BCA37FC5DDDA",
            last_results_hash: "",
            evidence_hash: "",
            proposer_address: "724FCF87E77AE1EB4CB730A4EC30BF69BC94FE8B"
        },
        data: {
            txs: [
                "<parsed_tx_data>"
            ]
        },
        evidence: {
            evidence: null
        },
        last_commit: {
            block_id: {
                hash: "FB82EAC2527DF97F1B6D602DDD666BC33A93ED89F4C6767E1001E26FD5DF1156",
                parts: {
                    total: "1",
                    hash: "A34081FFB9D23E43CDFC54A025C8F22FD8D59EB04B02689080A57A52565419F1"
                }
            },
            precommits: [
                {
                    type: 2,
                    height: "5",
                    round: "0",
                    block_id: {
                        hash: "FB82EAC2527DF97F1B6D602DDD666BC33A93ED89F4C6767E1001E26FD5DF1156",
                        parts: {
                            total: "1",
                            hash: "A34081FFB9D23E43CDFC54A025C8F22FD8D59EB04B02689080A57A52565419F1"
                        }
                    },
                    timestamp: "2019-12-21T12:55:31.817734Z",
                    validator_address: "724FCF87E77AE1EB4CB730A4EC30BF69BC94FE8B",
                    validator_index: "0",
                    signature: "lUFY8r/bSED9/PV2u41O56KrFsATI3fN8HwNQq7xl6sqtj6y5Go6rSAtUp/21XdjIkuaRpb95ybk7hZSWRL5Dw=="
                }
            ]
        }
    }
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://localhost:1317" path="/auth/accounts/<friday\_address>" %}
{% api-method-summary %}
Get account info
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="friday\_address" type="string" required=false %}
Address of friday system
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "height": "552",
    "result": {
        "type": "friday/Account",
        "value": {
            "address": "friday1lgharzgds89lpshr7q8kcmd2esnxkfpwmfgk32",
            "coins": [
                {
                    "denom": "dummy",
                    "amount": "99226000"
                }
            ],
            "public_key": {
                "type": "tendermint/PubKeySecp256k1",
                "value": "A49sjCd3Eul+ZXyof7qO460UaO73otrmySHyTNSLW+Xn"
            },
            "account_number": "8",
            "sequence": "2"
        }
    }
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://localhost:1317" path="/txs/<tx\_hash>" %}
{% api-method-summary %}
Check Tx
{% endapi-method-summary %}

{% api-method-description %}
Check the whole JSON data of given tx hash
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="tx\_hash" type="string" required=true %}
Broadcasted tx hash
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
  "hash": "string",
  "height": 0,
  "tx":{
        "fee":{
            "amount":[],
            "gas":"37000"
        },
        "memo":"",
        "msg":[
            {
                "type":"executionengine/Execute",
                "value":{
                    "block_hash":"AA==",
                    "contract_owner_account":"friday1lgharzgds89lpshr7q8kcmd2esnxkfpwmfgk32",
                    "exec_account":"friday1lgharzgds89lpshr7q8kcmd2esnxkfpwmfgk32",
                    "gas_price":"2000000",
                    "payment_args":"AQAAAAQAAAADgIQe",
                    "session_args":"AgAAABgAAAAUAAAAFX2WU5Tkt49ZzKlXxfh9zBFKLkQIAAAAAAAAAAAAAAA="
                }
            }
        ],
        "signatures":[
            {
                "account_number":"11335",
                "pub_key":{
                    "type":"tendermint/PubKeySecp256k1",
                    "value":"A49sjCd3Eul+ZXyof7qO460UaO73otrmySHyTNSLW+Xn"},"sequence":"0","signature":"spk/FpIIwMvPv1aKKPCxGWgJ0jdfATpAd2Z0Go+onOhPgMXJtNdiyl+MDaqPLevVlGaZPw42BbhHxrt/EtXFLg=="
                }
            }
        ]
    }
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="https://localhost:1317" path="/txs" %}
{% api-method-summary %}
Send Tx
{% endapi-method-summary %}

{% api-method-description %}
**\[NOTE\]** Content-type: application/json
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-body-parameters %}
{% api-method-parameter name="mode" type="string" required=true %}
:= block, sync, async
{% endapi-method-parameter %}

{% api-method-parameter name="tx" type="string" required=true %}
Signed StdTx body
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://localhost:1317" path="/executionlayer/balance?address=friday15kvpva2u57vv6l5czehyk69s0wnq9hrkqulwfz" %}
{% api-method-summary %}
Get balance
{% endapi-method-summary %}

{% api-method-description %}
This endpoint allows you to get HDAC balance
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-query-parameters %}
{% api-method-parameter name="address" type="string" required=true %}
Wallet of Hdac wallet address starting from 'friday'
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Balance successfully retrieved.
{% endapi-method-response-example-description %}

```text
{
    "value": 5000000
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="https://localhost:1317" path="/executionlayer/bond" %}
{% api-method-summary %}
Bond balance
{% endapi-method-summary %}

{% api-method-description %}
Bond balance of validator \(Experimental endpoint for WASM execution\)  
**\[NOTE\]** Content-Type: application/json
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-form-data-parameters %}
{% api-method-parameter name="memo" type="string" required=true %}
Brief memo of the Tx
{% endapi-method-parameter %}

{% api-method-parameter name="chain\_id" type="string" required=true %}
ID of the chain
{% endapi-method-parameter %}

{% api-method-parameter name="amount" type="integer" required=true %}
The integer amount you want to bond
{% endapi-method-parameter %}

{% api-method-parameter name="gas\_price" type="integer" required=true %}
Tx gas fee. If less than the required, tx will be failed
{% endapi-method-parameter %}

{% api-method-parameter name="address" type="string" required=true %}
Wallet of Hdac wallet address starting from 'friday'
{% endapi-method-parameter %}
{% endapi-method-form-data-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "mode":"sync",
    "tx":{
        "fee":{
            "amount":[],
            "gas":"37000"
        },
        "memo":"",
        "msg":[
            {
                "type":"executionengine/Execute",
                "value":{
                    "block_hash":"AA==",
                    "contract_owner_account":"friday1lgharzgds89lpshr7q8kcmd2esnxkfpwmfgk32",
                    "exec_account":"friday1lgharzgds89lpshr7q8kcmd2esnxkfpwmfgk32",
                    "gas_price":"2000000",
                    "payment_args":"AQAAAAQAAAADgIQe",
                    "session_args":"AgAAABgAAAAUAAAAFX2WU5Tkt49ZzKlXxfh9zBFKLkQIAAAAAAAAAAAAAAA="
                }
            }
        ],
        "signatures":[
            {
                "account_number":"11335",
                "pub_key":{
                    "type":"tendermint/PubKeySecp256k1",
                    "value":"A49sjCd3Eul+ZXyof7qO460UaO73otrmySHyTNSLW+Xn"},"sequence":"0","signature":"spk/FpIIwMvPv1aKKPCxGWgJ0jdfATpAd2Z0Go+onOhPgMXJtNdiyl+MDaqPLevVlGaZPw42BbhHxrt/EtXFLg=="
                }
            }
        ]
    }
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="https://localhost:1317" path="/executionlayer/unbond" %}
{% api-method-summary %}
Unbond balance
{% endapi-method-summary %}

{% api-method-description %}
Unbond balance of validator \(Experimental endpoint for WASM execution\)  
**\[NOTE\]** Content-Type: application/json
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-form-data-parameters %}
{% api-method-parameter name="memo" type="string" required=true %}
Brief memo of Tx
{% endapi-method-parameter %}

{% api-method-parameter name="chain\_id" type="string" required=true %}
ID of the chain
{% endapi-method-parameter %}

{% api-method-parameter name="amount" type="integer" required=true %}
The integer amount you want to unbond
{% endapi-method-parameter %}

{% api-method-parameter name="gas\_price" type="integer" required=true %}
Tx gas fee. If less than the required, tx will be failed
{% endapi-method-parameter %}

{% api-method-parameter name="address" type="string" required=true %}
Wallet of Hdac wallet address starting from 'friday'
{% endapi-method-parameter %}
{% endapi-method-form-data-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "mode":"sync",
    "tx":{
        "fee":{
            "amount":[],
            "gas":"37000"
        },
        "memo":"",
        "msg":[
            {
                "type":"executionengine/Execute",
                "value":{
                    "block_hash":"AA==",
                    "contract_owner_account":"friday1lgharzgds89lpshr7q8kcmd2esnxkfpwmfgk32",
                    "exec_account":"friday1lgharzgds89lpshr7q8kcmd2esnxkfpwmfgk32",
                    "gas_price":"2000000",
                    "payment_args":"AQAAAAQAAAADgIQe",
                    "session_args":"AgAAABgAAAAUAAAAFX2WU5Tkt49ZzKlXxfh9zBFKLkQIAAAAAAAAAAAAAAA="
                }
            }
        ],
        "signatures":[
            {
                "account_number":"11335",
                "pub_key":{
                    "type":"tendermint/PubKeySecp256k1",
                    "value":"A49sjCd3Eul+ZXyof7qO460UaO73otrmySHyTNSLW+Xn"},"sequence":"0","signature":"spk/FpIIwMvPv1aKKPCxGWgJ0jdfATpAd2Z0Go+onOhPgMXJtNdiyl+MDaqPLevVlGaZPw42BbhHxrt/EtXFLg=="
                }
            }
        ]
    }
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="https://localhost:1317" path="/executionlayer/transfer" %}
{% api-method-summary %}
Transfer
{% endapi-method-summary %}

{% api-method-description %}
Transfer token from one to another \(under construction\)  
**\[NOTE\]** Content-Type: application/json
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-form-data-parameters %}
{% api-method-parameter name="memo" type="string" required=true %}
Brief memo of the tx
{% endapi-method-parameter %}

{% api-method-parameter name="chain\_id" type="string" required=true %}
ID of the chain
{% endapi-method-parameter %}

{% api-method-parameter name="gas\_price" type="integer" required=true %}
Tx gas fee. If less than the required, tx will be failed
{% endapi-method-parameter %}

{% api-method-parameter name="recipient\_address" type="string" required=true %}
Receiver address
{% endapi-method-parameter %}

{% api-method-parameter name="sender\_address" type="string" required=true %}
Address of token sender
{% endapi-method-parameter %}

{% api-method-parameter name="amount" type="string" required=true %}
Amount of token you want to send with "dummy" if you followed "Genesis running" \(ex. "1000dummy"\)
{% endapi-method-parameter %}
{% endapi-method-form-data-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "mode":"sync",
    "tx":{
        "fee":{
            "amount":[],
            "gas":"37000"
        },
        "memo":"",
        "msg":[
            {
                "type":"executionengine/Execute",
                "value":{
                    "block_hash":"AA==",
                    "contract_owner_account":"friday1lgharzgds89lpshr7q8kcmd2esnxkfpwmfgk32",
                    "exec_account":"friday1lgharzgds89lpshr7q8kcmd2esnxkfpwmfgk32",
                    "gas_price":"2000000",
                    "payment_args":"AQAAAAQAAAADgIQe",
                    "session_args":"AgAAABgAAAAUAAAAFX2WU5Tkt49ZzKlXxfh9zBFKLkQIAAAAAAAAAAAAAAA="
                }
            }
        ],
        "signatures":[
            {
                "account_number":"11335",
                "pub_key":{
                    "type":"tendermint/PubKeySecp256k1",
                    "value":"A49sjCd3Eul+ZXyof7qO460UaO73otrmySHyTNSLW+Xn"},"sequence":"0","signature":"spk/FpIIwMvPv1aKKPCxGWgJ0jdfATpAd2Z0Go+onOhPgMXJtNdiyl+MDaqPLevVlGaZPw42BbhHxrt/EtXFLg=="
                }
            }
        ]
    }
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}



