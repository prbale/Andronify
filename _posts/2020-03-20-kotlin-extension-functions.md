---
layout: post
title:  "Kotlin Extension Functions"
author: prashant
featured: false
image: assets/images/android_kotlin_extension.jpg
categories: [ coding, development, kotlin ]
tags: featured
---


#### What is an Extension Function? What is an Extension Function?

Kotlin provides an ability to add more functionality to an existing classes without inheriting them.

To add an extension function to a class, define a new function appended to the classname as below:

```
fun String.removeFirstLastChar(): String =  this.substring(1, this.length - 1)

fun main(args: Array<String>) {
    val myString= "prashant"
    val result = myString.removeFirstLastChar()
    println("First character is: $result")
}
```
Output:
```
First character is: rashan
```
---
layout: post
title:  "Kotlin Extension Functions"
author: prashant
featured: true
image: assets/images/android_kotlin_extension.jpg
categories: [ coding, development, kotlin ]
---


#### What is an Extension Function? What is an Extension Function?

Kotlin provides an ability to add more functionality to an existing classes without inheriting them.

To add an extension function to a class, define a new function appended to the classname as below:

```
fun String.removeFirstLastChar(): String =  this.substring(1, this.length - 1)

fun main(args: Array<String>) {
    val myString= "prashant"
    val result = myString.removeFirstLastChar()
    println("First character is: $result")
}
```
Output:
```
First character is: rashan
```

In above example, we have added a method as an extension to existing `string` class. This method will remove the first and last character of the string provided.

[![https://cdn.programiz.com/sites/tutorial2program/files/kotlin-extension-function.jpg](https://cdn.programiz.com/sites/tutorial2program/files/kotlin-extension-function.jpg "Example")](https://cdn.programiz.com/sites/tutorial2program/files/kotlin-extension-function.jpg "https://cdn.programiz.com/sites/tutorial2program/files/kotlin-extension-function.jpg")

> Basically, an extension function is a member function of a class that is defined outside the class.

#### Best Practices
- To replace setters and getters
- To map fields on a property with a better name
- For tiny functions working on a single field.


Thank you.

Happy Coding.. :+1: !!
