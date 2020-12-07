# [Welcome to a tour of Go](https://tour.golang.org/list)

## Basics

The starting point, learn all the basics of the language.

Declaring variables, calling functions, and all the things you need to know before moving to the next lessons.

### [Packages, variables, and functions](https://tour.golang.org/basics)

Learn the basic components of any Go program.

- Packages
  - Every Go program is made up of packages.
  - Programs start running in package `main` .
- Imports
  - It is good style to use the factored import statement.

``` Go
package main

import (
    "fmt"
    "math"
)

func main() {
    fmt.Printf("Now you have %g problems.\n", math.Sqrt(7))
}
```

- Exported names
  - A name is exported if it begins with a capital letter.
    - `Pizza` is an exported name, as is `Pi` , which is exported from the math package.
    - `pizza` and `pi` do not start with a capital letter, so they are not exported.
  - When importing a package, you can refer only to its exported names.
    - Any "unexported" names are not accessible from outside the package.
- Functions
  - A function can take zero or more arguments.
  - Notice that the type comes after the variable name.
    - [Go's Declaration Syntax](https://blog.golang.org/declaration-syntax)
  - When two or more consecutive named function parameters share a type, you can omit the type from all but the last.

``` Go
func add(x, y int) int {
    return x + y
}
```

- Multiple Results
  - A function can return any number of results.
  - The swap function returns two strings.

``` Go
package main

import "fmt"

func swap(x, y string) (string, string) {
    return y, x
}

func main() {
    a, b := swap("hello", "world")
    fmt.Println(a, b)
}
```

- Named return values
  - Go's return values may be named. If so, they are treated as variables defined at the top of the function.
  - A return statement without arguments returns the named return values. This is known as a "naked" return.
  - Naked return statements should be used only in short functions, as with the example shown here. They can harm readability in longer functions.

``` Go
package main

import "fmt"

func split(sum int) (x, y int) {
    x = sum * 4 / 9
    y = sum - x
    return
}

func main() {
    fmt.Println(split(17))
}
```

- Variables
  - The var statement declares a list of variables; as in function argument lists, the type is last.
  - A var statement can be at package or function level. We see both in this example.

``` Go
package main

import "fmt"

var c, python, java bool

func main() {
    var i int
    fmt.Println(i, c, python, java)
}
```

- Variables with initializers
  - A var declaration can include initializers, one per variable.
  - If an initializer is present, the type can be omitted; the variable will take the type of the initializer.

``` Go
package main

import "fmt"

var i, j int = 1, 2

func main() {
    var c, python, java = true, false, "no!"
    fmt.Println(i, j, c, python, java)
}
```

- Short variable declarations
  - Inside a function, the `:=` short assignment statement can be used in place of a var declaration with implicit type.
  - Outside a function, every statement begins with a keyword (var, func, and so on) and so the := construct is not available.

``` Go
package main

import "fmt"

func main() {
    var i, j int = 1, 2
    k := 3
    c, python, java := true, false, "no!"

    fmt.Println(i, j, k, c, python, java)
}
```

- Basic types
  - Go's basic types are

``` Go
bool

string

int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr

byte // alias for uint8

rune // alias for int32
    // represents a Unicode code point

float32 float64

complex64 complex128
```

  - The example shows variables of several types, and also that variable declarations may be "factored" into blocks, as with import statements.
  - The `int` , `uint` , and `uintptr` types are usually 32 bits wide on 32-bit systems and 64 bits wide on 64-bit systems.
  - When you need an integer value you should use `int` unless you have a specific reason to use a sized or unsigned integer type.

- Zero values
  - Variables declared without an explicit initial value are given their zero value.
  - The zero value is:
    - 0 for numeric types,
    - false for the boolean type, and
    - "" (the empty string) for strings.

``` Go
package main

import "fmt"

func main() {
    var i int
    var f float64
    var b bool
    var s string
    fmt.Printf("%v %v %v %q\n", i, f, b, s)
}
```

- Type conversions
  - The expression T(v) converts the value v to the type T.
  - Some numeric conversions:

``` Go
var i int = 42
var f float64 = float64(i)
var u uint = uint(f)
```

  - Or, put more simply:

``` Go
i := 42
f := float64(i)
u := uint(f)
```

- Type inference
  - When declaring a variable without specifying an explicit type (either by using the := syntax or var = expression syntax), the variable's type is inferred from the value on the right hand side.
  - When the right hand side of the declaration is typed, the new variable is of that same type:

``` Go
var i int
j := i // j is an int
```

  - But when the right hand side contains an untyped numeric constant, the new variable may be an `int` , `float64` , or `complex128` depending on the precision of the constant:

``` Go
i := 42           // int
f := 3.142        // float64
g := 0.867 + 0.5i // complex128
```

- Constants
  - Constants are declared like variables, but with the const keyword.
  - Constants can be character, string, boolean, or numeric values.
  - Constants cannot be declared using the := syntax.

``` Go
package main

import "fmt"

const Pi = 3.14

func main() {
    const World = "世界"
    fmt.Println("Hello", World)
    fmt.Println("Happy", Pi, "Day")

    const Truth = true
    fmt.Println("Go rules?", Truth)
}
```

- Numeric Constants
  - Numeric constants are high-precision values.
  - An untyped constant takes the type needed by its context.
  - Try printing `needInt(Big)` too.
  - (An `int` can store at maximum a 64-bit integer, and sometimes less.)

### [Flow control statements: for, if, else, switch and defer](https://tour.golang.org/flowcontrol)

Learn how to control the flow of your code with conditionals, loops, switches and defers.

- For
  - Go has only one looping construct, the `for` loop.
  - The basic `for` loop has three components separated by semicolons:
    - the init statement: executed before the first iteration
    - the condition expression: evaluated before every iteration
    - the post statement: executed at the end of every iteration
  - The loop will stop iterating once the boolean condition evaluates to false.
- Note: Unlike other languages like C, Java, or JavaScript there are no parentheses surrounding the three components of the `for` statement and the braces `{ }` are always required.

``` Go
package main

import "fmt"

func main() {
	sum := 0
	for i := 0; i < 10; i++ {
		sum += i
	}
	fmt.Println(sum)
}
```

- For continued
  - The init and post statements are optional.

``` Go
package main

import "fmt"

func main() {
	sum := 1
	for ; sum < 1000; {
		sum += sum
	}
	fmt.Println(sum)
}
```

- For is Go's "while"
  - At that point you can drop the semicolons: C's while is spelled `for` in Go.

``` Go
package main

import "fmt"

func main() {
	sum := 1
	for sum < 1000 {
		sum += sum
	}
	fmt.Println(sum)
}
```

- Forever
  - If you omit the loop condition it loops forever, so an infinite loop is compactly expressed.

``` Go
package main

func main() {
	for {
	}
}
```

- If
  - Go's `if` statements are like its `for` loops; the expression need not be surrounded by parentheses `( )` but the braces `{ }` are required.

``` Go
package main

import (
	"fmt"
	"math"
)

func sqrt(x float64) string {
	if x < 0 {
		return sqrt(-x) + "i"
	}
	return fmt.Sprint(math.Sqrt(x))
}

func main() {
	fmt.Println(sqrt(2), sqrt(-4))
}
```

- If with a short statement
  - Like `for` , the `if` statement can start with a short statement to execute before the condition.
  - Variables declared by the statement are only in scope until the end of the `if` .

``` Go
package main

import (
	"fmt"
	"math"
)

func pow(x, n, lim float64) float64 {
	if v := math.Pow(x, n); v < lim {
		return v
	}
	return lim
}

func main() {
	fmt.Println(
		pow(3, 2, 10),
		pow(3, 3, 20),
	)
}
```

- If and else
  - Variables declared inside an if short statement are also available inside any of the `else` blocks.
  - (Both calls to `pow` return their results before the call to fmt. `Println` in `main` begins.)

``` Go
package main

import (
	"fmt"
	"math"
)

func pow(x, n, lim float64) float64 {
	if v := math.Pow(x, n); v < lim {
		return v
	} else {
		fmt.Printf("%g >= %g\n", v, lim)
	}
	// can't use v here, though
	return lim
}

func main() {
	fmt.Println(
		pow(3, 2, 10),
		pow(3, 3, 20),
	)
}
```

- Switch
  - A `switch` statement is a shorter way to write a sequence of `if - else` statements. It runs the first case whose value is equal to the condition expression.
  - Go's switch is like the one in C, C++, Java, JavaScript, and PHP, except that Go only runs the selected case, not all the cases that follow. In effect, the break statement that is needed at the end of each case in those languages is provided automatically in Go. Another important difference is that Go's switch cases need not be constants, and the values involved need not be integers.

``` Go
package main

import (
	"fmt"
	"runtime"
)

func main() {
	fmt.Print("Go runs on ")
	switch os := runtime.GOOS; os {
	case "darwin":
		fmt.Println("OS X.")
	case "linux":
		fmt.Println("Linux.")
	default:
		// freebsd, openbsd,
		// plan9, windows...
		fmt.Printf("%s.\n", os)
	}
}
```

- Switch evaluation order
  - Switch cases evaluate cases from top to bottom, stopping when a case succeeds.

(For example,

``` Go
switch i {
case 0:
case f():
}
```

does not call f if i==0.)

- Switch with no condition
- Switch without a condition is the same as `switch true` .
- This construct can be a clean way to write long if-then-else chains.

``` Go
package main

import (
	"fmt"
	"time"
)

func main() {
	t := time.Now()
	switch {
	case t.Hour() < 12:
		fmt.Println("Good morning!")
	case t.Hour() < 17:
		fmt.Println("Good afternoon.")
	default:
		fmt.Println("Good evening.")
	}
}
```

- Defer
  - A defer statement defers the execution of a function until the surrounding function returns.
  - The deferred call's arguments are evaluated immediately, but the function call is not executed until the surrounding function returns.

``` Go
package main

import "fmt"

func main() {
	defer fmt.Println("world")

	fmt.Println("hello")
}
```

- Stacking defers
  - Deferred function calls are pushed onto a stack. When a function returns, its deferred calls are executed in `last-in-first-out` order.
  - To learn more about defer statements read this [blog post](https://blog.golang.org/defer-panic-and-recover).

``` Go
package main

import "fmt"

func main() {
	fmt.Println("counting")

	for i := 0; i < 10; i++ {
		defer fmt.Println(i)
	}

	fmt.Println("done")
}
```

### [More types: structs, slices, and maps](https://tour.golang.org/moretypes)

Learn how to define types based on existing ones: this lesson covers structs, arrays, slices, and maps.

- Pointers
  - Go has pointers. A pointer holds the memory address of a value.
  - The type `*T` is a pointer to a `T` value. Its zero value is `nil` .

``` Go
var p *int
```

- The `&` operator generates a pointer to its operand.

``` Go
i := 42
p = &i
```

- The `*` operator denotes the pointer's underlying value.

``` Go
fmt.Println(*p) // read i through the pointer p
*p = 21         // set i through the pointer p
```

- This is known as "dereferencing" or "indirecting".
- Unlike C, Go has no pointer arithmetic.

``` Go
package main

import "fmt"

func main() {
	i, j := 42, 2701

	p := &i         // point to i
	fmt.Println(*p) // read i through the pointer
	*p = 21         // set i through the pointer
	fmt.Println(i)  // see the new value of i

	p = &j         // point to j
	*p = *p / 37   // divide j through the pointer
	fmt.Println(j) // see the new value of j
}
```

- Structs
  - A `struct` is a collection of fields.

``` Go
package main

import "fmt"

type Vertex struct {
	X int
	Y int
}

func main() {
	fmt.Println(Vertex{1, 2})
}

```

- Struct Fields
  - Struct fields are accessed using a dot.

``` Go
package main

import "fmt"

type Vertex struct {
	X int
	Y int
}

func main() {
	v := Vertex{1, 2}
	v.X = 4
	fmt.Println(v.X)
}

```

- Pointers to structs
  - Struct fields can be accessed through a struct pointer.
  - To access the field `X` of a struct when we have the struct pointer `p` we could write `(*p).X` .
  - However, that notation is cumbersome, so the language permits us instead to write just `p.X` , without the explicit dereference.

``` Go
package main

import "fmt"

type Vertex struct {
	X int
	Y int
}

func main() {
	v := Vertex{1, 2}
	p := &v
	p.X = 1e9          // or (*p).X
	fmt.Println(v)
}

```

- Struct Literals
  - A struct literal denotes a newly allocated struct value by listing the values of its fields.
  - You can list just a subset of fields by using the `Name:` syntax. (And the order of named fields is irrelevant.)
  - The special prefix `&` returns a pointer to the struct value.

``` Go
package main

import "fmt"

func main() {
	var a [2]string
	a[0] = "Hello"
	a[1] = "World"
	fmt.Println(a[0], a[1])
	fmt.Println(a)

	primes := [6]int{2, 3, 5, 7, 11, 13}
	fmt.Println(primes)
}

```

- Arrays
  - The type `[n]T` is an array of `n` values of type `T` .

- The expression

``` Go
var a [10]int
```

- declares a variable a as an array of ten integers.
- An array's length is part of its type, so arrays cannot be resized.
- This seems limiting, but don't worry; Go provides a convenient way of working with arrays.

``` Go
package main

import "fmt"

func main() {
	var a [2]string
	a[0] = "Hello"
	a[1] = "World"
	fmt.Println(a[0], a[1])
	fmt.Println(a)

	primes := [6]int{2, 3, 5, 7, 11, 13}
	fmt.Println(primes)
}

```

- Slices
  - An array has a fixed size.
  - A slice, on the other hand, is a dynamically-sized, flexible view into the elements of an array.
  - In practice, slices are much more common than arrays.
  - The type `[]T` is a slice with elements of type `T` .
  - A slice is formed by specifying two indices, a low and high bound, separated by a colon:
  - This selects a half-open range which includes the first element, but excludes the last one.

``` Go
a[low : high]
```

  - The following expression creates a slice which includes elements 1 through 3 of a:

``` Go
a[1:4]
```

``` Go
package main

import "fmt"

func main() {
	primes := [6]int{2, 3, 5, 7, 11, 13}

	var s []int = primes[1:4]
	fmt.Println(s)
}
```

- Slices are like references to arrays
  - A slice does not store any data, it just describes a section of an underlying array.
  - Changing the elements of a slice modifies the corresponding elements of its underlying array.
  - Other slices that share the same underlying array will see those changes.

``` Go
package main

import "fmt"

func main() {
	names := [4]string{
		"John",
		"Paul",
		"George",
		"Ringo",
	}
	fmt.Println(names)

	a := names[0:2]
	b := names[1:3]
	fmt.Println(a, b)

	b[0] = "XXX"
	fmt.Println(a, b)
	fmt.Println(names)
}

```

- Slice literals
  - A slice literal is like an array literal without the length.
  - This is an array literal:

``` Go
[3]bool{true, true, false}
```

  - And this creates the same array as above, then builds a slice that references it:

``` Go
[]bool{true, true, false}
```

``` Go
package main

import "fmt"

func main() {
	q := []int{2, 3, 5, 7, 11, 13}
	fmt.Println(q)

	r := []bool{true, false, true, true, false, true}
	fmt.Println(r)

	s := []struct {
		i int
		b bool
	}{
		{2, true},
		{3, false},
		{5, true},
		{7, true},
		{11, false},
		{13, true},
	}
	fmt.Println(s)
}

```

- Slice defaults
  - When slicing, you may omit the high or low bounds to use their defaults instead.
  - The default is zero for the low bound and the length of the slice for the high bound.
  - For the array

``` Go
var a [10]int
```

  - these slice expressions are equivalent:

``` Go
a[0:10]
a[:10]
a[0:]
a[:]
```

``` Go
package main

import "fmt"

func main() {
	s := []int{2, 3, 5, 7, 11, 13}

	s = s[1:4]
	fmt.Println(s)

	s = s[:2]
	fmt.Println(s)

	s = s[1:]
	fmt.Println(s)
}

```

- Slice length and capacity
  - A slice has both a length and a capacity.
  - The length of a slice is the number of elements it contains.
  - The capacity of a slice is the number of elements in the underlying array, counting from the first element in the slice.
  - The length and capacity of a slice `s` can be obtained using the expressions `len(s)` and `cap(s)` .
  - You can extend a slice's length by re-slicing it, provided it has sufficient capacity.
  - Try changing one of the slice operations in the example program to extend it beyond its capacity and see what happens.

``` Go
package main

import "fmt"

func main() {
	s := []int{2, 3, 5, 7, 11, 13}
	printSlice(s)

	// Slice the slice to give it zero length.
	s = s[:0]
	printSlice(s)

	// Extend its length.
	s = s[:4]
	printSlice(s)

	// Drop its first two values.
	s = s[2:]
	printSlice(s)
}

func printSlice(s []int) {
	fmt.Printf("len=%d cap=%d %v\n", len(s), cap(s), s)
}

```

- Nil slices
  - The zero value of a slice is `nil` .
  - A nil slice has a length and capacity of 0 and has no underlying array.

``` Go
package main

import "fmt"

func main() {
	var s []int
	fmt.Println(s, len(s), cap(s))
	if s == nil {
		fmt.Println("nil!")
	}
}

```

- Creating a slice with make
  - Slices can be created with the built-in make function; this is how you create dynamically-sized arrays.
  - The make function allocates a zeroed array and returns a slice that refers to that array:

``` Go
a := make([]int, 5)  // len(a)=5
```

  - To specify a capacity, pass a third argument to make:

``` Go
b := make([]int, 0, 5) // len(b)=0, cap(b)=5

b = b[:cap(b)] // len(b)=5, cap(b)=5
b = b[1:]      // len(b)=4, cap(b)=4
```

- Slices of slices
  - Slices can contain any type, including other slices.

``` Go
package main

import (
	"fmt"
	"strings"
)

func main() {
	// Create a tic-tac-toe board.
	board := [][]string{
		[]string{"_", "_", "_"},
		[]string{"_", "_", "_"},
		[]string{"_", "_", "_"},
	}

	// The players take turns.
	board[0][0] = "X"
	board[2][2] = "O"
	board[1][2] = "X"
	board[1][0] = "O"
	board[0][2] = "X"

	for i := 0; i < len(board); i++ {
		fmt.Printf("%s\n", strings.Join(board[i], " "))
	}
}
```

- Appending to a slice
  - It is common to append new elements to a slice, and so Go provides a built-in `append` function. The documentation of the built-in package describes `append` .

``` Go
func append(s []T, vs ...T) []T

```

  - The first parameter `s` of append is a slice of type `T` , and the rest are `T` values to append to the slice.
  - The resulting value of `append` is a slice containing all the elements of the original slice plus the provided values.
  - If the backing array of `s` is too small to fit all the given values a bigger array will be allocated. The returned slice will point to the newly allocated array.
  - [Slices: usage and internals](https://blog.golang.org/go-slices-usage-and-internals)

``` Go
package main

import "fmt"

func main() {
	var s []int
	printSlice(s)

	// append works on nil slices.
	s = append(s, 0)
	printSlice(s)

	// The slice grows as needed.
	s = append(s, 1)
	printSlice(s)

	// We can add more than one element at a time.
	s = append(s, 2, 3, 4)
	printSlice(s)
}

func printSlice(s []int) {
	fmt.Printf("len=%d cap=%d %v\n", len(s), cap(s), s)
}

```

- Range
  - The `range` form of the `for` loop iterates over a slice or map.
  - When ranging over a slice, two values are returned for each iteration. The first is the index, and the second is a copy of the element at that index.

``` Go
package main

import "fmt"

var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}

func main() {
	for i, v := range pow {
		fmt.Printf("2**%d = %d\n", i, v)
	}
}

```

- Range continued
  - You can skip the index or value by assigning to `_` .

``` Go
for i, _ := range pow
for _, value := range pow

```

  - If you only want the index, you can omit the second variable.

``` Go
for i := range pow
```

``` Go
package main

import "fmt"

func main() {
	pow := make([]int, 10)
	for i := range pow {
		pow[i] = 1 << uint(i) // == 2**i
	}
	for _, value := range pow {
		fmt.Printf("%d\n", value)
	}
}

```

- Maps
  - A `map` maps keys to values.
  - The zero value of a `map` is `nil` . A `nil`  `map` has no keys, nor can keys be added.
  - The make function returns a map of the given type, initialized and ready for use.

``` Go
package main

import "fmt"

type Vertex struct {
	Lat, Long float64
}

var m map[string]Vertex

func main() {
	m = make(map[string]Vertex)
	m["Bell Labs"] = Vertex{
		40.68433, -74.39967,
	}
	fmt.Println(m["Bell Labs"])
}

```

- Map literals
  - Map literals are like struct literals, but the keys are required.

``` Go
package main

import "fmt"

type Vertex struct {
	Lat, Long float64
}

var m = map[string]Vertex{
	"Bell Labs": Vertex{
		40.68433, -74.39967,
	},
	"Google": Vertex{
		37.42202, -122.08408,
	},
}

func main() {
	fmt.Println(m)
}

```

- Map literals continued
  - If the top-level type is just a type name, you can omit it from the elements of the literal.

``` Go
package main

import "fmt"

type Vertex struct {
	Lat, Long float64
}

var m = map[string]Vertex{
	"Bell Labs": {40.68433, -74.39967},
	"Google":    {37.42202, -122.08408},
}

func main() {
	fmt.Println(m)
}
```

- Mutating Maps
  - Insert or update an element in map m:

``` Go
m[key] = elem
```

  - Retrieve an element:

``` Go
elem = m[key]
```

  - Delete an element:

``` Go
delete(m, key)
```

  - Test that a key is present with a two-value assignment:

``` Go
elem, ok = m[key]
```

- If key is in m, ok is true. If not, ok is false.

- If key is not in the `map` , then elem is the zero value for the `map's` element type.

Note: If `elem` or `ok` have not yet been declared you could use a short declaration form:

``` Go
elem, ok := m[key]
```

- Function values
- Functions are values too. They can be passed around just like other values.
- Function values may be used as function arguments and return values.

``` Go
package main

import (
	"fmt"
	"math"
)

func compute(fn func(float64, float64) float64) float64 {
	return fn(3, 4)
}

func main() {
	hypot := func(x, y float64) float64 {
		return math.Sqrt(x*x + y*y)
	}
	fmt.Println(hypot(5, 12))

	fmt.Println(compute(hypot))
	fmt.Println(compute(math.Pow))
}

```

- Function closures
  - Go functions may be closures. A closure is a function value that references variables from outside its body. The function may access and assign to the referenced variables; in this sense the function is "bound" to the variables.
  - For example, the `adder` function returns a closure. Each closure is bound to its own sum variable.

``` Go
package main

import "fmt"

func adder() func(int) int {
	sum := 0
	return func(x int) int {
		sum += x
		return sum
	}
}

func main() {
	pos, neg := adder(), adder()
	for i := 0; i < 10; i++ {
		fmt.Println(
			pos(i),
			neg(-2*i),
		)
	}
}

```

## Methods and interfaces

Learn how to define methods on types, how to declare interfaces, and how to put everything together.

### [Methods and interfaces](https://tour.golang.org/methods)

This lesson covers methods and interfaces, the constructs that define objects and their behavior.

- Methods
  - Go does not have classes. However, you can define methods on types.
  - A method is a function with a special receiver argument.
  - The receiver appears in its own argument list between the `func` keyword and the method name.
  - In this example, the `Abs` method has a receiver of type `Vertex` named `v` .

``` Go
package main

import (
	"fmt"
	"math"
)

type Vertex struct {
	X, Y float64
}

func (v Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
	v := Vertex{3, 4}
	fmt.Println(v.Abs())
}

```

- Methods are functions
  - Remember: a `method` is just a `function` with a receiver argument.
  - Here's `Abs` written as a regular function with no change in functionality.

``` Go
package main

import (
	"fmt"
	"math"
)

type Vertex struct {
	X, Y float64
}

func Abs(v Vertex) float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
	v := Vertex{3, 4}
	fmt.Println(Abs(v))
}

```

- Methods continued
  - You can declare a method on non-struct types, too.
  - In this example we see a numeric type `MyFloat` with an `Abs` method.
  - You can only declare a method with a receiver whose type is defined in the same package as the method.
  - You cannot declare a method with a receiver whose type is defined in another package (which includes the built-in types such as `int` ).

``` Go
package main

import (
	"fmt"
	"math"
)

type MyFloat float64

func (f MyFloat) Abs() float64 {
	if f < 0 {
		return float64(-f)
	}
	return float64(f)
}

func main() {
	f := MyFloat(-math.Sqrt2)
	fmt.Println(f.Abs())
}

```

- Pointer receivers
  - You can declare methods with pointer receivers.
  - This means the receiver type has the literal syntax `*T` for some type `T` . (Also, `T` cannot itself be a pointer such as `*int` .)
  - For example, the `Scale` method here is defined on `*Vertex` .
  - Methods with pointer receivers can modify the value to which the receiver points (as `Scale` does here). Since methods often need to modify their receiver, pointer receivers are more common than value receivers.
  - Try removing the `*` from the declaration of the `Scale` function on line 16 and observe how the program's behavior changes.
  - With a value receiver, the `Scale` method operates on a copy of the original `Vertex` value. (This is the same behavior as for any other function argument.) The `Scale` method must have a pointer receiver to change the `Vertex` value declared in the `main` function.

``` Go
package main

import (
	"fmt"
	"math"
)

type Vertex struct {
	X, Y float64
}

func (v Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func (v *Vertex) Scale(f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}

func main() {
	v := Vertex{3, 4}
	fmt.Println(v.Abs())
	v.Scale(10)
	fmt.Println(v.Abs())
}

```

- Pointers and functions
  - Here we see the `Abs` and `Scale` methods rewritten as functions.
  - Again, try removing the `*` from line 16. Can you see why the behavior changes? What else did you need to change for the example to compile?

``` Go
package main

import (
	"fmt"
	"math"
)

type Vertex struct {
	X, Y float64
}

func Abs(v Vertex) float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func Scale(v *Vertex, f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}

func main() {
	v := Vertex{3, 4}
	Scale(&v, 10)
	fmt.Println(Abs(v))
}

```

- Methods and pointer indirection
- Comparing the previous two programs, you might notice that functions with a pointer argument must take a pointer:

``` Go
var v Vertex
ScaleFunc(v, 5)  // Compile error!
ScaleFunc(&v, 5) // OK
```

- while methods with pointer receivers take either a value or a pointer as the receiver when they are called:

``` Go
var v Vertex
v.Scale(5)  // OK
p := &v
p.Scale(10) // OK
```

- For the statement `v.Scale(5)`, even though `v` is a value and not a pointer, the method with the pointer receiver is called automatically. That is, as a convenience, Go interprets the statement `v.Scale(5)` as `(&v).Scale(5)` since the `Scale` method has a pointer receiver.

``` Go
package main

import "fmt"

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

func main() {
	v := Vertex{3, 4}
	v.Scale(2)
	ScaleFunc(&v, 10)

	p := &Vertex{4, 3}
	p.Scale(3)
	ScaleFunc(p, 8)

	fmt.Println(v, p)
}

```

- Methods and pointer indirection (2)
  - The equivalent thing happens in the reverse direction.

- Functions that take a value argument must take a value of that specific type:

``` Go
var v Vertex
fmt.Println(AbsFunc(v))  // OK
fmt.Println(AbsFunc(&v)) // Compile error!

```

- while methods with value receivers take either a value or a pointer as the receiver when they are called:

``` Go
var v Vertex
fmt.Println(v.Abs()) // OK
p := &v
fmt.Println(p.Abs()) // OK

```

- In this case, the method call p.Abs() is interpreted as (*p).Abs().

``` Go
package main

import (
	"fmt"
	"math"
)

type Vertex struct {
	X, Y float64
}

func (v Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func AbsFunc(v Vertex) float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
	v := Vertex{3, 4}
	fmt.Println(v.Abs())
	fmt.Println(AbsFunc(v))

	p := &Vertex{4, 3}
	fmt.Println(p.Abs())
	fmt.Println(AbsFunc(*p))
}

```

- Choosing a value or pointer receiver
  - There are two reasons to use a pointer receiver.
    - The first is so that the method can modify the value that its receiver points to.
    - The second is to avoid copying the value on each method call. This can be more efficient if the receiver is a large struct, for example.
  - In this example, both `Scale` and `Abs` are with receiver type `*Vertex`, even though the `Abs` method needn't modify its receiver.
  - In general, all methods on a given type should have either value or pointer receivers, but not a mixture of both. (We'll see why over the next few pages.)

``` Go
package main

import (
	"fmt"
	"math"
)

type Vertex struct {
	X, Y float64
}

func (v *Vertex) Scale(f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}

func (v *Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
	v := &Vertex{3, 4}
	fmt.Printf("Before scaling: %+v, Abs: %v\n", v, v.Abs())
	v.Scale(5)
	fmt.Printf("After scaling: %+v, Abs: %v\n", v, v.Abs())
}

```

- Interfaces
  - An interface type is defined as a set of method signatures.
  - A value of interface type can hold any value that implements those methods.
- Note: There is an error in the example code on line 22. `Vertex` (the value type) doesn't implement `Abser` because the `Abs` method is defined only on `*Vertex` (the pointer type).

``` Go
package main

import (
	"fmt"
	"math"
)

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
	//a = v

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

- Interfaces are implemented implicitly
  - A type implements an interface by implementing its methods. There is no explicit declaration of intent, no "implements" keyword.
  - Implicit interfaces decouple the definition of an interface from its implementation, which could then appear in any package without prearrangement.

``` Go
package main

import "fmt"

type I interface {
	M()
}

type T struct {
	S string
}

// This method means type T implements the interface I,
// but we don't need to explicitly declare that it does so.
func (t T) M() {
	fmt.Println(t.S)
}

func main() {
	var i I = T{"hello"}
	i.M()
}

```







## Concurrency

Go provides concurrency features as part of the core language.

### [Concurrency](https://tour.golang.org/concurrency)

Go provides concurrency constructions as part of the core language. This lesson presents them and gives some examples on how they can be used.

This module goes over `goroutines` and `channels` , and how they are used to implement different concurrency patterns.

## Resources

- [Go's Declaration Syntax](https://blog.golang.org/declaration-syntax) - Go's declarations read left to right.
- [Defer, Panic, and Recover](https://blog.golang.org/defer-panic-and-recover) - Go has the usual mechanisms for control flow: if, for, switch, goto. It also has the go statement to run code in a separate goroutine. Here I'd like to discuss some of the less common ones: defer, panic, and recover.
- [Share Memory By Communicating](https://blog.golang.org/codelab-share) - Go's concurrency primitives - goroutines and channels - provide an elegant and distinct means of structuring concurrent software.

## Contributing

Pull requests are welcome. For major changes, please open an issue first to discuss what you **would** like to change.

Please make sure to update tests as appropriate.

## License

[MIT](https://choosealicense.com/licenses/mit/)
