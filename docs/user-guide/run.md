Running i2pd
============

Starting, stopping and reloading configuration
----------------------------------------------

This chapter explains how to start and manage i2pd daemon under \*nix operation systems.

After you have built i2pd from source, just run a binary:

    ./i2pd

To display all available options:

    ./i2pd --help

i2pd can be controlled with signals. Process ID by default is written to file `~/.i2pd/i2pd.pid` or `/var/run/i2pd/i2pd.pid`.
You can use `kill` utility to send signals like this:

    kill -INT $( cat /var/run/i2pd/i2pd.pid )

i2pd supports the following signals:

* INT - Graceful shutdown. i2pd will wait for 10 minutes and stop. Send second INT signal to shutdown i2pd immediately.
* HUP - Reload configuration files.


### systemd unit

Some Linux packages have a systemd control unit, so it is possible to managage i2pd with it.

Start/stop i2pd:

    sudo systemctl start i2pd.service
    sudo systemctl stop i2pd.service

Enable/disable i2pd to be started on bootup:

    sudo systemctl enable i2pd.service
    sudo systemctl disable i2pd.service


Recommended way to run i2pd built from source
---------------------------------------------

This way all i2pd-related files will be stored at `$HOME/dist`.

    mkdir $HOME/dist
    cp i2pd $HOME/dist
    cp -R contrib/certificates $HOME/dist
    cp contrib/i2pd.conf $HOME/dist
    cd $HOME/dist
    ulimit -n 4096  # only on Linux, increasing open file limit
    ./i2pd --datadir .

