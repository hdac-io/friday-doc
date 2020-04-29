# Token

## Get balance

**\[GET\]** https://localhost:1317/hdac/balance?address=&lt;address\_or\_nickname&gt;

Request:

```text
https://localhost:1317/hdac/balance?address=fridayxxxx...
or
https://localhost:1317/hdac/balance?address=princesselsa
```

Response:

```javascript
{
    "value": 500000000000000
}
```

## Bond balance

**\[POST\]** http://localhost:1317/hdac/bond

Request:

```javascript
{
	"base_req": {
		"chain_id": "testnet",
		"gas": "20000000",
		"memo": "",
		"from": "<address_or_nickname>"
	},
	"fee": "100000000",
	"amount":"1000000"
}
```

Response:

```javascript
{
    "type": "friday/StdTx",
    "value": {
        "msg": [
            {
                "type": "executionengine/Bond",
                "value": {
                    "contract_address": "",
                    "from_address": "friday19sw6aqmy2qdwg54lnkzl7qjxcds8pnx3dclu8n",
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

## Unbond balance

**\[POST\]** http://localhost:1317/hdac/unbond

Request:

```javascript
{
	"base_req": {
		"chain_id": "testnet",
		"gas": "20000000",
		"memo": "",
		"from": "<address_or_nickname>"
	},
	"fee": "100000000",
	"amount":"1000000"
}
```

Response:

```javascript
{
    "type": "friday/StdTx",
    "value": {
        "msg": [
            {
                "type": "executionengine/UnBond",
                "value": {
                    "contract_address": "",
                    "from_address": "friday19sw6aqmy2qdwg54lnkzl7qjxcds8pnx3dclu8n",
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

## Transfer

**\[POST\]** http://localhost:1317/hdac/transfer

Request:

```javascript
{
	"base_req": {
		"chain_id": "testnet",
		"gas": "20000000",
		"memo": "",
		"from": "<address_or_nickname>"
	},
	"recipient_address_or_nickname":"friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv",
	"amount":"1000000",
	"fee": "100000000"
}
```

Response

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

## Create validator

**\[POST\]** http://localhost:1317/hdac/validators

Request:

```javascript
{
	"base_req": {
		"chain_id": "testnet",
		"gas": "20000000",
		"memo": "",
		"from": "friday19sw6aqmy2qdwg54lnkzl7qjxcds8pnx3dclu8n"
	},
	"cons_pub_key":"fridayvalconspub16jrl8jvqq9cj7569gec8sd6dv4e4w46g25uk7vm52fpk6nfn23p5zv60xekxuse3df39sjz4g9d92nfc0f3rs3ec2fpxxum9wej52428w968xwzww9erw53ng5u9zu23fdvk226z0fnx5mn2f4qk542ygf9xjvptve6y7a6fxe8kjm6s2e3kja2twezyvmt5245ystm4ga94y4zjwfg523c477wna",
	"description":{
		"moniker": "testnode1",
		"identity": "",
		"website": "https://hdac.io",
		"details": ""
	}
}
```

Response:

```javascript
{
    "type": "friday/StdTx",
    "value": {
        "msg": [
            {
                "type": "executionengine/CreateValidator",
                "value": {
                    "validator_address": "friday19sw6aqmy2qdwg54lnkzl7qjxcds8pnx3dclu8n",
                    "cons_pubkey": "fridayvalconspub16jrl8jvqq9cj7569gec8sd6dv4e4w46g25uk7vm52fpk6nfn23p5zv60xekxuse3df39sjz4g9d92nfc0f3rs3ec2fpxxum9wej52428w968xwzww9erw53ng5u9zu23fdvk226z0fnx5mn2f4qk542ygf9xjvptve6y7a6fxe8kjm6s2e3kja2twezyvmt5245ystm4ga94y4zjwfg523c477wna",
                    "description": {
                        "moniker": "testnode1",
                        "identity": "",
                        "website": "https://hdac.io",
                        "details": ""
                    }
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

## Edit validator

**\[PUT\]** http://localhost:1317/hdac/validators

Request:

```javascript
{
	"base_req": {
		"chain_id": "testnet",
		"gas": "20000000",
		"memo": "",
		"from": "friday19sw6aqmy2qdwg54lnkzl7qjxcds8pnx3dclu8n"
	},
	"description":{
		"moniker": "testnode1",
		"identity": "",
		"website": "https://hdac.io",
		"details": ""
	}
}
```

Response:

```javascript
{
    "type": "friday/StdTx",
    "value": {
        "msg": [
            {
                "type": "executionengine/EditValidator",
                "value": {
                    "address": "friday19sw6aqmy2qdwg54lnkzl7qjxcds8pnx3dclu8n",
                    "Description": {
                        "moniker": "testnode1",
                        "identity": "",
                        "website": "https://hdac.io",
                        "details": ""
                    }
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

