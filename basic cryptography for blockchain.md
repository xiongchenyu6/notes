---
title: "basic cryptography for blockchain"
date: 2023-03-10
---

# Zero Knowledge Prove

1. What are zero-knowledge proofs, and why are they important?
2. Basic concepts of cryptography, such as encryption and decryption.
3. The different types of zero-knowledge proofs.
4. Examples of zero-knowledge proofs that are used in real-world applications, such as blockchain technology.

# What are zero-knowledge proofs

Zero-knowledge proofs are a type of cryptographic protocol that allow one party (the "prover") to demonstrate to another party (the "verifier") that they know a specific piece of information, without revealing any details about that information beyond its truthfulness.

## Why are they important

Zero-knowledge proofs are important because they allow for the creation of secure, private, and anonymous transactions.

# Cryptography

- Hash Function
- Mac and Key derivation
- Secure Random Generator
- Key exchange and DHKE
- Encryption
  - Symmetric Encryption
  - Asymmetric Encryption
  - Digital Signature

## Hash Function Property

- One-way function
- Collision resistant

## Hash Function

### Hash Function

- MD5, sha 1, 2
- sha 3, 256, 512
- sha2-256, sha2-512
- sha3-256, sha3-512

### Secure Hash Function

- SHA-2
- SHA-3

### example

```text
SHA-256('hello') = 2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e73043362938b9824
SHA-384('hello') = 59e1748777448c69de6b800d7a33bbfb9ff1b463e44354c3553bcdb9c666fa90125a3c79f90397bdf5f6a13de828684f
SHA-512('hello') = 9b71d224bd62f3785d96d46ad3ea3d73319bfbc2890caadae2dff72519673ca72323c3d99ba5c11d7c7acc6e14b8c5da0c4663475c2e5c3adef46f73bcdec043
SHA3-256('hello') = 3338be694f50c5f338814986cdf0686453a888b84f424d792af4b9202398f392
Keccak-256('hello') = 1c8aff950685c2ed4bc3174f3472287b56d9517b9c948127319a09a7a36deac8
SHA3-512('hello') = 75d527c368f2efe848ecf6b073a36767800805e9eef2b1857d5f984f036eb6df891d75f72d9b154518c1cd58835286d1da9a38deba3de98b5a53e5ed78a84976
SHAKE-128('hello', 256) = 4a361de3a0e980a55388df742e9b314bd69d918260d9247768d0221df5262380
SHAKE-256('hello', 160) = 1234075ae4a1e77316cf2d8000974581a343b9eb
```

### Hmac

1. length extension attacks

    Hash(message1) and the length of message1 to calculate Hash(message1 ‖ message2)

2. let says we use sha2 hash function as signature

    ```text
    Original Data: count=10&lat=37.351&user_id=1&long=-119.827&waffle=eggo
    Original Signature: 6d5f807e23db210bc254a28be2d6759a0f5f5d99
    ```

3. attacker want to change the data

    ```text
    Desired New Data: count=10&lat=37.351&user_id=1&long=-119.827&waffle=eggo&waffle=liege
    ```

4. attacker can created a new message

    ```text
    New Data: count=10&lat=37.351&user_id=1&long=-119.827&waffle=eggo\x80\x00\x00
              \x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00
              \x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00
              \x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00
              \x00\x00\x00\x02\x28&waffle=liege
    ```

    - original key\'s length was 14 bytes
    - 0x80 followed by a number of 0x00s
    - 0x228 = 552 = (14+55)\*8

5. attacker can calculate the new signature

    ```text
    New Signature: 0e41270260895979317fff3898ab85668953aaa2
    ```

### KDF: Deriving Key from Password

add salt to password

- PBKDF2
- scrypt
- bcrypt
- Argon2

### Secure Random Generator

- laptop management
  - /dev/random (blocking) secure
  - /dev/urandom (non-blocking) pseudo
- hardware
  - TRNG (True Random Number Generator)

```bash
tpm2 getrandom 20
```

- Software

```bash
apg -m 16 -a 1 -n 1 (db password)
openssl rand -hex 32 (eth private key)
```

## Encryption

### Group

::: ai
clock group loop math mod logarithm plusa ring
:::

### (before 1976) Symmetric Encryption(rot13)

nopq

nopq ---- abcd

### Asymmetric Encryption

