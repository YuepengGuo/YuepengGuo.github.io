---
title: Scala
date: 2016-08-06 19:57:36
tags: 工具
top: 100
---

### CH1 A Scalable Language

 #### growing new type

An example, scala.BigInt: arbitrarily large without overflow 
```
def factorial(x:BigInt) : BigInt =
	if(x==0) 1 else x * factorial(x-1)
```
BigInt looks like a built-in type because you can use integer literals and operators such as * and - with values of that type.

 #### growing new control
```
	recipient ! msg
```
actor model as a pleasant means for concurrent and distributed computations.

scalable by combination of oo and functional programming.

scala is oo lang in pure form: every value is an object and every operation is a method call.
1 + 2 in scala is actually invoking a method named + in class Int. Similarly, ! in previous example is a method of the Actor class.

traits are like interface in java, but they can have method implementation and even fields. 
objects are constructed by mixin composition, which takes the members of a class and adds the members of a number of traits to them.

functional : 
 * functions are first-class values provide a convenient means for abstracting over operations and creating new control structures. this generalization of functions provides great expressiveness, which often leads to very legible and concise programs.

	```
	val xs = 1 to 3
	val it = xs.iterator
	eventually {it.next() shouldBe 3}
	```
	eventually will repeatedly execute the function until the assertion succeeds.
 * the second main idea of functional programming is that the operations of a program should map input values to output values rather than change data in place.
 	Immutable data structures are one of the cornerstones of functional lang. immutable lists,tuples,maps, and sets.
 	methods should not have any side effects, to make it referentially transparent.(scala avoid imperative style, it favors functional alternatives.)

compatible
scala not only reuse java's types, but also dresses them up to make them nicer. scala's strings support toInt or toFloat. scala compiler will do that for you.

concise
scala compiler will do a lot of things for us , like
```
class MyClass(index: Int, name: String)
```
will convert to...

mix different libs together by traits.

high-level
scala helps you manage complexity by letting you raise the level of abstraction in the interfaces you design and use.

```
	val nameHasUpperCase = name.exists(_.isUpper)

	s.exists(p) || s.exists(q) == s.exists(x => p(x) || q(x)) 
```
scala treats strings as higher-level sequences of characters that can be queried with predicates.

statically typed
scala has a very sophisticated type inference system.


### CH2 First steps in scala

scala expression if(x>y) x else y behaves similarly to "(x>y)? x : y" in java.
**if a function consists of just one statement, you can optionally leave off the curly braces.**

command line arguments to a scala script are available via scala array named args. you can access an element by specifying an index in parentheses, **args(0) not args[0]** not square brackets.

iterate with foreach and for

```
args.foreach(arg => println(arg))
args.foreach((arg:String) => println(arg))
for(arg<-args) println(arg)
```

### CH3 Next steps in scala
```
for (i<- 0 to 2)
	print(greetStrings(i))
##explicitly specify the receiver of the method call.
Console println 10
```
the first line of code illustrates one general rule of scala, if a method takes only one parameter, you can call it without a dot or parentheses. to is actually a method takes only one int argument.

All operations are method calls in scala.

When you apply parentheses surrounding one or more values to a variable, scala will transform  the code into an invocation of a method named **apply** on that variable. So greetStrings(i) get transformed into greetStrings.apply(i). Thus accessing an element of an array in scala is simply a method call like any other.

similaryly, when an assignment is made to a variable to which parentheses and one or more arguments have been applied, the compiler will transform that into an invocation of an **update** method .

Scala way to create and iniitialize arrays 

```
val numNames = Array("zero","one","two")
#calling factory method named apply on the Array companion object.
```

1::List(2,3) cons is right operand. as List(2,3).::(1)

Read lines from file
scala.io.Source.fromFile(args(0)).getLines()


### CH4 Classes and Objects

one important characteristic of method parameters in scala is that they are vals, not vars.
if you attempt to reassign a parameter inside a method in scala, it won't compile.

The recommended style for methods is in fact to avoid having explicit, and especially multiple, return statements. Instead, think of each method as an expression that yield one value, which is returned.

this philosophy will encourage you to make methods quite small.

Singleton

when a singleton object shares the same name with a class, it is called that class's companion object.
You must define both the class and its companion object in the same source file.

App trait.

### CH5 Basic Types and Operations

Symbol literals are typically used in situations where you would use just an identifier in a dynamically typed lang.

