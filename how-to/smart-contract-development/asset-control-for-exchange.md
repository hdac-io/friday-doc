# Asset deposit control for exchanges

## Goal

* Know how to manage asset deposit for exchanges

## 1. Memo on transaction

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
      "memo": "4426cc61-f814-4519-8a17-650fd93e9ed4"
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

We support memo feature. You can set your own account and scheduler to lookup your incoming transactions and designated memo.

## 2. Let holders run a deposit contract

1\) Abstract table for deposit control

```rust
     ID on Exchange  |    Hdac Address      |    Asset
=====================|======================|=========
psy2848048           |   friday1dnajdfe     |    12.12
yoosah               |   friday1jfjaef      |    15.46
caramis              |   friday1fbnfek      |    21.02
brew                 |   firday1fkvdnv      |   124.14
```

2\) Brief snippet for deposit contract

```rust
pub fn register(exchange_account: String){
    let deposit_account = runtime::get_caller();
    let hdac_address_exchange_account_relations: BTreeMap<PublicKey, String> =
        get_relations_map();
    hdac_address_exchange_account_relations
        .put(deposit_account, exchange_account);
    write_relations_map(hdac_address_exchange_account_relations);
}

pub fn deposit(amount: U512) {
    let deposit_account = runtime::get_caller();
    let requester_purse = account::get_main_purse();

    deposit_record(requester_account, amount * exchange_rate);
    system::transfer_from_purse_to_account(requester_purse, get_main_purse(), amount);
}
```

You can run your own contract for deposit control.

First, you can plan and make your own control table. There is 2 keys and 1 value. You can concat 2 keys with one-lined string and implement as `BTreeMap` or concat everything in one key with dummy value, and parse it.

Second, implement 2 methods. `register` and `deposit`

* `register`: Register exchange account of holders and New mainnet address. And you are ready to track holder's incoming deposit. It can be run by both of the exchange and holders, depending on your UX plan.
* `deposit`: For recording asset data, deposit should not be run by direct transfer, but be run by method of contract. It updates asset and let holders' asset transfer to your exchange. This method should be run by holders themselves.

