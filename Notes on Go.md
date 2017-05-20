# Go

Go does not let you have unused variables
Uppercase shit is exported, lowercase isn't

## Variable declaration

```
var x int
var x int = 10
x := 10
```

Everything goes in a package. Packages work exactly like Java.

## Hello world

```Go
package main
import "fmt" // required for printing

func main() { // Always have to have a main function
	fmt.Println("Hello world")
}
```

## Functions

A function that returns nothing

```Go
func print(message string) {
	fmt.Println("%s", message)
}
```

A function that returns an int

```Go
func add(x int, y int) int {
	return x + y
}
```

A function that returns **more than one value**

```Go
func div(x int, y int) (int, bool) {
	if y != 0 {
		return x/y, true
	}
	return x, false
}
```

### Calling functions

```Go
func main() {
	// Prints a message
	print("Hello, world")
	fmt.Println("add(5, 6) = %d", add(5,6))
	n, result := div(3,0)
	fmt.Println("div(3, 0) = %d, %t", n, result)
}
```

## Control flow

### Conditionals

- **Curly braces are mandatory**.
- The `{` after the `if` and `else` must be on the same line
- The `else if` and `else` keywords must be **on the same line as the closing** `}` of the previous part of the structure

```Go
if condition { // no parentheses
	// Do something
} else if condition2 {
	
} else {
	
}
```

You can perform operations on an if clause, and the scope of that variable is the `if` block:

```Go
if value := 60; value > 10 { 
	// Do something
}
```

#### Switch

Switches `break` automatically.

```Go
switch i {
case 0: fmt.Println("Zero")
case 1: fmt.Println("One")
case 2,3,5: fmt.Println("Two")
case 3: fmt.Println("Three")
case 4: fmt.Println("Four")
case 5: fmt.Println("Five")
default: fmt.Println("Unknown Number")
}
```

The special keyword `fallthrough` will make a particular case to fall through to the other clauses.

### Loops

### `for`

- `for` is Go's `while`

```Go
sum := 1
for sum < 10 {
	sum += sum
}
```

## Structures

- Structures can be associated with methods
- A class and a structure are very similar but Go doesn't support classes per se

```Go
type Bicycle struct {
	wheels int
	Brand int
}

// Instantiation:

var bike Bicycle = Bicycle{2, "Fender"}
velo := Bicycle{Brand : "brand2"}
emptybike := Bicycle{}


func changeWheels(b Bicycle) {
	// Note: this will NOT modify the original b object
	b.wheels *= 2
	fmt.Println("b's wheels: %d", b.wheels)
}
```

### Methods 

We can associate functions to structures like so:

```Go
func (s * Bicycle) changeWheels() {
	s.wheels *= 2
}
func main() {
	bike := &Bicycle{2, "Barn"}
	bike.changeWheels()
}
```

**Note:** In the above structure, `Brand` is accesible outisde the package but `wheels` is not

Structs are **passed by value** by default, which means that a copy of the object is created when passed to a function.

## Pointers

- Go supports memory address manipulation through pointers
- `&` is the **address operator** which gets the memory address of an object

```Go
func changeWheels(b *Bicycle) { // pass a pointer
	b.wheels *= 2
}

var unicycle * Bicycle = &Bicycle{wheels : 1, Brand : "Uniccle Inc"}
fmt.Println("Wheels: %d", unicycle.wheels)
changeWheels(unicycle)
fmt.Println("Wheels: %d", unicycle.wheels)
```

**NOTE:** The pointer is also passed as a copy

When passing values, consider the cost of creating a copy of large structures.



## `new`

Go has a **`new`** function that is used to **allocate memory** needed by a type:

```Go
bike := &Bicycle{2, "Brand"}

// produces the same result as:
bike := new(Bicycle)
bike.Brand = "Brand"
bike.wheels = 2
```


## Arrays

- Arrays are fixed size
- Declaring an array requires size specification
- `len()` gets the length of the array
- Arrays are almost never used, instead we use **slices**

```Go
var scores [5]int
scores[0] = 10
fmt.Println(scores) // [10 0 0 0 0]
```


### Array traversal


```Go
// Option 1:

for i := 0; i < len(scores); i++ {
	fmt.Println(scores[i])
}

// Option 2:

for index, score := range scores {
	fmt.Println(score, "at position", index)
}

// Option 3:

for _, score := range scores {
	fmt.Println(score)
}

```

## Slices

- A slice is a lightweight structure that wraps and represents a portion of an array

```Go
scores := []int{1,2,3}
fmt.Println(scores)
// -> [1 2 3]
```

### `make`

You can also create slices using `make`:

```
make(type, length, capacity)
```

> The make built-in function allocates and initializes an object of type slice, map, or chan (only). Like new, the first argument is a type, not a value. Unlike new, make's return type is the same as the type of its argument, not a pointer to it.

```Go
scores := make([]int, 3)
scores[0] = 1
scores[1] = 2
scores[2] = 3
// -> [1 2 3]

// Generate an empty slice with capacity 10:

scores := make([]int, 0, 10)
fmt.Println(scores) // -> []

// Unknown number of elements:

var names []string

```


### `append`

You can expand the slice with `append`: Append adds **at the end** (as it should)

```Go
scores := make([]int, 0, 10)
scores[0] = 100 // this will fail! (can't directly put elements)
scores = append(scores, 100) // correct way this works!
```

### Subset an array

```Go
scores := []int{1,2,3,4,5}
fmt.Println(scores[:3]) // -> [1 2 3]
fmt.Println(scores[2:]) // -> [3 4 5]
```

### Using slices to modify arrays

```Go
scores := []int{1,2,3,4,5}
slice := scores[2:4]
slice[0] = 999
fmt.Println(scores) // -> [1 2 999 4 5]
```
