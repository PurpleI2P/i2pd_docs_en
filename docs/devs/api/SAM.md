SAM interface
=============

The SAM V3 commands and parameters **actually implemented** in i2pd, grouped by SAM protocol version.

## Line Endings

i2pd expects **`\n` only** as line terminators. 


## Supported Protocol Versions

| SAM Version | i2pd Status |
|-------------|-------------|
| 3.0 | Implemented |
| 3.1 | Implemented |
| 3.2 | Partially (PING/PONG only; port/protocol/header options not parsed) |
| 3.3 | Partially (SESSION ADD/REMOVE only; datagram send options not parsed) |

---

## SAM 3.0 — Implemented

### Connection Handshake

#### HELLO VERSION

Negotiate the SAM protocol version before any other command.

```
-> HELLO VERSION [MIN=3.x] [MAX=3.x]
<- HELLO REPLY RESULT=OK VERSION=3.3
```

- `MIN` and `MAX` are optional (SAM 3.1+).
- i2pd returns the highest supported version within the requested range.
- If no suitable version exists: `RESULT=NOVERSION`.
- On error: `RESULT=I2P_ERROR MESSAGE="..."`.

### Sessions

#### SESSION CREATE

Create a SAM session bound to a destination.

```
-> SESSION CREATE STYLE={STREAM,DATAGGRAM,DATAGRAM1,DATAGRAM2,DATAGRAM3,RAW}
     ID=<nickname>
     DESTINATION=<base64 private key>|TRANSIENT
     [PORT=<port>]                   (DATAGRAM*/RAW only)
     [HOST=<host>]                   (DATAGRAM*/RAW only)
     [option=value]*                 (I2CP/streaming options passed through)
<- SESSION STATUS RESULT=OK DESTINATION=<base64 private key>
```

Possible error results:

- `RESULT=DUPLICATED_ID` — session nickname already in use
- `RESULT=DUPLICATED_DEST` — destination already in use (rarely triggered due to refcount sharing)
- `RESULT=INVALID_KEY` — invalid destination key
- `RESULT=I2P_ERROR MESSAGE="..."` — other error

**Behavior notes:**

- Tunnel building happens asynchronously. If the destination is not ready, i2pd polls with a readiness timer and replies when tunnels are established (can take 10–60 seconds).
- **Two sessions can share the same destination** — i2pd reference-counts `ClientDestination` and does not reject duplicate destinations (only duplicate session IDs are rejected).

**Not implemented in SESSION CREATE (3.0 context):** none — all 3.0 parameters are supported.

---

### STREAM CONNECT

Establish a virtual stream to a remote I2P destination.

```
-> STREAM CONNECT ID=<nickname> DESTINATION=<base64 dest|b32|.i2p> [SILENT={true,false}]
<- STREAM STATUS RESULT=OK
<- <remote destination base64>   (if SILENT=false and connection succeeds)
```

- `SILENT=false` (default): sends `STREAM STATUS RESULT=OK`, then sends the remote peer's base64 destination followed by `\n`, then bridges bidirectional data.
- `SILENT=true`: no status message; if connection fails, socket is closed.
- Immediate data can be included after the command line (SAM v3 style).

---

### STREAM ACCEPT

Listen for one incoming stream connection.

```
-> STREAM ACCEPT ID=<nickname> [SILENT={true,false}]
<- STREAM STATUS RESULT=OK
<- <remote destination base64>   (if SILENT=false, when connection arrives)
```

- `SILENT=false` (default): when a connection arrives, sends the remote peer's base64 destination on the socket, then bridges data.
- `ACCEPT` is not allowed while `FORWARD` is active on the same session.

---

### STREAM FORWARD

Forward incoming I2P connections to a local TCP server.

```
-> STREAM FORWARD ID=<nickname> PORT=<port> [HOST=<host>] [SILENT={true,false}]
<- STREAM STATUS RESULT=OK
```

- If `HOST` is omitted, uses the IP of the SAM control socket.
- `SILENT=false` (default): always replies with `STREAM STATUS`; forwards the remote peer's destination on the accepted local socket.
- `FORWARD` is not allowed while `ACCEPT` is pending on the same session.

---

### DATAGRAM SEND / RAW SEND (V1/V2 compatible)

Send repliable or anonymous datagrams through the SAM bridge socket.

```
-> DATAGRAM SEND SIZE=<bytes> DESTINATION=<base64 dest>
     <raw payload bytes>
-> RAW SEND SIZE=<bytes> DESTINATION=<base64 dest>
     <raw payload bytes>
```

