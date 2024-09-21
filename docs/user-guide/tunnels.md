I2P tunnel configuration
========================

Overview
--------

`tunnels.conf` is designed to support multiple I2P tunnels. The configuration file must be located in 
``~/.i2pd`` (per-user) or ``/var/lib/i2pd`` (system-wide) on Unix-based systems, and ``%APPDATA%/i2pd`` (per-user) on Windows.

This file uses the .ini file format. It consists of multiple sections each with a unique name.

Tunnel types
------------

Section type is specified by the *type* parameter.

Available tunnel types:

Type          | Description
------------- | --------------------------------------
client        | Client tunnel to remote I2P destination (TCP)
server        | Generic server tunnel to setup any TCP service in I2P network
http          | HTTP server tunnel to setup a website in I2P
irc           | IRC server tunnel to setup IRC server in I2P
udpclient     | Forwards local UDP endpoint to remote I2P destination
udpserver     | Forwards traffic from N I2P destinations to local UDP endpoint
socks         | Custom Socks proxy service to use I2P with
httpproxy     | Custom HTTP proxy service to use I2P with

Signature types
------------

Parameter `signaturetype = <code>` in a tunnel config.

Available signature types:

Type                                 | Code | Comment
------------------------------------ | ---- | -----------
DSA-SHA1                             | 0    | Deprecated
ECDSA-P256                           | 1    | *None, actively used*
ECDSA-P384                           | 2    | *None, actively used*
ECDSA-P521                           | 3    | *None, actively used*
RSA-2048-SHA256                      | 4    | Deprecated
RSA-3072-SHA384                      | 5    | Deprecated
RSA-4096-SHA512                      | 6    | Deprecated
ED25519-SHA512                       | 7    | **Default**
*ED25519ph-SHA512*                   | 8    | Not implemented
GOSTR3410-A-GOSTR3411-256            | 9    | Not compatible with Java router
GOSTR3410-TC26-A-GOSTR3411-512       | 10   | Not compatible with Java router
RED25519-SHA512                      | 11   | For keys blinding (encrypted LeaseSet)

LeaseSet
------------

Available LeaseSet **types** (parameter `i2cp.leaseSetType = <code>` in a tunnel config):

Type        | Code | Comment
----------- | ---- | -----------
OLD         | 1    | Deprecated
STANDARD    | 3    | **Default**
ENCRYPTED   | 5    | Encrypted LeaseSet. Hiding information from floodfill
META        | 7    | Not implemented

*0, 2, 4, 6 types are reserved for routers (RouterInfo types).*

Available LeaseSet **encryption** types (parameter `i2cp.leaseSetEncType = <code>` in a tunnel config):

Type                                 | Code | Comment
------------------------------------ | ---- | -----------
ELGAMAL                              | 0    | **Default** (only for support old routers)
ECIES_P256_SHA256_AES256CBC          | 1    | Not compatible with Java router
*ECIES_P384_SHA384_AES256CBC*        | 2    | Not implemented
*ECIES_P521_SHA512_AES256CBC*        | 3    | Not implemented
ECIES_X25519_AEAD                    | 4    | **Default**

Client tunnels
--------------

Mnemonic: we can connect to someone as client

Each client tunnel must contain a few mandatory parameters, along with some optional ones.

Here is an example of a client tunnel:

```ini
[irc-out]
type = client
address = 127.0.0.1
port = 6668
destination = irc.ilita.i2p
keys = irc.dat
```

If *keys* is empty, transient keys will be created on every restart. If the keys file is not found, new keys will be created and stored into the specified file.
If *keys* starts from *transient*, new keys will be created, but not stored into a file.

Client tunnels might share the same local destination, if the keys file contains the same identity.

Optional parameters:

Option              | Description
--------------------|--------------------
address             | Local interface tunnel binds to, '127.0.0.1' for connections from local host only, '0.0.0.0' for connections from everywhere. (default: 127.0.0.1)
port                | Port of client tunnel.
signaturetype       | Signature type for new keys. RSA signatures (4,5,6) are not allowed and will be changed to 7. (default: 7)
cryptotype          | Crypto type for new keys. Experimental. Should be always 0
destinationport     | Connect to particular port at destination. 0 by default (targeting first tunnel on server side for destination)
keepaliveinterval   | Send ping to the destination after this interval in seconds. (default: 0 - no pings)
keys                | Keys for destination. When same for several tunnels, will be using same destination for every tunnel.

