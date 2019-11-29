FROM ubuntu:latest

LABEL version="1.0"
LABEL description="TWA Scala 101"

RUN apt-get update && apt-get install -y \
  openjdk-8-jdk \
  vim \
  scala \
  gnupg 

RUN echo "deb https://dl.bintray.com/sbt/debian /" | tee -a /etc/apt/sources.list.d/sbt.list

RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 2EE0EA64E40A89B84B2DF73499E82A75642AC823

RUN apt-get update && apt-get install -y sbt

RUN cd /tmp && sbt new sbt/scala-seed.g8

# I know you are not supposed to do this, it's not very docker-like
ENTRYPOINT ["tail", "-f", "/dev/null"]
