Build requirements
==================

In general, for building i2pd you need several things:

* compiler with c++17 support (for example: gcc >= 8, clang)
* boost >= 1.62
* openssl library >= 1.1.0
* zlib library (openssl already depends on it)

Optional tools:

* cmake >= 2.8 (or 3.3+ if you want to use precompiled headers on windows)
* miniupnp library (for upnp support)  