- `SIZE` is required; payload follows on the same line after the header.
- No reply is expected (SAM v1/v2 behavior).

---

### UDP Datagram Port (7655)

Repliable and raw datagrams can be sent to i2pd's UDP port 7655. The first line of the datagram must contain the session nickname, destination, and optional port/protocol parameters.

---

### DATAGRAM RECEIVED

Delivered on the control socket of the datagram session (if no forwarding PORT is set in SESSION CREATE).

```
<- DATAGRAM RECEIVED DESTINATION=<base64> SIZE=<numBytes> FROM_PORT=<nnn> TO_PORT=<nnn>
<raw payload bytes>
```

---

### RAW RECEIVED

Delivered on the control socket of the raw session.

```
<- RAW RECEIVED SIZE=<numBytes>
<raw payload bytes>
```

---

### NAMING LOOKUP

Resolve `.i2p` addresses, b32, b33, and `ME` to base64 destination keys.

```
-> NAMING LOOKUP NAME=<name>
<- NAMING REPLY RESULT=OK NAME=<name> VALUE=<base64 dest>
```

- `NAME=ME` returns the local destination's own identity.
- If the address is not in the local address book, i2pd can request the leaseset remotely and reply asynchronously.

Error results:

- `RESULT=INVALID_KEY NAME=<name>` — address not found
- `RESULT=KEY_NOT_FOUND NAME=<name>` — key not found

---

## SAM 3.1 — Implemented

All SAM 3.0 features above, plus:

### DEST GENERATE

Generate a new private key and destination.

```
-> DEST GENERATE [SIGNATURE_TYPE=value] [CRYPTO_TYPE=value]
<- DEST REPLY PUB=<base64 public> PRIV=<base64 private>
```

- `SIGNATURE_TYPE`: name (e.g. `ECDSA_SHA256_P256`) or number (e.g. `7` for Ed25519). Default: `DSA_SHA1`.
- `CRYPTO_TYPE`: encryption key type. Default: `ELGAMAL`.

### HELLO VERSION Enhancements

- `MIN` and `MAX` parameters are now optional.
- Single-digit versions such as `"3"` are supported.

---

## SAM 3.2 — Partially Implemented

All SAM 3.0 and 3.1 features above, plus:

### PING / PONG

```
-> PING <arbitrary text>
<- PONG <arbitrary text>
```

- Echoes the arbitrary text back to the client.

**Not implemented in SAM 3.2:**
- `USER=`/`PASSWORD=` in HELLO (authorization) — accepted but ignored
- `AUTH` command
- `SSL/TLS` on control socket
- `STREAM FORWARD SSL=true`
- `FROM_PORT`/`TO_PORT` on SESSION CREATE
- `PROTOCOL` on SESSION CREATE (RAW only)
- `HEADER=true` on SESSION CREATE (RAW)
- `FROM_PORT`/`TO_PORT` on STREAM CONNECT
- `FROM_PORT`/`TO_PORT` on DATAGRAM SEND / RAW SEND
- `PROTOCOL` on DATAGRAM/RAW SEND
- `RAW RECEIVED` including `FROM_PORT=`/`TO_PORT=`/`PROTOCOL=`

---

## SAM 3.3 — Partially Implemented

All SAM 3.0, 3.1, and 3.2 features above, plus:

### SESSION ADD

Add a subsession to a MASTER session.

```
-> SESSION ADD STYLE={STREAM,DATAGGRAM,DATAGRAM1,DATAGRAM2,DATAGRAM3,RAW} ID=<nickname> ...
<- SESSION STATUS RESULT=OK DESTINATION=<key>
```

Requires `SAM_VERSION_33`.

### SESSION REMOVE

Remove a subsession from a MASTER session.

```
-> SESSION REMOVE ID=<nickname>
<- SESSION STATUS RESULT=OK
```

Requires `SAM_VERSION_33`.

**Not implemented in SAM 3.3:**
- `SEND_TAGS`/`TAG_THRESHOLD`/`EXPIRES`/`SEND_LEASESET` on DATAGRAM/RAW SEND
- PRIMARY session port restrictions (`FROM_PORT`/`TO_PORT`/`PORT`/`HOST`/`PROTOCOL` not forbidden on PRIMARY sessions)

---


For the complete SAM V3 protocol specification, see [here](https://i2p.net/en/docs/api/samv3/)
