+++
title = "Register"
description = "Register Documentation"
weight = 3
+++

Here you can authenticate yourself with the system with **your own RSA key pair**.
Only people who are enrolled to the class can open Gradecoin accounts, with some exceptions for people who asked nicely.
This is enforced with your Student ID (e123456) and a one time password you received with your complementary *Welcome to Gradecoin* email.

# Authentication Process
> The cryptographic outputs you are sending over the network are all Base64 Encoded

- Gradecoin's Public Key (`gradecoin_public_key`) is listed on our Moodle page and [here](/gradecoin.pub). Download and load it to your client.
- Create a JSON object (`P_AR`) with your `metu_id` ("e"+`6 chars`) and the `public key` you have created before in base64 (PEM) format (`S_PK`) [reference](https://tls.mbed.org/kb/cryptography/asn1-key-structures-in-der-and-pem)
```json
{
    "student_id": "e123456",
    "passwd": "32 char secret",
    "public_key": "---BEGIN PUBLIC KEY..."
}
```

## Cipher Initialization
> Since we are working with AES-128, both key and IV should be 128 bits (or 32 hexadecimal characters)

- Pick a short temporary key (`k_temp`)
- Pick a random IV `iv` ([1](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Initialization_vector_(IV))) ([2](https://en.wikipedia.org/wiki/Initialization_vector)).

## Encryption
- Encrypt the serialized string of `P_AR` with 128 bit block [AES](https://en.wikipedia.org/wiki/Initialization_vector) in [CBC](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#CBC) mode with [Pkcs7 padding](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Padding) using the temporary key (`k_temp`), the result is `C_AR`. Encode this with base64.
- The temporary key you have picked `k_temp` is encrypted using RSA with [OAEP](https://en.wikipedia.org/wiki/Optimal_asymmetric_encryption_padding) padding scheme using SHA-256 with `gradecoin_public_key`, giving us `key_ciphertext`. Encode this with base64.
- Base64 encode the IV (`iv`) as well.

{% tidbit() %}
The available tools and libraries might warn you about how using the primitives given above are "hazardous". They are, crypto is hard.
{% end %}

- The payload JSON object (`auth_request`) can be serialized now:

```json
{
    "c": "C_AR",
    "iv": "iv",
    "key": "key_ciphertext"
}
```

If your authentication process was valid, you will be given access and your public key fingerprint that is your address.
Please note it down.
You can now sign [JWTs](@/JWT.md) to send authorized transaction requests.
After all this, you might want to bask in the glory of having successfully managing your way through a home-made cryptographic system.
Maybe the Gradecoin you got given as the registration reward will help.

{% tidbit() %}
Seriously, congratulations
{% end %}
