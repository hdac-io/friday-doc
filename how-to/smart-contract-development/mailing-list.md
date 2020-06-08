# Store value with multiple keys

## Goal

* Know how to store information with multiple keys

## Implementation

In this guide, a complete logic is not provided, but provides the concept.

Basically, internal `name_key` is implemented with `BTreeMap`. It means that the index is implemented based on B-Tree, and it is key-value store, not RDB. But RDB-like feature is needed in many dApp. How to implement it?

```text
Key: "key1_key2_key3_..."
Value: "value1_value2_value3_..."
```

You may concat multiple keys with delimiter, organize one string, and use as a key. And you may do as in value.

Then, if one data cannot be parsed/restore into/from string, how to implement it? You may implement custom `from/to` method by implementing `bytesrepr::{self, FromBytes, ToBytes}` trait.

Here is the snippet below:

```rust
use types::{
    bytesrepr::{self, FromBytes, ToBytes},
    ...
};

impl<T: RequestKey> FromBytes for RequestQueue<T> {
    fn from_bytes(bytes: &[u8]) -> result::Result<(Self, &[u8]), bytesrepr::Error> {
        let (len, mut bytes) = u64::from_bytes(bytes)?;
        let mut queue = Vec::new();
        for _ in 0..len {
            let (entry, rest) = RequestQueueEntry::from_bytes(bytes)?;
            bytes = rest;
            queue.push(entry);
        }
        Ok((RequestQueue(queue), bytes))
    }
}

impl<T: RequestKey> ToBytes for RequestQueue<T> {
    fn to_bytes(&self) -> result::Result<Vec<u8>, bytesrepr::Error> {
        let mut bytes = (self.0.len() as u64).to_bytes()?; // TODO: Allocate correct capacity.
        for entry in &self.0 {
            bytes.append(&mut entry.to_bytes()?);
        }
        Ok(bytes)
    }
}
```

Or, you can implement lambda function:

```rust
let to_publickey = |hex_str: &str| -> Result<PublicKey> {
    if hex_str.len() != 64 {
        return Err(Error::VoteKeyDeserializationFailed);
    }
    let mut key_bytes = [0u8; 32];
    let _bytes_written = base16::decode_slice(hex_str, &mut key_bytes)
        .map_err(|_| Error::VoteKeyDeserializationFailed)?;
    debug_assert!(_bytes_written == key_bytes.len());
    Ok(PublicKey::from(key_bytes))
};
```

And you can use this method in serializing/deserializing from multiple keys.

