Encrypted LeaseSet2 Configuration
=================================

[Encrypted LeaseSet](https://i2p.net/en/docs/specs/encryptedleaseset/) is a blinded and encrypted variant of [LeaseSets2 (LS2)](https://i2p.net/en/docs/specs/common-structures/#leaseset2), it hides information from floodfill.

Encrypted LeaseSets are supported only for server tunnels (types: `server`, `http`, `irc`). Client tunnels cannot publish them.

### To enable Encrypted LeaseSet2 in i2pd:

- Signature type must be Red25519 (`signatureType=11`)
- Set the I2CP parameter `i2cp.leaseSetType=5` in the tunnel section.

Once the tunnel starts (or is reloaded), i2pd automatically generates blinded keys daily, encrypts the LeaseSet, and publishes it as an Encrypted LeaseSet2.

### Example (No Authentication)

Add this to your `~/.i2pd/tunnels.conf`:

```
[MyEncryptedService]
type = http    
host = 127.0.0.1
port = 8080              # local port your web server listens on
keys = encrypted.dat     # persistent key file (will be created if missing)
signatureType = 11
i2cp.leaseSetType = 5
```

Save and reload tunnels:  
Visit http://127.0.0.1:7070/?page=commands then reload tunnels config (or restart i2pd). i2pd will now publish an Encrypted LeaseSet2.

### Adding Client Authentication

base parameters:

```
i2cp.leaseSetAuthType = 0     # 0 = no auth (default), 1 = DH, 2 = PSK
i2cp.leaseSetPrivKey = ...    # (client side only) your private key/base64 PSK
```

Option 1: DH Authentication

1. Generate [X25519](https://github.com/PurpleI2P/i2pd-tools/blob/master/x25519.cpp) keypair for each client (use `x25519` tool from [i2pd-tools](https://github.com/PurpleI2P/i2pd-tools/blob/master/x25519.cpp)):

   ```
   x25519
   ```

   E.g output:
   ```
   PublicKey:  Ihr8wZS8fuA5Q-Exwb-9tGHpyV6lZfFilYPCx8bNoG0=
   PrivateKey: 8HlgzxO-pJ3A1rtcc~7nnz774nyKXLLsVeuDGGlc63o=
   ```

2. On server side:

   ```
   signatureType = 11
   i2cp.leaseSetType = 5
   i2cp.leaseSetAuthType = 1
   i2cp.leaseSetClient.dh.210 = friend1:Ihr8wZS8fuA5Q-Exwb-9tGHpyV6lZfFilYPCx8bNoG0=
   # Add more clients: i2cp.leaseSetClient.dh.<number> = name:PublicKey
   ```

3. On client side:

   ```
   i2cp.leaseSetPrivKey = 8HlgzxO-pJ3A1rtcc~7nnz774nyKXLLsVeuDGGlc63o=
   ```

Option 2: PSK Authentication

1. Generate a random 32 byte PSK (`openssl rand -base64 32` or any secure source).

   E.g:
   ```
   i2cp.leaseSetPrivKey = dGhpcyBpcyBhIHJhbmRvbSAzMi1ieXRlIHByZS1zaGFyZWQga2V5Cg==
   ```

2. On server:

   ```
   i2cp.leaseSetAuthType = 2
   # No per-client entries needed - all clients use the same PSK
   # you can add if you want - i2cp.leaseSetClient.psk.nnn
   ```

3. On client:

   Same `i2cp.leaseSetPrivKey = ...` as above.

**Note**: Start with no authentication, verify your b33 works then add auth once basic setup is confirmed.