If you want to compare two objects for equality, you can use either == or its inverse !=.
== has been carefully crafted so that you get just the equality comparison you want in most cases.
first check left side for null, then call equals method.
Scala provides a facility for comparing reference equality, eq and its opposite ne.

### CH6 Functional Objects

Rational number , as 1/2 ,112/339, numerator/denominator
Compared to floating-point numbers, rational numbers have the advantage that fractions are represented exactly without rounding or approximation.
```
#in scala, classes can take parameters directly.
class Rational(n:Int,d:Int){
#checking preconditions
	require(d!=0)

#override toString method
	override def toString = n +"/"+ d	
}

```

To access the numerator and denominator on that, you need to make them into fields.

```
class Rational (n :Int, d: Int){
	require(d!=0)
	val numer: Int = n
	val denom: Int = d
	override def toString = numer + "/" + denom
	def add(that: Rational) : Rational = 
		new Rational(numer*that.denom+that.numer*denom,denom*that.denom)
}
```

Implicit conversions


```
implicit def intToRational(x:Int) = new Rational(x)
```

### CH7 Built-in Control Structures

one thing you will notice is that almost all of scala's control structures result in some value.
```
val filename = if(!args.isEmpty) args(0) else "default.txt"
#filter with for loop
var filesHere = (new java.io.File(".").listFiles)
for (file <- filesHere if file.getName.endsWith(".scala"))
	println(file)

#generate new collection
def scalaFiles = 
	for{
		file <- filesHere
		if file.getName.endsWith(".scala")
	} yield file	
```

it's usually best to avoid returning values from finally clauses.

### CH8 Functions and Closures
the private-method approach works in scala as well, but scala offers an additional approach:
you can define functions inside other functions. local functions , just like local variables.

```
import scala.io.Source

object LongLines{
	def processFile(filename: String, width:Int) = {
		def processLine(line: String) = {
			if(line.length > width)
				println(filename +" : "+line.trim)
		}

		val source = Source.fromFile(filename)
		
		for(line<-source.getLines())
			processLine(line)
	}
}
```

partially applied functions

```
def sum(a:Int,b:Int,c:Int) = a + b + c
sum(1,2,3)
#scala compiler instantiates a function value that takes three missing parameters from partially applied function expression, sum _
val a = sum _
a(1,2,3)
a.apply(1,2,3)
val b = sum(1,_:Int,3)
b(2)
```

Closures
free variable vs bound variable
the function value(the object) that's created at runtime from this function literal is called a closure.
but scala's closures capture variables themselves, not the value to which variables refer.

The closure see the change to free variable , the same is true in the opposite direction.

repeated parameters _*

named arguments allow you to pass arguments to a function in a different order.

default parameter values

tail recursive
functions call themselves as their last action, the scala compiler detects tail recursion and replaces it with a jump back to the beginning of the function, after updating the parameters with new values.


### CH9 Control Abstraction
reducing code duplication
```
object FileMatcher{
	private def filesHere = (new java.io.File(".")).listFiles
	private def fileMatching(matcher: String => Boolean) =
		for(file<-filesHere;if matcher(file.getName))
			yield file

	#free variable query, in closure.
	def filesEnding(query:String) = filesMatching(_.endsWith(query))
	def filesContaining(query:String) = filesMatching(_.contains(query))
	def filesRegex(query:String) = filesMatching(_.matches(query))
}
```


The exists method represents a control abstraction.

```
def containsOdd(nums: List[Int]) = nums.exists( _ % 2 ==1)
#curried function
def first(x: Int) = (y:Int) => x +y
val second = first(1)
second(2)

#new control
def twice(op: Double=>Double,x:Double) =  op(op(x))
twice(_+1,5)

```

Any time you find a control pattern repeated in multiple parts of your code, you should think about implementing it as a new used coding pattern.

```
def withPrintWriter(file:File,op: PrintWriter => Unit) ={
	val writer = new PrintWriter(file)
	try{
		op(writer)
	}finally{
		writer.close()
	}
}

#call
withPrintWriter(new File("data.txt"), writer=>writer.println(new java.util.Date))

#more
def withPrintWriter(file:File)(op: PrintWriter => Unit) ={
	val writer = new PrintWriter(file)
	try{
		op(writer)
	}finally{
		writer.close()
	}
}

#call again
val file = new File("data.txt")
withPrintWriter(file){ writer =>
	writer.println(new java.util.Date)
}

```

