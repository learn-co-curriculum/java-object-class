# Java's Object Class

## Learning Goals

- Explain the Java `Object` Class
- Learn about the various methods that every class inherits

## Introduction

Every class in Java directly or indirectly inherits Java's `Object` class.
Consider the following diagram:

![Class Hierarchy](https://curriculum-content.s3.amazonaws.com/6677/pillars/object_class_hierarchy.png)

In this diagram, the following hierarchy is represented:

1. Classes that do not extend any other class explicitly will directly
   inherit Java's `Object` class:
    1. `String` extends `Object`.
    2. `Number` extends `Object`.
    3. `Person` extends `Object`.
2. Classes that do extend other classes will then indirectly inherit from
   Java's `Object` class:
    1. `Integer` extends `Number`, which extends `Object`.
    2. `Double` extends `Number`, which extends `Object`.
    3. `BigDecimal` extends `Number`, which extends `Object`.
    4. `Student` extends `Person`, which extends `Object`.
    5. `Teacher` extends `Person`, which extends `Object`.

Note: Java's `Object` class is poorly named because an "object" is usually an
instance of a class. But the class that all classes ultimately and implicitly
extend from is called `Object`. In this course, as in most technical content, we
use the term "object" to refer to the instance of a class. We will make sure the
use the term "Java's Object class" when we are referring to the root class in
Java.

## Object Class Methods

Since every class inherits from the `Object` class, this means that every class
inherits the Java `Object` class' built-in methods. These are methods that can
be used by any class or even overridden. Let's look at some of these methods
and how they might be helpful:

### `getClass()`

The method `getClass()` will return the runtime class of the calling object.
This means that the method will return the class that is evaluated at runtime.
Here is an example using the `getClass()` method on an instance of `String` and
an instance of `Bicycle`:

```java
public class Bicycle {
   public static void main(String[] args) {
      String greeting = "Hello World!";
      System.out.println(greeting.getClass());
      Bicycle bicycle = new Bicycle();
      System.out.println(bicycle.getClass());
   }
}
```

The output of the above code would look like this:

```text
class java.lang.String
class Bicycle
```

### `hashCode()`

In computer science, there is a process called **hashing** which maps data to
some integer value using the concept of hashing algorithms. Each object in Java
is linked with some integer value that we call a **hash code**. Hashing helps
us create distinct identifiers for objects in Java.

Java's `Object` class has a method called `hashCode()` which allows us to get
a hashed value of the object. The hashed value that the
`hashCode` method returns is an identifier for the object and can help us
determine if two objects in Java are "equal".

Here is an example of using the `hashCode()` method:

```java
public class Example {
   public static void main(String[] args) {
      String greeting1 = "Hello World";
      String greeting2 = "Hello World";
      String greeting3 = "Welcome";

      System.out.println(greeting1.hashCode());
      System.out.println(greeting2.hashCode());
      System.out.println(greeting3.hashCode());
   }
}
```

An output for the above code may look like this:

```text
-862545276
-862545276
-1397214398
```

It should be noted that the hashed values here may differ from the hashed
values if you were to run this code on your machine.

### `equals()`

The Java `Object` class also has an `equals()` method to help us compare if
two objects are "equal". It will take in another object as a parameter and
compare it to the calling object. If the objects are equal to each other, then
the method will return true, otherwise it will return false. For example:

```java
public class Example {
    public static void main(String[] args) {
        String greeting1 = "Hello World";
        String greeting2 = "Hello World";
        String greeting3 = "Welcome";

        System.out.println(greeting1.equals(greeting2));
        System.out.println(greeting2.equals(greeting3));
    }
}
```

If we were to run the code above, the result would look like this:

```text
true
false
```

It should also be noted that when comparing `String` values that we will want
to use the `equals()` method as the `==` only works on numbers.

It is highly recommended to override the `equals()` method and the `hashCode()`
method together when we create our own classes to ensure that the objects are
equal in a logical sense. For example, if we had a `Bicycle` class, we would
want to override the `equals()` and `hashCode()` methods to ensure that a red
bike would be indeed equal to another red bike.

```java
public class Bicycle {
    private String color;
    
    Bicycle(String color) {
        this.color = color;
    }
    
    @Override
    public boolean equals(Bicycle bicycle) {
        return this.color.equals(bicycle.color);
    }
    
    @Override
    public int hashCode() {
        return this.color.hashCode();
    }
}
```

### `toString()`

The `toString()` method returns a `String` representation of the instance and is
used to convert the object to a `String`.

If the `toString()` method is not overridden, then the default `String` that
will be returned. The default `String` will have the following syntax

```text
getClass().getName() + '@' + Integer.toHexString(hashCode())
// Bicycle@d716361
```

The first part of the default `String` is the name of the class followed by the
`@` symbol. The last part of the default `String` is a hexadecimal value of the
hash code. Hexadecimal is a different number system that uses a base of 16 rather
than 10, which is the decimal system, and uses characters 0-9 and a-f. It should
be noted again, that the default `String` of `className@hashCode` is **not**
returning the address in memory. It is a common misconception that the hashcode
returns a point in memory.

```java
public class Bicycle {
   public static void main(String[] args) {
      Bicycle bicycle = new Bicycle();
      System.out.println(bicycle.toString());
   }
}
```

The result of the above code could look something like this:

```text
Bicycle@d716361
```

The hexadecimal hashed value output here may differ from the hashed value if
you were to run this code on your machine.

It is always recommended to override the `toString()` method to provide a more
helpful and useful `String` value, just as it is recommended to override the
`equals()` and `hashCode()` methods:

```java
public class Bicycle {
    private String color;
    
    Bicycle(String color) {
        this.color = color;
    }
    
    @Override
    public boolean equals(Bicycle bicycle) {
        return this.color.equals(bicycle.color);
    }
    
    @Override
    public int hashCode() {
        return this.color.hashCode();
    }
    
    // Override the toString() method too
   @Override
   public String toString() {
        return "Bicycle " + this.color;
   }
}
```

## Conclusion

Class `Object` is the root of the class hierarchy, which means
every class has `Object` as a superclass.
It is common for a class to override the `toString()`, `equals()`, and `hashCode()`
methods.

## Resources

[Java 17 Object API](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/lang/Object.html)