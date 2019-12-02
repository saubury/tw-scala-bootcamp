This document covers the following getting started topics:

- Using the REPL
- Making Hello World
- Variables with Vals and Vars
- Scala Types
- Reading Input
- Splitting strings
- Use a sliding window on an Array

Before we get started, we want to set the scene with this scenario [Moving Averages](https://git.thoughtworks.net/afraser/moving_averages)

1. REPL and Hello World
    
    You can now run scala and type commands interactively on the console, this is called a REPL.
    
        root@6be6a0df45c1:/# scala
    
        Welcome to Scala 2.11.12 (OpenJDK 64-Bit Server VM, Java 1.8.0_222).
        Type in expressions for evaluation. Or try :help.
    
        scala>println("Hello")
        Hello
    
2. Variables with Vals and Vars
    
    In scala,
    - Use var keyword to declare a variable that is mutable
    - Use val keyword to declare a variable that is immutable
    
    Using val is preferred as they are safer for concurrency and often faster to
    
    See explanation from Dick Wall [here](https://drive.google.com/open?id=1QDwUvuXvw9LelKeWS27PQ2XBWuEmzRzi)

3. Scala Types

    See explanation from Dick Wall [here](https://drive.google.com/open?id=12USAowhtmiDYDFMNUvo2TRUcbP90bDZV)

4. Collections

    Scala has versatile collections support. Let's count word frequencies in the REPL.

    We'll start with some text from [this book](https://www.gutenberg.org/files/11/11-h/11-h.htm#link2HCH0001).

        scala> val lines = Seq("So she was considering in her own mind (as well as she could, for the hot day made her feel very sleepy and stupid),", "whether the pleasure of making a daisy-chain would be worth the trouble of getting up and picking the daisies, when suddenly a White Rabbit with pink eyes ran close by her.")
        
        lines: Seq[String] = List(So she was considering in her own mind (as well as she could, for the hot day made her feel very sleepy and stupid),, whether the pleasure of making a daisy-chain would be worth the trouble of getting up and picking the daisies, when suddenly a White Rabbit with pink eyes ran close by her.)

    Let's remove punctuation characters.

        scala> val linesWithoutPunctuation = lines.map(line => line.replaceAll("[(),.]", ""))

        linesWithoutPunctuation: Seq[String] = List(So she was considering in her own mind as well as she could for the hot day made her feel very sleepy and stupid, whether the pleasure of making a daisy-chain would be worth the trouble of getting up and picking the daisies when suddenly a White Rabbit with pink eyes ran close by her)

    And split into words.

        scala> val words = linesWithoutPunctuation.map(line => line.split(" "))

        words: Seq[Array[String]] = List(Array(So, she, was, considering, in, her, own, mind, as, well, as, she, could, for, the, hot, day, made, her, feel, very, sleepy, and, stupid), Array(whether, the, pleasure, of, making, a, daisy-chain, would, be, worth, the, trouble, of, getting, up, and, picking, the, daisies, when, suddenly, a, White, Rabbit, with, pink, eyes, ran, close, by, her))

    That doesn't look like an ideal list of words. Let's flatten this list of lists.

        scala> val words = linesWithoutPunctuation.flatMap(line => line.split(" "))

        words: Seq[String] = List(So, she, was, considering, in, her, own, mind, as, well, as, she, could, for, the, hot, day, made, her, feel, very, sleepy, and, stupid, whether, the, pleasure, of, making, a, daisy-chain, would, be, worth, the, trouble, of, getting, up, and, picking, the, daisies, when, suddenly, a, White, Rabbit, with, pink, eyes, ran, close, by, her)

    We'll group matching words together in order to move towards our final result. "_" is a shorthand that's useful when we don't care to name the function variable.

        scala> words.groupBy(_.toLowerCase)

        res0: scala.collection.immutable.Map[String,Seq[String]] = HashMap(in -> List(in), whether -> List(whether), ran -> List(ran), a -> List(a, a), getting -> List(getting), eyes -> List(eyes), by -> List(by), with -> List(with), could -> List(could), hot -> List(hot), close -> List(close), trouble -> List(trouble), for -> List(for), picking -> List(picking), feel -> List(feel), would -> List(would), pleasure -> List(pleasure), suddenly -> List(suddenly), own -> List(own), up -> List(up), so -> List(So), as -> List(as, as), well -> List(well), she -> List(she, she), daisies -> List(daisies), making -> List(making), was -> List(was), mind -> List(mind), worth -> List(worth), be -> List(be), her -> List(her, her, her), stupid -> List(stupid), pink -> List(pink), made -> List(made), very -> L...

    And here it is.

        scala> words.groupBy(_.toLowerCase).view.mapValues(_.length).toMap

        res1: scala.collection.immutable.Map[String,Int] = HashMap(in -> 1, whether -> 1, ran -> 1, a -> 2, getting -> 1, eyes -> 1, by -> 1, with -> 1, could -> 1, hot -> 1, close -> 1, trouble -> 1, for -> 1, picking -> 1, feel -> 1, would -> 1, pleasure -> 1, suddenly -> 1, own -> 1, up -> 1, so -> 1, as -> 2, well -> 1, she -> 2, daisies -> 1, making -> 1, was -> 1, mind -> 1, worth -> 1, be -> 1, her -> 3, stupid -> 1, pink -> 1, made -> 1, very -> 1, white -> 1, considering -> 1, when -> 1, day -> 1, of -> 2, and -> 2, sleepy -> 1, rabbit -> 1, daisy-chain -> 1, the -> 4)

    Or more efficiently:

        scala> words.groupMapReduce(_.toLowerCase)(_ => 1)(_ + _)

        res2: scala.collection.immutable.Map[String,Int] = HashMap(in -> 1, whether -> 1, ran -> 1, a -> 2, getting -> 1, eyes -> 1, by -> 1, with -> 1, could -> 1, hot -> 1, close -> 1, trouble -> 1, for -> 1, picking -> 1, feel -> 1, would -> 1, pleasure -> 1, suddenly -> 1, own -> 1, up -> 1, so -> 1, as -> 2, well -> 1, she -> 2, daisies -> 1, making -> 1, was -> 1, mind -> 1, worth -> 1, be -> 1, her -> 3, stupid -> 1, pink -> 1, made -> 1, very -> 1, white -> 1, considering -> 1, when -> 1, day -> 1, of -> 2, and -> 2, sleepy -> 1, rabbit -> 1, daisy-chain -> 1, the -> 4)

5. Moving beyond the REPL writing programs, compiling and running

    Let's make a program, create a text file like so:
    
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
    
    Let's run it
    
        scala  HelloWorld2
    
    To reinforce the point Scala is built as a library on top of Java, you can run your scala code with the JRE if you include the scala library in the classpath
    
        java -classpath /usr/share/scala-2.11/lib/scala-library.jar:. HelloWorld2

6. Using a build tool (sbt)
    
    With scala projects, it is common to use the `Simple build tool` commonly known as `sbt`. This is akin to maven in the java world or rake in the ruby world.
    
    This should be installed in the docker image. To get started, we need to make a build configuration called `build.sbt`

        cat > build.sbt << EOF
            name := "HelloWorld3"
            version := "0.1"
            scalaVersion := "2.12.7"
        EOF

    We can then use commands with sbt like:
    
    - sbt package
    - sbt run

    We can add dependencies to the project by adding ines like:
    
        cat >> build.sbt << EOF
            libraryDependencies += "org.scalatest" % "scalatest_2.12" % "3.0.5" % "test"
        EOF
    
    `sbt` also supports plug-ins that provide additional funcationality such as making runnable JAR's but we wont cover them here.
    
7. Reading Input

    To read input from the console use:
    
        import scala.io.{Source, StdIn}
        
        object HelloWorld3 {
        
          def main(args: Array[String]) {
        
            // Read Input data
            val input = StdIn.readLine()
            println(input)
          }
        }

8. Splitting strings

    We first need to start with a test, so lets try:
    
        cat > HelloWorld3Test.scala << EOF
            import org.scalatest.FlatSpec
            
            class HelloWorld3Test extends FlatSpec {
              "String to Array of doubles" should "return an array" in {
                // Given
                val input = "1 2 3 4 5"
                val expected = Array(1,2,3,4,5).map(_.toDouble)
                // When
                val result = HelloWorld3.stringToDoubles(input, " ")
                // Then
                assert(result sameElements expected)
              }
            }
            EOF

    To test, run:

        sbt test

    To make the test pass and process the text, implement the method to do it
        
        def stringToDoubles(input: String, delimiter: String): Array[Double] =
            input.split(delimiter).map(_.toDouble)

9. Use a sliding window function

    We now need a method to calculate the average over a sliding window, so lets start with a test for this:
    
        head -n -1 HelloWorld3Test.scala > HelloWorld3Test.txt ; mv HelloWorld3Test.txt HelloWorld3Test.scala
        cat >> HelloWorld3Test.scala << EOF
              "getMovingAvg with 3" should "return an array of doubles based on window size of 3" in {
                // Given ..
                val input = (1 to 100).map(_.toDouble).toArray
                val expected = (2 to 99).map(_.toDouble).toArray
                // When ..
                val result = Calculator.getMovingAvg(input, 3)
                // Then ..
                assert(result sameElements expected)
              }
              }
              EOF
            
    We can then create a method to perform a series of maps:

        def getMovingAvg(inputValues: Array[Double], windowSize: Int): Array[Double] =
            inputValues.sliding(windowSize)
                .map(_.sum)
                .map(_ / windowSize)
                .toArray

    Now run the tests to make sure they pass:

        sbt test
    
10. Tie it all together

    Now that we have our program, it has 3 methods:

    - main()
    - stringToDoubles()
    - getMovingAvg()
    
    If we modify the `main()` method we can call the 2 subsequent methods to run from the command line.
    
    Add the following lines:
    
        // Parse the input
        val data = stringToDoubles(input, delimiter)
    
        // Call Calculator.getMovingAvg()
        val result = getMovingAvg(data, windowSize)

        // Format the output
        println(result.mkString(delimiter))
