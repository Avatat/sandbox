# Topic
Comparison, choice and implementation of a modern encryption algorithm for VoIP communication.

# Purpose
A purpose is to compare, choose the best one, and implement a modern encryption algorithm for an open source VoIP communicator - [Mumble](https://github.com/mumble-voip/mumble).
It's required, because Mumble uses old and slow algorithm - AES-OCB. The second reason is that OCB mode is patented and can't be used for free in commercial solutions (like games, in Mumble case).
It's an answer to [this issue](https://github.com/mumble-voip/mumble/issues/3918).

I would like to benchmark below algorithms:
* [AES-256-GCM](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-38d.pdf)
* [ChaCha20-Poly1305](https://tools.ietf.org/html/rfc8439)
* [AEGIS](http://competitions.cr.yp.to/round3/aegisv11.pdf)

Compare and rate their suitability for VoIP communication.
VoIP communication is sensitive to latency and its variation (jitter), so the most suitable solution should have the lowest latency. We will use encryption on the server side, so CPU usage and throughput are also important metrics.

### Comparison table:
| Algorithm | Library | Latency | CPU usage | Throughput | License | Limitations |
| --------- | ------- | ------- | --------- | ---------- | ------- | ------- |
| AES-128-OCB | [OpenSSL](https://www.openssl.org/) | | | | Apache | Patented |
| AES-256-GCM | [OpenSSL](https://www.openssl.org/) | | | | Apache |  |
| ChaCha20-Poly1305 | [OpenSSL](https://www.openssl.org/) | | | | Apache |  |
| AES-256-GCM | [NSS](https://nss-crypto.org/) | | | | MPL 2 |  |
| ChaCha20-Poly1305 | [NSS](https://nss-crypto.org/) | | | | MPL 2|  |
| AES-256-GCM | [wolfCrypt](https://www.wolfssl.com/products/wolfcrypt-2/) | | | | GPLv2 | Commercial license |
| ChaCha20-Poly1305 | [wolfCrypt](https://www.wolfssl.com/products/wolfcrypt-2/) | | | | GPLv2 | Commercial license |
| AES-256-GCM | [libsodium](https://doc.libsodium.org/) | | | | ISC | [Requires SSSE3, AES-NI and CLMUL](https://doc.libsodium.org/secret-key_cryptography/aead/aes-256-gcm#limitations) |
| ChaCha20-Poly1305 | [libsodium](https://doc.libsodium.org/) | | | | ISC |  |
| AEGIS-128L | ? | | | | ISC |  |
| AEGIS-256 | [libsodium](https://doc.libsodium.org/) | | | | ISC |  |
