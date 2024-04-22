# ubuntu:jammy-20240405 is a fixed version for ubuntu:22.04
# requirements: docker version > 20.10.9
FROM ubuntu:jammy-20240405

# utils
RUN apt-get update && \
  apt-get install -y zip \
  RUN apt-get update && \
      apt-get install -y zip \
         build-essential \
         curl \
         wget \
         libncurses5-dev \
         openssl \
         libssl-dev \
         build-essential \
         pkg-config \
         libc6-dev \
         bison \
         flex \
         libelf-dev \
         zlib1g-dev \
         minizip \
         libidn11-dev \
         qttools5-dev \
         liblz4-tool \
         git \
         podman && \
         apt-get clean

# USER jenkins
WORKDIR /home/jenkins

ENV SONAR_SCANNER_VERSION 3.3.0.1492
RUN curl -o sonar_scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-${SONAR_SCANNER_VERSION}-linux.zip && \
    unzip sonar_scanner.zip && rm sonar_scanner.zip \
    && rm -rf sonar-scanner-$SONAR_SCANNER_VERSION-linux/jre && \
    sed -i 's/use_embedded_jre=true/use_embedded_jre=false/g' /home/jenkins/sonar-scanner-$SONAR_SCANNER_VERSION-linux/bin/sonar-scanner && \
    mv /home/jenkins/sonar-scanner-$SONAR_SCANNER_VERSION-linux /usr/bin
ENV PATH $PATH:/usr/bin/sonar-scanner-$SONAR_SCANNER_VERSION-linux/bin

COPY ./ ./
ENV EXCLUDE_DOCKER 1
RUN ./hack/install_utils.sh && rm -rf ./*

COPY storage.conf /etc/containers/storage.conf
COPY containers.conf /etc/containers/containers.conf

VOLUME /var/lib/containers

CMD ["podman", "info"]
