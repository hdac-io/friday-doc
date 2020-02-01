# Hdac specific

{% api-method method="get" host="https://localhost:1317" path="/contract/balance?address=friday15kvpva2u57vv6l5czehyk69s0wnq9hrkqulwfz" %}
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
Wallet of Hdac wallet address
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

{% api-method method="post" host="https://localhost:1317" path="/contract/bond" %}
{% api-method-summary %}
Bond balance
{% endapi-method-summary %}

{% api-method-description %}
Bond balance of validator \(Experimental endpoint for WASM execution\)  
**\[NOTE\]** Content-Type: application/json
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-body-parameters %}
{% api-method-parameter name="chain\_id" type="string" required=true %}
Chain ID
{% endapi-method-parameter %}

{% api-method-parameter name="token\_contract\_address" type="string" required=true %}
Token contract address to bond  
\(Currently dummy\)
{% endapi-method-parameter %}

{% api-method-parameter name="address\_or\_nickname" type="string" required=true %}
Address or registered nickname
{% endapi-method-parameter %}

{% api-method-parameter name="amount" type="string" required=true %}
Stringlyfied integer amount which you want to bond
{% endapi-method-parameter %}

{% api-method-parameter name="gas\_price" type="string" required=true %}
Stringlyfied integer Tx gas fee. If less than the required, tx will be failed  
ex\) "500"
{% endapi-method-parameter %}

{% api-method-parameter name="memo" type="string" required=true %}
Brief memo of the Tx
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
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

{% api-method method="post" host="https://localhost:1317" path="/contract/unbond" %}
{% api-method-summary %}
Unbond balance
{% endapi-method-summary %}

{% api-method-description %}
Unbond balance of validator \(Experimental endpoint for WASM execution\)  
**\[NOTE\]** Content-Type: application/json
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-body-parameters %}
{% api-method-parameter name="chain\_id" type="string" required=true %}
Chain ID
{% endapi-method-parameter %}

{% api-method-parameter name="token\_contract\_address" type="string" required=true %}
Token contract address to unbond  
\(Currently dummy\)
{% endapi-method-parameter %}

{% api-method-parameter name="address\_or\_nickname" type="string" required=true %}
Address or registered nickname
{% endapi-method-parameter %}

{% api-method-parameter name="amount" type="string" required=true %}
Stringlyfied integer amount which you want to unbond
{% endapi-method-parameter %}

{% api-method-parameter name="gas\_price" type="string" required=true %}
Stringlyfied integer Tx gas fee. If less than the required, tx will be failed  
ex\) "500"
{% endapi-method-parameter %}

{% api-method-parameter name="memo" type="string" required=true %}
Brief memo of the Tx
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
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

{% api-method method="post" host="https://localhost:1317" path="/contract/transfer" %}
{% api-method-summary %}
Transfer
{% endapi-method-summary %}

{% api-method-description %}
Transfer token from one to another
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-body-parameters %}
{% api-method-parameter name="chain\_id" type="string" required=true %}
Chain ID
{% endapi-method-parameter %}

{% api-method-parameter name="token\_contract\_address" type="string" required=true %}
Token contract address to transfer  
\(Currently dummy\)
{% endapi-method-parameter %}

{% api-method-parameter name="sender\_address\_or\_nickname" type="string" required=true %}
Address or nickname of sender
{% endapi-method-parameter %}

{% api-method-parameter name="recipient\_address\_or\_nickname" type="string" required=true %}
Address or nickname of recipient
{% endapi-method-parameter %}

{% api-method-parameter name="amount" type="string" required=true %}
Stringlyfied integer value. Amount of transfer
{% endapi-method-parameter %}

{% api-method-parameter name="fee" type="string" required=true %}
Stringlyfied integer value. Tx fee.
{% endapi-method-parameter %}

{% api-method-parameter name="gas\_price" type="string" required=true %}
Stringlyfied integer value. Tx gas fee  
If less than the required, tx will be failed
{% endapi-method-parameter %}

{% api-method-parameter name="memo" type="string" required=true %}
Brief memo of the Tx
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "type": "friday/StdTx",
    "value": {
        "msg": [
            {
                "type": "executionengine/Transfer",
                "value": {
                    "token_contract_address": "friday15evpva2u57vv6l5czehyk69s0wnq9hrkqulwfz",
                    "from_address": "friday15evpva2u57vv6l5czehyk69s0wnq9hrkqulwfz",
                    "to_address": "friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv",
                    "transfer_code": "//WASM bindary byte//",
                    "transfer_args": "AgAAACAAAABmcmlkYXliZWdpbnMimmflkKW1c3LpGlu9+dh0BgG/0QgAAABAQg8AAAAAAA==",
                    "payment_code": "//WASM binary byte//",
                    "payment_args": "AQAAAAEAAAAA",
                    "gas_price": "0"
                }
            }
        ],
        "fee": {
            "amount": [],
            "gas": "0"
        },
        "signatures": null,
        "memo": ""
    }
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

