# SSL/TLS: Seuring Network Comm

## Cryptography Overview

- SSL and TLS are cryptographic protocols that provide security for network communications.

- TLS supersedes SSL and is the current standard for secure communication.

- Cryptography is the method of securing data such that it's trusted and is accessible only by authorized parties.

  ![](./imgs/ssl-encryption-banner-img.webp)

- Cryptographic Keys need to be stored somewhere in order to partake in securing things like network communication or data at rest.

- One way to store cryptographic keys is in a Public Key infrastructure(PKI) certificate.

- Keys can also be stored on a smart card or a hardware security module(HSM). Smart cards would be used for instance to authenticate to a VPN or perhaps to authenticate to a secured or restricted system. Common access cards(CAC) are a type of smart card, but they can do more, and so that card that we use to authenticate to restricted computer systems or VPN might also be the same card we use to gain access to a building.

- Cryptographic Keys can also be stored in files. Depending on the type of key you are storing such as a private key we want to make sure that is a password-protected file, because it's private to the enetity to which it was issued. It's not designed to be shared with others. And, that's why that file containing the private key be password-protected, but it should also be stored in a secure location.

- Trusted Platform Model(TPM) is a firmware and this firmaware can store cryptographic keys that are used to encrypt or decrypt entire disc volumes. TPM can also store information about the startup sequence on a machine, and if it's been tampered with TPM can detect it.

- Cryptographic keys can also be stored on token devices, which can be a physical token, such as a key fob device that's used to gain access to a restricted environment, or system.

- Token devices these days can be virtual as well, like a smartphone app, but either way these token devices can store PKI Certification information, including keys in order to enable security.

- The General Encryption Process starts with plain text, plaintext is origin data before it's been encrypted or scrambled. And, so that plaintext gets fed into a encryption algorithm along with the key. The result of which is encrypted data, otherwise known as ciphertext.

  So, once the data is encrypted such as sending data over the network, this would happen before it's sent out over the wire or wirelessly. So, while it's traversing the network, if the data is encrypted, anyone that can see the network traffic or capture it would normally be able to see the addressing onformation in the transmission.

  Because normally, encryption over the network would encrypt only the payload or the data of the packet. We can encrypt more than that but that's the general norm.

- Only, those parties that having the required  decryption key would be able to decrypt the ciphertext back into plaintext.
  
  ![](./imgs/Screen-Shot-2018-10-26-at-1.webp)

## Where is Cryptography Used?

  ![](./imgs/Cryptography101_Artboard-1024x584%201%20(traced).svg)

- Well, let's take a couple of few simple examples but alos on a mobile device.

- We might encrypt all of the contents of the mobile device itself including any removable mediua like MicroSD cards.

- We can use crypto to encryp the file system for protection of data at rest.

- We can also use it to encrypt network traffic, such as encrypting http-secured website.

- We can use cryptography for file hashing, where we can generate a unique file hash or value, and than compare that in future when we take the hash again to the orignal hash, to see if a change has been made.

- We can also use cryptography with cryptocurrency blockchain transactions.