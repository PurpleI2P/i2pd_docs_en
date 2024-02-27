Anonymous filesharing
=====================

**BitTorrent software with I2P support**

- [qBittorrent](http://qbittorrent.org/) (qBittorrent experimental I2P support). qBittorrent uses SAM protocol and can be used in mixed mode or I2P-only mode. Currently setting qBittorrent up for I2P support is not user-friendly and there is no official documentation, so you have to combine [this](http://www.i2pforum.net/viewtopic.php?t=1224) and [this](https://strict3443.codeberg.page/i2p-info/hugo/public/posts/how-to-use-i2p-on-qbittorrent-nox/) guide. Remember it is experimental, so you need qBittorrent version v4.6.0 or higher, bugs can happen. *Use carefully and pay attention to the settings. The software works both in I2P and via clearnet. Unwanted leaks of your addresses are likely.*

*Warning: SAM API can currently crash i2pd*

- [I2PSnark standalone](https://gitlab.com/i2pplus/I2P.Plus/-/jobs/artifacts/master/raw/i2psnark-standalone.zip?job=Java8) (standalone build provided by [I2P+ project](https://i2pplus.github.io/)). I2PSnark uses I2CP protocol, i2pd must be run with parameter `--i2cp.enabled=true` or with a similar setting in the configuration file.

- [Vuze](https://en.wikipedia.org/wiki/Vuze) and [BiglyBT](https://www.biglybt.com) (Vuze fork). Vuse and BiglyBT uses I2CP protocol, i2pd must be run with parameter `--i2cp.enabled=true` or with a similar setting in the configuration file. *Use carefully and pay attention to the settings. The software works both in I2P and via clearnet. Unwanted leaks of your addresses are likely.*

- [XD](https://github.com/majestrate/XD). *Warning: XD uses SAM API which can currently crash i2pd.*

- [Robert](http://en.wikipedia.org/wiki/Robert_%28P2P_Software%29). Robert uses BOB protocol, i2pd must be run with parameter `--bob.enabled=true` or with a similar setting in the configuration file.

Also, visit [postman tracker](http://tracker2.postman.i2p).
