i2pd-specific changes to I2P protocols
======================================

# SAM
SESSION CREATE and DEST GENERATE support additional parameter `CRYPTO_TYPE` specifying crypto type for new local destination  

new values for `SIGNATURE_TYPE`:
- `GOST_GOSTR3411256_GOSTR3410CRYPTOPROA` or `9`
- `GOST_GOSTR3411512_GOSTR3410TC26A512` or `10`

# I2PControl
`i2p.router.status` is 1 if shared local destination is ready, and 0 if not  
`i2p.router.net.tunnels.successrate` returns tunnel creation success rate in percents  
`i2p.router.net.total.received.bytes` returns total received bytes since last restart  
`i2p.router.net.total.sent.bytes` returns total sent bytes since last restart  
# BOB
Unlike Java-I2P, i2pd keep supporting BOB with the following extensions:

## Session Management

- zap — Terminates the BOB service entirely, closing all active sessions and stopping the service from accepting new connections.
- quit — Ends the current user session with BOB without shutting down the service itself.

## Tunnel Control

- start — Initiates the tunnel associated with the current nickname.
- stop — Stops the tunnel associated with the current nickname.
- status `<NICKNAME>` — Displays the status of the tunnel identified by the specified nickname.
- list — Lists all configured tunnels.
- clear — Removes the current nickname from the list of configured tunnels.

## Nickname and Key Management

- setnick `<NICKNAME>` — Creates a new nickname for a tunnel.
- getnick `<TUNNELNAME>` — Sets the current nickname to the one associated with the specified tunnel name.
- newkeys `[signaturetype]` — Generates a new key pair (public and private keys) for the current nickname.
By default, this uses the DSA signature type and ElGamal encryption.
To generate keys with a specific signature type (e.g., EdDSA), specify the desired type:
For example, newkeys 7 generates an EdDSA key pair (supported only in i2pd).
- getkeys — Retrieves the current key pair for the current nickname.
- setkeys `<BASE64_KEYPAIR>` — Sets the key pair for the current nickname using the provided BASE64-encoded key pair.
- getdest — Returns the destination for the current nickname.

## Tunnel Configuration

- outhost `<HOSTNAME|IP>` — Sets the outbound hostname or IP address for the tunnel.
- outport `<PORT_NUMBER>` — Sets the outbound port number that the tunnel will contact.
- inhost `<HOSTNAME|IP>` — Sets the inbound hostname or IP address for the tunnel.
- inport `<PORT_NUMBER>` — Sets the inbound port number that the tunnel will listen on.
- quiet `<True|False>` — Determines whether to send the incoming destination information.
- option `<KEY>=<VALUE>` — Sets an option for the current tunnel. Note: Do not use spaces in the key or value.
- settunneltype `<socks|httpproxy>` — Sets the tunnel type to either SOCKS or HTTP proxy.

## Additional Commands

- lookup `<I2P_HOSTNAME>` — Performs a lookup for the specified I2P hostname and returns its destination.
- lookuplocal `<I2P_HOSTNAME>` — looks for LeaseSet with specified address in router's netdb
- ping `<I2P_HOSTNAME>` - Pings remote destination. Returns "pong `<BASE32_ADDRESS>`: time=`<VALUE` ms" if success and "timeout" is not
- help `<COMMAND>` — Provides help information for the specified command. If no command is specified, lists all available commands.
