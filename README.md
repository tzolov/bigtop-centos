# bigtop-centos
Ready to use Docker environment for [Apache Bigtop](http://bigtop.apache.org/). Based on CentOS and equipped with all necessary tools and libraries. 

### 1. Installation

* Install [Docker](https://www.docker.io/).
* Configure Docker Host - Use at least 4GB of memory. You can add 8GB of memory to the [boot2docker](http://boot2docker.io/) host like this: `boot2docker delete; boot2docker init -m 8192; boot2docker up; export DOCKER_HOST=tcp://<Docker Host IP>:2375`)
* Download [trusted build](https://registry.hub.docker.com/u/tzolov/bigtop-centos/) from public [Docker Registry](https://index.docker.io/): `docker pull tzolov/bigtop-centos` (alternatively, you can build an image from Dockerfile: `docker build -t="tzolov/my-apache-bigtop-environment:1.0.0" github.com/tzolov/bigtop-centos.git`)
* Start a container with the latest image: `docker run --rm -t -i -v -v /rpm:/home/bigtop/bigtop/build/ tzolov/bigtop-centos /bin/bash`

### 2. How to Build BigTop RPMs

Created RPMs are stored into `/rpm/**/rpm/RPMS/**/*.rpm` folder on the host.  

To build an RPM for a single project use `./gradlew <project name>-rpm`. For example to build Spark RPM do:

  # Build Spark 
    cd ~/bigtop
    ./gradlew spark-rpm

To build all BigTop RPMS use `./gradlew <project name>-rpm`:

  # Build Spark 
    cd ~/bigtop
    ./gradlew rpm

To list all gradle tasks run `cd ~/bigtop && ./gradlew tasks`.

You can copy the `/rpm` folder over SSH to the Docker host or another server: `scp -rp /rpm docker@<Docker Host IP>: .`. In turn you can copy from the Docker Host into local folder: `scp -rp docker@<Docker Host IP>:rpm <Your Local Folder>`.
    
