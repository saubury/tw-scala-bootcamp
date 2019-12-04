# Extra Scala Examples
Taken from https://www.scala-lang.org/

# Type Inference
```
import scala.util.Random

class Person(val name: String, val age: Int) {
  override def toString = s"$name ($age)"
}

def underagePeopleNames(persons: List[Person]) = {
  for (person <- persons; if person.age < 18)
    yield person.name
}

def createRandomPeople() = {
  val names = List("Alice", "Bob", "Carol",
      "Dave", "Eve", "Frank")
  for (name <- names) yield {
    val age = (Random.nextGaussian()*8 + 20).toInt
    new Person(name, age)
  }
}

val people = createRandomPeople()

underagePeopleNames(people)
```

