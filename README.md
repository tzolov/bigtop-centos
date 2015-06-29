# bigtop-centos
Ready to use Docker environment for [Apache Bigtop](http://bigtop.apache.org/). Based on CentOS and equipped with all necessary tools and libraries. 

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

## 3. Download built RPMs

Copy the RPMs from the local folder `/home/bigtop/bigtop/build/**/rpm/RPMS/**/*.rpm` into the shared with the host folder `/rpm`

    sudo cp /home/bigtop/bigtop/build/**/rpm/RPMS/**/*.rpm /rmp

In turn you can copy from the Docker Host into local folder: 
    
    scp -rp docker@<Docker Host IP>:/rpm/*.rpm <your local folder>

(default docer password: `tcuser`)
    
