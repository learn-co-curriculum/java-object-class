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

```java
public final Class<?> getClass()
```

The method `getClass()` will return the runtime class of the calling object.
This means that the method will return the class that is evaluated at runtime.
Assume we have a class named `Person`:


```java
public class Person {
   private String name;
   private int age;

   public Person(String name, int age) {
      this.name = name;
      this.age = age;
   }
   
}
```

We can use the `getClass()` method on an instance of `String` and
an instance of `Person`:

```java
public class ExampleGetClass {
    public static void main(String[] args) {
        String greeting = "Hello World!";
        System.out.println(greeting.getClass());
        Person p = new Person("Uri", 37);
        System.out.println(p.getClass());
    }
}
```

The output of the above code would look like this:

```text
class java.lang.String
class Person
```

### `hashCode()`

```java
public int hashCode()
```

In computer science, there is a process called **hashing** which maps data to
some integer value using the concept of hashing algorithms. Each object in Java
is linked with some integer value that we call a **hash code**. Hashing helps
us create distinct identifiers for objects in Java.

Java's `Object` class has a method called `hashCode()` which allows us to get
a hashed value of the object. The hashed value that the
`hashCode` method returns is an identifier for the object and can help us
determine if two objects in Java are equal in terms of having the same state.

Here is an example of using the `hashCode()` method:

```java
public class ExampleHash {
   public static void main(String[] args) {
      String greeting1 = "Hello World";
      String greeting2 = "Hello World";
      String greeting3 = "Welcome";
      Person person1 = new Person("Uri", 37);
      Person person2 = new Person("Han", 21);
      Person person3 = new Person("Han", 21);

      System.out.println(greeting1.hashCode());
      System.out.println(greeting2.hashCode());
      System.out.println(greeting3.hashCode());

      System.out.println(person1.hashCode());
      System.out.println(person2.hashCode());
      System.out.println(person3.hashCode());
   }
}
```

An output for the above code shows the same hashcode when the string
literals are equal, or when the object fields have the same values:

```text
-862545276
-862545276
-1397214398
2646042
2242561
2242561
```

A given `String` object will return the same hashed value each time every
time `hashCode()` is called.

For classes such as `Person` that inherit `hashCode()` from the `Object` class,
the same hashed value is returned per object each time `hashCode()` is called within
a single execution of an application, provided the object's state has not changed.
However, the value need not remain the same from one execution of an application
to another execution of the same application.

### `equals()`

```java
public boolean equals(Object obj)
```

The Java `Object` class also has an `equals()` method to help us compare if
two objects are "equal". It will take in another object as a parameter and
compare it to the calling object. The default implementation of `equals()` checks
for identity, thus `p1.equals(p2)` returns `true` if `p1==p2`, meaning both variables
reference the same object.

```java
public class Person {
private String name;
private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

}
```

We can use the `equals` method to check if two variables reference the same `Person`
object as shown:

```java
public class ExampleEquals1 {

   public static void main(String[] args) {
      Person p1 = new Person("Uri", 37);
      Person p2 = new Person("Han", 21);
      Person p3 = new Person("Uri", 37);
      Person p4 = p1;  //same object

      System.out.println(p1.equals(p2));  //false, different objects
      System.out.println(p1.equals(p3));  //false, different objects
      System.out.println(p1.equals(p4));  //true, same object
   }
}
```

As `p1` and `p4` reference the same `Person` object in memory,
only `p1.equals(p4)` evaluates to `true`.

