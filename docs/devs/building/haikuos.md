Building on HaikuOS system
=============================

* [HaikuOS](#HaikuOS)

## HaikuOS
```bash
pkgman refresh
pkgman install boost1.91_devel
# if not installed 
pkgman install make
pkgman install gcc
pkgman install git 
# ^^^ will be
git clone https://github.com/PurpleI2P/i2pd.git
cd i2pd
#make -j#count of your cpu or just make
make
```

after compilation you can run
```bash
./i2pd
```

also click on your time with right mouse button, and open settings, found synchronization and synchronize time its need!
after on Control+C close and your workdir is ~/config/settings/i2pd
You can create there i2pd.conf also tunnels.conf and create tunnels
Also for haiku exists primitive GUI.
