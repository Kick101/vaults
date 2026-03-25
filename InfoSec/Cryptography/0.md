__Definition:__ It is the practice and study of techniques for secure communication in the presence of adversarial behavior.

__Encryption:__ It is a means of securing digital data using one or more mathematical techniques, along with a password or "key" used to decrypt the information.

__Decryption:__ The conversion of encrypted data into its original form.
<hr>

### Symmetric
__Definition:__ Symmetric-key Algorithms are algorithms for cryptography that use the same cryptographic keys for both the encryption of plaintext and the decryption of ciphertext. 
- The keys may be identical, or there may be a simple transformation to go between the two keys. The keys, in practice, represent a shared secret between two or more parties that can be used to maintain a private information link.

__Data Encryption Standard (DES):__ The Data Encryption Standard is a symmetric-key algorithm for the encryption of digital data. Although its short key length of 56 bits makes it too insecure for applications, it has been highly influential in the advancement of cryptography.

__Advanced Encryption Standard (AES):__ also known by its original name _Rijndael_

- AES is based on a design principle known as a substitution–permutation network, and is efficient in both software and hardware. Unlike its predecessor DES, AES does not use a Feistel network. AES is a variant of Rijndael, with a fixed block size. "Block size of 128 bits, and a key size of 128, 192, or 256 bits. By contrast, Rijndael _per se_ is specified with block and key sizes that may be any multiple of 32 bits, with a minimum of 128 and a maximum of 256 bits.

__Substitution Ciphers:__ It is a method of encrypting in which units of plaintext are replaced with the ciphertext, in a defined manner, with the help of a key; the "units" may be single letters (the most common), pairs of letters, triplets of letters, mixtures of the above, and so forth. The receiver deciphers the text by performing the inverse substitution process to extract the original message.

__Caesar Cipher:__ 
__Columnar Cipher:__
<hr>

### Asymmetric
__Definition:__

__RSA:__
<hr>

### Message Authentication Code (MAC)
__Definition:__
<hr>

### Hashing
__Definition:__
<hr>

### Encoding
__Definition:__
<hr>


### Tools

---
<center><h3>SSL/TLS</h3></center>

__Why Certificate Authority(CA)?__
- To generate a certificate for server's public key integrity

__How server gets certificate?__
- The server goes to CA for certificate
- CA takes identity proof of owner and proof of ownership of the server
- Then CA takes server's public key and generate certificate with following fields:
	- server's public key
	- signature -> CA signs/encrypts the server's public key with it's own (CA's) private key 

__How client verifies a server's public key?__
- When client gets certificate from a server
- It sees who's the issued CA for the certificate
- Then client fetches CA's public key and encrypts the server's public key and 
checks the integrity by matching it with the signature it got in certificate

__How client verifies CA's public key?__
- It verifies with root certificate

__what is root certificate?__
- 

### Resources
- [badssl.com](https://badssl.com)