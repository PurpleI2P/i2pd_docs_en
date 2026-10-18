# i2pd Built-in Torrent Client Guide

> **Requirements:** Compiled in by default unless built with `NO_TORRENTS`. The JSON-RPC interface additionally requires Boost ≥ 1.81 and is not available on Android.

## Overview

i2pd includes a BitTorrent client that runs entirely over I2P. It is implemented as a dedicated
tunnel type (`type = torrents`) and can be managed through a built-in web UI or a
Transmission-compatible JSON-RPC API.

All tunnel-like settings (keys, signature type, I2CP parameters, etc.) are documented in
[Torrents tunnels](user-guide/tunnels/#torrents-tunnels). The full RPC method reference is in [Torrents JSON-RPC](/tutorials/Filesharing/I2PD Built-in Torrent/torrents-rpc/).

---

## 1. Configuration

Add a `torrents` tunnel section to `tunnels.conf`. The only mandatory parameter is `torrentsdir`;

the rest are optional and documented in [Torrents tunnels](user-guide/tunnels/#torrents-tunnels).

```ini
[torrents]
type = torrents
torrentsdir = /path/to/torrents
keys = torrents-keys.dat
trackers = http://atia.i2p/announce
signaturetype = 7
dht = true
dhtfile = torrents.dht
web = true
rpcport = 7657
rpcpath = torrents
rpcaddress = 127.0.0.1
rpcauth = true
rpcuser = admin
rpcpass = secret
```

Restart i2pd after changing `tunnels.conf`.

---

## 2. Getting started

### Add a magnet link

Open the web UI at `http://127.0.0.1:<rpcport>/<rpcpath>/web/` (or `/web/` when `rpcpath` is empty),
paste your magnet link, and click **Add magnet**.

### Add a `.torrent` file

Click **Add .torrent** in the web UI and select the file. You can also drag `.torrent` files
anywhere on the page.

### Add via RPC

```bash
curl -s -X POST http://127.0.0.1:7657/torrents/rpc \
  -H 'Content-Type: application/json' \
  -d '{"method":"torrent-add","arguments":{"filename":"magnet:?xt=urn:btih:..."},"tag":1}'
```

---

## 3. Web interface

When `web = true`, the management UI is served from `<data-dir>/torrents/web/` (bundled copy at
`contrib/torrents/web/`).

### URL

```
http://<rpcaddress>:<rpcport>/[<rpcpath>/]web/
```

### Features

- Add / remove torrents, start / stop individually or all at once
- Progress bars, rates, ETA, peer list, tracker stats
- Delete local data toggle on remove

Besides the built-in panel, [TrguiNG](https://github.com/transmission/transmission) is recommended
as a more feature-complete Transmission-compatible GUI that works with the same RPC endpoint.

---

## 4. JSON-RPC endpoint

When `rpcport` is non-zero, a JSON-RPC server listens on `rpcaddress:rpcport`.

### URL

```
http://<rpcaddress>:<rpcport>/[<rpcpath>/]rpc
```

Requests are `POST` with `Content-Type: application/json`. If `rpcauth` is enabled, include
`Authorization: Basic <credentials>`.

### Common operations

| Operation | curl example |
|-----------|-------------|
| List torrents | `curl -s -X POST .../rpc -d '{"method":"torrent-get","arguments":{"fields":["id","name","status"]}}'` |
| Start torrent | `curl -s -X POST .../rpc -d '{"method":"torrent-start","arguments":{"ids":[42]}}'` |
| Stop all | `curl -s -X POST .../rpc -d '{"method":"torrent-stop","arguments":{}}'` |
| Remove torrent | `curl -s -X POST .../rpc -d '{"method":"torrent-remove","arguments":{"ids":[42]}}'` |
| Reannounce all | `curl -s -X POST .../rpc -d '{"method":"torrent-reannounce","arguments":{}}'` |
| Session stats | `curl -s -X POST .../rpc -d '{"method":"session-stats","arguments":{}}'` |
| DHT stats | `curl -s -X POST .../rpc -d '{"method":"dht-stats","arguments":{}}'` |

Full parameter and response reference: [Torrents JSON-RPC](/tutorials/Filesharing/I2PD Built-in Torrent/torrents-rpc/).

