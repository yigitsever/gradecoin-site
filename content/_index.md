+++
title = "Gradecoin"
sort_by = "weight"
+++

# Welcome to Gradecoin!
Blockchains are incredibly simple yet can appear very complicated, we will see how they work and practice programming _production_ cryptography code.

This server is the sandbox for the PA1, it's currently running the Gradecoin application. Gradecoin is the faux currency we will use to simulate a blockchain network. At the end of the simulation, the amount of Gradecoin you hold will be your PA1 grade.

**A quick summary**: authenticate yourself to the system using public key encryption.
Craft [Transaction](@/transaction_docs.md) proposals and tag them using [JWTs](@/JWT.md).
When there are enough transactions then you can propose [Blocks](@/block_docs.md) in the same way.
Blocks need to be _mined_ beforehand using Proof-of-work, or brute force.

Gradecoin offers 3 endpoints at [/register](/register), [/block](/block) and [/transaction](/transaction). You can only send GET requests to /block and /transaction without authorization.
The server is programmed in [RESTful](https://www.service-architecture.com/articles/web-services/representational_state_transfer_rest.html) architecture, there are no `DELETE`, `PUT` or `UPDATE` operations, though.

Gradecoin uses a Proof-of-work block accepting mechanism. It uses single round [Blake2s](https://www.blake2.net/) hashing which produces 256-bit (64 hexadecimal characters) output. The [target](https://wiki.bitcoinsv.io/index.php/Target) hash is _24 bits_ or _6 hexadecimal characters_ of 0.

> We're expecting you to use existing tools and implementations. Standards are hard. [Don't roll your own crypto](https://www.reddit.com/r/crypto/comments/2coqsy/dont_roll_your_own/). Feel free to ask questions. Collaborate.

You might ask,

> But if nobody has any Gradecoin then how do we have transactions?

You get rewarded for your hard work during the authentication with some Gradecoin to start with!
Then you can earn block rewards by proposing blocks, create some Gradecoins by generating traffic on the system, or transact with our new highly trained AI bots!

# Coinbase
The first transactions of a block is called the `coinbase`. They are the **author** of the block proposal and if the block is accepted then they get compensated for their efforts with some Gradecoin.

# Public Key Signatures
Gradecoin uses 2048 bit RSA keypairs.

# Services
## /register
- Create your own 2048 bit RSA `keypair`
- Download `Gradecoin`'s Public Key from [Moodle](https://odtuclass.metu.edu.tr/my/)
- Encrypt your [JSON](https://www.json.org/json-en.html) wrapped `Public Key`, `Student ID` and one time `passwd` using Gradecoin's Public Key
- Your public key is now in our database and can be used to sign your JWT's during requests
- **Don't forget your Public Key**
- For more information, check the [register](@/register_docs.md) page

## /transaction
- You can offer a [Transaction](@/transaction_docs.md) with a POST request
    - The request should have `Authorization`
    - The request header should be signed by the Public Key of the `by` field in the transaction
- Fetch the list of `Transaction`s with a GET request
- For more information, check our [transaction](@/transaction_docs.md) page

## /block
- Offer a [Block](@/block_docs.md) with a POST request
    - The request should have `Authorization`
    - The `transaction_list` of the block should be a subset of pending transactions, available on [/transaction](/transaction)
- Fetch the last accepted `Block` with a GET request
- For more information, check our [block](@/block_docs.md) page

> `Authorization`: The request header should have Bearer JWT.Token signed with Student Public Key

## /user
- Looking for people to conduct business with? Everyone is listed here! 🤖👋 are bots who are very eager to transact with you. I've trained them personally.

# Questions
## This all sound complicated!
- I've drawn inspiration from [actual Bitcoin transactions](https://explorer.bitcoin.com/btc) and [warp](https://github.com/seanmonstar/warp/blob/master/examples/todos.rs). The simplicity of the system is how little interfaces it has.
- Don't know where to start? Gradecoin uses RESTful API; simple `curl` commands or even your browser will work! [This website can help as well](https://curl.trillworks.com/).
- [JWT Debugger](https://jwt.io) and the corresponding [RFC](https://tools.ietf.org/html/rfc7519).
- Remember that you are absolutely encouraged to grab off-the-shelf implementations for every cryptography primitive you will use. You can start by finding a code snippet to generate a RSA keypair?
- Check out [misc](@/misc_docs.md) for everything else you might be curious about.

## How do you actually earn Gradecoin?
- Register yourself to at [/register](@/register_docs.md)
- Create transactions at [/transaction](@/transaction_docs.md)
- Create blocks to commit transactions at [/block](@/block_docs.md)
- See how everyone is doing and find people to trade with at [/user](/user)

## I found a bug!
Thank you! Please [let me know](mailto:yigit@ceng.metu.edu.tr) so we can solve it.

## I hacked the server!
That wasn't supposed to happen :( I did not place any intentional vulnerabilities to the system so if you cracked something, it was not intended. Please don't abuse it and let me know so I can patch it.

## Submission?
At the end of the _simulation_, your Gradecoin balance will be your grade. I will also expect a unique client programmed in either;
- c
- c++
- perl
- rust
- python
- dart/typescript
- random assortment of bash scripts

If your favourite programming language is missing please let me know 🤷?

## Can my friends play?
Probably not at this point. I've allowed a couple of people during the testnet phase but don't intend to any more.

## How and or Why?
- [Built](https://xkcd.com/2314/), [with](https://lofi.cafe/) [Rust](https://xkcd.com/2418/)
