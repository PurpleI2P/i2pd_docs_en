Family configuration
====================

You might want to specify a family your router belongs to.
There are two ways to do this: create a new family or join to an existing one.

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

TODO: List common errors