![person equals](https://curriculum-content.s3.amazonaws.com/6677/pillars/equals_person.png)

The program prints:

```text
false
false
true
```

It should also be noted that when comparing `String` values that we will want
to use the `equals()` method as the `==` only works on numbers.

It is highly recommended to override the `equals()` method and the `hashCode()`
method together when we create our own classes to ensure that the objects are
equal in a logical sense. Usually we consider two objects "equal" if they
have the same state, so we override the `equals` method to compare instance
variables. For example, if we had a `Bicycle` class, we would
want to override the `equals()` and `hashCode()` methods to ensure that a red
bike would be indeed equal to another red bike, even though they are different objects.

The logic for overriding `equals` is a bit complex since the parameter
type is `Object`, not `Bicycle`.  

```java
public boolean equals(Object obj)
```

We want to compare `Bicycle` objects by comparing their `color`.
However, the method parameter may reference an object
that may not have a `color` field since we can
pass in *any* class instance.  For example, the code below
shows that we can pass either a `Bicycle` object or a `Person` object
as an argument:


```java
public static void main(String[] args) {
  Bicycle bike1 = new Bicycle("red");
  Bicycle bike2 = new Bicycle("green");
  Bicycle bike3 = new Bicycle("red");
  Bicycle bike4 = bike1;
  Bicycle bike5 = new Bicycle();
  Person person = new Person("Fred", 50);

  //true when hashcodes are equal
  System.out.println(".equals");
  System.out.println(bike1.equals(bike2)); //false
  System.out.println(bike1.equals(bike3)); //true
  System.out.println(bike1.equals(bike4)); //true
  System.out.println(bike1.equals(bike5)); //false
  System.out.println(bike1.equals(person)); //false
}
```

This means we need to use an operator called `instanceof` to check
if the parameter object is a `Bicycle`, then cast the parameter object
to `Bicycle` before getting the `color` field.  The `instanceof` operator
also handles the case of the parameter being `null` and immediately returns `false`.

```java
@Override
public boolean equals(Object o) {
    if (o == this) {
        return true;
    }
    if (!(o instanceof Bicycle)) {
        return false;
    }
    Bicycle other = (Bicycle)o;
    return (this.color == null && other.color == null)
          || (this.color != null && this.color.equals(other.color));
}
```

There are many ways to override `hashCode`, but you should use the same
fields to create a hash value as used for comparison in the `equals` method.
The following implementation returns `0` if `color` is `null`, otherwise
it returns the hashcode based on the string's value.

```java
@Override
public int hashCode() {
  return color == null? 0 : color.hashCode();
}
```


The `Bicycle` class thus overrides `equals` and `hashCode` based on the `color` field
as shown:

```java
import java.util.Objects;

public class Bicycle {
   private String color;

   Bicycle() {
      //color is null
   }

   Bicycle(String color) {
      this.color = color;
   }

   @Override
   public boolean equals(Object o) {
      if (o == this) {
         return true;
      }
      if (!(o instanceof Bicycle)) {
         return false;
      }
      Bicycle other = (Bicycle)o;
      return (this.color == null && other.color == null)
              || (this.color != null && this.color.equals(other.color));
   }

   @Override
   public int hashCode() {
      return color == null? 0 : color.hashCode();
   }
}
```

We can test the `equals` and `hashCode` implementations:

```java
public class ExampleEquals2 {
   public static void main(String[] args) {
      Bicycle bike1 = new Bicycle("red");
      Bicycle bike2 = new Bicycle("green");
      Bicycle bike3 = new Bicycle("red");
      Bicycle bike4 = bike1;
      Bicycle bike5 = new Bicycle();
      Person person = new Person("Fred", 50);

      System.out.println("hashCode");
      System.out.println(bike1.hashCode()); //112816
      System.out.println(bike2.hashCode()); //98619170
      System.out.println(bike3.hashCode()); //same as bike1
      System.out.println(bike4.hashCode()); //same as bike1
      System.out.println(bike5.hashCode()); //31

      //true when same object
      System.out.println("==");
      System.out.println(bike1==bike2); //false
      System.out.println(bike1==bike3); //false
      System.out.println(bike1==bike4); //true
      System.out.println(bike1==bike5); //false

      //true when hashcodes are equal
      System.out.println(".equals");
      System.out.println(bike1.equals(bike2)); //false
      System.out.println(bike1.equals(bike3)); //true
      System.out.println(bike1.equals(bike4)); //true
      System.out.println(bike1.equals(bike5)); //false
      System.out.println(bike1.equals(person)); //false
   }
}
```

The `==` operator evaluates to `true` when the two variables reference the same object, i.e. 
`bike1==bike4`.  We would get the same result if we didn't override `equals()`.  However,
overriding `equals()` to compare the `color` field results in `true` for `Bicycle` objects
having the same color "red"  (`bike1`, `bike3`, and `bike4`).

![bicycle equals](https://curriculum-content.s3.amazonaws.com/6677/pillars/equals_bicycle.png)

The program prints:

```text
hashCode
112785
98619139
112785
112785
0
==
false
false
true
false
.equals
false
true
true
false
false
```


Another possible implementation of `hashCode` is to call a helper
method `hash` defined in the `Objects` utility
class, which can handle a `null` value passed as a parameter.

```java
    @Override
    public int hashCode() {
        return Objects.hash(color);
    }
```

`Objects.hash()` can be passed any number of arguments, for example if
we added a `speed` field we could call `Objects.hash(color, speed)`.


### `toString()`

```java
public String toString()
```

The `toString()` method returns a `String` representation of the instance and is
used to convert the object to a `String`.

If the `toString()` method is not overridden, then the default `String` that
will be returned. The default `String` will have the following syntax

```text
getClass().getName() + '@' + Integer.toHexString(hashCode())
//Bicycle@1b891
```

The first part of the default `String` is the name of the class followed by the
`@` symbol. The last part of the default `String` is a hexadecimal value of the
hash code. Hexadecimal is a different number system that uses a base of 16 rather
than 10, which is the decimal system, and uses characters 0-9 and a-f. It should
be noted again, that the default `String` of `className@hashCode` is **not**
returning the address in memory. It is a common misconception that the hashcode
returns a point in memory.

```java
public class ExampleToString {
   public static void main(String[] args) {
      Bicycle bike = new Bicycle("red");
      System.out.println(bike.toString());
   }
}
```

The result of the above code could look something like this:

```text
Bicycle@1b891
```

The hexadecimal hashed value output here may differ from the hashed value if
you were to run this code on your machine.

It is always recommended to override the `toString()` method to provide a more
helpful and useful `String` value, just as it is recommended to override the
`equals()` and `hashCode()` methods:

```java
import java.util.Objects;

public class Bicycle {
   private String color;

   Bicycle() {
      //color is null
   }

   Bicycle(String color) {
      this.color = color;
   }

   @Override
   public boolean equals(Object o) {
      if (o == this) {
         return true;
      }
      if (!(o instanceof Bicycle)) {
         return false;
      }
      Bicycle other = (Bicycle)o;
      return (this.color == null && other.color == null)
              || (this.color != null && this.color.equals(other.color));
   }

   @Override
   public int hashCode() {
      return color == null? 0 : color.hashCode();
   }

   // Override the toString() method too
   @Override
   public String toString() {
      return "Bicycle " + this.color;
   }
}
```

Now the program outputs:

```text
Bicycle red
```

Finally, let's update `Person`  to override `equals()`, `hashCode()`, and `toString()`:
Notice we use `.equals` to compare `String` object values, and `==` to compare `int`
primitive values.



```java
import java.util.Objects;

public class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public boolean equals(Object o) {
        if (o == this) {
           return true;
        }
        if (!(o instanceof Person)) {
           return false;
        }
        Person other = (Person)o;
        return (this.name == null && other.name == null && this.age == other.age)
                || (this.name != null && this.name.equals(other.name) &&  this.age == other.age);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }

    // Override the toString() method too

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```

## Conclusion

Class `Object` is the root of the class hierarchy, which means
every class has `Object` as a superclass.
It is common for a class to override the `toString()`, `equals()`, and `hashCode()`
methods.

## Resources

- [Java 17 Object](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/lang/Object.html)   
- [Java 17 String hashCode()](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/lang/String.html#hashCode())   
- [Java 17 Objects](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/Objects.html)