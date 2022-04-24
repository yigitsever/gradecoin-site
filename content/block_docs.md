+++
title = "Blocks"
description = "Block Documentation"
weight = 10
+++

> Blocks commit proposed transactions into the ledger.
> A transaction that do not appear on a valid block is not accepted by the network.

We use Blocks to commit proposed [Transactions](@/transaction_docs.md) to the ledger in order to realize them.
`transaction_list` of the Block is filled with valid transactions.
In order to create a valid block, the proposer must find a suitable `nonce` value that makes the `hash` of the block valid.
The properties a valid hash should have will be explained in subsequent sections.

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
The proposer of the block is identified by the source of the first transaction in `transaction_list`.
This transaction is called the *coinbase*, and its source will get awarded by the block mining reward for their work.
The amount of the reward is determined by `block_reward` field of [`/config`](/config).

> Place one of your own transactions as the first item in `transaction_list`.
> Otherwise, the server will not be able to authenticate you.

# Mining
The _mining_ process for the hash involves;
- Creating a temporary JSON object with `transaction_list`, `nonce`, and `timestamp` values
- Serializing it
    - **NOTE:** Serialized JSON must comply with the rules explained in hash section of [transaction](@/transaction_docs.md) page.
    - The order of keys should be as follows: `transaction_list`, `nonce`, `timestamp`.
	- Example:
```json
{"transaction_list":["a1a3","cde4","60e7","4e04"],"nonce":5342433,"timestamp":"2022-04-23T23:49:24.622651"}
```
- Calculating blake2s hash of the serialized string
- Checking if the hash is valid
    - The hash is considered valid if its hexadecimal representation starts with an arbitrary number of zeros.
    - This number is given by `hash_zeros` field of [`/config`](/config).
    - For instance, if `hash_zeros` is 6, a valid hash must start with 6 hexadecimal zeros.

If the resulting hash is valid, then you can create a `Block` JSON object with the found `nonce` and `hash`.

# Hash
`tha` field in [jwt documentation](@/JWT.md) stands for "The Hash" in the context of blocks.
Fill this with the `hash` value you found during the mining process.

# Block Rules
- Blocks have to include a minimum number of transactions.
    - `block_transaction_count` field of [`/config`](/config) yields this value.
    - See [misc Page](@/misc_docs.md) for more information about configuration.
- Blocks cannot have duplicate transactions.

# References
- [ISO 8601 Reference](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations)
