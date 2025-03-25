# ChordDHT-C – A Minimal Distributed Hash Table in C with HTTP/UDP Interface

This project implements a distributed hash table (DHT) system in C based on the [Chord](https://pdos.csail.mit.edu/papers/chord:sigcomm01/chord_sigcomm.pdf) protocol, developed as coursework for *Rechnernetze und Verteilte Systeme* at TU Berlin. It features both a static and dynamic DHT, supporting peer-to-peer key-based routing and HTTP-accessible resources over UDP.

---

## 🚀 Features

- Peer-to-peer DHT system using the Chord protocol
- Static and dynamic node configurations
- Lookup requests and response forwarding over UDP
- Resource access via standard HTTP methods (`GET`, `PUT`, `DELETE`)
- Hashing with OpenSSL (SHA-256, 16-bit space)
- Graceful client redirect via `303 See Other`
- Stateless retry mechanism with `503 Service Unavailable`
- Basic stabilization protocol for maintaining ring structure

---

## Architecture Overview

- Nodes maintain information about their **predecessor** and **successor**
- All inter-node communication via UDP (custom protocol)
- Clients interact over HTTP (`curl`, browsers, etc.)
- Keys are derived from path hashes (first 2 bytes of SHA256)

---

## Supported Commands

### HTTP (Client-facing)
- `GET /path`: fetch a resource (returns 404/200/303)
- `PUT /path`: create a resource (returns 201)
- `DELETE /path`: delete a resource (returns 200/204)

### Internal Messages (UDP)
- `LOOKUP` (0), `REPLY` (1), `STABILIZE` (2), `NOTIFY` (3), `JOIN` (4)

---

## Usage

### Static DHT
```bash
# Start Node A
PRED_ID=49152 PRED_IP=127.0.0.1 PRED_PORT=2002 \
SUCC_ID=49152 SUCC_IP=127.0.0.1 SUCC_PORT=2002 \
./build/webserver 127.0.0.1 2001 16384

Dynamic DHT
bash
复制
编辑
# Start Node with optional anchor
./build/webserver 0.0.0.0 1000 42 127.0.0.1 1395
Testing
Run with test infrastructure:

bash
复制
编辑
make test
pytest test/test_praxis2.py
pytest test/test_praxis3.py
Use wireshark + rn.lua dissector for debugging


