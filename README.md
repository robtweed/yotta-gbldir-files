# yotta-gbldir-files: Initial Global Directory Files for YottaDB
 
Rob Tweed <rtweed@mgateway.com>  
22 December 2017, M/Gateway Developments Ltd [http://www.mgateway.com](http://www.mgateway.com)  

Twitter: @rtweed

Google Group for discussions, support, advice etc: [http://groups.google.co.uk/group/enterprise-web-developer-community](http://groups.google.co.uk/group/enterprise-web-developer-community)

## Purpose

These files allow you to use the [YottaDB](https://github.com/YottaDB/YottaDB) database within a Docker Container, but retain persistence between invocations of the Container.  They are a set of initialised YottaDB Global Directory files to which you should map your Docker's YottaDB global directory.

Note: the files in this repository are for use with various YottaDB versions up to 1.24

You'll find a folder for each YottaDB version, for example version 1.24 files are in the
*/r1.24* folder.

Inside each version folder, you'll find two sub-folders, one for Linux machines (*/linux*) and one for
the Raspberry Pi (*/rpi*).

Inside each of these subfolders you'll find three files:

- yottadb.dat (the actual YottaDB database file)
- yottadb.gld (the YottaDB global directory file)
- yottadb.mjl (the initial YottaDB Journal file)

These are pre-initialised files that contain no actual data, but, if mapped to the folder where
YottaDB within the QEWD Container expects to find them, then YottaDB will read from and write to
them.  They will continue to exist on the host machine when you stop the QEWD Container, and whenever
you restart the Container, YottaDB within the Container will use them again, and any previously-stored
data will be available.


## Example

If you're using the latest *rtweed/qewd-server* Docker Container which uses YottaDB version 1.24, 
and you want YottaDB database persistence within your host system:

- clone these files into a directory on your host machine, eg:

       cd ~
       git clone https://github.com/robtweed/yotta-gbldir-files

- change the permissions to match those expected by YottaDB

       cd yotta-gbldir-files/r1.24/linux
       sudo chmod 666 yottadb.dat
       sudo chmod 664 yottadb.gld
       sudo chmod 666 yottadb.mjl

- start *qewd-server* and map this directory

       sudo docker run -it -p 8081:8080 -v ~/qewd:/opt/qewd/mapped -v ~/yotta-gbldir-files/r1.24/linux:/root/.yottadb/r1.24_x86_64/g rtweed/qewd-server

  The key bit is this volume mapping parameter:

       v ~/yotta-gbldir-files/r1.24/linux:/root/.yottadb/r1.24_x86_64/g

  This tells YottaDB within the *qewd-server* Docker Container to map its global directory (and hence its database storage) to the files you cloned into the host's *~/yotta-gbldir-files/r1.24/linux* directory, instead of using its internal version.

Now, every time you stop and restart the *qewd-server* Docker Container, data will be retained on the host machine's file system, instead of being lost within the Container's ephemeral file system.


## Raspberry Pi

If you are running QEWD on a Raspberry Pi:

- change the *docker run* command to map the */rpi* versions of the files to the directory 
where YottaDB expects to find them.

- use the *rtweed/qewd-server-rpi* Container.

For example:

       sudo docker run -it -p 8081:8080 -v ~/qewd:/opt/qewd/mapped -v ~/yotta-gbldir-files/r1.24/rpi:/root/.yottadb/r1.24_armv7l/g rtweed/qewd-server-rpi



## Licensing

See [the YottaDB Github Repository](https://github.com/YottaDB/YottaDB) for details of its licensing, which should apply to these files.


