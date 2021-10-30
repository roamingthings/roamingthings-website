+++
author = "Alexander Sparkowsky"
categories = ["Kotlin", "JPA"]
date = 2017-07-09T07:45:49Z
description = ""
draft = false
slug = "kotlin-data-class-in-bi-directional-jpa-relations"
tags = ["Kotlin", "JPA"]
title = "Using Kotlin \"data class\" in bi-directional JPA relations"

+++

[`Data classes`](https://kotlinlang.org/docs/reference/data-classes.html) in Kotlin provide an easy way to implement entity beans for JPA without a lot of boilerplate code. This also includes `toString()`, `hashCode()` and `equals()` methods.

However when you are modelling a bi-directional relationship between to entities you run into trouble since the `toString()` methods of both entities will call each other recursively ending in a `StackOverflowException` eventually.

```
@Entity
data class Child(
        @Id @GeneratedValue
        val id: Long? = null,
        val name: String,
        @ManyToOne
        val parent: Parent? = null)

@Entity
data class Parent(
        @Id @GeneratedValue
        val id: Long? = null,
        val name: String,
        @ManyToOne(mappedBy = "parent")
        val children: MutableList<Child>? = null)

```

Solving this problem is easy. Since you can overwrite the generated `toString()` method with your own implementation you can do so handling this special case e.g. by not calling `toString()` on the parent class in the `toString() method of the child class. Alternatively you can print a single property of the parent or any other value

```
@Entity
data class Child(
        @Id @GeneratedValue
        val id: Long? = null,
        val name: String,
        @ManyToOne
        val parent: Parent? = null) {

    override fun toString(): String {
        return "Child(id=$id, name='$name', parent=${parent?.name})"
    }
}

```

