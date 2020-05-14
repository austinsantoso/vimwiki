# Go (Golang)
Go can be downloaded from [golang.org](https://golang.org)

This language is like imagine C and Python were merged together.
* Strong and statically typed
* Simple
* Fast compile times
* Garbage Collection
* Built-in concurrency
* Compile to standalone binaries

Some notes:
* Go does not have exceptions, they instead have panic.
* Go does not need semi colons
* Go does not suppoer prefix version of increment and decrement `++i or --i`
	* We also cannot put them inside expression `array[i++]` is not allowed
* To format, run `go fmt`

This summary is based on [A Tour of Go](https://tour.golang.org/list).
Some basici programming knowledge is expectd.

Simple Go code
```go
package main

import "fmt"

func main() {
	fmt.Println("Hello World")
}
```

## Basics
Packages, variables and functions. Basic components of any Go program.

### Packages
Every Go program is made up of packages.

Programs start running in package `main`

By convention, the package name is the same as the last element of the import path.
For instance, the `"math/rand"` package comprises files that begin with the statement `package rand`.

### Imports
Imports the libraries/packages that will be insed in the file.

```go
import "fmt"
import "math"

// This is "factore" import statement. Prefered style
import (
	"fmt"
	"math"
)
```

### Exported names
In Go, a name is exported if it begins with a capital lettter.
For example `Pi` is an exported name from the `math` package.

When importing a package, we can only refer to its exported names.
Any "unexported" names are not accessible.

### Functions
Note: unlike other laguages, the type comes after the variable name.

A function can take zero or more arguments.
It is declared with the `func` keyword, followed by the function name.
Then the parameters and return type.
```go
func add(x int, y int) int {
	return x + y
}
```
When two or more consecutive function parameters share a type, we can omit the tpe from all but the last.
```go
x int, y int
```
can be shortened to
```go
x, y int
```

A function can return any number of results
```go
func swap(x, y string) (string, string) {
	return y, x
}
```

The return values can also be named in the declaration. If so, they are treated as variables defined just before the function.

A `return` statement withoutu arguments returns the named return values.
This is known as a "naked" return.
Naked return statements should be used in short fuctions, or else it could harm readability.
```go
func split(sum int) (x, y int) {
	x = sum * 2 / 3
	y = sum - x
	return
```

If we want nothing to be returned (void) don't decalre anything to be returned.

### Variables
Unlike other laguages, the type comes after the variable name.

The `var` statement declares a list of variables, the type is last.
A `var` statement can be at package or funtion level.
```go
var c, python, java bool

func main() {
	var i int
	fmt.Println(c, python, java, i)
}
```

The basic types available in Go:
```
bool

string

int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr

byte // alias for uint8

rune // alias for int32
	 // represents a Unicode code point

float 32 float64

complex64 complex128 // complex numbers 10 + 5i
```

We can initialize variables as well.
```go
var i int = 1
var j, k bool = true, false
```

The declaration may also be "factored" into blocks.
```go
var (
	ToBe bool = false
	maxInt uint64 = 1<<64 - 1
)
```

Alternatively the `:=` short assignment statement can be used.
This can be used inside a function only.
This causes Go to interpret the type by itself.
There are defaults that Go will interpret them based on the precision.
```go
k := 3
```

If a variable is uninitialized, they would be assigned their default zero value.
* `0` for numeric types
* `false` for boolean type
* `""` for string type

We can convert one type to another by using `T(v)` where `T` is the type to cast it to, `v` is the value to cast.
```go
var i int = 42
var f float64 = float64(i)
```

To know the type of a variable:
```go
fmt.Println("%T", var)
```

### Constants
Constants are declared with the `const` keyword.
They can be of any type.
They cannot be declared using the `:=` syntax.
```go
const Pi = 3.14
```

## For
Go only has one looping construct the `for` loop.
Similar to other languages, `for` loop has 3 components seperated by a semicolon:
* the init statement; executed before the first iteration
* the condition expression: evaluated before every iteration
* the post statement: executed at the end of every iteration
The `for` loop will keep iterating until the condition evaluates to `false`
```go
sum := 0
for i := 0; i < 10; i++ {
	sum += i
}
```
The init and post statement are optional.
Without them, it would just be a `while` loop.
```go
sum := 1
for sum < 100 {
	sum += sum
}
```
We can also omit the loop condition to make an infinite loop.

### If
`if-else` statements are like `for` loops;
the expression need not to be surrounded by parantheses `( )` but the braces `{ }` are required.

Like `for`, the `if` statement can start with a short statement to exeecute before the condition.
Variables declared here are only in scope until the end of the `if` block and `else` blocks.
```go
if v := rand.Int(); v < 10 {
	return 10
} else {
	return v
}
```

## Switch
A `switch-case` statement is a shorter way to write a sequence of `if-else` block.
Unlike other languages, the `break` statement is not needed, Go will only run the corresponsing `case` blcok
```go
switch word := "apple"; word {
case "orange":
	fmt.Println("orange")
default:
	fmt.Println("i hate orange")
}
```
Switch cases are evaluated from top to bottom, stopping when a case succeds.
There can be multiple conditions for one case.

In the example below `complicatedFunction` is never ran.
```go
switch i := 0; i {
case 0:
	fmt.Println("zero")
case complicatedFunction(), 10:
	fmt.Println("long function ran")
}
```
Switch without a condition is the case as `switch true`.

## Defer
A defer statement defers the execution of a function after the `defer` keyword until the function that it is in returns something.
```go
func main() {
	defer fmt.Println("world")

	fmt.Println("Hello ")
}

// returns Hello World
```
Deferred functon calls are fushed on to a stack.
When a function returns, its deferred calls are executed in last-in-first-out order.
```go
func main() {
	fmt.Println("Counting")

	for i:= 0; i < 3; i++ {
		defer fmt.Println(i)
	}
	fmt.Println("Done")
}
/*
Returns:
Counting
Done
2
1
0
*/
```

### Pointers
Go has Pointers. A pointer hold the memory address of a value.

The `*T` is a pointer to a `T` value. its zero value is `nil`.
```go
var p *int
```

The `&` operator generates a pointer to its operand.
```go
i := 42
p = &i
```

The `*` operator denotes the pointer's underlying value.
We read the value located at the memory location.

```go
fmt.Println(*p)
*p = 21
```
This is known as "dereferencing".

Unlike C, Go has no pointer arithmetic.

### Structs
A `Struct` is a collection of fields.
```go
type Point struct {
	x int
	y int
}
```
Struct are created by calling its name, followed by its fields.
We can list a subset of fields by using `fieldName:` syntax, with this the order does not matter.

Fields of a struct are accessed using a dot.
```go
var v1 = Point{1, 2}
var v1 = Point{y:1}

fmt.Println(v1.x)
```

### Arrays
Arrays are declared by the type `[n]T` indicating an array of `n` values of type `T`.
Arrays have fixed length, it cannot be exanded.
```go
var a [10]int
```
We can set the value at a particular index
```go
a[2] = 10
```

We can declare an array with inital values:
```go
primes := [6]int{2,3,5,7,11,13}
```

### Slice
An array has a fixed size.
A slice, on the oher hand, is a dynaically-sized flexible view in to the elements of an array.
the type `[]T` is a slice with elements of type `T`.
A slice is formed by specifying two indiced, a low and high bound, seperated by a colon.
`a[low:high]`
```go
numbers := [6]int{1,2,3,4,5,6}

// creates a slice which inclused element 1, exclused element 4
var s []int = numbers[1:4]
```
We can reference slices just like an array.

A slice does nto store any data, it just describes a section of an undelying array.
Changing the elements of a slice omdified the corresponding element of its underlying array.
Slices that share the same underlying array, will equally be impacted to change of values.

A slice literal is like an array literal without the length
```go
// Array
[3]int{1,2,3}

// Slice
[]int{1,2,3}
```

A Slice has both a _length_ and a _capacity_.
The length of a slice is the number of elements that it contains.
The capacity of a slice is the number of elements in the underlying array counting form the first element in the slice.
we can get these value by using `len(s)` and `cap(s)`

The zero value of a snile is `nil`.

## Make
The `make` function is like `new` in java or `malloc` in C. (I think).

We can use `make` function to allocate a zeroes array and return a slice that referst to that array.
```go
a := make([]int, 5) // len(a) = 5
b := make([]int, 0, 5) // len(b) = 0, cap(b) = 5

```

We can also creat multidimentional slices:
```go
board := [][]string{
	[]string{"_", "_", "_"},
	[]string{"_", "_", "_"},
	[]string{"_", "_", "_"}
}
```

### Appending to a slice
To add a new element to a slice, we use the built-in `append` function.
```go
`func append(s []T, vs ...T) []T {}
```
we append all `vs` to the slice `s`.
If `s` is too small, a new array willl be created.

### Maps
A map maps a key to a value. A zero map is `nil`.
A `nil` map has no keys, nor can keys be added.
We can create a map using `make`.
```go
// map[key]value
var m map[string]int
m = make(map[string]int)
m["earth"] = 10
```
We can also create Map literals, but the keys are required.
```go
var m = map[string]vertex {
	"home": Vertex{10, 10},
	"work": {100, 10}, // if the top-level type is just a type name, we can oit it from the elements of literal.
}
```
To insert or update an element in a map `m`:
```go
m[key] = value
```
Retrieve an element:
```go
elem = m[key]
```
Delete an element:
```go
delete(m, key)
```
Test that an element is present or not with a two-value assignment
```go
elem, ok = m[key]
```
if `key` is in m, the `ok` is `true`.
Otherwise `ok` is false, elem is the zero value for the map's element type.


### Range
The `range` form of the `for` loop iterates over a slice or map.
```go
for index, value := range sliceA {
	fmt.printf("element %d is %d", index, value)
}
```
We can skip the index or value by assigning it `_`.
If we want the index only, we can ommit the value.
```go
for _, value := range sliceA {
	fmt.printf("element %d is %d", index, value)
}
```

We can also iterate over a map:
```go
for key, value := range mapA {
	fmt.Println(key, value)
}
```
However, each iteration will produce different resutls.
Go runtime goes the extra mile to randomize them.

### More on Functions
Functions are values too.
They can be passed around just like other values.
They can be used are function arguments and return values.

Fibonacci function:
```go
func fibonacci() func() int {
	cur := 0
	next := 1
	return func() {
		tempCur := cur
		var temp int = next
		next += cur
		cur = temp
		return tempCur
	}
}
func main() {
	f := fibonacci()
	for i := 0; i < 10; i++ {

	}
}
```

## Methods and interfaces
Go does not have classes. However, you can define methods for a specific type.
Methods are functions with a special receiver argument.
The receiverappears in its own argument list between the `func` keyword and the method name.
```go
type Vertex struct {
	X, Y float64
}

func (v Vertex) Abs() float64 {
	return math.Sqrt(v.X * v.X + v.Y * v.Y)
}
func main() {
	v := Vertex{3, 4}
	fmt.Println(v.abs())
}
```
We can only declare a method with a receiver whose type is defined in the same package as the method.
So we cannot make methods for build-in types, such as `int`.

Methods are not limited to structs. Types are not limited to structs. We can declare `MyFloat` which is a `float64`.
In this case we use parantheses `( )` rather than brackets `{ }`.
```go
type MyFloat float64

func (f MyFloat) Abs() float64 {
	if f < 0 {
		return float64(-f)
	}
	return float64(f)
}
```
(Pass by value)
To achieve this, we make a copy of the original variable value.
This causes values to not be modified if we used value receivers.

Above we are passing the values directly in, we can also pass in a pointer, called these receivers are called pointer receivers.
The receiver has literal syntax `*T` for some type `T`.
(Also, `T` cannot itself be a pointer such as `*int`)
```go
type Vertex struct {
	X, Y float64
}

func (v *Vertex) scale(f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}
```
Methods with pointer receivers can modify the value to which the receiver points.
This is because it is modifying the actual data, not a copy of it.
This is often more useful.

### Functions vs Methods
```go
type Vertex struct {
	X, Y float64
}

func (v *Vertex) Scale(f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}

func ScaleFunc(v *Vertex, f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}
```
Functions with a value argument must take a value of that specific type.
Functions with a pointer argument must take a pointer:
```go
var v Vertex
ScaleFunc(v, 5)  // Error
ScaleFunc(&v, 5) // OK
```
Where as Methods with pointer receivers take either a value or a pointer as the receiver when they are called.
Methods with value receivers take either a value or a pointer as the receiver when they are called.
Go will automatically interpret and adjust accordingly.
```go
var v Vertex
v.Scale(5) // OK
p := &v
p.Scale(5) // OK
```

### Interfaces
An interface type is defined as a set of methodo signatures. A value of interface type can hold any value that implement those methods.
There is no "implements" keyword, A type implements an interface by implementing the mothods of the interface.

Implicit interfaces decouple the definition of an interface from its implementation, which could then appear in any package without prearrangement.

```go
type Abser interface {
	Abs() float64
}

func main() {
	var a Abser
	f := MyFloat(-math.Sqrt2)
	v := Vertex{3, 4}

	a = f  // a MyFloat implements Abser
	a = &v // a *Vertex implements Abser

	// In the following line, v is a Vertex (not *Vertex)
	// and does NOT implement Abser.
	// Even though of the auto inferencing above
	a = v

	fmt.Println(a.Abs())
}

type MyFloat float64

func (f MyFloat) Abs() float64 {
	if f < 0 {
		return float64(-f)
	}
	return float64(f)
}

type Vertex struct {
	X, Y float64
}

func (v *Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}
```

Interfaces do not hold a value of a specific concrete type.
Interface values can be thought of as a tuple of a value and a type.
`(value, type)`
Because of this, the value itself can be `nil`.
This would normally cause null pointer exception in other languages, but in Go, we allow the method to run the method, we usually place `nil` checker.
```go
func (t *T) M() {
	if t == nil {
		fmt.Println("<nil>")
		return
	}
	fmt.Println(t.S)
}
```
But the value cannot be `nil`.
Or else a run time error occures, since we don't know what type's method to call.
A nil interface value holds neither value nor concrete type.

An interface that specifies zero methods is known as the _empty interface_.
```go
interface{}
```
An empty interface may hold values of any type, since every type implements at least zero methods.
This is useful for code that handles unknown type.
```go
func describe(i interface{}) {
	fmt.Printf("(%v, %T)\n", i, i) // easy way to print value and Type
}
```

### Type assertion
A type assertion provides access to an interface value's underlying concrete value.
```go
t, ok := i.(T)
```
The code above checks that the value `i` holds the concrete type `T`,
then assigns the value to t.

The `ok` variable is optional.
Without it, if `i` does not hold a `T`, then the statement will trigger a panic.

if `i` does not hold `T`, `ok` will be `false`.
if `i` does hold `T`, `ok` will be `true`.

### Type switches
We can use a `switch-case` based on the type of a value.
We make use of the `type` keyword.
```go
switch v := i.(type) {
	case int:
		fmt.Printf("Twice %v is %v\n", v, v*2)
	case string:
		fmt.Printf("%q is %v bytes long\n", v, len(v))
	default:
		fmt.Printf("I don't know about type %T!\n", v)
	}
```

### Common Interfaces
#### Stringer
One of the most common interfaces is `Stringer`,
defined by the `fmt` package.
```go
type Stringer interface {
	String() string
}
```
Usually used for a type to describe itself as a string.

#### Errors
Go expresses error state with `error` values.
The `error` type has a built-in interface similar to `fmt.Stringer`:
```go
type error interface {
	Error() string
}
```
Similar to `fmt.Stringer`, the fmt package looks for the `error` interface	when printing.
Errors are often returned by functions.

## Concurrency
The Synchronization problem.
### Goroutines
A _goroutine_ is a lightweight thread managed by the Go runtime.
```go
go f(x, y, z)
```
this creates a new goroutine `f(x, y, z)`.
`f`, `x`, `y`, `z` are evaluated at the current goroutine, whereas the execution happens in another goroutine.
Goroutines run in the same address space (thread not process).
Need to worry about synchronization.
Use `sync` package, or other primitives.

### Channels
This is message passing.

Channels are a typed conduit through which you can send and receive values with the challen operator, `<-`.
To create a channel use make.
`chan` keyword is used to indcate a channel.
`T` is used to specify the type to be sent, in the case below is `int`.
```go
ch := make(chan int)

ch <- v     // send v to channel ch
v := <- ch  // Receive from ch, then assign to v
```
By default send and receive block until the other side is ready.
We do not need locks or condition variables.

### Buffered Channels
Channels can be buffered.
This is created by creating a buffer length as the second argument
```go
ch := make(chan int, 100)
```
Sending to a buffered channel is blocked only when the buffer is full.
Receiving from a buffered channel in blocked only when buffer is empty.

### Channel Util
A sender can `close` a channel to indicate that no more values will be sent.
Receivers can test whether a channel has been closed by assigning a second parameter to the receive expression
Closing a channel does not remove all already sent values,
it just prevents new values to be sent.

Only the sender should close the channel, never the receiver.
Secnding to a closed channel will cause a panic.
```go
v, ok := <- ch
```
if `ok` is `false`, there are no more values to receive and channel is closed.

We can also look through it.
The loop `for i := range c` receives values from the channel repeatedly until it is closed.

### Select
The `select` statement lets a goroutine wait on multiple communication operations.
A `select` statement blocks execution until one of its cases can run, then it executes that case.
It randomly chooses one if multiple are ready.

We can also add a `default` case,
which runs if no other case can run.
```go
// a channel c and quit
x, y := 0, 1
for {
	select {
	case c <- x: // sends data normally
		x, y = y, x + y
	case <- quit: // receives a signal to quit
		fmt.Println("quit")
	default:
		fmt.Println("Sleeping")
		time.Sleep(50 * time.Milisecond)
	}
}
```

To make the main goroutine wait for all its children.
Remember that Go passes by value, so pass in a pointer to update the same WaitGroup.
```go
import "sync"

func main() {
	var wg sync.WaitGroup
		// make threads do things
		// Each thread notify that need to wait for 1 more
		// Think of it as a counter
		wg.Add(1)

		// When each of the thread is finished do, easily done by defer
		wg.Done()

	// wait until all tasks are done
	wg.Wait() // Blocks until everything is finished
}
```