In any method invocation in scala in which you're passing in exactly one argument, you can opt to use
curly braces to surround the argument instead of parentheses.

By-name parameters , you give the parameter a type starting with => , instead of ()=>.

a parameter of type A acts like a val(its body is evaluated once, when bound)
and type => A acts like a def(its body is evaluated whenever it is used.)

```
def when[A](test: Boolean, whenTrue: => A, whenFalse: => A): A = 
  test match {
    case true  => whenTrue
    case false => whenFalse
  }

// Try that again...

scala> when(1 == 1, println("foo"), println("bar"))
foo

scala> when(1 == 2, println("foo"), println("bar"))
bar
```

### CH10 Composition and Inheritance


### CH11 Scala's Hierachy

### CH12 Traits
traits are a fundamental unit of code reuse in scala.
a trait encapsulates method and field, which can then be reused by mixing them into classes.

A trait looks just like a class definition except that it uses keyword trait
two exceptions.

first, a trait cannot have any "class constructor" parameters
the other difference is that whereas in classes, super calls are statically bound, in traits, they are dynamically bound.


Turning a thin interface to a rich one is one major use of traits.
the second major use : providing stackable modifications to classes.
```
class MyQueue extends BasicIntQueue with Doubling
val queue = (new BasicIntQueue with Incrementing with Filtering)
```

**the order of mixins is significant, roughly speaking. traits further to the right take effect first.**

That's a lot of flexibility for a small amount of code, so you shoule keep your eyes open for opportunites to arrange code as stackable modifications.

With traits, the super call is determined by a linearization of the classes and traits that are mixed into a class. Scala's special treatment of super in traits makes all (supers) work out.

### CH13 Packages and Imports
scala's import
may appear anywhere
may refer to objects 
let you rename and hide some of imported members(rename something to _, means hiding it altogether)
catch-all _' come last in the list of import selectors.

### CH14 Assertions and Tests

TODO

### CH15 Case Classes and Pattern Matching

match is an expression in scala(it always results in a value)
scala's alternative expressions never fall through into the next case
you always have to make sure all cases are covered.


Constructor patterns to support deep matches
sequence patterns matches
typed patterns
variable binding @
match expression with a pattern guard
```
case n: Int if 0 < n => ...
case s: String if s(0) == 'a' => ...
#the most common way to take optional values apart it through a pattern match
def show(x:Option[String]) = x match{
	case Some(s) => x
	case None => "?"
}
#patterns in variable definitions
val myTuple = (123, "abc")
val (number,string)= myTuple
#patterns in for expressions
for ((country,city) <- capitals)
val results = List(Some("apple"),None,Some("orange"))

for (Some(fruit) <- results) println(fruit)
```

### CH16 Working with Lists

lists are immutable, and lists have a recursive structure, whereas arrays are flat.

homogeneous, cons(::), Nil represents the empty list. 

basic operations: 
head returns the first element,
tail returne a list consisting of all elements except the first.

head+tail vs init+last

```
  def insert(x:Int,xs:List[Int]):List[Int] = {
      if(xs.isEmpty || x< xs.head) x::xs
      else xs.head :: insert(x, xs.tail)
  }

  def isort(xs:List[Int]) : List[Int] = {
      if(xs.isEmpty) Nil
      else insert(xs.head,isort(xs.tail))
  }

  #pattern match
  def insert(x:Int,xs:List[Int]):List[Int] =  xs match{
      case List() => List(x)
      case y::ys  => if (x<=y) x::xs else y::insert(x,ys)
  }

  def isort(xs:List[Int]) : List[Int] = xs match {
      case List() => List(x)
      case x::xs1 => insert(x,isort(xs1))
  }
```

List patterns
val List(a,b,c) = fruit

Display list : toString and mkString
pre + sep + post
List mkString ("pre","sep","post")

Higher-order methods on List
transforming every element of a list in some way,
verifying whether a property holds for all elements of a list, filter/partition/forall/exists
extracting from a list elements satisfying a certain criterion, find/takeWhile/dropWhile
or combining the elements of a list using some operator
```
List(1,2,3,4,5) foreach (sum += _)

List(1,2,3,-4,5) span (_ > 0)

List(1,2,-3,4,-5) sortWith ( _ < _)

words sortWith (_.length > _.length)

List.apply(1,2,3)

List.range(1,5)

List.fill(3)("Hi")

#multi-dimensional
List.fill(3,3)(1)
#List(List(1, 1, 1), List(1, 1, 1), List(1, 1, 1))

List.concat(List('a'),List('b'),List('c'))
```

