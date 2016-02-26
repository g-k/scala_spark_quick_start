#Scala Quick Start

##About Scala

- Compiled language that runs on the JVM
- uses type inference. This means all variables has a definite type - (but you don't neccsarily need to declare them)
- supports interop with Java and Clojure

##Scala Basics

###Variables can be declared with as mutable or immutable using the "var" or "val" keywords

```scala
var count = 1
count += 1  // no problem

val age = 39 
age = age + 1 //error: reassignment to val
```

###Expression based rather than statement based. This means that every piece of code returns a value

```scala
val count = 3
val evenOrOdd = if (count % 2 == 0) { "even" } else  { "odd" } // equals to the string "odd"
```

###supports functional methods like map, filter, reduce as well as pattern matching

- (also traditional imperitive flow control works as expected - if, for, while, switch)

```scala
def isEven(x:Int) = x % 2 == 0
val nums = Range(1,10)
val evens = nums.filter( x => isEven(x) ) /// 2,4,6,8
val totalEvens = evens.reduce(_ + _) // 20

//or more succinctly:
val totalEvens = Range(1,10)
  .filter(isEven)
  .reduce(_+_)  // "_" is a place holder for the collection elements being reduced
```

###failure and empty states are represented with Option types, and are handled safely with for and flatMap

Perhaps we have a list of people, and we wish to transform the data:
```scala
val candidates = List( ("Hillary Clinton", "68"), ("Bernie Sanders", "74"), ("Donald Trump", ""), ("Ted Cruz","45"), ("Marco Rubio", "44"))

//naively, we might apply a mapping:
val newCandidates = candidates.map{  cn => (cn._1 , cn._2.toInt) };
//but encounter an error: java.lang.NumberFormatException: For input string: ""
```

so we can use a type to represent the success or failure of our operation:

```scala
  def parseCandidate(name:String, age:String) : Option[(String,Int)]  = {
    try {
      Some( (name, age.toInt)  ) //return the candidate with the parsed age
    } catch  {
      case _ : Throwable => None // returns and empty Option
    }
  }

```

now if we perform our mapping, we get:

```scala 
candidates.map( cn => parseCandidate(cn._1,cn._2))
//returns List(Some((Hillary Clinton,68)), Some((Bernie Sanders,74)), None, Some((Ted Cruz,45)), Some((Marco Rubio,44)))
```

Which is definitely an improvement because the offending candidate has been excised.
But a problem remains: we don't want "Some" objects - we want the names and ages.
Furthermore, its bothersome to filter out the empty "None" object. Fortunately we have the "flatMap" function which will releive us of this burden:

```scala
candidates.flatMap( cn => parseCandidate(cn._1,cn._2))
//returns  List((Hillary Clinton,68), (Bernie Sanders,74), (Ted Cruz,45), (Marco Rubio,44))
```

The 'flatMap' function is [useful in many other ways](http://twitter.github.io/effectivescala/#Functional programming-`flatMap`), but I won't go into that right now.

##Using scala in the REPL

We'll be using the scala REPL in one of three ways:

1. Standalone (just type "scala")
2. SBT console (running "sbt console" will fire up the REPL with all you dependencies and classes available for import)
3. Spark Shell (executing the spark-shell gives you a full interactive REPL that can perform any spark function)

Let's start with the last one because its the most interesting. To experiment with Spark, (download a prebuilt package)[https://spark.apache.org/downloads.html].
As of February 2016, Spark 1.6 is the latest version supported by (Amazon EMR)[https://aws.amazon.com/elasticmapreduce/], so that's what we'll use.

Once you've unpacked it, go to the spark directory and start the shell:

```
~/project $ cd spark-1.6.0-bin-hadoop2.6
~/project/spark-1.6.0-bin-hadoop2.6 $ ./bin/spark-shell
...
Welcome to
      ____              __
     / __/__  ___ _____/ /__
    _\ \/ _ \/ _ `/ __/  '_/
   /___/ .__/\_,_/_/ /_/\_\   version 1.6.0
      /_/
scala>
```

