+++
title = "Gradecoin"
sort_by = "weight"
+++

# Welcome to Gradecoin!
Blockchains are incredibly simple but they can seem very complicated.
We will see how they work and practice programming _production grade_ cryptography code.

This server is the sandbox for PA1 and it iss currently running the Gradecoin application.
Gradecoin is the faux currency we will use to simulate a blockchain network.
**At the end of the simulation, the amount of Gradecoin you hold will be your PA1 grade.**

**A quick summary**: authenticate yourself to the system using public key encryption.
Craft [Transaction](@/transaction_docs.md) proposals and tag them using [JWTs](@/JWT.md).
When there are enough transactions then you can propose [Blocks](@/block_docs.md).
Blocks need to be _mined_ beforehand using Proof-of-work a.k.a. brute force.

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

Please respect the system and others.
Keep your request rate below a reasonable limit.
Programming a bot is absolutely fine as long as it's not aggressively sending requests.

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
    - The request header should be signed by the Public Key of the `source` field in the transaction
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
- Looking for people to conduct business with? Everyone is listed here!
🤖👋 are bots who are very eager to transact with you.
I've trained them personally using state-of-the-art neural networks running on thousands of TPUs.

# Questions
## This all sound complicated!
- I've drawn inspiration from [actual Bitcoin transactions](https://explorer.bitcoin.com/btc) and [warp](https://github.com/seanmonstar/warp/blob/master/examples/todos.rs). The system has only 3 interfaces. It's simple once you read everything over a couple of times.
- Don't know where to start? Gradecoin uses RESTful API; simple `curl` commands or even your browser will work! [This website can help as well](https://curl.trillworks.com/).
- Check out [JWT Debugger](https://jwt.io) and the corresponding [RFC](https://tools.ietf.org/html/rfc7519).
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

## I want to contribute!
Thank you! The code for Gradecoin and this site are open source so you can take a look and let me know if you have any improvements, corrections, typos to point out or whatever.
Both documentation (this site) and code contributions are appreciated.
[My git server](https://git.yigitsever.com/) will be somewhat ahead of the [GitHub](https://github.com/yigitsever/gradecoin) repository but I will sync them at every major milestone.

## Submission?
At the end of the _simulation_, your Gradecoin balance will be your grade. I will also expect your client for submission, programmed in either;

- c
- c++
- dart/typescript
- go
- perl
- python
- rust
- random assortment of bash scripts

If your favourite programming language is missing please let me know 🤷?

## Can my friends play?
Sure! They can get in contact with me until the end of the testnet phase.

## How and or Why?
- [Built](https://xkcd.com/2314/), [with](https://lofi.cafe/) [Rust](https://xkcd.com/2418/)
