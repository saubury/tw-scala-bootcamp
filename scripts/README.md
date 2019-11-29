This document covers the following getting started topics:

- Using the REPL
- Making Hello World
- Variables with Vals and Vars
- Scala Types

1. REPL and Hello World
    
    You can now run scala and type commands interactively on the console, this is called a REPL.
    
        root@6be6a0df45c1:/# scala
    
        Welcome to Scala 2.11.12 (OpenJDK 64-Bit Server VM, Java 1.8.0_222).
        Type in expressions for evaluation. Or try :help.
    
        scala>println(“Hello”)
        Hello
    
2. Making Hello World
    Lets make a program, create a text file like so:
    
        cat > HelloWorld2.scala << EOF
        object HelloWorld2 {
          def main(args: Array[String]): Unit = {
            println("Hello, world!")
          }
        }
        EOF
    
    Compile it
    
        scalac  HelloWorld2.scala
    
    Note the additional file, its complicated, but the class with the $ has the concrete implementation.
    
        root@6be6a0df45c1:/# ls -l HelloWorld2*
        -rw-r--r-- 1 root root 637 Nov 28 00:30 'HelloWorld2$.class'
        -rw-r--r-- 1 root root 586 Nov 28 00:30  HelloWorld2.class
        -rw-r--r-- 1 root root  97 Nov 28 00:29  HelloWorld2.scala
    
    Lets run it
    
        scala  HelloWorld2
    
    To reinforce the point Scala is built as a library on top of Java, you can run your scala code with the JRE if you include the scala library in the classpath
    
        java -classpath /usr/share/scala-2.11/lib/scala-library.jar:. HelloWorld2


2. Variables with Vals and Vars
    
    In scala, 
    - Use var keyword to declare a variable that is mutable
    - Use val keyword to declare a variable that is immutable
    
    Using val is preferred as they are safer for concurrency and often faster to
    
    See explanation from Dick Wall [here](https://drive.google.com/open?id=1QDwUvuXvw9LelKeWS27PQ2XBWuEmzRzi)

3. Scala Types

    See explanation from Dick Wall [here](https://drive.google.com/open?id=12USAowhtmiDYDFMNUvo2TRUcbP90bDZV)
