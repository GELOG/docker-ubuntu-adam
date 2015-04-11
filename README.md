# Supported tags and respective `Dockerfile` links
- [`0.15.0`/Dockerfile](https://github.com/GELOG/docker-ubuntu-adam/blob/master/0.15.0/Dockerfile)
- [`0.14.0`/Dockerfile](https://github.com/GELOG/docker-ubuntu-adam/blob/master/0.14.0/Dockerfile)
 
# What is Adam ?
ADAM provides both an application programming interface (API) and a command line interface (CLI) for manipulating genomic data on a computing cluster. ADAM operates on data stored inside of [Parquet](http://www.parquet.io/) with the [bdg-formats](http://bdgenomics.org/projects/bdg-formats/) schemas, using [Apache Spark](http://spark.apache.org/), and provides scalable performance on clusters larger than 100 machines.

ADAM is on [Github](https://github.com/bigdatagenomics/adam). Quick start guides are available for [running ADAM on EC2](https://github.com/bigdatagenomics/adam/wiki/Running-ADAM-on-EC2), and for [building ADAM for specific CDH releases](https://github.com/bigdatagenomics/adam/wiki/Running-ADAM-on-CDH-4-or-5).

http://bdgenomics.org/projects/adam/

# What is Docker?
Docker is an open platform for developers and sysadmins to build, ship, and run distributed applications. Consisting of Docker Engine, a portable, lightweight runtime and packaging tool, and Docker Hub, a cloud service for sharing applications and automating workflows, Docker enables apps to be quickly assembled from components and eliminates the friction between development, QA, and production environments. As a result, IT can ship faster and run the same app, unchanged, on laptops, data center VMs, and any cloud.

https://www.docker.com/whatisdocker/

## What is a Docker Image?
Docker images are the basis of containers. Images are read-only, while containers are writeable. Only the containers can be executed by the operating system.

https://docs.docker.com/terms/image/

# Dependencies
* [Install Docker](https://docs.docker.com/installation/)

# Base Docker image
* [gelog/spark:1.1.0-bin-hadoop2.3](https://registry.hub.docker.com/u/gelog/spark/)

# How to use this image?
### 1) Get the aligned file of a genome (or chromosome) 
#### 1.1) Get it from [Snap](https://github.com/GELOG/docker-ubuntu-snap)
#### 1.2) Download it from an external source (Warning: This file is 14.5 GB)
    mkdir /data/
    wget -O /data/HG00096.mapped.ILLUMINA.bwa.GBR.low_coverage.20120522.bam ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/data/HG00096/alignment/HG00096.mapped.ILLUMINA.bwa.GBR.low_coverage.20120522.bam
### 2) Transform the genome (or chromosome) with Adam
#### 2.1) From [Snap](https://github.com/GELOG/docker-ubuntu-snap)
    docker run --rm=true -ti -v /data/:/data gelog/adam adam-submit transform /data/SRR062634.sam /data/SRR062634.adam
#### 2.2) From an external source
    docker run --rm=true -ti -v /data/:/data gelog/adam adam-submit transform /data/HG00096.mapped.ILLUMINA.bwa.GBR.low_coverage.20120522.bam /data/HG00096.adam
