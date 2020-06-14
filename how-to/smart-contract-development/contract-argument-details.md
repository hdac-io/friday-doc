# Contract argument details

## Goal

* Know how to use contract argument.

## Explain

Smart contracts can be parametrized. A list of contract arguments can be specified on command line when the contract is deployed.

The clif & rest has `contract run` command accepts parameter `<argument>` that can be used to specify types and values of contract arguments as a serialized sequence of [Arg](https://github.com/hdac-io/CasperLabs/blob/40f4b9de7d8693acfc62f71fac70afc854a6cd28/protobuf/io/casperlabs/casper/consensus/consensus.proto#L91) values in a [protobuf JSON format](https://developers.google.com/protocol-buffers/docs/proto3#json), with binary data represented in Base64 format.

Note: contract arguments are positional, and so the `"name"` attribute is currently not used. However, we plan to change contract arguments to be keyword \(named\) arguments. The structure of the `Arg` protobuf message and its JSON serialized form is ready for this change.

**Supported types of contract arguments**

| protobuf [Arg](https://github.com/hdac-io/CasperLabs/blob/40f4b9de7d8693acfc62f71fac70afc854a6cd28/protobuf/io/casperlabs/casper/consensus/consensus.proto#L91) | Contract API type | Example value in [protobuf JSON format](https://developers.google.com/protocol-buffers/docs/proto3#json) |
| :--- | :--- | :--- |
| `I32` | `i32` | `[{"name":"amount","value":{"cl_type":{"simpleType":"I32"},"value":{"i32":123456}}}]` |
| `I64` | `i64` | `[{"name":"amount","value":{"cl_type":{"simpleType":"I64"},"value":{"i64":"123456"}}}]` |
| `U32` | `u32` | `[{"name":"amount","value":{"cl_type":{"simple_type":"U32"},"value":{"u32":123456}}}]` |
| `U64` | `u64` | `[{"name":"amount","value":{"cl_type":{"simple_type":"U64"},"value":{"u64":"123456"}}}]` |
| `U128` | `U128` | `[{"name":"amount","value":{"cl_type":{"simple_type":"U128"},"value":{"u128":{"value":"123456"}}}}]` |
| `U256` | `U256` | `[{"name":"amount","value":{"cl_type":{"simple_type":"U256"},"value":{"u256":{"value":"123456"}}}}]` |
| `U512` | `U512` | `[{"name":"amount","value":{"cl_type":{"simple_type":"U512"},"value":{"u512":{"value":"123456"}}}}]` |
| `STRING` | `String` | `[{"name":"surname","value":{"cl_type":{"simple_type":"STRING"},"value":{"str_value":"Nakamoto"}}}]` |
| `option_value` | `Option<U512>` | `[{"name":"maybe_u512","value":{"cl_type":{"option_type":{"inner":{"simple_type":"U512"}}},"value":{"u512":{"value":"123456"}}}}]` |
| `hash` | `Key::Hash` | `[{"name":"my_hash","value":{"cl_type":{"simple_type":"KEY"},"value":{"key":{"hash":{"hash":"kyNqkmPSrGGYxe0hF3THRdXcYqkQy4Qnb4p8SVkgiRU="}}}}}]` |
| `address` | `Key::Address` | `[{"name":"my_address","value":{"clType":{"simpleType":"KEY"},"value":{"key":{"address":{"account":"kyNqkmPSrGGYxe0hF3THRdXcYqkQy4Qnb4p8SVkgiRU="}}}}}]` |
| `uref` | `Key::URef` | `[{"name":"my_uref","value":{"clType":{"simpleType":"KEY"},"value":{"key":{"uref":{"uref":"kyNqkmPSrGGYxe0hF3THRdXcYqkQy4Qnb4p8SVkgiRU=","access_rights":"READ_ADD_WRITE"}}}}}]` |
| `local` | `Key::Local` | `[{"name":"my_uref","value":{"clType":{"simpleType":"KEY"},"value":{"key":{"local":{"hash":"kyNqkmPSrGGYxe0hF3THRdXcYqkQy4Qnb4p8SVkgiRWTI2qSY9KsYZjF7SEXdMdF1dxiqRDLhCdvinxJWSCJFQ=="}}}}}]` |
| `byte_list` | `[u8]` | `[{"name":"address","value":{"cl_type":{"list_type":{"inner":{"simple_type":"U8"}}},"value":{"bytes_value":"1wJD3Z0NZG/W3ygqj3qPoFpmKb7AHYAkw2EescH7n4Q="}}}]` |
| List value | `Vec<type_of_above>` | `[{"name":"my_list","value":{"cl_type":{"list_type":{"inner":{"simple_type":"<type>"}}},"value":{"list_value":{"values":[{"<type>_value":"02c4ef70543e18889167ca67c8aa28c1d4c259e89cb34483a8ed6cfd3a03e8246b"},{..}]}}}}]` |

Numeric values of `access_rights` in `uref` are defined in [\`enum AccessRights in state.proto](https://github.com/hdac-io/CasperLabs/blob/40f4b9de7d8693acfc62f71fac70afc854a6cd28/protobuf/io/casperlabs/casper/consensus/state.proto#L239).

