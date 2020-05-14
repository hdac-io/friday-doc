# Validator

## Create validator

**\[POST\]** https://localhost:1317/hdac/validators

Request:

```javascript
{
	"base_req": {
		"chain_id": "testnet",
		"gas": "20000000",
		"memo": "",
		"from": "<address_or_nickname>"
	},
	"cons_pub_key": cons_pub_key,
  "description": {
      "moniker": moniker,
      "identity": identity,
      "website": website,
      "details": details,
  },
}
```

## Edit validator

**\[PUT\]** https://localhost:1317/hdac/validators

Request:

```javascript
{
	"base_req": {
		"chain_id": "testnet",
		"gas": "20000000",
		"memo": "",
		"from": "<address_or_nickname>"
	},
  "description": {
      "moniker": moniker,
      "identity": identity,
      "website": website,
      "details": details,
  },
}
```

## Check validators' list

**\[GET\]** https://localhost:1317/hdac/validators?address=xxxx

* `address`: \(Optional\) Validator's address

Response:

```bash
[
  {
    "consensus_pubkey": "fridayvalconspub16jrl8jvqq9k9yj3sfpe42jz089yxgsjt25hnqcmjxqck5vj3vdqhv623ggeh5vzxwat8yu33f38zkmf3va25znjhgd3xukf4x5cx7466xdg5k52t9a9hqkf3wf3x2enc9a94swrxfpy8xupsfv45sarnxdfxkkzp0fm4qc2v9dmh2vjd29rxunfsf33hscj3v448x5j9f9s4gmnkfd9k23smxng29",
    "description": {
      "moniker": "testnode",
      "identity": "",
      "website": "",
      "details": ""
    },
    "stake": "1000000000000000000"
  }
]
```

