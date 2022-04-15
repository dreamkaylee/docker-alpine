FROM centos:7 as jlink-builder

WORKDIR /opt
RUN yum install -y wget && \
    wget -O jdk.tar.gz https://github.com/alibaba/dragonwell8/releases/download/dragonwell-8.10.11_jdk8u322-ga/Alibaba_Dragonwell_8.10.11_x64_linux.tar.gz && \
    tar zxf jdk.tar.gz && \
    ln -s dragonwell-8.10.11 java && \
    rm -f jdk.tar.gz && \
    rm -rf /opt/java/man \
           /opt/java/sample && \
    rm -f  /opt/java/src.zip
 
FROM dreamkaylee/alpine_glibc:latest
LABEL maintainer="dreamkaylee@outlook.com"

ENV JAVA_HOME=/usr/local/java
ENV PATH="$PATH:$JAVA_HOME/bin"

COPY --from=jlink-builder /opt/dragonwell-8.10.11 /usr/local/java
RUN ln -s $JAVA_HOME/bin/java /usr/bin/java