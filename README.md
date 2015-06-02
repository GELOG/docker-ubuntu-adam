# ADAM: Avro Data Alignment Map

[![dockeri.co](http://dockeri.co/image/gelog/adam)](https://registry.hub.docker.com/u/gelog/adam/)

[![stars](https://img.shields.io/github/stars/bigdatagenomics/adam.svg) ![forks](https://img.shields.io/github/forks/bigdatagenomics/adam.svg) ![issues](https://img.shields.io/github/issues/bigdatagenomics/adam.svg) ](https://github.com/bigdatagenomics/adam)


## Supported tags and respective `Dockerfile` links
- [`0.14.0` _(adam-0.14/Dockerfile)_](https://github.com/GELOG/docker-ubuntu-adam/tree/v0.14.0/0.14/Dockerfile) based on [`gelog/spark:1.2-bin-hadoop2.3`](https://registry.hub.docker.com/u/gelog/adam/) image.
- [`0.15.0`, `latest` _(adam-0.15/Dockerfile)_](https://github.com/GELOG/docker-ubuntu-adam/tree/v0.15.0/0.15/Dockerfile) based on [`gelog/spark:1.2-bin-hadoop2.3`](https://registry.hub.docker.com/u/gelog/adam/) image.
- [`0.16.0` _(adam-0.16/Dockerfile)_](https://github.com/GELOG/docker-ubuntu-adam/tree/v0.16.0/adam-0.16/Dockerfile) based on [`gelog/spark:1.2-bin-hadoop2.3`](https://registry.hub.docker.com/u/gelog/adam/) image.


## What is Adam ?
ADAM provides both an application programming interface (API) and a command line interface (CLI) for manipulating genomic data on a computing cluster. ADAM operates on data stored inside of [Parquet](http://www.parquet.io/) with the [bdg-formats](http://bdgenomics.org/projects/bdg-formats/) schemas, using [Apache Spark](http://spark.apache.org/), and provides scalable performance on clusters larger than 100 machines.

ADAM is on [Github](https://github.com/bigdatagenomics/adam). Quick start guides are available for [running ADAM on EC2](https://github.com/bigdatagenomics/adam/wiki/Running-ADAM-on-EC2), and for [building ADAM for specific CDH releases](https://github.com/bigdatagenomics/adam/wiki/Running-ADAM-on-CDH-4-or-5).

http://bdgenomics.org/projects/adam/


## What is Docker?
Docker is an open platform for developers and sysadmins to build, ship, and run distributed applications. Consisting of Docker Engine, a portable, lightweight runtime and packaging tool, and Docker Hub, a cloud service for sharing applications and automating workflows, Docker enables apps to be quickly assembled from components and eliminates the friction between development, QA, and production environments. As a result, IT can ship faster and run the same app, unchanged, on laptops, data center VMs, and any cloud.

https://www.docker.com/whatisdocker/

### What is a Docker Image?
Docker images are the basis of containers. Images are read-only, while containers are writeable. Only the containers can be executed by the operating system.

https://docs.docker.com/terms/image/


## How to use this image?

### Converting a BAM / SAM file to the ADAM format

1) (Optional step) Download a small SAM file, if you don't have one handy.
```bash
mkdir -p $HOME/data
wget -O $HOME/data/small.sam \
    https://raw.githubusercontent.com/bigdatagenomics/adam/master/adam-core/src/test/resources/small.sam
```

2) Convert to the ADAM format in a new Adam container
```bash
docker run --rm=true -ti -v /data/:/data gelog/adam \
    adam-submit transform \
    $HOME/data/small.sam \
    $HOME/data/small.adam
```


## Known issues

### Memory allocation is hardcoded in `adam-submit` launch script
If there is not enough memory allocated to Adam / Spark, Adam may crash. By default, only 512MB is allocated to java processes. Here's a workaround to fix this issue

Download a [larger BAM file](ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/data/HG00096/alignment/HG00096.mapped.ILLUMINA.bwa.GBR.low_coverage.20120522.bam) and start a bash shell in an ADAM container:
```bash
$ wget -O /data/HG00096.mapped.ILLUMINA.bwa.GBR.low_coverage.20120522.bam \
    ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/data/HG00096/alignment/HG00096.mapped.ILLUMINA.bwa.GBR.low_coverage.20120522.bam
```

Then, start an ADAM container:
```bash
$ docker run --rm -ti -v $HOME/data:/data gelog/adam bash
root@42c257dcfbcc:/# 
```

Once inside the container, run ADAM with 1.5GB of RAM (the `SPARK_DRIVER_MEMORY` and `SPARK_EXECUTOR_MEMORY` variables):
```bash
root@42c257dcfbcc:/# SPARK_DRIVER_MEMORY=1500m SPARK_EXECUTOR_MEMORY=1500m \
    adam-submit transform \
    /data/HG00096.chrom20.ILLUMINA.bwa.GBR.low_coverage.20120522.bam \
    /data/hg00096.chrom20.adam
root@42c257dcfbcc:/#
```

**[UPDATE: June 2nd 2015]:** The `adam-submit` script in Adam 0.16.0 now allocates 4 GB by default [see here](https://github.com/bigdatagenomics/adam/blob/master/bin/adam-submit#L89-L90). If you need more than 4GB, then we need to find a way to override this parameter when running the container. The adam-submit script accepts the `--conf spark.executor.memory="1g"` option, but it has no effect.

Please [contact us](https://gitter.im/GELOG/adamcloud) if you find a more elegant way to solve this issue.
