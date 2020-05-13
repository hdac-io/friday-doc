# Validator

## Create validator

Create and join as a validator with an address for reward and BLS-based validator consensus key

```bash
clif hdac create-validator <fee> --from <wallet_alias> --pubkey <BLS_valconspub_key> [--moniker <moniker>] [--identity <identity>] [--website <website>] [--details <details>]
```

```bash
clif hdac create-validator 0.01 \
--from princesselsa \
--pubkey $(nodef tendermint show-validator) \
--moniker frozen
```

## Edit validator

```bash
clif hdac edit-validator <fee> --from <from> [--moniker <moniker>] [--identity <identity>] [--website <site_address>] [--details <detail_description>]
```

```bash
clif hdac edit-validator 0.01 \
--from anna \
--moniker valval \
--website tothemoon.io
```

## Check consensus public key \(BLS-based\)

```bash
nodef tendermint show-validator

fridayvalconspub16jrl8jvqq9k9yj3sfpe42jz089yxgsjt25hnqcmjxqck5vj3vdqhv623ggeh5vzxwat8yu33f38zkmf3va25znjhgd3xukf4x5cx7466xdg5k52t9a9hqkf3wf3x2enc9a94swrxfpy8xupsfv45sarnxdfxkkzp0fm4qc2v9dmh2vjd29rxunfsf33hscj3v448x5j9f9s4gmnkfd9k23smxng29
```

## Check validators' list

```bash
clif hdac validator

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

