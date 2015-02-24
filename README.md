# What is Adam ?
ADAM provides both an application programming interface (API) and a command line interface (CLI) for manipulating genomic data on a computing cluster. ADAM operates on data stored inside of [Parquet](http://www.parquet.io/) with the [bdg-formats](http://bdgenomics.org/projects/bdg-formats/) schemas, using [Apache Spark](http://spark.apache.org/), and provides scalable performance on clusters larger than 100 machines.

ADAM is on [Github](https://github.com/bigdatagenomics/adam). Quick start guides are available for [running ADAM on EC2](https://github.com/bigdatagenomics/adam/wiki/Running-ADAM-on-EC2), and for [building ADAM for specific CDH releases](https://github.com/bigdatagenomics/adam/wiki/Running-ADAM-on-CDH-4-or-5).

http://bdgenomics.org/projects/adam/

# What is Docker?
Docker is an open platform for developers and sysadmins to build, ship, and run distributed applications. Consisting of Docker Engine, a portable, lightweight runtime and packaging tool, and Docker Hub, a cloud service for sharing applications and automating workflows, Docker enables apps to be quickly assembled from components and eliminates the friction between development, QA, and production environments. As a result, IT can ship faster and run the same app, unchanged, on laptops, data center VMs, and any cloud.

https://www.docker.com/whatisdocker/

## What is a Docker Image?
In Docker terminology, a read-only Layer is called an image. An image never changes.

https://docs.docker.com/terms/image/

## What is a Docker Layer?
When Docker mounts the rootfs, it starts read-only, as in a traditional Linux boot, but then, instead of changing the file system to read-write mode, it takes advantage of a union mount to add a read-write file system over the read-only file system. In fact there may be multiple read-only file systems stacked on top of each other. We think of each one of these file systems as a layer.

https://docs.docker.com/terms/layer/

# Dependencies
* [Docker](https://docs.docker.com/installation/)
* Docker image of [gelog/spark:1.1.0-bin-hadoop2.3](https://registry.hub.docker.com/u/gelog/spark/)

# How to use this image?
### 1) Get the aligned file of a genome (or chromosome) 
#### 1.1) Get it from [Snap](https://github.com/GELOG/docker-ubuntu-snap)
#### 1.2) Download it from an external source (Warning: This file is 14.5 GB)
```
mkdir /docker-volume/
wget -O /docker-volume/HG00096.mapped.ILLUMINA.bwa.GBR.low_coverage.20120522.bam ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/data/HG00096/alignment/HG00096.mapped.ILLUMINA.bwa.GBR.low_coverage.20120522.bam
```
### 2) Transform the genome (or chromosome) with Adam
#### 2.1) From [Snap](https://github.com/GELOG/docker-ubuntu-snap)
```
docker run --rm=true -ti -v /docker-volume/:/docker-volume gelog/adam adam-submit transform /docker-volume/SRR062634.sam /docker-volume/SRR062634.adam
```
#### 2.2) From an external source
```
docker run --rm=true -ti -v /docker-volume/:/docker-volume gelog/adam adam-submit transform /docker-volume/HG00096.mapped.ILLUMINA.bwa.GBR.low_coverage.20120522.bam /docker-volume/HG00096.adam
```

