# History

​[​​![Travis](https://travis-ci.com/hdac-io/friday.svg?token=bhU3g7FdixBp5h3M2its&branch=master)​](https://travis-ci.com/hdac-io/friday/branches) [​​![codecov](https://codecov.io/gh/hdac-io/friday/branch/master/graph/badge.svg?token=hQEgzmULjh)​](https://codecov.io/gh/hdac-io/friday)​

## 0.8.0 - 2020-04-11 <a id="0-4-0-2020-01-20"></a>

#### Updates

* Added query CLI for user delegation
* Added vote feature \(both of transaction & query\)
* Added Reward calculation based on delegated amount \(Claim feature will be added at the next release\)

## 0.7.0 - 2020-03-26 <a id="0-4-0-2020-01-20"></a>

#### Updates

* Added delegation feature \(delegate, undelegate, redelegate system contract & CLI interface\)
* Apply structure of system contract \(hdac\_mint & pop\)
* Refactored stored value

#### Known issues

* Delegation statistics querying CLI doesn't work properly, so we decided not to include it this time. This feature will be included with next release

## 0.6.0 - 2020-03-02 <a id="0-4-0-2020-01-20"></a>

**Changes**

* Align CLI commands and unifying cli flags\(--wallet, --nickname, --name\) to --from.
* Add `client_api_proxy` contract hash to SYSTEM ACCOUNT's named key, so we can request common client APIs such as `bond`, `unbond`, `transfer_to_account` and `standard_payment` without transient session contracts.
* Introduce HDAC unit and modify default gas price.
* Add system account address alias so we can refer the system account address as `system` in CLI.

## 0.5.0 - 2020-02-17 <a id="0-4-0-2020-01-20"></a>

**Features**

* Supported external WASM file execution
* Shortened Tx message of Hdac-specified execution
* Organized & optimized internal keeper logic

#### Interface

* Reorganized CLI & JSON RPC interface
* Supported passing arguments as JSON to WASM \(currently CLI only\)
* Unified address system

## 0.4.0 - 2020-01-20 <a id="0-4-0-2020-01-20"></a>

Changes:

* Implemented Readable ID service
* Bumped CasperLabs version v0.9.0 -&gt; v0.10.0

## 0.3.0 - 2020-01-06 <a id="0-3-0-2020-01-06"></a>

Changes:

* enlarge default tx bytes to 3MiB
* add staking feature
* resolve some errors\(bls key compare, transfer for nonexistent account\)

## 0.2.0 - 2019-12-23 <a id="0-2-0-2019-12-23"></a>

Changes:

* Add load-chainspec command [\#39](https://github.com/hdac-io/friday/pull/39)​
* REST API support [\#38](https://github.com/hdac-io/friday/pull/38)​
* Change node key type from ed25519 to BLS 12-381 [hdac-io/tendermint\#9](https://github.com/hdac-io/tendermint/pull/9)​

## 0.1.0 - 2019-12-09 <a id="0-1-0-2019-12-09"></a>

* Integrated with Casperlabs EE
* Token send / querying enabled

