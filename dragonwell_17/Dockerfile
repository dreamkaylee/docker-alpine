FROM centos:7 as jlink-builder

WORKDIR /opt
RUN yum install -y wget && \
    wget -O jdk.tar.gz https://github.com/alibaba/dragonwell17/releases/download/dragonwell-17.0.2.0.2%2B8_jdk-17.0.2-ga/Alibaba_Dragonwell_17.0.2.0.2+8_x64_linux.tar.gz && \
    tar zxf jdk.tar.gz && \
    ln -s dragonwell-17.0.2.0.2+8-GA java && \
    rm -f jdk.tar.gz.tar.gz

RUN /opt/java/bin/jlink \
     --module-path /opt/java/jmods \
     --compress=2 \
     --add-modules jdk.charsets,jdk.jfr,jdk.management.agent,java.base,java.logging,java.xml,jdk.unsupported,java.sql,java.sql.rowset,java.naming,java.desktop,java.management,java.security.jgss,java.instrument \
     --no-header-files \
     --no-man-pages \
     --strip-debug \
     --output /opt/jdk-17-mini-runtime
 
FROM dreamkaylee/alpine_glibc:latest
LABEL maintainer="dreamkaylee@outlook.com"

ENV JAVA_HOME=/usr/local/java
ENV PATH="$PATH:$JAVA_HOME/bin"

COPY --from=jlink-builder /opt/jdk-17-mini-runtime /usr/local/java
RUN ln -s $JAVA_HOME/bin/java /usr/bin/java