FROM centos:7 as jlink-builder

WORKDIR /opt
RUN yum install -y wget && \
    wget "https://github.com/alibaba/Sentinel/releases/download/1.8.4/sentinel-dashboard-1.8.4.jar" -O sentinel-dashboard.jar

FROM dreamkaylee/dragonwell:11.0.14
LABEL maintainer="dreamkaylee@outlook.com"

WORKDIR /opt/sentinel

COPY --from=jlink-builder /opt/sentinel-dashboard.jar /opt/sentinel/sentinel-dashboard.jar

EXPOSE 8858
EXPOSE 8719

ENTRYPOINT [ \
  "java", \
  "-Djava.security.egd=file:/dev/./urandom", \
  "-Dserver.port=8858", \
  "-Dcsp.sentinel.api.port=8719", \
  "-Dcsp.sentinel.dashboard.server=localhost:8858", \
  "-Dproject.name=sentinel-dashboard", \
  "-jar",\ 
  "sentinel-dashboard.jar" \
]