So, given the example above, if you connected to 127.0.0.1:6668 on localhost, i2pd would tunnel that connection to irc.ilita.i2p.

Server/generic tunnels
----------------------

Mnemonic: we serving some service to others in network

Here is an example of a server tunnel:

```ini
[smtp-in]
type = server
host = 127.0.0.1
port = 25
keys = smtp-in.dat
```

If *keys* is empty, transient keys will be created on every restart. If the *keys* file is not found, new keys will be created and stored into the specified file.

Destination address from *keys* file will be loaded and the LeaseSet of address will be published.
The server tunnel must use its own destination such as host 127.0.0.1 and port 80.

The *port* is (non-I2P) TCP listening port on IP host that the listening local destination gets connected to.

This tunnel type should be used for any protocol other than HTTP, even HTTP with SSL encryption (HTTPS).

Optional parameters:

Option              | Description
--------------------|--------------------
host                | IP address of server (on this address i2pd will send data from I2P)
port                | Port of server tunnel.
inport              | (non-TCP non-UDP) I2P local destination port to listen to; an unsigned 16-bit integer. What port at local destination server tunnel listens to (default: same as *port*)
accesslist          | List of comma-separated of b32 address (without .b32.i2p) allowed to connect. Everybody is allowed by default
gzip                | Turns internal compression off if set to false. (default: false)
signaturetype       | Signature type for new keys. (default: 7)
cryptotype          | Crypto type for new keys. Experimental. Should be always 0
enableuniquelocal   | If true, connection to local address will look like 127.x.x.x where x.x.x is first 3 bytes of incoming connection peer's ident hash. (default: true)
address             | IP address of an interface tunnel is connected to *host* from. Usually not used
keys                | Keys for destination. When same for several tunnels, will be using same destination for every tunnel.

Server/http tunnels
-------------------

*http* tunnels are configured just like regular server tunnels, except the 'Host:' field
must be assigned to the address provided in configuration. i2pd will also resolve it if necessary.

Here's an example of an http tunnel:

```ini
[http-in]
type = http
host = 127.0.0.1  
port = 80
keys = our-website.dat
```

Optional parameters:

Option              | Description
--------------------|--------------------
hostoverride        | Value to send in 'Host:' header, default: the same as *host* parameter
ssl                 | Use SSL connection to upstream server. `hostoverride` parameter can be used to set SNI domain. default: false (since 2.44.0)

Server/IRC tunnels
-------------------

IRC tunnels are supposed to connect to an IRC server through WEBIRC. It replaces IP address (usually 127.0.0.1) to user's .b32 I2P address.

Optional parameters:

Option              | Description
--------------------|--------------------
webircpassword      | Password to send with WEBIRC command

UDP Tunnels
-----------

There are 2 types of UDP tunnels: `udpclient` and `udpserver`


`udpclient` forwards 1 local UDP endpoint to 1 remote I2P destination

```ini
[openvpn-client-simple]
type = udpclient
destination = something.b32.i2p
port = 1194
```

Option              | Description
--------------------|--------------------
destination         | The I2P destination of a udpserver tunnel, required parameter
address             | IP address to bind local UDP endpoint to (default: `127.0.0.1`)
port                | Port to bind local UDP endpoint to, required parameter
gzip                | Turns internal compression off if set to false. (default: false)
keys                | Keys for destination. When same for several tunnels, will be using same destination for every tunnel.

`udpserver` forwards traffic from N I2P destinations to 1 local UDP endpoint

```ini
[openvpn-simple-server]
type = udpserver
keys = openvpn.dat
host = 127.0.0.1
port = 1194
```

Option              | Description
--------------------|--------------------
address             | IP address to use for local UDP endpoints (default: `127.0.0.1`)
host                | IP address to forward traffic to, required parameter
port                | UDP port to forward traffic on, required parameter
gzip                | Turns internal compression off if set to false. (default: false)
keys                | Keys for destination. When same for several tunnels, will be using same destination for every tunnel.

Socks proxy
-----------

