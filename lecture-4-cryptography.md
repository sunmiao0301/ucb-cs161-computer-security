##### Cryptography

| 对称密钥     | 非对称密钥                             |                                     |
| ------------ | -------------------------------------- | ----------------------------------- |
| 保密         | 具有链接模式的分组密码（例如 AES-CBC） | 公钥加密（例如 El Gamal、RSA 加密） |
| 完整性和认证 | MAC（例如，AES-CBC-MAC）               | 数字签名（例如 RSA 签名）           |

##### Asymmetric cryptography

SSL 和 TLS 都是通信协议，用于加密服务器、应用程序、用户和系统之间的数据。这两种协议都会对通过网络连接的双方进行身份验证，以便他们能安全交换数据。

Taher Elgamal 领导了 SSL 的开发，并于 1995 年公开发布了 SSL 2.0。SSL 旨在确保万维网上的通信安全。在 SSL 经历多次迭代后，Tim Dierks 和 Christopher Allen 于 1999 年创建了 TLS 1.0，作为 SSL 3.0 的后继者。 TLS 是 SSL 的直接后继者，所有版本的 SSL 目前均已弃用。但是，使用术语 *SSL* 来描述 TLS 连接的情况很常见。在大多数情况下，术语 *SSL* 和 *SSL/TLS* 都是指 TLS 协议和 TLS 证书。

##### 非对称加密的一个小问题There is a slight catch.

Alice和Bob之间将如何拿到对方的公钥？如果使用广播，那么就存在如下问题场景：Attila could send a spoofed broadcast message that appears to be from Bob, but that contains a public key that Attila generated. If Alice trustingly uses that public key to encrypt messages to Bob, then Attila will be able to intercept Alice’s encrypted messages and decrypt them using the private key Attila chose.

We’ll soon see some methods that help somewhat with that problem.

# 目前看到public key exchange还没看，跳到websec看看