# How to work with RPC server

## Run RPC server

```text
clif rest-server
```

## How to send Tx via RPC server

* Execute appropriate API endpoint
  * If sending tx purpose, take the response and proceed to sign tx
  * If query purpose, use the response
* Get account number & sequence from \`Get account info\` API
* Sign tx in local, and take the result
  * The response is unsigned tx
  * The API endpoint itself do not send the tx
* Send the response above with \`Send Tx\` API

## Useful materials

Example python SDK \(Unofficial\) is in [https://github.com/psy2848048/hdacpy](https://github.com/psy2848048/hdacpy)  
In Javascript, you may contribute from forking with [https://github.com/cosmostation/cosmosjs](https://github.com/cosmostation/cosmosjs)

