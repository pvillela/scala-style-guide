Scala Style Guide
=================

Contents
--------


## Attribution and License

This guide is based on the [Databricks Scala Guide](https://github.com/databricks/scala-style-guide) as of 2016-08-12.  That work is licensed under the [Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License](https://creativecommons.org/licenses/by-nc-sa/4.0/).  Use and adaptation of that work are in accordance with the aforementioned license.

This guide includes portions of the official [Scala Style Guide](http://docs.scala-lang.org/style/, ) and modifications thereof.  The content was downloaded from https://github.com/scala/scala.github.com on 2016-08-09.  That work is licensed under the [Creative Commons Attribution-Share Alike 3.0 Unported license ("CC-BY-SA")](https://creativecommons.org/licenses/by-sa/3.0/).  Use and adaptation of that work are in accordance with the aforementioned license.

**License:**
<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png" /></a><br /> The present work is licensed under the [Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License](https://creativecommons.org/licenses/by-nc-sa/4.0/).


## Introduction

Code is **written once** by its author, but **read and modified multiple times** by lots of other engineers. As most bugs actually come from future modification of the code, we need to optimize our codebase for long-term, global readability and maintainability. The best way to achieve this is to write simple code.


## Document History

- 2016-08-15: Initial version.


## Syntactic Style


### General Alignment and Spacing

#### General Indentation

- Use 2-space indentation in general.

  ```scala
  if (true) {
    println("Wow!")
  }
  ```

- **Do NOT use vertical alignment**. It draws attention to the wrong parts of the code and makes the aligned code harder to change in the future.

  ```scala
  // Don't align vertically
  val plus     = "+"
  val minus    = "-"
  val multiply = "*"

  // Do the following
  val plus = "+"
  val minus = "-"
  val multiply = "*"
  ```

#### Line Length

- Limit lines to 100 characters.

- The only exceptions are import statements and URLs (although even for those, try to keep them under 100 chars).

#### Line Wrapping

- There are times when a single expression reaches a length where it becomes unreadable to keep it confined to a single line. In such cases, the preferred approach is to simply split the expression up into multiple expressions by assigning intermediate results to values. However, this is not always a practical solution.

  * When it is absolutely necessary to wrap an expression across more than one line, each successive line should be indented two spaces from the first. Also remember that Scala requires each “wrap line” to either have an unclosed parenthetical or to end with an infix method in which the right parameter is not given (or the first non-blank character on the next line must be a '.'):

    ```scala
    val result = 1 + 2 + 3 + 4 + 5 + 6 +
      7 + 8 + 9 + 10 + 11 + 12 + 13 + 14 +
      15 + 16 + 17 + 18 + 19 + 20
    ```

    Without this trailing method, Scala will infer a semi-colon at the end of a line which was intended to wrap, throwing off the compilation sometimes without even so much as a warning.

- Line wrapping may be advisable to improve readability even in cases where the line is not very long, when methods are chained:

  ```scala
  // This fits in one line, but not very readable
  val w = List(1, 2, 3, 4).map(x => a*x^5 + b*x^4 + c*x^3).filter(x => d*(x^2) + e*x + f > g)

  // This is more readable
  val z =
    List(1, 2, 3, 4)
    .map(x => a*x^5 + b*x^4 + c*x^3)
    .filter(x => d*(x^2) + e*x + f > g)
  ```

#### Blank Lines

- A single blank line appears:

  * Between consecutive members (or initializers) of a class: fields, constructors, methods, nested classes, static initializers, instance initializers.

    + Exception: A blank line between two consecutive fields (having no other code between them) is optional. Such blank lines are used as needed to create logical groupings of fields.

  * Within method bodies, as needed to create logical groupings of statements.

  * Optionally before the first member or after the last member of the class (neither encouraged nor discouraged).

- Use one or two blank line(s) to separate class definitions.

- Do not use more than two consecutive blank lines.

#### Curly Braces

- Opening curly braces `{` must be on the same line as the declaration they represent:

  ```scala
  def foo = {
    ...
  }
  ```

  *Technically, Scala’s parser does support GNU-style notation with opening braces on the line following the declaration. However, the parser is not terribly predictable when dealing with this style due to the way in which semi-colon inference is implemented. Many headaches will be saved by simply following the curly brace convention demonstrated above.*

- Curly braces should be separated from the code within them by a one-space gap, to give the visually busy braces “breathing room”.

#### Parentheses

- When the opening and closing parentheses are on the same line, there should be no space between parentheses and the code they contain.

- In the rare cases when parenthetical blocks wrap across lines, the opening and closing parentheses should be unspaced and generally kept on the same lines as their content (Lisp-style):

  ```scala
  (this + is a very ++ long *
    expression)
    ```

- The following style is also allowed for long lines with parentheses:

  ```scala
  (  someCondition
  || someOtherCondition
  || thirdCondition
  )
	```

  A trailing parenthesis on the following line is acceptable in this case, for aesthetic reasons.


### Naming Convention

In general, Scala uses "camel case" naming. That is, each word is capitalized, except possibly the first word:

```scala
UpperCamelCase
lowerCamelCase
```

We mostly follow Java's and Scala's standard naming conventions.


#### Classes, Traits, Objects

- Classes, traits, objects should follow Java class convention, i.e., UpperCamelCase style.

  ```scala
  class ClusterManager

  trait Expression

  Object Boot
  ```

  * The exception is objects which are used as private vals inside an enclosing scope (class, object, or method):

    ```scala
    class Foo {

      private object init {
        ...
        val bar: Connection = ...
        val baz: List[String] = ...
        ...
      }

      private val local = fun(init.bar)

      ...
    }
    ```

#### Packages

- Packages should follow Java package naming conventions, i.e. all-lowercase ASCII letters.

  ```scala
  package com.databricks.resourcemanager
  ```


#### Variables, Methods, and Functions

- Variables, methods, and functions should be named in camelCase style and should have self-evident names.


##### Special Note on Brevity

- Because of Scala’s roots in the functional languages, it is quite normal for local names to be very short for: local variables in very short and simple methods; parameters of very short and simple private and local methods; parameters of very simple and short anonymous functions.

  ```scala
  private def add(a: Int, b: Int) = a + b
  
  def foo(): Unit = {
    ...
    val myAddFunction = (a: Int, b: Int) = a + b
    ...
  }
  ```

  While this would be bad practice in languages like Java, it is good practice in Scala. This convention works because properly-written Scala methods are quite short, only spanning a single expression and rarely going beyond a few lines. Very few local fields are ever used (including parameters), and so there is no need to contrive long, descriptive names. This convention substantially improves the brevity of most Scala sources. This in turn improves readability, as most expressions fit in one line and the arguments to methods have descriptive type names.

  This convention does NOT apply to arguments of public methods. Everything in the public interface should be descriptive.

  Do NOT use "l" (as in Larry) as an identifier, because it is very difficult to differentiate "l" from "1", "|", "I".


#### Constants

- Constants should be all uppercase letters and be put in a companion object.

  ```scala
  object Configuration {
    val DEFAULT_PORT = 10000
  }
  ```

#### Enums

- Enums should be UpperCamelCase.

#### Annotations

- Annotations should also follow Java convention, i.e. UpperCamelCase. Note that this differs from Scala's official guide.

  ```scala
  final class MyAnnotation extends StaticAnnotation
  ```

#### Underscores in Names

- Do not use underscores (`_`) in names, except in constants, or in property write methods (`xxx_=`), or (rarely) as a single leading underscore:

  ```scala
  // wrong
  val my_val: Int = ...

  // right
  val MY_CONSTANT: Int = ...

  // OK
  class Company {
    // contains property value
    private var _name: String = _

    // property reader
    def name: String = _name

    // property writer
    def name_=(name: String): Unit = {
      _name = name
    }
  }
  ```

#### Type Parameters

- For simple type parameters, a single upper-case letter (from the English alphabet) should be used, starting with `A` (this is different than the Java convention of starting with `T`). For example:

  ```scala
  class List[A] {
    def map[B](f: A => B): List[B] = ...
  }
  ```

- If the type parameter has a more specific meaning, a descriptive name should be used, following the class naming conventions (as opposed to an all-uppercase style).  Suffixing the name with a 'T' is OK to make it clear that it is a type parameter:

  ```scala
  // Right
  class Map[Key, Value] {
    def get(key: Key): Value
    def put(key: Key, value: Value): Unit
  }

  // Right
  class Map[KeyT, ValueT] {
    def get(key: KeyT): ValueT
    def put(key: KeyT, value: ValueT): Unit
  }

  // Wrong; don't use all-caps
  class Map[KEY, VALUE] {
    def get(key: KEY): VALUE
    def put(key: KEY, value: VALUE): Unit
  }
  ```

 * If the scope of the type parameter is small enough, a mnemonic can be used in place of a longer, descriptive name:

   ```scala
   class Map[K, V] {
     def get(key: K): V
     def put(key: K, value: V): Unit
   }
   ```


#####  Parameterized Type Parameters

- The naming conventions for parameterized type parameters (a.k.a. higher kinds) are generally similar, however it is preferred to use a descriptive name rather than a single letter, for clarity:

  ```scala
  class HigherOrderMap[Key[_], Value[_]] { ... }
  ```

- The single letter form is acceptable for fundamental concepts used throughout a codebase, such as `F[_]` for Functor and `M[_]` for Monad.

  In such cases, the fundamental concept should be something well known and understood to the team, or have tertiary evidence, such as the following:

  ```scala
  def doSomething[M[_]: Monad](m: M[Int]) = ...
  ```

  Here, the type bound `: Monad` offers the necessary evidence to inform the reader that `M[_]` is the type of the Monad.


### Class Declarations

#### Header and Constructor

Class constructors should be declared all on one line, unless the line becomes “too long” (beyond 100 characters). In that case, put each constructor argument on its own line, indented four spaces:

```scala
class Person(name: String, age: Int) {
  def firstMethod: Foo = ...
  ...
}

class Person(
    name: String,
    age: Int,
    birthdate: Date,
    astrologicalSign: String,
    shoeSize: Int,
    favoriteColor: java.awt.Color) {
  def firstMethod: Foo = ...
  ...
}
```

If a class/object/trait extends anything, the same general rule applies, put it on one line unless it goes over about 100 characters.  If it exceeds 100 characters, indent four spaces with each item being on its own line and two spaces for extensions; this provides visual separation between constructor arguments and extensions.  Also put a blank line at the top of the body of the class:

```scala
class Person(
    name: String,
    age: Int,
    birthdate: Date,
    astrologicalSign: String,
    shoeSize: Int,
    favoriteColor: java.awt.Color)
  extends Entity
  with Logging
  with Identifiable
  with Serializable {

  def firstMethod: Foo = ...
  ...
}
```

#### Ordering Of Class Elements

All class/object/trait members should be declared interleaved with newlines. The only exceptions to this rule are var and val. These may be declared without the intervening newline, but only if none of the fields have Scaladoc and if all of the fields have simple (max of 20-ish chars, one line) definitions:

```scala
class Foo {
  val bar = 42
  val baz = "Daniel"

  def doSomething(): Unit = { ... }

  def add(x: Int, y: Int): Int = x + y
}
```

Fields should precede methods in a scope. 

- The only exception is if the val has a block definition (more than one expression) and performs operations which may be deemed “method-like” (e.g., computing the length of a List). In such cases, the non-trivial val may be declared at a later point in the file as logical member ordering would dictate. 
  * This exception only applies to val and lazy val (**never to var**)! It becomes very difficult to track changing aliases if var declarations are strewn throughout class file.

If a class is long and has many methods, group them logically into different sections, and use comment headers to organize them.

```scala
class DataFrame {

  /////////////////////////////////////
  // DataFrame operations

  ...

  /////////////////////////////////////
  // RDD operations

  ...
}
```

Of course, the situation in which a class grows this long is strongly discouraged, and is generally reserved only for building certain public APIs.


### Imports

- **Avoid using wildcard imports**, unless you are importing more than 6 entities, or implicit methods. Wildcard imports make the code less robust to external changes.

- Always import packages using absolute paths (e.g. `scala.util.Random`) instead of relative ones (e.g. `util.Random`).

- In addition, sort imports in the following order:
  * Project classes
  * `scala.*`
  * `play.*`
  * `akka.*`
  * `java.*`
  * `javax.*`
  * Third-party libraries (`org.*`, `com.*`, etc)

- Within each group, imports should be sorted in alphabetic ordering.

- You can use IntelliJ's import organizer to handle this automatically, using the following config:

  ```
  com.mycompany
  _______ blank line _______
  scala
  play
  akka
  _______ blank line _______
  java
  javax
  _______ blank line _______
  all other imports
  ```


### Method Declarations

#### Return Type

**Specify a return type for all public members**. Consider it documentation checked by the compiler. It also avoids confusion as weird subtypes may result from type inference.  Finally, it also helps in preserving binary compatibility in the face of changing type inference (changes to the method implementation may propagate to the return type if it is inferred).

Local methods or private methods may omit their return type:

```scala
private def foo(x: Int = 6, y: Int = 7) = x + y
```

#### Modifiers

Method modifiers should be given in the following order (when each is applicable):

1. Annotations, each on their own line
2. Override modifier (override)
3. Access modifier (protected, private)
4. Final modifier (final)
5. def

```scala
@Transaction
@throws(classOf[IOException])
override protected final def foo() {
  ...
}
```

#### Declaring Methods of Arity 0

Methods that take no arguments should be declared without parentheses **if and only if** they are accessors that have no side-effect (state mutation, I/O operations are considered side-effects).

```scala
class Job {
  // Wrong: killJob changes state. Should have ().
  def killJob: Unit

  // Correct:
  def killJob(): Unit
}

case class Person(firstName: String, lastName: String) {
  // Correct:
  def fullName: String = firstName + " " + lastName
}
```

Note that internal domain-specific languages have a tendency to break the guidelines given above for the sake of syntax. Such exceptions should not be considered a violation so much as a time when these rules do not apply. In a DSL, syntax should be paramount over convention.

#### Short Functional Methods

When a method body comprises a single expression which is less than 30 (or so) characters, it should be given on a single line with the method:

```scala
def add(a: Int, b: Int): Int = a + b
```

When the method body is a single expression longer than 30 (or so) characters but still shorter than 70 (or so) characters, it should be given on the following line, indented two spaces:

```scala
def sum(ls: List[String]): Int =
  ls.map(_.toInt).foldLeft(0)(_ + _)
```

The distinction between these two cases is somewhat artificial. Generally speaking, you should choose whichever style is more readable on a case-by-case basis. For example, your method declaration may be very long, while the expression body may be quite short. In such a case, it may be more readable to put the expression on the next line rather than making the declaration line too long.

#### Longer Methods or Methods With Side-Effects

When the body of a method cannot be concisely expressed in a single line or is of a non-functional nature (some mutable state, local or otherwise), the body must be enclosed in braces:

```scala
def sum(ls: List[String]): Int = {
  val ints = ls map (_.toInt)
  ints.foldLeft(0)(_ + _)
}
```

When method declarations don't fit in a single line, use 4 space indentation for its parameters. Return types can be either on the same line as the last parameter, or put to next line with a 2 space indent.

```scala
def newAPIHadoopFile[K, V, F <: NewInputFormat[K, V]](
    path: String,
    fClass: Class[F],
    kClass: Class[K],
    vClass: Class[V],
    conf: Configuration = hadoopConfiguration): RDD[(K, V)] = {
  // method body
}

def newAPIHadoopFile[K, V, F <: NewInputFormat[K, V]](
    path: String,
    fClass: Class[F],
    kClass: Class[K],
    vClass: Class[V],
    conf: Configuration = hadoopConfiguration)
  : RDD[(K, V)] = {
  // method body
}
```

#### Methods With Default Parameter Values

Methods with default parameter values should be declared in an analogous fashion
to the above, with a space on either side of the equals sign:

```scala
def foo(x: Int = 6, y: Int = 7): Int = x + y

def sum(ls: List[String] = Nil): Int = {
  val ints = ls map (_.toInt)
  ints.foldLeft(0)(_ + _)
}

```

#### Methods With a Single Match Expression

Methods that contain a single match expression should be declared in the following way:

```scala
// right!
def sum(ls: List[Int]): Int = ls match {
  case hd :: tail => hd + sum(tail)
  case Nil => 0
}
```

Not like this:

```scala
// wrong!
def sum(ls: List[Int]): Int = {
  ls match {
    case hd :: tail => hd + sum(tail)
    case Nil => 0
  }
}
```

#### Procedure Syntax

**Do not use the procedure syntax**.  It is confusing and it is in the process
of being deprecated.

```scala
// don't do this
def printBar(bar: Baz) {
  println(bar)
}

// write this instead
def printBar(bar: Bar): Unit = {
  println(bar)
}
```

### Field Declarations

Fields should follow the declaration rules for methods, taking special note of access modifier ordering and annotation conventions.

Lazy vals should use the `lazy` keyword directly before the `val`:

```scala
private lazy val foo = bar()
```


### Method Invocations

#### Invoking Methods of Arity 0

Call a method of arity-0 with parentheses if and only if it is declared with parentheses (see [Declaring Methods of Arity 0](#declaring-methods-of-arity-0)). _Note that this is not just syntactic. It can affect correctness when `apply` is defined in the returned object._

```scala
class Foo {
def apply(args: String*): Int
}

class Bar {
def foo: Foo
}

new Bar().foo  // This returns a Foo
new Bar().foo()  // This returns an Int!
```

Calling methods of arity-0 using suffix notation is unsafe and deprecated:

```scala
names.toList

// is the same as

names toList // Unsafe, don't use!
```

Failure to follow this rule may result in unexpected compile errors at best, and happily compiled faulty code at worst.

#### Infix Methods

**Avoid infix notation** for methods that aren't symbolic methods (i.e., other than operator overloading).
```scala
// Correct
list.map(func)
string.contains("foo")

// Wrong
list map (func)
string contains "foo"

// But overloaded operators should be invoked in infix style
arrayBuffer += elem
```


#### Methods With Multiple Arguments

When a method invocation with several arguments doesn't fit in a single line, put the method invocation on the next line, indented two spaces, and each argument on a line by itself, indented two additioinal spaces from the current indent level:

```scala
// right!
val myOnerousAndLongFieldNameWithNoRealPoint =
  foo(
    someVeryLongFieldName,
    andAnotherVeryLongFieldName,
    "this is a string",
    3.1415)

// wrong!
val myOnerousAndLongFieldNameWithNoRealPoint = foo(someVeryLongFieldName,
                                                   andAnotherVeryLongFieldName,
                                                   "this is a string",
                                                   3.1415)
```

Better yet, just try to avoid defining methods which take more than two or three parameters!

#### Methods Taking a Function as a Parameter

When calling a method with a closure (or partial function) as a parameter, if there is only one case, put the case on the same line as the method 
invocation.

```scala
list.zipWithIndex.map { case (elem, i) =>
  // ...
}
```

If there are multiple cases, indent and wrap them.

```scala
list.map {
  case a: Foo =>  ...
  case b: Bar =>  ...
}
```


### Control Structures

All control structures should be written with a space following the defining keyword:

```scala
// right!
if (foo) bar else baz
for (i <- 0 to 10) { ... }
while (true) { println("Hello, World!") }

// wrong!
if(foo) bar else baz
for(i <- 0 to 10) { ... }
while(true) { println("Hello, World!") }
```

Use or omit braces in control structures according to the following rules:

- **if** - Omit braces if you have an else clause and both branches are single-line pure-functional expressions (no side-effects). Otherwise, surround the contents with curly braces even if the contents are only a single line.

- **while** - Never omit braces (while cannot be used in a pure-functional manner).

- **for** - Omit braces after the yield clause if its expression fits on the same line or as a single next line. If there is no yield clause, surround the contents with curly-braces, even if the contents are only a single line.

- **case** - Always omit braces in case clauses.

- **Trivial conditionals** - There are certain situations where it is useful to create a short if/else expression for nested use within a larger expression. In Java, this sort of case would traditionally be handled by the ternary operator (?/:), a syntactic device which Scala lacks. In these situations (and really any time you have a extremely brief if/else expression) it is permissible to place the “then” and “else” branches on the same line as the if and else keywords. Note that this style should never be used with imperative if expressions nor should curly braces be employed.

Examples:

```scala
val news = if (foo)
  goodNews()
else
  badNews()

if (foo) {
  println("foo was true")
}

while (bar()) {
  doSomething()
}

for (x <- baz)
  yield x * 2

for (x <- baz) {
  println(x * 2)
}

for (x <- baz)
yield {
  val y = something(x)
  val z = somethingElse(x)
  y + z
}

// Equivalent to above
for {
  x <- baz)
  y = something(x)
  z = somethingElse(x)
} yield y + z

news match {
  case "good" =>
    println("Good news!")
    goodNews()
  case "bad" =>
    println("Bad news!")
    badNews()
}

val res = if (foo) bar else baz
```


#### Comprehensions

Scala has the ability to represent for-comprehensions with more than one generator (usually, more than one `<-` symbol). In such cases, there are two alternative syntaxes which could be used:

```scala
// wrong!
for (x <- board.rows; y <- board.files)
  yield (x, y)

// right!
for {
  x <- board.rows
  y <- board.files
} yield (x, y)
```

Use the second form for all **for-comprehensions of more than one generator**.

- The exceptions to this rule are **for-comprehensions which lack a yield clause**. In such cases, the construct is actually a loop rather than a functional comprehension and it is usually more readable to string the generators together between parentheses rather than using the syntactically-confusing `} {` construct:

  ```scala
  // wrong!
  for {
    x <- board.rows
    y <- board.files
  } {
    printf("(%d, %d)", x, y)
  }
  // right!
  for (x <- board.rows; y <- board.files) {
    printf("(%d, %d)", x, y)
  }
  ```

**Comprehensions with only a single generator** (e.g., `for (i <- 0 to 10) yield i`) should use the first form (parentheses rather than curly braces).

Finally, `for` comprehensions are preferred to chained calls to `map`, `flatMap`, and `filter`, as they can get difficult to read (this is one of the purposes of the `for` comprehension).


### Function Values

#### Single-Expression Functions

Scala provides a number of different syntactic options for declaring function
values (a.k.a. anonymous functions).  Although the following four declaration
styles are possible, (1) and (4) are allowed, (2) and (3) are not:

```scala
// yes
val f1 = ((a: Int, b: Int) => a + b)
val f1a = (a: Int, b: Int) => a + b

// no
val f2 = (a: Int, b: Int) => a + b

// no
val f3 = (_: Int) + (_: Int)

// yes
val f4: (Int, Int) => Int = (_ + _)
val f4a: (Int, Int) => Int = ((a, b) => a + b)
val f4b: (Int, Int) => Int = (a, b) => a + b
```

#### Multi-Expression Functions

Most function values are less trivial than the examples given above and contain
more than one expression. In such cases, the declaration should follow the
declaration style for methods, with the opening brace on the same line as the
assignment or use of the function, while the closing brace is on its own line
immediately following the last line of the function. Parameters should be on
the same line as the opening brace, as should the “arrow” (`=>`):

```scala
val f1 = { (a: Int, b: Int) =>
  val sum = a + b
  sum
}
```

**Avoid excessive parentheses and curly braces** for function values:

```scala
// Correct
list.map { item =>
  ...
}

// Correct
list.map(item => ...)

// Wrong
list.map(item => {
  ...
})

// Wrong
list.map { item => {
  ...
}}

// Wrong
list.map({ item => ... })
```

#### Type Inference

As noted elsewhere in this guide, function values should leverage type inference whenever possible.


### Long Literals

Suffix long literal values with uppercase `L`. It is often hard to differentiate lowercase `l` from `1`.

```scala
val longValue = 5432L  // Do this

val longValue = 5432l  // Do NOT do this

```


### Rule of 30

"If an element consists of more than 30 subelements, it is highly probable that there is a serious problem" - see [Refactoring in Large Software Projects](http://www.amazon.com/Refactoring-Large-Software-Projects-Restructurings/dp/0470858923).

In general:

- A method should contain no more than 30 lines of code.
- A class should contain no more than 30 methods (in most cases, no more than **10**).


## Scala Language Features


### apply Method

Avoid defining apply methods on classes that do not implement a function interface. These methods tend to make the code less readable, especially for people less familiar with Scala. It is also harder for IDEs (or grep) to trace. In the worst case, it can also affect correctness of the code in surprising ways, as demonstrated in [Method Invocations](#method-invocations).

It is acceptable to define apply methods on companion objects as factory methods. In these cases, the apply method should return the companion class type.

```scala
object TreeNode {
  // This is OK
  def apply(name: String): TreeNode = ...

  // This is bad because it does not return a TreeNode
  def apply(name: String): String = ...
}
```


### override Modifier
Always add override modifier for methods, both for overriding concrete methods and implementing abstract methods. The Scala compiler does not require `override` for implementing abstract methods. However, we should always add `override` to make the override obvious, and to avoid accidental non-overrides due to non-matching signatures.

```scala
trait Parent {
  def hello(data: Map[String, String]): Unit = {
    print(data)
  }
}

class Child extends Parent {
  import scala.collection.Map

  // The following method does NOT override Parent.hello,
  // because the two Maps have different types.
  // If we added "override" modifier, the compiler would've caught it.
  def hello(data: Map[String, String]): Unit = {
    print("This is supposed to override the parent method, but it is actually not!")
  }
}
```


### Call by Name

**Exercise extreme caution when using call by name parameters**.  Implementations using call by name parameters must ensure the expression passed in is executed only once (e.g., assign it to a val and then use the val).

Background: Scala allows method parameters to be defined by-name, e.g. the following would work:

```scala
def print(value: => Int): Unit = {
  println(value)
  println(value + 1)
}

var a = 0
def inc(): Int = {
  a += 1
  a
}

print(inc())
```
in the above code, `inc()` is passed into `print` as a closure and is executed (twice) in the print method, rather than being passed in as a value `1`. The main problem with call-by-name is that the caller cannot differentiate between call-by-name and call-by-value, and thus cannot know for sure whether the expression will be executed or not (or maybe worse, multiple times). This is especially dangerous for expressions that have side-effects.

If the print method above is modified as follows then the problem is avoided:

```scala
def print(_value: => Int): Unit = {
  val value = _value
  println(value)
  println(value + 1)
}
```


### Multiple Parameter Lists

**Avoid using multiple parameter lists**:

```scala
// Avoid this!
case class Person(name: String, age: Int)(secret: String)
```

Such methods (or similarly declared functions) have a more verbose declaration and invocation syntax and are harder for less-experienced Scala developers to understand.

There are exceptions to this rule, but **these exceptions only apply to architecture framework developers**:

1. For a **fluent API**

   Multiple parameter lists allow you to create your own “control structures”:
  
  ```scala
  def unless(exp: Boolean)(code: => Unit): Unit = if (!exp) code

  unless(x < 5) {
    println("x was not less than five")
  }
  ```

2. **Implicit Parameters**

  When using implicit parameters, and you use the implicit keyword, it applies to the entire parameter list. Thus, if you want only some parameters to be implicit, you must use multiple parameter lists.  That said, [implicits should be avoided](#implicits)!

3. **For type inference**

   When invoking a method using only some of the parameter lists, the type inferencer can allow a simpler syntax when invoking the remaining parameter lists.


### Accessors/Mutators

Scala does *not* follow the Java convention of prepending `set`/`get` to
mutator and accessor methods (respectively). Instead, the following
conventions are used:

-   For accessors of properties, the name of the method should be the
    name of the property.

-   In some instances, it is acceptable to prepend "\`is\`" on a boolean
    accessor (e.g. `isEmpty`). This should only be the case when no
    corresponding mutator is provided.

-   For mutators, the name of the method should be the name of the
    property with "`_=`" appended. As long as a corresponding accessor
    with that particular property name is defined on the enclosing type,
    this convention will enable a call-site mutation syntax which
    can be expressed as an assignment to the property.

```scala
class Foo {

  def bar = ...

  def bar_=(bar: Bar) {
    ...
  }

  def isBaz = ...
}

val foo = new Foo
foo.bar             // accessor
foo.bar = bar2      // mutator
foo.isBaz           // boolean property
```


### Symbolic Methods -- Operator Overloading

**Do NOT define methods with symbolic names**, unless you are defining them for natural arithmetic operations (e.g. `+`, `-`, `*`, `/`). Under no other circumstances should they be used. Symbolic method names make it very hard to understand the intent of the methods. Consider the following two examples:

```scala
// symbolic method names are hard to understand
channel ! msg
stream1 >>= stream2

// self-evident what is going on
channel.send(msg)
stream1.join(stream2)
```


### Type Inference

Use type inference where possible, but put clarity first, and favour explicitness in public APIs.

- You should almost never annotate the type of a private field or a local variable, as their type will usually be immediately evident in their value:

  ```scala
  private val name = "Daniel"
  ```

There are a few cases where explicit typing should be used:

- **Public methods and fields should be explicitly typed**, for API  clarity and also as otherwise the compiler's inferred type can often surprise you.

- **Local variables, private fields, and closures with non-obvious types should be explicitly typed**. A good litmus test is that explicit types should be used if a code reviewer cannot determine the type in 3 seconds.

- **Implicit methods should be explicitly typed**, otherwise it can crash the Scala compiler with incremental compilation.

- Where type annotations are appropriate, they should be patterned according to the following template:

  ```scala
  value: Type
  ```

  There is no space between the value identifier and the colon, there is one space between the colon and the type.


### Function Types

Function types should be declared with a space between the parameter type, the arrow and the return type:

```scala
def foo(f: Int => String) = ...
def bar(f: (Boolean, Double) => List[String]) = ...
```

For functions of arity-1, the parentheses should be omitted from the parameter type:

```scala
// right
def foo(f: Int => String) = ...

// wrong
def foo(f: (Int) => String) = ...
```

In general, omit unnecessary parentheses in funtion types.  A more extreme example with a curried function parameter:

```scala
// wrong!
def foo(f: (Int) => ((String) => ((Boolean) => Double))) = ...

// wrong!
def foo(f: (Int) => (String) => (Boolean) => Double) = ...

// right!
def foo(f: Int => String => Boolean => Double) = ...
```

### Return Statements

**Do not use return.**


### Recursion and Tail Recursion

**Avoid using recursion**, unless the problem can be naturally framed recursively (e.g. graph traversal, tree traversal) and the required traversal combinators don't already exist in the standard libraries.

Use of combinators such as map, filter, foreach, and fold, as well as for expressions, obviate the need for direct use of recursion in the vast majority of situations. The solutions using the combinators are almost always shorter and clearer.

For methods that are meant to be tail recursive, apply `@tailrec` annotation to make sure the compiler can check it is tail recursive. (You will be surprised how often seemingly tail recursive code is actually not tail recursive due to the use of closures and functional transformations.)

In some cases, using an imperative loop may be the best answer, but this should be avoided in most cases unless there is a performance tuning justification.


### Implicits

**Avoid using implicits**, unless:
- you are building a domain-specific language
- you are using it for implicit type parameters (e.g. `ClassTag`, `TypeTag`)
- you are using it privately to your own class to reduce verbosity of converting from one type to another (e.g. Scala closure to Java closure)

When implicits are used, we must ensure that another engineer who did not author the code can understand the semantics of the usage without reading the implicit definition itself. Implicits have complicated resolution rules and make the code base difficult to understand. From Twitter's Effective Scala guide: "If you do find yourself using implicits, always ask yourself if there is a way to achieve the same thing without their help."

If you must use them (e.g. enriching some DSL), do not overload implicit methods, i.e. make sure each implicit method has distinct names, so users can selectively import them.

```scala
// Don't do the following, as users cannot selectively import only one of the methods.
object ImplicitHolder {
  def toRdd(seq: Seq[Int]): RDD[Int] = ...
  def toRdd(seq: Seq[Long]): RDD[Long] = ...
}

// Do the following:
object ImplicitHolder {
  def intSeqToRdd(seq: Seq[Int]): RDD[Int] = ...
  def longSeqToRdd(seq: Seq[Long]): RDD[Long] = ...
}
```


### Exception Handling (Try vs try)

- **Do not catch Throwable or Exception.** Use `scala.util.control.NonFatal`:

  ```scala
  try {
    ...
  } catch {
    case NonFatal(e) =>
      // handle exception; note that NonFatal does not match InterruptedException
    case e: InterruptedException =>
      // handle InterruptedException
  }
  ```

  This ensures that we do not catch `NonLocalReturnControl` (as explained in [Return Statements](#return-statements)).

- **Do not use `Try` in API signatures**, that is, do NOT return `Try` in any public methods. Instead, prefer explicitly throwing exceptions for abnormal execution.

  *Background information: Scala provides monadic error handling (through `Try`, `Success`, and `Failure`) that facilitates chaining of actions.*

  As a contrived example:

  ```scala
  class UserService {
    /** Look up a user's profile in the user database. */
    def get(userId: Int): Try[User]
  }
  ```

  is better written as

  ```scala
  class UserService {
    /**
     * Look up a user's profile in the user database.
     * @return None if the user is not found.
     * @throws DatabaseConnectionException when we have trouble connecting to the database
     */
    @throws(DatabaseConnectionException)
    def get(userId: Int): Option[User]
  }
  ```

  The 2nd one makes it very obvious error cases the caller needs to handle.

- **Using Try versus try/catch is not black-and-white** and depends on the situation.  The use of Try in many cases makes the error-handling logic cleaner.  But there are situations where using Try leads to more levels of nesting that are harder to understand than equivalent try/catch/finally blocks.


### Options

- Use `Option` when the value can be empty. Compared with `null`, an `Option` explicitly states in the API contract that the value can be `None`.
- When constructing an `Option`, use `Option` rather than `Some` to guard against `null` values.

  ```scala
  def myMethod1(input: String): Option[String] = Option(transform(input))

  // This is not as robust because transform can return null, and then
  // myMethod2 will return Some(null).
  def myMethod2(input: String): Option[String] = Some(transform(input))
  ```

- Do not use None to represent exceptions. Instead, throw exceptions explicitly.

- Do not call `get` directly on an `Option`, unless you know absolutely for sure the `Option` has some value.


### Monadic Chaining

One of Scala's powerful features is monadic chaining. Almost everything (e.g. collections, Option, Future, Try) is a monad and operations on them can be chained together. This is an incredibly powerful concept, but chaining should be used sparingly. In particular:

- Avoid chaining (and/or nesting) more than 3 operations.
- If it takes more than 5 seconds to figure out what the logic is, try hard to think about how you can express the same functionality without using monadic chaining. As a general rule, watch out for flatMaps and folds.
- A chain should almost always be broken after a flatMap (because of the type change).

A chain can often be made more understandable by giving the intermediate result a variable name, by explicitly typing the variable, and by breaking it down into more procedural style. As a contrived example:

```scala
class Person(val data: Map[String, String])
val database = Map[String, Person]
// Sometimes the client can store "null" value in the  store "address"

// A monadic chaining approach
def getAddress(name: String): Option[String] = {
  database.get(name).flatMap { elem =>
    elem.data.get("address")
      .flatMap(Option.apply)  // handle null value
  }
}

// A more readable approach, despite much longer
def getAddress(name: String): Option[String] = {
  if (!database.contains(name)) {
    return None
  }

  database(name).data.get("address") match {
    case Some(null) => None  // handle null value
    case Some(addr) => Option(addr)
    case None => None
  }
}

```


## Source Files

As a default rule, files should contain a *single* logical compilation unit, i.e., a single class, trait or object.

The exceptions to this rule are:

- Classes or traits which have companion objects. Companion objects should be grouped with their corresponding class or trait in the same file. These files should be named according to the class, trait or object they contain:

  ```scala
  package com.novell.coolness

  class Inbox { ... }

  // companion object
  object Inbox { ... }
  ```

  These compilation units should be placed within a file named
`Inbox.scala` within the `com/novell/coolness` directory. In short, the Java file naming and positioning conventions should be preferred, despite the fact that Scala allows for greater flexibility in this regard.

  The object definition should come after the class definiiton in the file.

- A sealed trait and its sub-classes (often emulating the ADT language feature available in functional languages):

  ```scala
  sealed trait Option[+A]

  case class Some[A](a: A) extends Option[A]

  case object None extends Option[Nothing]
```

  Because of the nature of sealed superclasses (and traits), all subtypes *must* be included in the same file.

- When multiple classes logically form a single, cohesive group, sharing concepts to the point where maintenance is greatly served by containing them within a single file.

  However, keep in mind that when multiple units are contained within a single file, it is often more difficult to find specific units when it comes time to make changes.

**All multi-unit files containing units with different names (not just a class and its companion object) should be given camelCase names with a lower-case first letter.** This is a very important convention. It differentiates multi- from single-unit files, greatly easing the process of finding declarations. These filenames may be based upon a significant type which they contain (e.g.,s `option.scala` for the example above), or may be descriptive of the logical property shared by all units within (e.g., `ast.scala`).


## Java Interoperability

This section covers guidelines for building Java compatible APIs. **These do not apply if the component you are building does not require interoperability with Java.**


### Traits and Abstract Classes

For interfaces that can be implemented externally, keep in mind that raits with default method implementations are not usable in Java. Use abstract classes instead.

```scala
// The default implementation doesn't work in Java
trait Listener {
  def onTermination(): Unit = { ... }
}

// Works in Java
abstract class Listener {
  def onTermination(): Unit = { ... }
}
```


### Type Aliases

Do NOT use type aliases. They are not visible in bytecode (and Java).


### Default Parameter Values

Do NOT use default parameter values. Overload the method instead.

```scala
// Breaks Java interoperability
def sample(ratio: Double, withReplacement: Boolean = false): RDD[T] = { ... }

// The following two work
def sample(ratio: Double, withReplacement: Boolean): RDD[T] = { ... }
def sample(ratio: Double): RDD[T] = sample(ratio, withReplacement = false)
```

### Multiple Parameter Lists

Do NOT use multi-parameter lists.

### Varargs

- Apply `@scala.annotation.varargs` annotation for a vararg method to be usable in Java. The Scala compiler creates two methods, one for Scala (bytecode parameter is a Seq) and one for Java (bytecode parameter array).

  ```scala
  @scala.annotation.varargs
  def select(exprs: Expression*): DataFrame = { ... }
  ```

- Note that abstract vararg methods does NOT work for Java, due to a Scala compiler bug ([SI-1459](https://issues.scala-lang.org/browse/SI-1459), [SI-9013](https://issues.scala-lang.org/browse/SI-9013)).

- Be careful with overloading varargs methods. Overloading a vararg method with another vararg type can break source compatibility.

  ```scala
  class Database {
    @scala.annotation.varargs
    def remove(elems: String*): Unit = ...

    // Adding this will break source compatibility for no-arg remove() call.
    @scala.annotation.varargs
    def remove(elems: People*): Unit = ...
  }

  // This won't compile anymore because it is ambiguous
  new Database().remove()
  ```
  Instead, define an explicit first parameter followed by vararg:

  ```scala
  class Database {
    @scala.annotation.varargs
    def remove(elems: String*): Unit = ...

    // The following is OK.
    @scala.annotation.varargs
    def remove(elem: People, elems: People*): Unit = ...
  }
  ```


### Implicits

Do NOT use implicits, for a class or method. This includes `ClassTag`, `TypeTag`.

```scala
class JavaFriendlyAPI {
  // This is NOT Java friendly, since the method contains an implicit parameter (ClassTag).
  def convertTo[T: ClassTag](): T
}
```


### Companion Objects, Static Methods, and Fields

There are a few things to watch out for when it comes to companion objects and static methods/fields.

- Methods in companion objects are automatically turned into static methods in the companion class (unless there is a method name conflict with a method with the same name in the class).

- Companion objects are awkward to use in Java (a companion object `Foo` is a static field `MODULE$` of type `Foo$` in class `Foo$`).

  ```scala
  object Foo

  // equivalent to the following Java code
  public class Foo$ {
    Foo$ MODULE$ = // instantiation of the object
  }
  ```

  If the companion object is important to use, create a Java static field in a separate class.

- There is no way to define a JVM static field in Scala. Create a Java file to define that if necessary.


## Concurrency

Concurrency concerns will be addressed by the provided architecture libraries/frameworks and any required custom extensions or frameworks.  Except for such specialized architecture frameworks, application code should not explicitly deal with concurrency concerns.


## Performance

For the vast majority of the code you write, performance should not be a concern. However, for performance sensitive code, here are some tips:

### Microbenchmarks

It is ridiculously hard to write a good microbenchmark because the Scala compiler and the JVM JIT compiler do a lot of magic to the code. More often than not, your microbenchmark code is not measuring the thing you want to measure.

Use [jmh](http://openjdk.java.net/projects/code-tools/jmh/) if you are writing microbenchmark code. Make sure you read through [all the sample microbenchmarks](http://hg.openjdk.java.net/code-tools/jmh/file/tip/jmh-samples/src/main/java/org/openjdk/jmh/samples/) so you understand the effect of deadcode elimination, constant folding, and loop unrolling on microbenchmarks.


### Traversal and zipWithIndex

Use `while` loops instead of `for` loops or functional transformations (e.g. `map`, `foreach`). For loops and functional transformations are very slow (due to virtual function calls and boxing).

```scala
val arr = // array of ints
// zero out even positions
val newArr = list.zipWithIndex.map { case (elem, i) =>
  if (i % 2 == 0) 0 else elem
}

// This is a high performance version of the above
val newArr = new Array[Int](arr.length)
var i = 0
val len = newArr.length
while (i < len) {
  newArr(i) = if (i % 2 == 0) 0 else arr(i)
  i += 1
}
```

### Option and null

For performance sensitive code, prefer `null` over `Option`, in order to avoid virtual method calls and boxing. Label the nullable fields clearly with Nullable.

```scala
class Foo {
  @javax.annotation.Nullable
  private[this] var nullableField: Bar = _
}
```

### Scala Collection Library

For performance sensitive code, prefer Java collection library over Scala ones, since the Scala collection library often is slower than Java's.

### private[this]

For performance sensitive code, prefer `private[this]` over `private`. `private[this]` generates a field, rather than creating an accessor method. In our experience, the JVM JIT compiler cannot always inline `private` field accessor methods, and thus it is safer to use `private[this]` to ensure no virtual method call for accessing a field.

```scala
class MyClass {
  private val field1 = ...
  private[this] val field2 = ...

  def perfSensitiveMethod(): Unit = {
    var i = 0
    while (i < 1000000) {
      field1  // This might invoke a virtual method call
      field2  // This is just a field access
      i += 1
    }
  }
}
```


## Miscellaneous

### Prefer nanoTime over currentTimeMillis

When computing a *duration* or checking for a *timeout*, avoid using `System.currentTimeMillis()`. Use `System.nanoTime()` instead, even if you are not interested in sub-millisecond precision.

`System.currentTimeMillis()` returns current wallclock time and will follow changes to the system clock. Thus, negative wallclock adjustments can cause timeouts to "hang" for a long time (until wallclock time has caught up to its previous value again).  This can happen when ntpd does a "step" after the network has been disconnected for some time. The most canonical example is during system bootup when DHCP takes longer than usual. This can lead to failures that are really hard to understand/reproduce. `System.nanoTime()` is guaranteed to be monotonically increasing irrespective of wallclock changes.

Caveats:

- Never serialize an absolute `nanoTime()` value or pass it to another system. The absolute value is meaningless and system-specific and resets when the system reboots.

- The absolute `nanoTime()` value is not guaranteed to be positive (but `t2 - t1` is guaranteed to yield the right result)


### Prefer URI over URL

When storing the URL of a service, you should use the `URI` representation.

The [equality check](http://docs.oracle.com/javase/7/docs/api/java/net/URL.html#equals(java.lang.Object)) of `URL` actually performs a (blocking) network call to resolve the IP address. The `URI` class performs field equality and is a superset of `URL` as to what it can represent.



## Scaladoc

It is important to provide documentation for all packages, classes, traits, methods, and other members. Scaladoc generally follows the conventions of Javadoc, however there are many additional features to make writing scaladoc simpler.

In general, you want to worry more about substance and writing style than in formatting. Scaladocs need to be useful to new users of the code as well as experienced users. Achieving this is very simple: increase the level of detail and explanation as you write, starting from a terse summary (useful for experienced users as reference), while providing deeper examples in the detailed sections (which can be ignored by experienced users, but can be invaluable for newcomers).

The general format for a Scaladoc comment should be as follows:

```scala
/** This is a brief description of what's being documented.
  *
  * This is further documentation of what we're documenting.  It should
  * provide more details as to how this works and what it does. 
  */
def myMethod = {}
```

For methods and other type members where the only documentation needed
is a simple, short description, this format can be used:

```scala
/** Does something very simple */
def simple = {}
```

Note, especially for those coming from Java, that the left-hand margin
of asterisks falls under the \_third\_ column, not the second, as is
customary in Java.

See the [AuthorDocs](https://wiki.scala-lang.org/display/SW/Writing+Documentation)
on the Scala wiki for more technical info on formatting Scaladoc.  See also this [Scaladoc tutorial](https://gist.github.com/VladUreche/8396624)

### General Style

It is important to maintain a consistent style with Scaladoc. It is also
important to target Scaladoc to both those unfamiliar with your code and
experienced users who just need a quick reference. Here are some general
guidelines:

-   Get to the point as quickly as possible. For example, say "returns
    true if some condition" instead of "if some condition return true".
-   Try to format the first sentence of a method as "Returns XXX", as in
    "Returns the first element of the List", as opposed to "this method
    returns" or "get the first" etc. Methods typically **return**
    things.
-   This same goes for classes; omit "This class does XXX"; just say
    "Does XXX"
-   Create links to referenced Scala Library classes using the
    square-bracket syntax, e.g. `[[scala.Option]]`
-   Summarize a method's return value in the `@return` annotation,
    leaving a longer description for the main Scaladoc.
-   If the documentation of a method is a one line description of what
    that method returns, do not repeat it with an `@return` annotation.
-   Document what the method *does do* not what the method *should do*.
    In other words, say "returns the result of applying f to x" rather
    than "return the result of applying f to x". Subtle, but important.
-   When referring to the instance of the class, use "this XXX", or
    "this" and not "the XXX". For objects, say "this object".
-   Make code examples consistent with this guide.
-   Use the wiki-style syntax instead of HTML wherever possible.
-   Examples should use either full code listings or the REPL, depending
    on what is needed (the simplest way to include REPL code is to
    develop the examples in the REPL and paste it into the Scaladoc).
-   Make liberal use of `@macro` to refer to commonly-repeated values
    that require special formatting.

### Packages

Provide Scaladoc for each package. This goes in a file named
`package.scala` in your package's directory and looks like so (for the
package `parent.package.name.mypackage`):

```scala
package parent.package.name

/** This is the Scaladoc for the package. */
package object mypackage {
}
```

A package's documentation should first document what sorts of classes
are part of the package. Secondly, document the general sorts of things
the package object itself provides.

While package documentation doesn't need to be a full-blown tutorial on
using the classes in the package, it should provide an overview of the
major classes, with some basic examples of how to use the classes in
that package. Be sure to reference classes using the square-bracket
notation:

```scala
  package my.package
  /** Provides classes for dealing with complex numbers.  Also provides
    * implicits for converting to and from `Int`.
    *
    * ==Overview==
    * The main class to use is [[my.package.complex.Complex]], as so
    * {{ "{{{" }}
    * scala> val complex = Complex(4,3)
    * complex: my.package.complex.Complex = 4 + 3i
    * }}}
    *
    * If you include [[my.package.complex.ComplexConversions]], you can 
    * convert numbers more directly
    * {{ "{{{" }}
    * scala> import my.package.complex.ComplexConversions._
    * scala> val complex = 4 + 3.i
    * complex: my.package.complex.Complex = 4 + 3i
    * }}} 
    */
  package complex {}
```

### Classes, Objects, and Traits

Document all classes, objects, and traits. The first sentence of the
Scaladoc should provide a summary of what the class or trait does.
Document all type parameters with `@tparam`.

#### Classes

If a class should be created using it's companion object, indicate as
such after the description of the class (though leave the details of
construction to the companion object). Unfortunately, there is currently
no way to create a link to the companion object inline, however the
generated Scaladoc will create a link for you in the class documentation
output.

If the class should be created using a constructor, document it using
the `@constructor` syntax:

```scala
/** A person who uses our application.
  *
  * @constructor create a new person with a name and age.
  * @param name the person's name
  * @param age the person's age in years 
  */
class Person(name: String, age: Int) {
}
```

Depending on the complexity of your class, provide an example of common
usage.

#### Objects

Since objects can be used for a variety of purposes, it is important to
document *how* to use the object (e.g. as a factory, for implicit
methods). If this object is a factory for other objects, indicate as
such here, deferring the specifics to the Scaladoc for the `apply`
method(s). If your object *doesn't* use `apply` as a factory method, be
sure to indicate the actual method names:

```scala
/** Factory for [[mypackage.Person]] instances. */
object Person {
  /** Creates a person with a given name and age.
    *
    * @param name their name
    * @param age the age of the person to create 
    */
  def apply(name: String, age: Int) = {}

  /** Creates a person with a given name and birthdate
    *
    * @param name their name
    * @param birthDate the person's birthdate
    * @return a new Person instance with the age determined by the 
    *         birthdate and current date. 
    */
  def apply(name: String, birthDate: java.util.Date) = {}
}
```

If your object holds implicit conversions, provide an example in the
Scaladoc:

```scala
/** Implicit conversions and helpers for [[mypackage.Complex]] instances.
  *
  * {{ "{{{" }}
  * import ComplexImplicits._
  * val c: Complex = 4 + 3.i
  * }}} 
  */
object ComplexImplicits {}
```

#### Traits

After the overview of what the trait does, provide an overview of the
methods and types that must be specified in classes that mix in the
trait. If there are known classes using the trait, reference them.

### Methods and Other Members

Document all methods. As with other documentable entities, the first
sentence should be a summary of what the method does. Subsequent
sentences explain in further detail. Document each parameter as well as
each type parameter (with `@tparam`). For curried functions, consider
providing more detailed examples regarding the expected or idiomatic
usage. For implicit parameters, take special care to explain where
these parameters will come from and if the user needs to do any extra
work to make sure the parameters will be available.
