Anonymous websites
==================

## Browse anonymous websites

To browse anonymous websites inside Invisible Internet, configure your web browser to use HTTP proxy 127.0.0.1:4444 (available by default in i2pd).

In Firefox: Preferences -> Advanced -> Network tab -> Connection Settings -> choose Manual proxy configuration, Enter HTTP proxy 127.0.0.1, Port 4444

In Chromium: run chromium executable with key

    chromium --proxy-server="http://127.0.0.1:4444"

Note that if you wish to stay anonymous too you'll need to tune your browser for better privacy. Do your own research, [can start here](http://www.howtogeek.com/102032/how-to-optimize-mozilla-firefox-for-maximum-privacy/).

Big list of Invisible Internet websites can be found at [identiguy.i2p](http://identiguy.i2p).

## Host anonymous website

If you wish to run your own website in Invisible Internet, follow those steps:

1) Run your webserver and find out which host:port it uses (for example, 127.0.0.1:8080).

2) Configure i2pd to create HTTP server tunnel. Put in your ~/.i2pd/tunnels.conf file:

    [anon-website]
    type = http
    host = 127.0.0.1
    port = 8080
    keys = anon-website.dat

3) Restart i2pd or send SIGHUP signal.

4) Find b32 destination of your website.

   Go to webconsole -> [I2P tunnels page](http://127.0.0.1:7070/?page=i2p_tunnels). Look for Server tunnels and you will see address like \<long random string\>.b32.i2p next to anon-website.

   Website is now available in Invisible Internet by visiting this address.

5) (Optional) Register short and memorable .i2p domain on [reg.i2p](http://reg.i2p).
