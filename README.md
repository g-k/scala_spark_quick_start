###Scala Quick Start

#About Scala

- Compiled language that runs on the JVM
- uses type inference. This means all variables has a definite type - (but you don't neccsarily need to declare them)
- supports interop with Java and Clojure

#Scala Basics

- variables can be declared with as mutable or immutable using the "var" or "val" keywords

```scala
var count = 1
count += 1  // no problem

val age = 39 
age = age + 1 //error: reassignment to val
```

- expression based rather than statement based. This means that every piece of code returns a value

```scala
val count = 3
val evenOrOdd = if (count % 2 == 0) { "even" } else  { "odd" } // equals to the string "odd"
```

- traditional imperitive flow control works as expected - if, for, while, switch
- also supports functional methods like map, filter, reduce as well as pattern matching
- failure and empty states are represented with Option types, and are handled safely with for and flatMap



