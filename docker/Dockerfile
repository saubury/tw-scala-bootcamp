FROM ubuntu:latest

LABEL version="1.0"
LABEL description="TWA Scala DE150"

RUN apt-get update && apt-get install -y \
  openjdk-8-jdk \
  vim \
  scala \
  wget \
  gnupg 

RUN echo "deb https://dl.bintray.com/sbt/debian /" | tee -a /etc/apt/sources.list.d/sbt.list

RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 2EE0EA64E40A89B84B2DF73499E82A75642AC823

RUN apt-get update && apt-get install -y sbt

RUN cd /tmp && echo my_project | sbt new sbt/scala-seed.g8

# Fix REPL - https://stackoverflow.com/questions/50567410/scala-repl-no-echo-on-input
RUN cd /tmp \
  && wget https://launchpad.net/~lokkju/+archive/ubuntu/java-compat/+build/16458066/+files/libjline2-java_2.14.6-1ubuntu1~bionicppa1_all.deb \
  && dpkg -i libjline2-java_2.14.6-1ubuntu1~bionicppa1_all.deb

# I know you are not supposed to do this, it's not very docker-like
ENTRYPOINT ["tail", "-f", "/dev/null"]
