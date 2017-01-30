I2P tunnel configuration
========================

Overview
--------

tunnels.conf is designed to support multiple I2P tunnels. Must be located in ``~/.i2pd`` (per-user) or ``/var/lib/i2pd`` (system-wide).

This file uses .ini file format. It consists of multiple sections with unique name each.

Section type is specified by *type* parameter  with possible values *client*, *server* or *http*. Each *client* specifies I2P client tunnel and each *server* specifies I2P server tunnel. *http* is special type of server tunnel for eepsites.

Client tunnels
--------------

Mnemonic: we can connect to someone as client

Must contain few mandatory parameters, some optional parameters might be also presented.

Example of client tunnel:

    [irc-out]
    type = client
    address = 127.0.0.1
    port = 6668
    destination = irc.echelon.i2p
    keys = irc.dat

If *keys* is empty, transient keys will be created on every restart. If keys file is not found, new keys will be created and stored into specified file.
Client tunnels might share same local destination, if keys file contains same identity.

Optional parameters:

* address         -- local interface tunnel binds to, '127.0.0.1' for connections from local host only, '0.0.0.0' for connections from everywhere. '127.0.0.1' by default.
* signaturetype   -- signature type for new keys. 0 (DSA), 1 (ECDSA-P256), 7 (EDDSA). 1 by default
* destinationport -- connect to particular port at destination. 0 by default

So, with example above, if you telnet to 127.0.0.1:6668 on localhost, i2pd will connect to irc.echelon.i2p:6668

Server/generic tunnels
----------------------

Mnemonic: we serving some service to others in network

Example of server tunnel:

    [smtp-in]
    type = server
    host = 127.0.0.1
    port = 25
    keys = smtp-in.dat

*keys* must be presented, LeaseSet of address from keys file will be published. Server tunnel must use its own local destination.

Optional parameters:

* inport        -- what port at local destination server tunnel listens to. Same as *port* by default.
* accesslist    -- list of comma-separated of b32 address (without .b32.i2p) allowed to connect. Everybody is allowed by default.  
* gzip          -- turns internal compression off if set to false. true by default.  
* signaturetype -- means signature type for new keys. 0 - DSA, 1- ECDSA-P256, 7 -EDDSA.  1 by default.  
* enableuniquelocal -- if true, connection to local address will look like 127.x.x.x where x.x.x is first 3 bytes of incoming connection peer's ident hash. true by default.  

Server/http tunnels
-------------------

*http* tunnel works same way as server tunnel, but replace 'Host:' field in HTTP header to address provided in configuration.
Also resolves it if necessary.

Example of http tunnel:

    [http-in]
    type = http
    host = ourwebsite.com
    port = 80
    keys = our-website.dat

Optional parameters:

* hostoverride -- value to send in 'Host:' header, default: the same as *host* parameter
* gzip         -- should we compress contents at i2p level. default: true

Server/IRC tunnels
-------------------

IRC tunnels are supposed to connect to an IRC server through WEBIRC.  
It replaces IP address (usually 127.0.0.1) to user's .b32 I2P address.  

Optional parameters:

* webircpassword -- password to send with WEBIRC command

UDP Tunnels
-----------

There are 2 types of UDP tunnels: `udpclient` and `udpserver`


`udpclient` forwards 1 local udp endpoint to 1 remote i2p destination


    [openvpn-client-simple]
    type = udpclient
    destination = something.b32.i2p
    port = 1194


* destination -- the i2p destination of a udpserver tunnel, required parameter
* address -- ip address to bind local udp endpoint to, defaults to `127.0.0.1`
* port -- port to bind local udp endpoint to, required parameter

`udpserver` forwards traffic from N i2p destinations to 1 local udp endpoint

    [openvpn-simple-server]
    type = udpserver
    keys = openvpn.dat
    port = 1194

* address -- ip address to use for local udp endpoints, defaults to `127.0.0.1`
* host -- ip address to forward traffic to, defaults to `127.0.0.1`
* port -- udp port to forward traffic on, required parameter


Socks proxy
-----------

Socks proxy interface can be defined in ``tunnels.conf``.

Example of Socks proxy:

    [alt-socks]
    type = socks
    address = 127.0.0.1
    port = 14447
    keys = socks-keys.dat 

* address -- local address Socks proxy binds to, defaults to `127.0.0.1`
* port -- TCP port Socks proxy binds to


I2CP parameters
---------------

I2CP parameter are common for all tunnel types and specify setting for a local destination.

* inbound.length    -- number of hops of an inbound tunnel. 3 by default;  lower value is faster but dangerous
* outbound.length   -- number of hops of an outbound tunnel. 3 by default; lower value is faster but dangerous
* inbound.quantity  -- number of inbound tunnels.  5 by default
* outbound.quantity -- number of outbound tunnels. 5 by default
* crypto.tagsToSend -- number of ElGamal/AES tags to send. 40 by default; too low value may cause problems with tunnel building
* explicitPeers     -- list of comma-separated b64 addresses of peers to use, default: unset


Other examples
--------------

    # outgoing tunnel sample, to remote service
    # mandatory parameters:
    # * type -- always "client"
    # * port -- local port to listen to
    # * destination -- i2p hostname
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
    # * host -- ip address of our service
    # * port -- port of our service
    # * keys -- file with LeaseSet of address in i2p
    # optional parameters (may be omitted)
    # * inport -- optional, i2p service port, if unset - the same as 'port'
    # * accesslist -- comma-separated list of i2p addresses, allowed to connect
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

