#
# Scala and sbt Dockerfile

# Pull base image
FROM openjdk:8u151

# Env variables
ENV SCALA_VERSION 2.11.12
ENV SBT_VERSION 0.13.15
ENV BOXFUSE_VERSION 1.32.2.1447
ENV BOXFUSE_USER 44a9e01ec7464b0fb6b74679e26d3172
ENV BOXFUSE_SECRET Kop46iNQGXg4uYKCLoeDL19LnI55cHiYBJZszg6DpqPFTRo5HFEB1QtMqZT91sHT

# Scala expects this file
RUN touch /usr/lib/jvm/java-8-openjdk-amd64/release

# Install boxfuse
RUN \
    curl -fsL boxfuse.tar.gz https://files.boxfuse.com/com/boxfuse/client/boxfuse-commandline/$BOXFUSE_VERSION/boxfuse-commandline-$BOXFUSE_VERSION-linux-x64.tar.gz | tar xfz - -C /root/

# Install Scala
## Piping curl directly in tar
RUN \
  curl -fsL https://downloads.typesafe.com/scala/$SCALA_VERSION/scala-$SCALA_VERSION.tgz | tar xfz - -C /root/ && \
  echo >> /root/.bashrc && \
  echo "export PATH=~/scala-$SCALA_VERSION/bin:~/boxfuse:$PATH" >> /root/.bashrc

# Install sbt
RUN \
  curl -L -o sbt-$SBT_VERSION.deb https://dl.bintray.com/sbt/debian/sbt-$SBT_VERSION.deb && \
  dpkg -i sbt-$SBT_VERSION.deb && \
  rm sbt-$SBT_VERSION.deb && \
  apt-get update && \
  apt-get  -y --force-yes install sbt && \
  sbt sbtVersion

# Install aws cli
RUN apt-get update && apt-get install -y \
  python-dev \
  zip \
  jq

RUN curl -O https://bootstrap.pypa.io/get-pip.py

RUN python get-pip.py
RUN pip install awscli

WORKDIR /root
CMD ["sbt"]
