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
* Sign tx in local, and take the result
  * The response is unsigned tx
  * The API endpoint itself do not send the tx
* Send the response above with \`Send Tx\` API

{% api-method method="post" host="https://localhost:1317" path="/txs" %}
{% api-method-summary %}
Send Tx
{% endapi-method-summary %}

{% api-method-description %}
**\[NOTE\]** Content-type: application/json  
Check here -&gt; https://cosmos.network/rpc/\#/
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

```text
TBD
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

```text

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

{% api-method-parameter name="amount" type="integer" required=true %}
Amount of token you want to send
{% endapi-method-parameter %}
{% endapi-method-form-data-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```text
TBD
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}



