# yotta-gbldir-files: Initial Global Directory Files for YottaDB
 
Rob Tweed <rtweed@mgateway.com>  
22 December 2017, M/Gateway Developments Ltd [http://www.mgateway.com](http://www.mgateway.com)  

Twitter: @rtweed

Google Group for discussions, support, advice etc: [http://groups.google.co.uk/group/enterprise-web-developer-community](http://groups.google.co.uk/group/enterprise-web-developer-community)

## Purpose

These files allow you to use the [YottaDB](https://github.com/YottaDB/YottaDB) database within a Docker Container, but retain persistence between invocations of the Container.  They are a set of initialised YottaDB Global Directory files to which you should map your Docker's YottaDB global directory.

Note: the files in this repository are for use with YottaDB version 1.2


## Example

If you're using the *rtweed/qewd-server* Docker Container and want YottaDB database persistence within your host system:

- clone these files into a directory on your host machine, eg:


       cd ~
       git clone https://github.com/robtweed/yotta-gbldir-files

- change the permissions to match those expected by YottaDB

       cd yotta-gbldir-files
       sudo chmod 666 yottadb.dat
       sudo chmod 664 yottadb.gld
       sudo chmod 666 yottadb.mjl

- start *qewd-server* and map this directory

       sudo docker run -it -p 8081:8080 -v ~/qewd:/opt/qewd/mapped -v ~/yotta-gbldir-files:/root/.yottadb/r1.20_x86_64/g rtweed/qewd-server

  The key bit is this volume mapping parameter:

       v ~/yotta-gbldir-files:/root/.yottadb/r1.20_x86_64/g

  This tells YottaDB within the *qewd-server* Docker Container to map its global directory (and hence its database storage) to the files you cloned into the host's *~/yotta-gbldir-files* directory, instead of using its internal version.

Now, every time you stop and restart the *qewd-server* Docker Container, data will be retained on the host machine's file system, instead of being lost within the Container's ephemeral file system.

## Licensing

See [the YottaDB Github Repository](https://github.com/YottaDB/YottaDB) for details of its licensing, which should apply to these files.


