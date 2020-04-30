---
layout: post
title:  "Kotlin Sealed Class"
author: prashant
featured: false
image: assets/images/kotlin-sealed-class-banner.png
categories: [ coding, development, kotlin ]
---

**Sealed classes** in Kotlin are another new concept which was not there in Java. As the name suggests, sealed classes are closed and making them restricted or bounded class hierarchies.

Restricted means, we have specific number of sub classes similar to Enum. But the major difference between Enum and Sealed classes are in the enum we only have one instance per type whereas sealed classes can have multiple instances of the same class.

Sealed classes ensure type-safety by restricting the types to be matched at compile-time rather than at runtime.

------------
Declaration of Sealed Class:

To define a sealed class, just precede the class modifier with the sealed keyword.

```
sealed class Demo
```


> The sealed classes also have one another distinct feature, their constructors are private by default.


------------

A sealed class is implicitly abstract and hence it cannot be instantiated.

```
sealed class Demo
fun main(args: Array)
{
    var demo = Demo()     //compiler error
}
```
------------

Lets consider Enum example and then we will discuss what benefits Sealed classes are giving us.

Using Enum,

```
enum class Result(val message: String) {
    SUCCESS("Success message"),
    ERROR("Error message")
}
```
But,
```
enum class Result(val message: String) {
    SUCCESS("Success message"),
    ERROR(val exception: Exception) // This is not possible using Enum.
}
```
To solve above issue, we need sealed classes.

```
sealed class Result() {
    class SUCCESS(val message: String): Result()
    class ERROR(val errormessage: String): Result()
}

fun eval(c: Result) =
    when (c) {
        is Result.SUCCESS -> println(c.message)
        is Result.ERROR -> println(c.errormessage)
        else -> println("Else Part")
	}

fun main() {
    val result = Result.SUCCESS("Successful message !!")
    eval(result)

	val error1 = Result.ERROR("404 Error !!")
    eval(error1)

	val error2 = Result.ERROR("501 Error !!")
    eval(error2)
}
```

You might think, Can we use Abstract class over Sealed class. But there is one benefit which makes the Sealed class win again.

Yes, Sealed classes are abstract and cannot be instantiated directly. But abstract classes can have their hierarchies anywhere in the project. And Sealed classes should contain all the hierarchies at the single place. This will also makes the code more restricted hence cleaner.  

> Sealed classes help us to write clean and concise code!


Thank you.

Happy Coding.. !!
