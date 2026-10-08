# I2PD Built-in Torrent Client

> **Requirements:** compiled in by default unless built with `NO_TORRENTS`. The JSON-RPC interface additionally requires Boost ≥ 1.81 and is not available on Android.

## 1. Configuration

Enable `javascript` in i2pd.conf:
```ini
[http]    
javascript = true
```

then add a `torrents` tunnel section to `tunnels.conf`:

```ini
[torrents]
type = torrents
torrentsdir = /path/to/torrents
keys = torrents-keys.dat
trackers = http://atia.i2p/announce,udp://opentracker.dg2.i2p:6969,udp://tracker.insulaocculta.i2p:6969,http://opentracker.skank.i2p/a
dht = true
signaturetype = 7
rpcport = 7652
rpcaddress = 127.0.0.1
```

### Parameters

| Parameter         | Description 
|-------------------|-----------------------
| `torrentsdir`     | Directory for downloads. `.torrent` files placed here are loaded automatically at startup; downloads, `.part` and `.resume` files are written here. The directory must already exist.
| `keys`            | Destination key file. `transient` by default — set a persistent filename so your peer identity survives restarts.
| `trackers`        | Comma-separated I2P tracker announce URLs.
| `dht`             | Enable the built-in DHT. Default `true`.
| `signaturetype`   | Defaults to Ed25519 (7).
| `rpcport`         | no Port for the Transmission-compatible JSON-RPC server.
| `rpcaddress`      | Bind address for the RPC server (default `127.0.0.1`).
| `rpcpath`         | URL path prefix for the RPC endpoint.



## 2. Adding torrents

Three ways:

1. **Webconsole UI** — open the torrent panel in the i2pd [webconsole](http://127.0.0.1:7070/?page=i2p_tunnels) (`http://127.0.0.1:7070/?page=i2p_tunnels`), then either paste a magnet link into the input and click "download", or click **Add torrent** and pick a `.torrent` file (it's base64-encoded in the browser and sent to the RPC endpoint). 
2. **Drop `.torrent` files into `torrentsdir`** — they are picked up automatically at startup. Only files with the `.torrent` extension in the top level of that directory are scanned.
3. **Via RPC directly** — `torrent-add` with a base64-encoded `.torrent` in `metainfo`, or a magnet link in `filename` (e.g., via `transmission-remote 127.0.0.1:7652 -a "magnet:..."`).


## 3. The RPC endpoint

The RPC server exposes a Transmission-compatible JSON-RPC API. The endpoint URL is:

```
http://<rpcaddress>:<rpcport>/[<rpcpath>/]rpc
```

- Default (`rpcpath` unset): `http://127.0.0.1:7652/rpc`
- With `rpcpath = torrents`: `http://127.0.0.1:7652/torrents/rpc`

`rpcpath` exists so multiple `type = torrents` tunnels can share one RPC server, each mounted under its own path.

Requests must be `POST` with `Content-Type: application/json` (or `application/x-www-form-urlencoded`). `OPTIONS` preflight is handled; CORS is open (`Access-Control-Allow-Origin: *`), and **there is no authentication** — do not bind `rpcaddress` to anything beyond localhost unless you understand the risk.

### Supported RPC methods

| Method            | Notes 
|-------------------|-----------
| `torrent-add`     | `metainfo` (base64 `.torrent`) or `filename` (magnet link). Returns `torrent-added` / `torrent-duplicate`.
| `torrent-remove`  | Supports `delete-local-data` to also delete files.
| `torrent-get`     | Supports `ids`, `fields`, and `format: "table"`.
| `torrent-start` / `torrent-stop` | By `ids`.
| `torrent-set`     | Adds trackers via `trackerAdd`.
| `session-get`     | Reports `rpc-version` 17 for client compatibility.
| `session-stats` |  Aggregate byte/rate counters and torrent counts.


