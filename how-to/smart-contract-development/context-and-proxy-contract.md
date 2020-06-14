# Context & Proxy contract

## Goal

* Know what is the context of the contract \(account context vs contract context\)
* Know which context is applied in each case and its effect

## Which named key does the context look?

There are two kinds of context. One is account context, and another is contract context. See the table below:

```text
Account A
|- "blah1": "abc"
|- "blah2": "cba"

Account B
|- "foo": "bbb"
|- "boo": "lol"

--------------------------------------

Contract 1
|- "blah1": "123"
|- "admin": "friday12341234"
```

**Basically,**

* Account context: Find the executor's named key
* Contract context: Find the contract's named key

**For example:**

* In account context and "A" executed a contract, the contract can lookup `"blah1":"abc"` and `"blah2":"cba"`
* In account context and "B" executed a contract, the contract can lookup `"foo":"bbb"` and `"boo":"lol"`
* In contract context and "A" executed a contract, the contract can lookup `"blah1": "123"` and `"admin": "friday12341234"`

**Problem is...**

If you have develop experience of existing contract such as solidity and EOSIO, all states are global. If somebody updates a table, following user can query & use this updated value. In here, it works like it if the contract is run in contract context. In account context, it looks up only executor's account value.

## How could I control the context

### Account context

If you read this article, you may know how to run the contract. All of this examples run under account context.

### Contract context

It needs **additional contract,** named "proxy contract". It works similar as reverse proxy in backend server development. It shares a session to all executors and help to run with same state data.

You can implement it with `runtime::call_contract()` on the existing contract. Let me make the state globalize of the contract.

This example, I'll use the [this example](stored-contract.md). Let assume that the address of the contract is:

```bash
fridaycontracthash1qydead29jg44yktvptwh6ql9yuy2xzgytwrf2zcqjxjf5mmurqaq9ymwvw
```

And the basic form of the proxy contract is like below: \(You may check the detail from [this repository](https://github.com/hdac-io/swap)\)

```rust
pub enum Api {
    Transfer(Hash, String, String, U512),
}

impl Api {
    pub fn from_args() -> Self {
        let method_name: String = runtime::get_arg(0)
            .unwrap_or_revert_with(ApiError::MissingArgument)
            .unwrap_or_revert_with(ApiError::InvalidArgument);

        match method_name.as_str() {
            "transfer" => {
                let contract_address: Hash = runtime::get_arg(1)
                    .unwrap_or_revert_with(ApiError::MissingArgument)
                    .unwrap_or_revert_with(ApiError::InvalidArgument);
                let from_address: String = runtime::get_arg(2)
                    .unwrap_or_revert_with(ApiError::MissingArgument)
                    .unwrap_or_revert_with(ApiError::InvalidArgument);
                let to_address: String = runtime::get_arg(3)
                    .unwrap_or_revert_with(ApiError::MissingArgument)
                    .unwrap_or_revert_with(ApiError::InvalidArgument);
                let amount: U512 = runtime::get_arg(4)
                    .unwrap_or_revert_with(ApiError::MissingArgument)
                    .unwrap_or_revert_with(ApiError::InvalidArgument);


                Api::Transfer(contract_address, from_address, to_address, amount)
            }
        }
        
    ...
    }

    pub fn invoke(&self) {
        match self {
            Self::Transfer(
                contract_address,
                from_address,
                to_address,
                amount,
            ) => {
                let contract_ref = contract_address.to_contract_ref().unwrap_or_revert();
                runtime::call_contract(
                    contract_ref,
                    ("transfer", *from_address, *to_address, *amount),
                )
            }
        }
    }
}
```

Please check 43rd-45th line. This line makes your contract be a contract context. You may install the proxy contract and the user put the address of the store contract as a parameter and run transfer as contract context.

Then, the execution runs as a global, and access the contract's named key. Now, `runtime::put_key()` and `runtime::get_key()` looks up contract's named key.

{% hint style="info" %}
For detail implementation & contract installation, please check [this repository](https://github.com/hdac-io/swap).
{% endhint %}