The SOCKS proxy interface can be defined in ``tunnels.conf``.

Here's an example of a Socks proxy:

```ini
[alt-socks]
type = socks
address = 127.0.0.1
port = 14447
keys = socks-keys.dat 
```

Option              | Description
--------------------|--------------------
address             | Local address Socks proxy binds to (default: `127.0.0.1`)
port                | TCP port Socks proxy binds to

I2CP parameters
---------------

These I2CP parameter are common for all tunnel types and specify settings for a local destination.

Parameter                     | Description
------------------------------|--------------------
inbound.length                | Number of hops of an inbound tunnel. 3 by default, 8 by max;  lower value is faster but have more deanonimize risks
outbound.length               | Number of hops of an outbound tunnel. 3 by default, 8 by max; lower value is faster but have more deanonimize risks
inbound.quantity              | Number of inbound tunnels. 5 by default, 16 by max
outbound.quantity             | Number of outbound tunnels. 5 by default, 16 by max
inbound.lengthVariance        | Random number of hops to add or subtract to an inbound tunnel between -3 and 3. 0 by default **(since 2.42.0)**
outbound.lengthVariance       | Random number of hops to add or subtract to an outbound tunnel between -3 and 3. 0 by default **(since 2.42.0)**
crypto.tagsToSend             | Number of ElGamal/AES tags to send. 40 by default; too low value may cause problems with tunnel building
crypto.ratchet.inboundTags    | None for now
explicitPeers                 | List of comma-separated b64 addresses of peers to use (default: unset)
i2p.streaming.initialAckDelay | Milliseconds to wait before sending Ack. (default: 200)
i2p.streaming.answerPings     | Enable sending pongs. true by default
i2p.streaming.maxOutboundSpeed| Max outbound speed of stream in bytes/sec. (default: 1730000000)
i2p.streaming.maxInboundSpeed | Max inbound speed of stream in bytes/sec. (default: 1730000000)
i2p.streaming.profile         | Bandwidth usage profile. 1 - bulk(high), 2- interactive(low). (default: 1)
i2cp.leaseSetType             | Type of LeaseSet to be sent. 1, 3 or 5. (default: 3)
i2cp.leaseSetEncType          | Comma separated encryption types to be used in LeaseSet type 3 or 5. (default: 0,4)
i2cp.leaseSetPrivKey          | Decryption key for encrypted LeaseSet in base64. PSK or private DH
i2cp.leaseSetAuthType         | Authentication type for encrypted LeaseSet. 0 - no authentication(default), 1 - DH, 2 - PSK
i2cp.leaseSetClient.dh.nnn    | Client name:client's public DH in base64, for authentication type 1, nnn is integer
i2cp.leaseSetClient.psk.nnn   | Client name:client's PSK in base64, for authentication type 2, nnn is integer
i2cp.dontPublishLeaseSet      | Don't publish LeaseSet if set to true. (default: false)

Other examples
--------------

```ini
# outgoing tunnel sample, to remote service
# mandatory parameters:
# * type -- always "client"
# * port -- local port to listen to
# * destination -- I2P hostname
# optional parameters (may be omitted)
# * keys -- our identity, if unset, will be generated on every startup,
#     if set and file missing, keys will be generated and placed to this file
# * address -- local interface to bind
# * signaturetype -- signature type for new destination. 0 (DSA/SHA1), 1 (EcDSA/SHA256) or 7 (EdDSA/SHA512)
[IRC]
type = client
address = 127.0.0.1
port = 6668
destination = irc.ilita.i2p
keys = irc-keys.dat
#
# incoming tunnel sample, for local service
# mandatory parameters:
# * type -- "server" or "http"
# * host -- IP address of our service
# * port -- port of our service
# * keys -- file with LeaseSet of address in i2p
# optional parameters (may be omitted)
# * inport -- optional, I2P service port, if unset - the same as 'port'
# * accesslist -- comma-separated list of I2P addresses, allowed to connect
#    every address is b32 without '.b32.i2p' part
[LOCALSITE]
type = http
host = 127.0.0.1
port = 80
keys = site-keys.dat
#
[IRC-SERVER]
type = server
host = 127.0.0.1
port = 6667
keys = irc.dat
```
