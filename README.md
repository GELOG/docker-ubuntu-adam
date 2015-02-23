# What is Adam ?
ADAM is a genomics analysis platform with specialized file formats built using Apache Avro, Apache Spark and Parquet. Apache 2 licensed.

https://github.com/bigdatagenomics/adam#introduction

http://bdgenomics.org

# What is Docker?
Docker is an open platform for developers and sysadmins to build, ship, and run distributed applications. Consisting of Docker Engine, a portable, lightweight runtime and packaging tool, and Docker Hub, a cloud service for sharing applications and automating workflows, Docker enables apps to be quickly assembled from components and eliminates the friction between development, QA, and production environments. As a result, IT can ship faster and run the same app, unchanged, on laptops, data center VMs, and any cloud.

https://www.docker.com/whatisdocker/

## What is a Docker Image?
In Docker terminology, a read-only Layer is called an image. An image never changes.

https://docs.docker.com/terms/image/

## what is a Docker Layer?
When Docker mounts the rootfs, it starts read-only, as in a traditional Linux boot, but then, instead of changing the file system to read-write mode, it takes advantage of a union mount to add a read-write file system over the read-only file system. In fact there may be multiple read-only file systems stacked on top of each other. We think of each one of these file systems as a layer.

https://docs.docker.com/terms/layer/

# Dependencies
* [Docker](https://docs.docker.com/installation/)
* Docker image of [gelog/spark:1.1.0-bin-hadoop2.3](https://registry.hub.docker.com/u/gelog/spark/)

# How to use this image?
```

```
