# Nickname

## Register nickname

**\[POST\]** http://localhost:1317/nickname/new

Request:

```javascript
{
	"base_req": {
		"chain_id": "testnet",
		"gas": "20000000",
		"memo": "",
		"from": "friday15evpva2u57vv6l5czehyk69s0wnq9hrkqulwfz"
	},
	"nickname": "bryan"
}
```

Response:

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

## Change address of given nickname

**\[PUT\]** http://localhost:1317/nickname/new

Request:

```javascript
{
	"base_req": {
		"chain_id": "testnet",
		"gas": "20000000",
		"memo": "",
		"from": "friday15evpva2u57vv6l5czehyk69s0wnq9hrkqulwfz"
	},
	"nickname": "bryan",
	"new_address": "friday1y2dx0evs5k6hxuhfrfdmm7wcwsrqr073htghpv"
}
```

Reponse:

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

