# Contract

## Contract run

```bash
clif contract run wasm|uref|name|hash <wasm-path>|<uref>|<name>|<hash> <argument> <fee> --from <from>
```

```bash
clif contract run hash McGYvl4ku8yeVuDmYXp/UmEVtiDXjOhYeG2sAelIhIk= '[{"name": "method", "value": {"string_value": "mint"}},{"name": "address", "value": {"string_value": "friday1jk2zrqqa98pwax7cq0xgkqw67qk2p8nhcpup8k"}},{"name": "amount", "value": {"big_int": {"value": "100000", "bit_width": 512}}}]' 0.02 50000000 --from elsa

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

## Contract query

```bash
clif contract query address|uref|hash|local <data> [path] [--blockhash <blockhash_since>] [flags]
```

```bash
clif contract query address $(clif keys show anna -a) friday1jk2zrqqa98pwax7cq0xgkqw67qk2p8nhcpup8k


```

## Query uref & hash status of the account

```bash
clif contract query address <address>
```

```bash
clif contract query address $(clif keys show anna -a) ""

{
  "account": {
    "publicKey": "ZnJpZGF5YmVnaW5zlZQhgB0pwu6b2APMiwHa8Cygnnc=",
    "purseId": {
      "uref": "j5ctqeDGJJi3D77MjKItpoxGegKJtBwBsvEYMeujYV8=",
      "accessRights": "READ_ADD_WRITE"
    },
    "namedKeys": [
      {
        "name": "mint",
        "key": {
          "uref": {
            "uref": "ZU2WKs0h72Ex2OttXsT+V8WCEEddat5QSpXFqOgN4KM=",
            "accessRights": "READ"
          }
        }
      },
      {
        "name": "pos",
        "key": {
          "uref": {
            "uref": "DGs89SSwUiSHOLn/ka0LU3/3c4dJdNgjakZUb2WTlkw=",
            "accessRights": "READ"
          }
        }
      }
    ],
    "associatedKeys": [
      {
        "publicKey": "AAAAZnJpZGF5YmVnaW5zlZQhgB0pwu6b2APMiwHa8Cw=",
        "weight": 2694739713
      }
    ],
    "actionThresholds": {
      "deploymentThreshold": 1,
      "keyManagementThreshold": 1
    }
  }
}
```

