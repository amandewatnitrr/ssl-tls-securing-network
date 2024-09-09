# SSL, TLS and HTTPS

- So when web server supplies you certificate, you verify signature in that certificate, you verify its validity, period, and so on.

- But certificate is not used itself for encryption of data between web browser and web server, and that's where SSL and TLS protocols come in.

- So, `SSL` stands for `Secure Sockets Layer`. `TLS` stands for `Transport Layer Security`. `TLS` & `SSL` is a cryptographic protocol that is used for secure communication over a computer network. And it is used primarily for encryption of data.

- Difference b/w `SSL` and `TLS` is that `TLS` is newer version of `SSL`. `TLS` is more secure and more reliable than `SSL`. And `TLS` is backward compatible with `SSL`.

- But what is the connection between SSL, TLS and certificate? Actually certificate itself is not dependent and does not set exactly which protocol either SSL or TLS must be used for data encryption.

- And it means that the same certificate may be used either for SSL or TLS.
So sometimes you hear such term as SSL certificate. So certificate doesn't depend on specific protocol SSL or TLS.

- So each certificate may be used either for SSL or TLS, but if you want to be more specific and tell others that you are talking about digital certificates that are used for encryption, you can add prefix SSL, TLS like that.

- There are total 3 versions of SSL protocol: `SSL 1.0`, `SSL 2.0`, `SSL 3.0`. And there are total 3 versions of TLS protocol: `TLS 1.0`, `TLS 1.1`, `TLS 1.2`, and `TLS 1.3`.

- SSL is currently considered to be a deprecated protocol, and it is not recommended to use it. And it is recommended to use TLS protocol instead.

- Even with the TLS protocol, it is recommended to use the latest version of TLS protocol, which is `TLS 1.3`.

### Why RSA is not used for data encryption in HTTPS?

- RSA is not used for data encryption in HTTPS because it is slow. RSA is used for key exchange and digital signatures in HTTPS.
- Also, Bi-directional encryption is not possible with RSA as it will require RSA Key Pairs on both sides.
- RSA is a symmetric key algorithm, and it is not suitable for bulk data encryption.
- While on the internet, we generally deal with scenarios that require asymmetric encrytpion.

### How TLS Session is established?

- When a client connects to a server via HTTPs, when TCP session is established, we start establishing a TLS Session.

- Web Browser first sends to Web Server a list of supported suites, and the server chooses the best Cipher Suite from this list. This step is called `Negotiation of Cipher Suite`.

- `Cipher Suite` is a combination of encryption, authentication, and message authentication code (MAC) algorithms used to secure data. It is a set of protocols that will be used in TLS Communication.

- For example, each Cipher Suite defines how symmetric key for data encryption will be generated. Also, it defines which algorithm will be used for data encryption and decryption. It also includes information about the Hashing Algorithm that will be used for data integrity.

- The server sends its certificate to the client/browser. If there are any Intermediate CA, they are also sent to the client.

- The client/web-browser verifies the certificate and generates symmetric key for data encryption.

  In some cases symmetric key is generated on web browser side and sent to the server in encrypted form, encrypted by the public key of the server.

  Another way of Generating Symmetric Key is by utilizing the Diffie-Hellman Key Exchange Algorithm. And, this case the communucation doesnot need to be encrypted, as it is designed for generation of same key on both sides.

  When both sides have same encryption key, we can start sending and reciving actual data from server to browser and vice versa.

## Analyzing TLS Session using WireShark

- So, let's do it for `wikipedia.org`. Let's open wikipedia.org in web browser and let's see how TLS session is established.

- On the `WireShark` select the network interface that you are using to connect to the internet, for our it is `Wi-Fi`, and stop the capture.

- Now, filter by ip address using `ip.addr == <ip_addr/>`

  ![](./imgs/Screenshot%202024-09-08%20at%209.17.23 PM.png)

  And, here in the image as we can see we see three TCP packets, this is what is also known as `Three-way Handshake`.

  So, first our computer sends a request for the TCP Session. The TCP server responds with flags `SYN`(Synchronize) and `ECN`(Explicit Congestion Notification). And, then our computer responds with `SYN` and `ACK` flags.

  ECN is a feature that allows end-to-end notification of network congestion without dropping packets. It is an optional feature that is only used when both endpoints support it and are willing to use it.

  The Server responds with `ACK` flag, and acknowledges the session setup. After, that our  computer sends another `ACK` flag to acknowledge the session setup, after which the `TCP` session is established.

  ![](./imgs/Screenshot%202024-09-08%20at%209.23.38 PM.png)

  After this, we see our first TLS Packet. We  see TLS 1.3 - Handshake Protocol - Client Hello. This is the first packet of the TLS Session marked as `Client Hello`.

  Here we see the `Cipher Suites` that are supported by our web browser. And, the server will choose the best Cipher Suite from this list. We have `16` Cipher Suites in our case, that was sent by our web browser to the server.

  The Server Suites names are of the format `Communication Protocol - Key Exchange Algorithm - Cipher Algorithm - Hash Creation Algorithm`.

- Now, We see a response from the server in this regard with TLS Packet marked as `Server Hello`.

  ![](./imgs/Screenshot%202024-09-08%20at%209.32.53 PM.png)

  We again see the TLS Version 1.2 - Handshake Protocol - Server Hello. This time we have a Session ID, and the Cipher Suite that was chosen by the server for this particular session.

- Looking at the next packet, we see the `Certificate` packet. This is the certificate that is sent by the server to the client.

  It is marked as `Handshake Protocol - Certificate`. Inside Handshake Protocol, we have multiple certificates, and we can see the certificate chain.

- Next, we see the `Certificate Status, Server Key Exchange` packet. This is the packet where the server sends the public key to the client.

  It is marked as `Handshake Protocol - Server Key Exchange`. Inside this packet, we have the public key of the server.

  It confirms here that the `Server Hello Done` packet is sent by the server.

- Next, we see the `Client Key Exchange` packet. This is the packet where the client sends the symmetric key to the server.

  This packet comes from the client to the server, and it is marked as `Handshake Protocol - Client Key Exchange`. And, in this messages clients tells the server which algorithm was used for secure key generation.

  And, along with this it send encrypted handshake message.

- After this, clients sends packet marked as Application Data. This is the actual data that is sent from the client to the server encrypted application data.

  After these packets, we see the response from the server.

  It is marked as `Application Data`. And, this is the actual data that is sent from the client to the server.

  ![](./imgs/Screenshot%202024-09-08%20at%209.55.46 PM%201.png)