### CH17 working with other collections

List("red","blue","green")
val fiveInts = new Array[Int](5)
val fiveToOne = Array(5,4,3,2,1)

ListBuffer build lists more efficiently when you need to append.

buf.toList

ArrayBuffer is like an array, except you can additionally add and remove elements from the beginning and end of the sequence.

Strings
because Predef has an implicit conversion from String to StringOps, you can treat any string like a sequence.

s.exists(_.isUpper)

If you want to use both mutable and immutable sets or maps in the same source file, one approach is to import the name of the package that contains the mutable variants.

```
import scala.collection.mutable

val mutaSet = mutable.Set(1,2,3)

val ts = TreeSet(9,3,1,8,0,2)

```

TreeSet implemented SortedSet trait.

### CH18 Mutable Objects

an initializer "= _ " of a field assigns a zero value to a field, the zero value depends on the field's type.

```
type Action = () => Unit

case class WorkItem(time:Int,action:Acton)

```

### CH19 Type Parameterization
Type parameterization allows you to write generic classes and traits. For example, sets are generic and take a type parameter.
Unlike java, which allows raw types, scala requires that you specify type parameters.

Variance defines inheritance relationships of parameterized types, Set[String] is a subtype of Set[AnyRef].

hide a constructor by making it private
```
trait Queue[T]{
	def head:T
	def tail: Queue[T]
	def enqueue(x:T): Queue[T]
}

object Queue{
	def apply[T](xs: T*) : Queue[T] = new QueueImpl[T](xs.toList,Nil)
	private class QueueImpl[T](private val leading:List[T],private val trailing:List[T]) extends Queue[T]{

	}
}

```

So, client can create queues with an expression such as Queue(1,2,3), this expression expands to Queue.apply(1,2,3).
Queue looks to clients as it was a globally defined factory method.

### CH20 Abstract Methods
the following traits declares one abstract member
```
trait Abstract{
	type T
	def transform[x:T] : T
	val initial: T
	val current: T
}

class Concrete extends Abstract{
	type T = String
	def transform[x:String] = x + x
	val initial: "hi"
	val current: initial
}

#abstract vals
val initial : String
#It gives a name and type of a val, but not its value, you don't know the correct value in the class, but it will be unchangeable.

```

Initializing abstract vals
**Abstract vals sometimes play a role analogous to superclass parameters: they let you provide details in a subclass that are missing in a superclass**

```
#anonymous class
trait RationalTrait{
	val numer : Int
	val denom : Int
}

new RationalTrait{
	val numer = 1
	val denom = 2
}
```

There is a subtle difference concerning the order in which expressions are initialized.

new Rational(expr1,expr2)

two expressions, expr1 and expr2 are evaluated before Class Rational is intialized.

For traits , the situation is the opposite. when you write :
new RationalTrait{val numer=1; val denom=2}

the expressions, expr1 and expr2 are evaluated as part of the initialization of the anonymous class, but the anonymous class is initialized after
the RationalTrait. So the values of numer and denom are NOT available during the initialization of RationalTrait(more precisely, a selection of each value would yield the default value for type Int, 0)

**This sutble difference will become a problem, when sometimes, in trait, logic depends on abstract vals.**

+ One solution is pre-initialized fields.

Simply place the field definition in braces before the superclass constructor call.

```
new {
	val numer = 1
	val denom = 2
} with RationalTrait

#Pre-initialized fields are not restricted to anonymous classes. they can used in objects or named subclasses.

object twoThirds extends{
	val numer = 2
	val denom = 3
} with RationalTrait

class RationalClass(n:Int,d:Int) extends {
	val numer = 2
	val denom = 3
} with RationalTrait{
	def...
}

```

Because pre-initialized fields are initialized before the superclass constructor is called, their initializers cannot refer to the object that's being constructed.
Consequently, if such an initializer refers to this, the reference goes to the object containing the class or object that's being constructed, not the constructed object itself.

If you prefix a val definition with a lazy modifier, the initalialing expression on the right-hand side will only be evaluated the first time the val is used.

