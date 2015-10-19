# For building this image, See https://github.com/GELOG/docker-ubuntu-spark
# Installation instructions: https://github.com/bigdatagenomics/adam/releases
FROM gelog/spark:1.2-bin-hadoop2.3


####################
# Dependencies
####################

ENV MAVEN_VERSION       3.2.3
ENV MAVEN_HOME          /usr/local/apache-maven
ENV MAVEN_OPTS          -Xmx512m -XX:MaxPermSize=128m
ENV PATH                $MAVEN_HOME/bin:$PATH


# Installing Maven
RUN wget http://archive.apache.org/dist/maven/maven-3/$MAVEN_VERSION/binaries/apache-maven-$MAVEN_VERSION-bin.tar.gz && \
    tar  -xzf apache-maven-$MAVEN_VERSION-bin.tar.gz && \
    rm   apache-maven-$MAVEN_VERSION-bin.tar.gz && \
    mv   apache-maven-$MAVEN_VERSION  $MAVEN_HOME && \
    mvn  -version



####################
# ADAM
####################

ENV ADAM_SCALA_VERSION	2.11
ENV ADAM_VERSION        $ADAM_SCALA_VERSION-0.17.1
ENV ADAM_HOME           /usr/local/adam
ENV PATH                $ADAM_HOME/bin:$PATH

# Installing ADAM
RUN wget https://github.com/bigdatagenomics/adam/archive/adam-parent_$ADAM_VERSION.tar.gz && \
    tar  -xzf adam-parent_$ADAM_VERSION.tar.gz && \
    rm   adam-parent_$ADAM_VERSION.tar.gz && \
    mv   adam-adam-parent_$ADAM_VERSION  $ADAM_HOME

# Updates the hadoop version in Adam's pom.xml file 
# FIXME: Why does ADAM requires the Hadoop version to be specified, 
#        if it is already specified in Spark's installation
#RUN ADAM_HADOOP_VERSION=2.3.0 && \
#    sed -i -e "s/\(<hadoop.version>\).*\(<\/hadoop.version>\)/\1$ADAM_HADOOP_VERSION\2/g" $ADAM_HOME/pom.xml

# FIXME: Investigate why Adam takes 11min to build
#RUN cd $ADAM_HOME && \
#    mvn clean package -DskipTests

# FIXME: the ADAM image takes 1.4 GB of virtual size (640 MB for Adam build)

# FIXME: Errors when building ADAM: 
# [...]
# Downloaded: https://repo.maven.apache.org/maven2/org/scala-lang/scala-library/2.10.0/scala-library-2.10.0.jar (6948 KB at 547.9 KB/sec)
# [ERROR] Error formatting /usr/local/adam-adam-parent-0.14.0/adam-core/src/test/scala/org/bdgenomics/adam/rdd/normalization/ZScoreNormalizationSuite.scala: java.nio.charset.MalformedInputException: Input length = 1
# [ERROR] Error formatting /usr/local/adam-adam-parent-0.14.0/adam-core/src/test/scala/org/bdgenomics/adam/instrumentation/DurationFormattingSuite.scala: java.nio.charset.MalformedInputException: Input length = 1
# [ERROR] Error formatting /usr/local/adam-adam-parent-0.14.0/adam-core/src/main/scala/org/bdgenomics/adam/rdd/normalization/ZScoreNormalization.scala: java.nio.charset.MalformedInputException: Input length = 1
# [ERROR] Error formatting /usr/local/adam-adam-parent-0.14.0/adam-core/src/main/scala/org/bdgenomics/adam/instrumentation/DurationFormatting.scala: java.nio.charset.MalformedInputException: Input length = 1
# [ERROR] Error formatting /usr/local/adam-adam-parent-0.14.0/adam-cli/src/main/scala/org/bdgenomics/adam/cli/PrintADAM.scala: java.nio.charset.MalformedInputException: Input length = 1
# [INFO] Modified 0 of 280 .scala files
# [...]


#EXPOSE 4040

# Default action: prints Adam's options
# See also: adam-submit, adam-shell, adam-pyspark 
#CMD ["adam-submit"]