1. RSA

    1.  coprime

        - Any two prime numbers form a coprime relationship, such as 13 and 61.
        - If one number is a prime number, and the other number is not a multiple of the former, then the two numbers form a coprime relationship, such as 3 and 10.
        - If the larger of two numbers is a prime number, then the two numbers form a coprime relationship, such as 97 and 57.
        - 1 and any natural number are coprime, such as 1 and 99.
        - If p is an integer greater than 1, then p and p-1 form a coprime relationship, such as 57 and 56.
        - If p is an odd integer greater than 1, then p and p-2 form a coprime relationship, such as 17 and 15.

    2.  Euler\'s totient function

        The formula for Euler\'s totient function is as follows: φ(n) = n x (1 - 1/p1) x (1 - 1/p2) x ... x (1 - 1/pk) where n is the given positive integer, and p1, p2, ..., pk are the distinct prime factors of n. φ(8) = 8 x (1 - 1/2) = 4

    3.  Euler\'s theorem

        ![](2023-03-24_14-23-12_screenshot.png)

    4.  modular multiplicative inverse

        If two positive integers a and n are coprime, then there always exists an integer b such that ab - 1 is divisible by n, or in other words, the remainder of ab divided by n is 1. In this case, b is called the "modular multiplicative inverse" of a modulo n.

2. ECC

## Key Exchange

### Diffie-Hellman

- Alice and Bob agree on a prime number p and a base g.
- Alice chooses a secret integer a, and sends Bob g^a^ mod p.
- Bob chooses a secret integer b, and sends Alice g^b^ mod p.
- Alice computes g^b^ mod p, and Bob computes g^a^ mod p.
- Alice and Bob now share the secret integer g^ab^ mod p.

# The different types of zero-knowledge proofs

## scenario 1. cave

![](2023-03-24_12-47-27_screenshot.png)

## scenario 2. Opaque Pricing

You and a competitor want to know if you are paying the same price without revealing how much each of you are paying.

![](2023-03-24_12-48-54_screenshot.png)

### design a zk

We obtain 4 lockable lockboxes, each with a small slot that can take only a piece of paper. They are labelled 100, 200, 300, and 400 for the price per kilogram, and placed in a secure, private room.

![](2023-03-24_12-53-29_screenshot.png)

### Game step 1

You go into the room alone first. Since you are paying 200 per kilogram, you take the key from the lockbox that is labelled 200 and destroy the keys for the other boxes. You leave the room.

![](2023-03-24_12-54-59_screenshot.png)

### Game step 2

Your competitor goes into the room alone with 4 pieces of paper, 1 with a check, and 3 with crosses. Because your competitor is paying 300 per kilogram, they slide the paper with a check inside the lockbox that is labelled 300, and slide the papers with crosses into the other lockboxes. They leave the room.

![](2023-03-24_12-55-28_screenshot.png)

### Game step 3

After they leave, you can return with your key that can only open the lockbox labelled 200. You find a piece of paper with a cross on it, so now you know that your competitor is not paying the same amount as you.

![](2023-03-24_12-56-02_screenshot.png)

## Interactive zero-knowledge proofs

iZKPs require an exchange of messages between the prover and verifier. The verifier sends random challenges to the prover, to which the prover must respond correctly in order to prove their knowledge. An example of iZKP is the Schnorr Protocol.

## example on interactive zero-knowledge proofs

### x ->

f(x) Simple one way function. Easy to go one way from x to f(x) but mathematically hard to go from f(x) to x.

### f(x) = g \^ x mod p

Known(public): g, p

- g is a constant
- p has to be prime

Easy to know x and compute g \^ x mod p but difficult to do in reverse.

### Interactive Proof

1. Alice publishes f(x): g^x^ mod p
2. Alice picks random number r
3. Alice sends Bob u = g^r^ mod p
4. Now Bob has artifact based on that random number, but can't actually calculate the random number
5. Bob returns a challenge e. Either 0 or 1
6. Alice responds with v:

If 0, v = r If 1, v = r + x Bob can now calculate:

1. If e = 0: Bob has the random number r, as well as the publicly known variables and can check if u == g^v^ mod p

If e = 1: u\*f(x) = g^v^ (mod p)

## Non-interactive zero-knowledge proofs

NIZKPs are proofs that can be validated by the verifier without any interaction with the prover. Instead of exchanging messages, the prover generates a proof that can be verified by the verifier using a public parameter. An example of NIZKP is zk-STARKs.

## preprocessing zk-SNARKs

Used in many blockchain applications. They require expensive computationally operations to generate proving keys but once these are generated, they can be used multiple times to generate very small succinct proofs or verify any number of similar proofs of knowledge with very small computational overheads.

## zk-STARKs

cons of zk-STARKS

- expensive to trusted setup

pro of zk-STARKS

- small proof size
- fast verification

## zk-SNARKs

cons of zk-SNARKS

- big proof size
- slow verification

pro of zk-SNARKS

- Scalability

## Zk Rollup

![](2023-03-24_14-05-08_screenshot.png)

## zk potential players

- zkSync
- Polygon's zkEVM
- Starkware
