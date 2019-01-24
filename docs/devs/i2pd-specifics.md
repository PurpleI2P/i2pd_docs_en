i2pd-specific changes to I2P protocols
======================================

# SAM
SESSION CREATE and DEST GENERATE support additional parameter CRYPTO_TYPE specifing crypto type for new local destination  

new values for SIGNATURE_TYPE
GOST_GOSTR3411256_GOSTR3410CRYPTOPROA or 9
GOST_GOSTR3411512_GOSTR3410TC26A512 or 10

# I2PControl
i2p.router.status is 1 if shared local destination is ready, and 0 if not  
i2p.router.net.tunnels.successrate returns tunnel creation success rate in percents  
# BOB
newkeys signaturetype cryptotype. DSA and ElGamal by default  
