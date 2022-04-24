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
Since there are many ways to convert an object to JSON, we enforce the following rules (alongside the regular rules of JSON syntax) for consistency:
- There shouldn't be any whitespace or newlines in the serialized string.
- The order of fields should be exactly as shown above.
- All keys and string values must be enclosed with quotation marks (`"`).

Here's an example demostrating how your JSON string should look like:
```
{"source":"bar","target":"baz","amount":2,"timestamp":"2021-04-18T21:49:00"}
```

# Transaction Rules
- Transactions should be sent from your account (`source`) to any other account (`target`).
- You cannot create multiple transactions with the same `source`/`target` pair.
- Transactions generate traffic which is something we desperately need in Gradecoin, so for every transaction you send, some Gradecoin will be generated out of thin air and will appear on your account.
    - The amount of Gradecoin that will be generated is given by `tx_traffic_reward` field of [`/config`](/config).
    - For example, if `tx_traffic_reward` is 1 and you send 2 coins, only 1 coin will be deduced from your account since you will be given 1 coin for generating traffic. The target will receive 2 coins.
- Don't worry if your transaction goes unaccepted! Transactions do not disappear until they are committed into the ledger with a block.
- Every transaction has a unique ID generated using the `source`, `target` and `timestamp` fields.
- Transactions have a lower and upper amount limit.
    - These are given by `tx_lower_limit` and `tx_upper_limit` fields of [`/config`](/config). See [misc page](@/misc_docs.md) for more information about configuration.
