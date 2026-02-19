Glossary
========

`Addressbook`  
An internal component in i2pd that manages service destinations for the I2P network, mapping Base64 hashes to human-readable names (e.g., .i2p domains). Destinations can be added manually or via subscriptions to hosts.txt files.

`Base32 (B32) / Base64 (B64)`  
A Base32 address (which always ends with .b32.i2p) is a hash of a Base64 destination. For services on the I2P network, the Base64 hash is often mapped to an .i2p domain in the addressbook.

`BOB`  
BOB (Basic Open Bridge) is a simple application-to-router protocol, supported in i2pd but largely superseded by SAM. For more information.

`Destination`  
The unique cryptographic identity of the inbound endpoint of a tunnel providing access to a service on the I2P network, represented as a Base32 or Base64 hash of the public key. Equivalent to an IP address and port.

`Eepsite`  
A website hosted on the I2P network. In i2pd, you can host one by configuring an HTTP server tunnel in tunnels.conf and pointing it to your local web server.

`Exploratory Tunnel`  
A tunnel used by the i2pd router for communicating with other routers to perform various housekeeping functions, such as learning and publishing LeaseSets, testing client tunnels, and learning about other routers in the network. These tunnels are normally low-bandwidth.

`Floodfill`  
A router on the I2P network tasked with providing and receiving information about other routers. Routers with sufficient bandwidth (at least 128 KB/s share) can act as floodfills; you can force this in i2pd.conf if needed.

`Garlic Routing`  
A variant of onion routing that encrypts multiple messages together to make it more difficult for attackers to perform traffic analysis and to increase the speed of data transfer.

`I2CP`  
The I2P Client Protocol (I2CP) enables external applications (clients) to communicate with the i2p router over a single TCP socket, by default on port 7654.

`I2NP`  
The I2P Network Protocol (I2NP) manages the routing and mixing of messages between routers, in addition to transport selection when communicating with peers.

`Lease`  
The information that defines the authorization for a particular tunnel to receive messages targeting a Destination.

`LeaseSet`  
A group of tunnel entry points (Leases) for a Destination. Note: A 0-hop server tunnel will only have one Lease, regardless of the number of tunnels configured. i2pd supports advanced LeaseSet types like encrypted LeaseSet2.

`Multihoming`  
Services may be hosted on multiple routers simultaneously by sharing the same private key for the Destination. This enhances security and stability by providing redundancy; if one router goes offline, the service remains available.

`Network Database`  
A distributed database containing router contact information (RouterInfos) and Destination contact information (LeaseSets). Each i2p router maintains a partial database (NetDb) for communicating with others. Stored on disk and loaded into memory on startup.

`NTCP / NTCP2`  
NTCP (NIO-based TCP) and NTCP2 are TCP-based transports used by i2p to deliver I2NP messages between routers. NTCP2 improves resistance to attacks and traffic identification. Enabled by default in i2p.

`Outproxy`  
A service on the I2P network that provides a proxy connection to the clearnet. By default, i2pd configures an HTTP outproxy on 127.0.0.1:4444 for web browsing.

`Participation`  
The act of contributing to the I2P network by allowing other routers to build tunnels through your i2pd router. Requires at least 12 KB/s upstream share bandwidth. Firewalled routers have limited participation.

`Reseeding`  
The process of acquiring router identities, usually from clearnet servers, to ensure your i2p router can connect to the I2P network. Essential for bootstrapping a new installation.

`Router`  
The core i2pd software, routes encrypted packets on the I2P network. By default, i2pd routers participate in the network to help the network and provide cover traffic.

`Router Identity`  
Information defining the unique identity of a router on the I2P network, including its IP address (or introducers), listening port, and public keys. Also called RouterInfo; correlates to peers in the NetDb.

`SAM`  
SAM (Simple Anonymous Messaging) is a protocol allowing client applications in any language to communicate over I2P via a socket interface to the i2pd router.

`SSU / SSU2`  
SSU (Secure Semi-reliable UDP) is an I2P transport in i2pd providing encrypted, connection-oriented communications, plus IP detection and NAT traversal. Enabled by default.

`Tunnel`  
A unidirectional encrypted communication pathway between a client or server on the I2P network. Similar to a Tor circuit but unidirectional.

`Tunnel Endpoint`  
The last router in a tunnel: either the Outbound Endpoint (where client tunnels meet the destination's Inbound Gateway) or the Inbound Endpoint (connecting to the destination server).

`Tunnel Gateway`  
The first router in a tunnel. For inbound tunnels, this is listed in the LeaseSet in the NetDb. For outbound, it's the originating router.

`Transit Tunnels`  
They are the tunnels that your router participates in on behalf of other users. Transit is what makes I2P truly decentralized.

`Family`  
A family is a group of routers operated by the same entity or individual. [Read more](https://i2p.net/en/docs/overview/network-database/#family-options).

`DHT (Distributed Hash Table) `  
Used in some projects to connect peers to each other by storing information in the form of key-value pairs in a distributed manner.

`Client Tunnels`   
Tunnels that are outbound TCP tunnels that allow local applications to connect to remote services within the I2P network. They forward traffic from a specified local port to a remote I2P destination (typically a .b32.i2p address).

`Router Caps`    
Router Capabilities are a set of short flags published in your router's [RouterInfo](https://i2p.net/en/docs/overview/network-database/#routerinfo) entry within the network database (netDb).

`Network Status`    
It shows your router's current integration and reachability within the I2P network. **OK** - Your router is fully integrated. **Firewalled** - Your router can't receive direct inbound connections, It still works as a client but relies on introducers and outbound-only path. **Testing** - Temporary state during startup or reachability probes. [Rread more](https://i2p.medium.com/i2p-for-beginners-java-software-guide-685d7a7ed57b#ac59).

`Exploratory tunnels`    
Used by your own router for network discovery, netDb lookups, and tunnel building (not for user data).