similar to the situation where x is defined as a parameterless method, using a def. unlike a def, a lazy val is never evaluated more than once.

```
#val vs lazy val vs def
val result1 = {println("hello val"); "returns val"}
result1
lazy val result2 = {println("hello lazy val"); "returns lazy val"}
result2
def result3 = {println("hello def"); "returns def"}
result3
result3
```

+ Another solution use lazy vals.

```
trait LazyRationalTrait{
	val numerArg: Int
	val denomArg: Int
	lazy val numer = numerArg / g
	lazy val denom = denomArg / g
	override def toString = numer + "/" +denom
	private lazy val g = {
		require(denomArg!=0)
		gcd(numerArg,denomArg)
	}
	private def gcd(a:Int,b:Int) : Int = if(b==0) a else gcd(b,a%b)
}
```

Enumeration defines an inner class named Value, and you can associate names with enumeration values by using a different overloaded variant of the Value method.

```
object Converter {
  var exchangeRate = Map(
    "USD" -> Map("USD" -> 1.0, "GBP" -> 1.25),
    "GBP" -> Map("USD" -> 0.8, "GBP" -> 1.0)
  )
}

abstract class CurrencyZone {
  // Currency must be a sub-type of AbstractCurrency
  // also sets this as an alias
  type Currency <: AbstractCurrency
  val CurrencyUnit: Currency

  // each CurrencyZone holds a factory for the Currency itself
  def make(amount: Long): Currency

  // definition for the AbstractCurrency class
  abstract class AbstractCurrency {
    val amount: Long
    def designation: String
    def +(that: Currency): Currency = make(this.amount + that.amount)
    def *(x: Double): Currency = make((this.amount * x).toLong)
    // creates a new amount of this Currency from another Currency
    def from(another: CurrencyZone#AbstractCurrency): Currency = {
      make(math.round(
        another.amount.toDouble * Converter.exchangeRate(another.designation)(this.designation)
      ))
    }
    private def decimals(n: Long): Int = if (n == 1) 0 else {1 + decimals(n / 10)}
    override def toString = ((amount.toDouble / CurrencyUnit.amount.toDouble) formatted ("%." + decimals(CurrencyUnit.amount) + "f") + " " + designation)
  }
}

object US extends CurrencyZone {
  abstract class Dollar extends AbstractCurrency {
    def designation = "USD"
  }
  type Currency = Dollar
  def make(cents: Long) = new Dollar {
    override val amount: Long = cents
  }
  val Cent = make(1)
  val Dollar = make(100)
  val CurrencyUnit = Dollar
}

object UK extends CurrencyZone {
  abstract class Pound extends AbstractCurrency {
    def designation = "GBP"
  }
  type Currency = Pound
  def make(pence: Long) = new Pound {
    override val amount: Long = pence
  }
  val Pence = make(1)
  val Pound = make(100)
  val CurrencyUnit = Pound
}

object CurrencyTest extends App {
  var current: CurrencyZone#AbstractCurrency =_
  current = US.Dollar from UK.Pound
  println(current)
  current = UK.Pound from current
  println(current)

  println(Converter.exchangeRate("USD")("GBP") * 10)
}
```


### CH21 Implicit Conversions and Parameters

scals's way to use someone else's libraries.

The first step is to write an implicit conversion between two types.

Because function2ActionListener is marked as implicit, it can be left out and the compiler will insert it automatically.

The compiler first tries to compile it as it is , but it sees a type error, before giving up, it looks for an implicit conversion that can repair the problem.

Rules for implicits

+ Only definitions marked as implicit are available.
+ Scope rule : must be in scope as a single identifier
+ Once-at-a-time rule : only one implicit is inserted
+ Explicit-First rule

Implicit conversions can have arbitrary names, the name matters only in two situations.

Implicit classes

```
case class Rectangle(width:Int, height:Int)

implicit class RectangleMaker(width:Int){
	def x(height:Int) = Rectangle(width,height)
}

#now
val myRec = 3 x 4
```

Implicit parameters

the remaining place the compiler inserts implicit is within argument lists.

```
import JoesPrefs._

object Greeter{
	def greet(name:String)(implicit prompt: PreferredPrompt) = {
		...
	}
}
```
The compiler will sometimes replace someCall(a) with someCall(a)(b) by adding a missing parameter list to complete a function call.

**This pattern is so common that the standard scala library provides implicit "ordering" methods for many common types.**







































