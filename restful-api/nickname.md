# Nickname

{% api-method method="post" host="https://localhost:1317" path="/nickname/new" %}
{% api-method-summary %}
Register readable ID by bech32 public key
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-body-parameters %}
{% api-method-parameter name="chain\_id" type="string" required=true %}
Chain ID
{% endapi-method-parameter %}

{% api-method-parameter name="nickname" type="string" required=true %}
Readable string nickname you want to use
{% endapi-method-parameter %}

{% api-method-parameter name="address" type="string" required=true %}
Bech32 address for mapping  
ex\) fridaypubxxxxxx...
{% endapi-method-parameter %}

{% api-method-parameter name="gas\_price" type="string" required=true %}
Stringlyfied integer value of tx gas price
{% endapi-method-parameter %}

{% api-method-parameter name="memo" type="string" required=true %}
Brief memo of Tx
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
                "type": "readablename/SetNick",
                "value": {
                    "nickname": {
                        "H": "208810712",
                        "L": "0"
                    },
                    "address": "friday15evpva2u57vv6l5czehyk69s0wnq9hrkqulwfz"
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
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="put" host="https://localhost:1317" path="/nickname/change" %}
{% api-method-summary %}
Change readable ID public key by Bech32 public key
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-body-parameters %}
{% api-method-parameter name="chain\_id" type="string" required=true %}
Chain ID
{% endapi-method-parameter %}

{% api-method-parameter name="name" type="string" required=true %}
Your current nickname
{% endapi-method-parameter %}

{% api-method-parameter name="old\_address" type="string" required=true %}
Old\(current\) address
{% endapi-method-parameter %}

{% api-method-parameter name="new\_address" type="string" required=true %}
New address
{% endapi-method-parameter %}

{% api-method-parameter name="gas\_price" type="string" required=true %}
Stringlyfied integer value of tx gas fee
{% endapi-method-parameter %}

{% api-method-parameter name="memo" type="string" required=true %}
Brief memo of Tx
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
                "type": "readablename/ChangeKey",
                "value": {
                    "nickname": "bryan",
                    "old_address": "friday15evpva2u57vv6l5czehyk69s0wnq9hrkqulwfz",
                    "new_address": "friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv"
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
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

