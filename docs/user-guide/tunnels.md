I2P tunnel configuration
========================

Overview
--------

`tunnels.conf` is designed to support multiple I2P tunnels. The configuration file must be located in 
``~/.i2pd`` (per-user) or ``/var/lib/i2pd`` (system-wide).

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
websocks      | WebSocket interface to use I2P with


Client tunnels
--------------

Mnemonic: we can connect to someone as client

Each client tunnel must contain a few mandatory parameters, along with some optional ones.

Here is an example of a client tunnel:

    [irc-out]
    type = client
    address = 127.0.0.1
    port = 6668
    destination = irc.echelon.i2p
    keys = irc.dat

If *keys* is empty, transient keys will be created on every restart. If the keys file is not found, new keys will be created and stored into the specified file. 
If *keys* is set to *transient*, new keys will be created, but not stored into a file.  
Client tunnels might share the same local destination, if the keys file contains the same identity.

Optional parameters:

* address         -- local interface tunnel binds to, '127.0.0.1' for connections from local host only, '0.0.0.0' for connections from everywhere. '127.0.0.1' by default.
* signaturetype   -- signature type for new keys. 0 (DSA), 1 (ECDSA-P256), 7 (EDDSA), 11 (RedDSA). RSA signatures (4,5,6) are not allowed and will be changed to 7. 7 by default
* cryptotype       -- crypto type for new keys. Experimental. Should be always 0
* destinationport -- connect to particular port at destination. 0 by default

So, given the example above, if you connected to to 127.0.0.1:6668 on localhost, i2pd would tunnel that to irc.echelon.i2p:6668

Server/generic tunnels
----------------------

Mnemonic: we serving some service to others in network

Here is an example of a server tunnel:

    [smtp-in]
    type = server
    host = 127.0.0.1
    port = 25
    keys = smtp-in.dat

The file in *keys* must be present, and the LeaseSet of address from keys file will be published. 
The server tunnel must use its own local destination such as host 127.0.0.1 and port 80.

Optional parameters:

* inport        -- what port at local destination server tunnel listens to. Same as *port* by default.
* accesslist    -- list of comma-separated of b32 address (without .b32.i2p) allowed to connect. Everybody is allowed by default.  
* gzip          -- turns internal compression off if set to false. true by default.  
* signaturetype -- means signature type for new keys. 0 - DSA, 1- ECDSA-P256, 7 -EDDSA, 11 -RedDSA.  7 by default.  
* cryptotype     -- crypto type for new keys. Experimental. Should be always 0.
* enableuniquelocal -- if true, connection to local address will look like 127.x.x.x where x.x.x is first 3 bytes of incoming connection peer's ident hash. true by default.  

Server/http tunnels
-------------------

*http* tunnels are configured just like regular server tunnels, except the 'Host:' field
must be assigned to the address provided in configuration. i2pd will also resolve it if necessary.

Here's an example of an http tunnel:

    [http-in]
    type = http
    host = ourwebsite.com
    port = 80
    keys = our-website.dat

Optional parameters:

* hostoverride -- value to send in 'Host:' header, default: the same as *host* parameter
* gzip         -- should we compress contents at I2P level. default: true

Server/IRC tunnels
-------------------

IRC tunnels are supposed to connect to an IRC server through WEBIRC.  
It replaces IP address (usually 127.0.0.1) to user's .b32 I2P address.  

Optional parameters:

* webircpassword -- password to send with WEBIRC command

UDP Tunnels
-----------

There are 2 types of UDP tunnels: `udpclient` and `udpserver`


`udpclient` forwards 1 local UDP endpoint to 1 remote I2P destination


    [openvpn-client-simple]
    type = udpclient
    destination = something.b32.i2p
    port = 1194


* destination -- the I2P destination of a udpserver tunnel, required parameter
* address -- IP address to bind local UDP endpoint to, defaults to `127.0.0.1`
* port -- port to bind local UDP endpoint to, required parameter

`udpserver` forwards traffic from N I2P destinations to 1 local UDP endpoint

    [openvpn-simple-server]
    type = udpserver
    keys = openvpn.dat
    port = 1194

* address -- IP address to use for local UDP endpoints, defaults to `127.0.0.1`
* host -- IP address to forward traffic to, defaults to `127.0.0.1`
* port -- UDP port to forward traffic on, required parameter


Socks proxy
-----------

The SOCKS proxy interface can be defined in ``tunnels.conf``.

Here's an example of a Socks proxy:

    [alt-socks]
    type = socks
    address = 127.0.0.1
    port = 14447
    keys = socks-keys.dat 

* address -- local address Socks proxy binds to, defaults to `127.0.0.1`
* port -- TCP port Socks proxy binds to


I2CP parameters
---------------

These I2CP parameter are common for all tunnel types and specify settings for a local destination.

* inbound.length    -- number of hops of an inbound tunnel. 3 by default;  lower value is faster but dangerous
* outbound.length   -- number of hops of an outbound tunnel. 3 by default; lower value is faster but dangerous
* inbound.quantity  -- number of inbound tunnels.  5 by default
* outbound.quantity -- number of outbound tunnels. 5 by default
* crypto.tagsToSend -- number of ElGamal/AES tags to send. 40 by default; too low value may cause problems with tunnel building
* explicitPeers     -- list of comma-separated b64 addresses of peers to use, default: unset
* i2p.streaming.initialAckDelay -- milliseconds to wait before sending Ack. 200 by default
* i2cp.leaseSetType -- type of LeaseSet to be sent. 1, 3 or 5. 1 by default
* i2cp.leaseSetEncType -- comma separated encryption types to be used in LeaseSet type 3 or 5. Identity's type by default
* i2cp.leaseSetPrivKey -- decryption key for encrypted LeaseSet in base64. PSK or private DH  
* i2cp.leaseSetAuthType -- authentication type for encrypted LeaseSet. 0 - no authentication(default), 1 - DH, 2 - PSK  
* i2cp.leaseSetClient.dh.nnn -- client name:client's public DH in base64, for authentication type 1, nnn is integer  
* i2cp.leaseSetClient.psk.nnn -- client name:client's PSK in base64, for authentication type 2, nnn is integer 

Other examples
--------------

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
    destination = irc.postman.i2p
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

