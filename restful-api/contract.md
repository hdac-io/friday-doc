# Contract

## Run contract

**\[POST\]** https://localhost:1317/contract

Request:

```javascript
{
	"base_req": {
		"chain_id": "testnet",
		"gas": "20000000",
		"memo": "",
		"from": "<address_or_nickname>"
	},
	"type": "<wasm,name,uref,hash>"
  "token_contract_address_or_key_name": "if not wasm, fill here"
  "base64_encoded_binary": "if wasm, encode wasm binary to base64 and input here"
  "args": "stringlyfied JSON",
  "fee": "stringed number",
}
```

Response:

```javascript
{
  "chain_id": "testnet",
  "account_number": "0",
  "sequence": "1",
  "fee": {
    "amount": [],
    "gas": "50000000"
  },
  "msgs": [
    {
      "type": "executionengine/Execute",
      "value": {
        "contract_address": "dummyAddress",
        "exec_address": "friday1qt8k20h3hmdx0qulgpppnlsg92hjjtvn59qkyd",
        "session_type": "1",
        "session_code": "McGYvl4ku8yeVuDmYXp/UmEVtiDXjOhYeG2sAelIhIk=",
        "session_args": "[{\"name\": \"method\", \"value\": {\"string_value\": \"mint\"}},{\"name\": \"address\", \"value\": {\"string_value\": \"friday1jk2zrqqa98pwax7cq0xgkqw67qk2p8nhcpup8k\"}},{\"name\": \"amount\", \"value\": {\"big_int\": {\"value\": \"100000\", \"bit_width\": 512}}}]",
        "fee": "20000000000000000",
        "gas_price": "50000000"
      }
    }
  ],
  "memo": ""
}
```

## Query contract

**\[GET\]** https://localhost:1317/contract?data\_type=xxx&data=xxxx&path=xxx

* `data_type`: Type of the data \(:= address,uref,hash,local\)
* `data`: Actual data
* `path` : path of named key

