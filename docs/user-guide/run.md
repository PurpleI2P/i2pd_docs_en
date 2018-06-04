Running i2pd
============

Starting, stopping and reloading configuration
----------------------------------------------

This chapter explains how to start and manage the i2pd daemon under \*nix operation systems.

After you have built i2pd from source, just run the binary:

    ./i2pd

To display all available options:

    ./i2pd --help

i2pd can be controlled with signals. The process ID by is written to the file `~/.i2pd/i2pd.pid` or 
`/var/run/i2pd/i2pd.pid` by default. You can use `kill` utility to send signals like this:

    kill -INT $( cat /var/run/i2pd/i2pd.pid )

i2pd supports the following signals:

* INT - Graceful shutdown. i2pd will wait for up to 10 minutes and stop. Send a second INT signal to shutdown i2pd immediately.
* HUP - Reload configuration files.


### systemd unit

Some i2pd packages come with a systemd control unit, and for those that use systemd, it is possible to manage i2pd with it.

To start/stop i2pd:

    sudo systemctl start i2pd.service
    sudo systemctl stop i2pd.service --no-block

The stop command initiates a graceful shutdown process, i2pd stops after finishing to route transit tunnels (maximum 10 minutes).

To enable/disable autostart of i2pd upon bootup:

    sudo systemctl enable i2pd.service
    sudo systemctl disable i2pd.service


Recommended way to run i2pd built from source
---------------------------------------------

The following commands display the recommended way to run i2pd built from source, without a package manager. Installing i2pd this 
way will ensure all i2pd-related files are stored at `$HOME/dist`.

    mkdir $HOME/dist
    cp i2pd $HOME/dist
    cp -R contrib/certificates $HOME/dist
    cp contrib/i2pd.conf $HOME/dist
    cd $HOME/dist
    ulimit -n 4096  # only on Linux, increasing open file limit

Then, to run i2pd, simply travel to the installation directory and type:

    ./i2pd --datadir .

