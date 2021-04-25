+++
title = "Transactions"
description = "Transaction documentation"
weight = 6
+++

A transaction request between `source` and `target` to move `amount` Gradecoin.

# Requests
## GET
A HTTP `GET` request to [/transaction](/transaction) endpoint will return the current list of pending transactions.

## POST
A HTTP `POST` request with Authorization using [JWT](@/JWT.md) to [/transaction](/transaction) will allow you to propose your own transactions.

# Fields
```
source: Fingerprint
target: Fingerprint
amount: unsigned 16 bit integer
timestamp: ISO 8601 <date>T<time>
```

# Hash
`tha` field in [jwt documentation](@/JWT.md) in fact stands for "The Hash".
In the context of a transaction proposal, you need the [Md5](https://en.wikipedia.org/wiki/MD5) hash of the serialized JSON representation of transaction.
Serializing in this context is a simple JSON to string conversion with key, value pairs enclosed with quotation marks (").
The resulting JSON string should look something like;

```
{"source":"bar","target":"baz","amount":2,"timestamp":"2021-04-18T21:49:00"}
```

Or; without any whitespace, separated with `:` and `,`.

# Transaction Rules
- Transactions should be sent from your account (`source`) to any other account (`target`).
- Transactions generate traffic which is something we desperately need in Gradecoin, so for every transaction you send, some Gradecoin will be generated out of thin air and will appear on the target.
- Don't worry if your transaction goes unaccepted! Transactions do not disappear until they are committed into the ledger with a block.
- Every transaction has a unique ID generated from the `source` and `target` fields. No two transaction with the same ID can appear on the pending transaction list [/transactions](/transaction)
- Transactions have an upper amount limit.
