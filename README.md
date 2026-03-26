# [CryptosystemWrite-Up](https://tryhackme.com/room/hfb1cryptosystem)
## Write-Up for TryHackMe's Cryptosystem

In this lab, we are given an RSA-encrypted message and we have to find the key to decrypt the flag.

### We are given:
- **n**, which is the public key
- **c**, which is the encrypted message
- **e** = `0x10001`, which is the public exponent
- **p** = `getPrime(1024)`, which is a random 1024-bit prime

### We want to:
1. Recover **p** and **q**,
2. Compute the private key, **d**,
3. and Decrypt **c**

We know that `p` and `q` are prime numbers. n = p * q is the modulus for RSA. Usually, finding p and q is hard because they are random and large. In the python snippet, we see `q = primo(p)`. We know that q is the next prime after p, which means p and q are very close to each other. This is a *vulnerability* as it makes it easier for us to brute-force to check numbers near the square root of n. We want to take the square root of n because, since p and q are close, the square root of n should roughly be the same size as p and q. We would keep brute-forcing until we find 2 numbers that multiply together to get n. Those 2 numbers will become p and q. This method is called **Fermat’s Factorization Method**.

Once we have p and q, to find d, we use the modular inverse of e modulo φ(n):
- Compute **φ(n)** = `(p - 1)(q - 1)`
- **d** = `inverse(e, φ(n))`

Once we recover the private key, d, d decrypts the ciphertext and displays the flag.

RSA decryption is:
- **m** = `(c^d) mod n` or with the help of python, `pow(c, d, n)`
