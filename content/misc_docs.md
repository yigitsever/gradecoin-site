+++
title = "Misc"
description = "Documentation about everything else"
weight = 10
+++

# Fingerprint
## Definition

Fingerprints are 256-bit, 64 character hexadecimal user identifiers. Fingerprints are used in defining users in [transactions](@/transaction_docs.md) and [blocks](@/block_docs.md).

## Fingerprint Generation
A user's fingerprint is generated via applying SHA256 sum of the user's public RSA key.

# Config
The [/config](/config) endpoint will return the current parameters that Gradecoin uses.

- `name`: Name of this Gradecoin network.
- `url_prefix`: URL prefix for the network. All API commands will be served under this prefix.
    - For example, if url_prefix is `example`, register at `gradecoin.xyz/example/register`.
    - It can be empty, in which case the endpoints are accessed directly from `/`. Example: `gradecoin.xyz/register`.
- `preapproved_users`: The name of the CSV file that contains the list of users who can register. Only relevant for the admins.
- `block_transaction_count`: A valid block should have at least this many transactions.
- `hash_zeros`: Determines the number of zero hexadecimal characters a correct hash should start with.
- `register_bonus`: Initial registration bonus. This will determine your balance after registration.
- `block_reward`: Coinbase reward. When a block is proposed successfully and added to ledger, the proposer will gain this amount of coins.
- `tx_gas_fee`: New transaction proposals must pay this amount
- `tx_upper_limit`: Upper limit for transaction amount.
- `tx_lower_limit`: Lower limit for transaction amount.
- `tx_traffic_reward`: Transaction traffic reward, used to incentivize users to make transactions. When an account sends money, it will receive this reward.
- `bots`: The configuration of the bots in this network.
    - Each key will be the fingerprint of a bot.
    - Each value will be another JSON object. Currently, it only contains one self-explanatory field: `starting_balance`.

# Version
The [/version](/version) endpoint will return the current version that's currently live on this server.
