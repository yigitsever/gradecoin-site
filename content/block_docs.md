+++
title = "Blocks"
description = "Block Documentation"
weight = 10
+++

> Blocks commit proposed transactions into the ledger.
> A transaction that do not appear on a valid block is not accepted by the network.

Blocks in Gradecoin are proposed to commit [Transactions](@/transaction_docs.md) that were proposed previously to the system.
`transaction_list` of the Block should be filled with valid transactions to be committed.
Blocks are valid when they are proposed with a `nonce` that produces a `hash` value with 6 zeroes (24 bits) at the left hand side.

We are _mining_ using [blake2s](https://www.blake2.net/) algorithm, which produces 256 bit hashes.
Hash/second is roughly {{ exp(num="20x10", exponent="3") }} on my machine, a new block can be mined in around 4-6 minutes.

{% tidbit() %}
We have seen blocks that came in within a minute during the testnet phase!
{% end %}

# Requests

## GET
A HTTP `GET` request to [/block](/block) endpoint will return the latest mined block.

## POST
A HTTP `POST` request with Authorization using [JWT](@/JWT.md) will allow you to propose your own blocks.

# Fields
```
transaction_list: [array of Transaction IDs]
nonce: unsigned 32-bit integer
timestamp: ISO 8601 Timestamp (<date>T<time>)
hash: String
```

## Coinbase
The proposer of the block is identified by the first transaction in the `transaction_list`.
This transaction is called the *coinbase* and will get awarded the block mining reward for their work.

> Place one of your own transactions as the first item in `transaction_list`

# Mining
The _mining_ process for the hash involves;
- Creating a temporary JSON object with `transaction_list`, `timestamp` and `nonce` values
- Serializing it
- Calculating blake2s hash of the serialized string

If the resulting hash is valid, then you can create a `Block` JSON object with the found `nonce` and `hash`.

# Hash
`tha` field in [jwt documentation](@/JWT.md) stands for "The Hash" in the context of blocks.
Fill this with the `hash` value you found during the mining process.

# Block Rules
- Blocks should include some minimum number of transactions.
- Blocks cannot have duplicate transactions.

# References
- [ISO 8601 Reference](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations)
