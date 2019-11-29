This document covers the following getting started topics:

- Installing Docker
- Getting a bas Ubuntu image
- Installing Java
- Making a Java Hello World
- Installing Scala
- Installing SBT

These steps should be done for you in the accompanying `Dockerfile`

1. Install Docker

    Download from https://www.docker.com/products/docker-desktop
        
    Once installed, open a command prompt and issue a command to see current running containers
    
        docker ps
    
    This should produce an empty list like so:
    
    `CONTAINER ID        IMAGE                                COMMAND                  CREAT...`

2. Get a base image
    
    Use a minimal Ubuntu image to simulate starting from scratch on a new system
    
        docker pull ubuntu
    
    Once downloaded you should be able to see the image by running the images command
    
        docker images
    
    Should return a list like this to show the image
    
        REPOSITORY                     TAG                   IMAGE ID            CREATED             SIZE
        ubuntu                         latest                775349758637        3 weeks ago         64.2MB
    
    You can now launch an instance of the image to make a running container.
    Specify:
    - --rm to cleanup the container once you exit
    - -ti to use the container interactively
    
    For example:
        
        mkdir -p my_project
        docker run --rm -ti -v my_project:/my_project ubuntu
    
    Please observe the new prompt, indicating elevated (root) privileges inside the container.
    
        root@8ab4810c1092:/#

3. Install Java (dependency of Scala)
    
    Once you have you container, you will need to install a version of Java, you can check that it does NOT have java by running:
    
        root@8ab4810c1092:/# java -version
        java: command not found
    
    To install, you can use the standard Ubuntu package manager apt.
    
        apt-get update && apt-get install -y openjdk-8-jdk
    
    You can confirm it is now installed by running java as above
    
        root@8ab4810c1092:/# java -version
        openjdk version "1.8.0_222"
        OpenJDK Runtime Environment (build 1.8.0_222-8u222-b10-1ubuntu1~18.04.1-b10)
        OpenJDK 64-Bit Server VM (build 25.222-b10, mixed mode)
    
    There is a separate command to compile plain text (code) into binary programs. 
    
        root@8ab4810c1092:/# javac -version
        javac 1.8.0_222
    
    Whilst we are setting up our Ubuntu machine, an editor will be useful and we will need some PGP support later on.
    
        apt-get install -y gnupg

4. Make Hello World

    You can skip this if your comfortable with Java, but it will verify that its installed and working. Create a text file with the contents as follows:
    
        root@8ab4810c1092:/# cat > HelloWorld.java << EOF
    
        public class HelloWorld {
            public static void main(String[] args) {
                    System.out.println("Hello, World");
            }
        }
        EOF
    
    Compile with javac confirming you have a JDK and not just a JRE 
    
       root@8ab4810c1092:/# javac HelloWorld.java
    
    You can now see the plain text file and the compiled binary file
    
       root@6be6a0df45c1:/# ls HelloWorld.*
       HelloWorld.class  HelloWorld.java
    
    Run the program to confirm it works as expected.
    
       root@6be6a0df45c1:/# java HelloWorld
       Hello, World
    
5. Install Scala
    
    Now that Java is working, Install the Scala interpreter which runs on top of Java
    
        root@8ab4810c1092:/# apt-get install scala -y
    
    Check its installed as we did with Java above
    
        root@6be6a0df45c1:/# scala -version
        Scala code runner version 2.11.12 -- Copyright 2002-2017, LAMP/EPFL
    
    Check we have the compiler scalac as with javac above
    
        root@6be6a0df45c1:/# scalac -version
        Scala compiler version 2.11.12 -- Copyright 2002-2017, LAMP/EPFL

6. Using SBT (Simple Build Tool)
    
    The best way to install sbt is via the package manager, though it could also be downloaded as a tarball and run locally.
    
        https://drive.google.com/open?id=1XSoQYyAQK7Byo3NSkUdudJc9t1-8La9E
    
    Configure the repo
    
        root@6be6a0df45c1:/# echo "deb https://dl.bintray.com/sbt/debian /" | tee -a /etc/apt/sources.list.d/sbt.list
        deb https://dl.bintray.com/sbt/debian /
    
        root@6be6a0df45c1:/# apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 2EE0EA64E40A89B84B2DF73499E82A75642AC823
        Executing: /tmp/apt-key-gpghome.tVWxcCVSDw/gpg.1.sh --keyserver hkp://keyserver.ubuntu.com:80 gpg: Total number processed: 1
        gpg:               imported: 1
    
        root@6be6a0df45c1:/# apt-get update
        root@6be6a0df45c1:/# apt-get install -y sbt
        root@6be6a0df45c1:/# sbt new sbt/scala-seed.g8
        [info] [launcher] getting org.scala-sbt sbt 1.3.4  (this may take some time)...
        ...
        sbt:root> console
        scala> println("Hello")
        Hello
    
    Configuring an SBT project
    
        root@6be6a0df45c1:/# mkdir -p /my_project/hello_world/src/main/scala
        root@6be6a0df45c1:/# cp HelloWorld2.scala /my_project/hello_world/src/main/scala/HelloWorld.scala
        root@6be6a0df45c1:/hello_world # cd hello_world
        root@6be6a0df45c1:/hello_world # sbt
        ….
        sbt:hello_world> exit
        root@6be6a0df45c1:/hello_world # sbt run
        Hello, world!
        [success] Total time: 11 s, completed Nov 28, 2019 4:39:48 AM
        
        root@3bb2a36810e4:/hello_world# ls -l target/scala-2.12/
        -rw-r--r-- 1 root root 1523 Nov 28 04:39 hello_world_2.12-0.1.0-SNAPSHOT.jar
    Try use package to just generate a JAR
    
        root@6be6a0df45c1:/hello_world # sbt package
