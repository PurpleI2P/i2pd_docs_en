Family configuration
====================

A family is a group of routers operated by the same entity or individual. This declaration helps maintain network diversity and security by ensuring that multiple routers from the same family are not used within a single tunnel. 
You may want to specify the family your router belongs to. There are two ways to do this: create a new family or join an existing one.

New family
-----------

To create a new family, you must first create a family self-signed certificate and key.  
The only key type supported is prime256v1.
Use the following list of commands to do this through openssl:  

    openssl ecparam -name prime256v1 -genkey -out <your family name>.key  
    openssl req -new -key <your family name>.key -out <your family name>.csr  
    touch v3.ext
    openssl x509 -req -days 3650 -in <your family name>.csr -signkey <your family name>.key -out <your family name>.crt -extfile v3.ext  

Specify &lt;your family name>.family.i2p.net for the CN (Common Name) when requested.

Once you are done generating it place &lt;your-family-name>.key and &lt;your-family-name>.crt in the <ip2d data>/family folder (for example ~/.i2pd/family).
You should provide these two files to other members joining your family.
If you want to register your family and let the I2P network recognize it, create a pull request for your .crt file into contrib/certificate/family.
Certificates added into the public repository this way will appear in i2pd and I2P next releases packages. Don't place .key file, it must be shared between you family members only.

How to join existing family
---------------------------

Once you and that family agree to do it, they must give you .key and .crt file and you must place in <i2pd datadir>/certificates/family/ folder.

Publish your family
-------------------

Run i2pd with the parameters 'family=&lt;your-family-name>', and make sure you have &lt;your-family-name>.key and &lt;your-family-name>.crt in your 'family' folder.
If everything is set properly, you router.info will contain two new fields: 'family' and 'family.sig'.
If not, your router will complain on startup with log messages starting with "Family:" prefix and severity 'warn' or 'error'.
    
Export to Java-I2P from i2pd
------------------
1. **Convert private key file to PKCS#8**
    
The private key is in an openssl "EC Parameter File" format:
```
-----BEGIN EC PARAMETERS-----
(base64)
-----END EC PARAMETERS-----
-----BEGIN EC PRIVATE KEY-----
(base64)
-----END EC PRIVATE KEY-----
```
It must be converted to PKCS#8 format first.
```
openssl pkcs8 -topk8 -nocrypt -in your-family-name.key -out your-family-name.pkcs8
```
Now you have a pkcs8 private key in the your-family-name.pkcs8 file: 
```
-----BEGIN PRIVATE KEY-----
(base64)
-----END PRIVATE KEY-----
```
2. **Combine PKCS#8 and certificate files**
    
Now combine the pkcs8 and certificate files into a single file: 
```
cat your-family-name.pkcs8 your-family-name.crt > your-family-name.secret
```
3. **Import combined file**
    
Now go to Java i2p console http://127.0.0.1:7657/configfamily page and Join Existing Router Family selecting the file your-family-name.secret to join that family.

([source](http://zzz.i2p/topics/3313))

Export to i2pd from Java-I2P
----------------------------

Go to Java i2p console http://127.0.0.1:7657/configfamily page and export family key. You'll have a file `family-your-family-name-secret.crt`. It contains both the private key and the public key certificate.

Copy it to `your-family-name.key` and `your-family-name.crt`.
    
Edit `your-family-name.key` in a text editor to remove the certificate part so it contains only the private key part.
    
Edit `your-family-name.crt` in a text editor to remove the private key part so it contains only the certificate part.

Move the `your-family-name.key` and `your-family-name.crt` files to the i2pd /certificates/family/ folder, as instructed [here](https://i2pd.readthedocs.io/en/latest/user-guide/family/).
    
This assumes that i2pd/openssl can handle the PKCS#8 format for the private key. 

([source](http://zzz.i2p/topics/3313))

------------------------

TODO: List common errors
