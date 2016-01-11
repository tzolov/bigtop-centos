# Apache Bigtop Docker Environment
Ready to use Docker environment for [Apache Bigtop](http://bigtop.apache.org/). Based on CentOS and equipped with all necessary tools and libraries. To use the image follow the instructions bellow or read the [How-To Guide](http://blog.tzolov.net/2015/06/leverage-apache-bigtop-to-build-hadoop.html?view=sidebar).

### 1. Installation

* Install [Docker](https://www.docker.io/).
* Configure Docker Host - Use at least 4GB of memory. You can add 8GB of memory to the [boot2docker](http://boot2docker.io/) host like this: `boot2docker delete; boot2docker init -m 8192; boot2docker up; export DOCKER_HOST=tcp://<Docker Host IP>:2375`)
* Download [trusted build](https://registry.hub.docker.com/u/tzolov/bigtop-centos/) from public [Docker Registry](https://index.docker.io/): `docker pull tzolov/bigtop-centos` (alternatively, you can build an image from Dockerfile: `docker build -t="tzolov/my-apache-bigtop-environment:1.0.0" github.com/tzolov/bigtop-centos.git`)
* Start a container with the latest image: `docker run --rm -t -i -v /rpm:/rpm tzolov/bigtop-centos /bin/bash`

### 2. How to Build Bigtop RPMs

Created RPMs are stored into `/home/bigtop/bigtop/build/**/rpm/RPMS/**/*.rpm` folder.  

To build an RPM for a single project use `./gradlew <project name>-rpm`. For example to build Spark RPM do:

    # Build Spark RPM
    cd ~/bigtop
    ./gradlew spark-rpm

To build all Bigtop RPMS use `./gradlew rpm`:

    # Build all RPMs
    cd ~/bigtop
    ./gradlew rpm

To list all gradle tasks run `cd ~/bigtop && ./gradlew tasks`.

To change the version of the target Hadoop project editing the `bigtop.mk` (up to `Bigtop 1.0.0`) or the `bigtop.bom` for newer (eg. > 1.0.0) versions. For example to change the Spark version from to the latest 1.6.0: 

* (Up to Bigtop 1.0.0 including) open the `bigtop.mk`, find the Spark section (Starts with # Spark) and set the `SPARK_BASE_VERSION`
and `SPARK_PKG_VERSION` properties like this:
```
    SPARK_BASE_VERSION=1.6.0
    SPARK_PKG_VERSION=1.6.0
```

* (For > Bigtop 1.0.0) open the `bigtop.bom`, find the Saprk section and set the version: 
```
    'spark' {
        name    = 'spark'
        pkg     = 'spark-core'
        relNotes = 'Apache Spark'
        version { base = '1.6.0'; pkg = base; release = 1 }
        .....
    }
```

## 3. Download built RPMs

Copy the RPMs from the local folder `/home/bigtop/bigtop/build/**/rpm/RPMS/**/*.rpm` into the shared with the host folder `/rpm`

    sudo cp /home/bigtop/bigtop/build/**/rpm/RPMS/**/*.rpm /rmp

In turn you can copy from the Docker Host into local folder: 
    
    scp -rp docker@<Docker Host IP>:/rpm/*.rpm <your local folder>

(default docer password: `tcuser`)
    
