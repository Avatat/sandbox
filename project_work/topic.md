# Topic
Comparison, choice and implementation of a modern encryption algorithm for VoIP communication.

# Purpose
A purpose is to compare, choose the best one, and implement a modern encryption algorithm for an open source VoIP communicator - [Mumble](https://github.com/mumble-voip/mumble).
It's required, because Mumble uses old and slow algorithm - AES-OCB.
It's an answer for the issue: https://github.com/mumble-voip/mumble/issues/3918

I would like to benchmark below algorithms:
* AES-GCM
* ChaCha20-Poly1305
* AEGIS

Compare and rate their suitability for VoIP communication.

### Comparison table:
| Algorithm | Library | Latency | CPU usage | Throughput | License / patents |
| --------- | ------- | ------- | --------- | ---------- | ----------------- |
| AES-GCM-? | OpenSSL |
| ChaCha20-Poly1305 | OpenSSL |
| AEGIS-? | ? |
