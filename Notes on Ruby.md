# Notes on Ruby

```Ruby
3.times { puts "Hello world "}
```
is equivalent to Java's
```Java
public static void main(){
	for(int i=0; i<3; i++){
		System.out.println("Hello world");
	}
}
```

- **Everything in Ruby is an object**
- No notion of "native" types (e.g. ints, bools, chars), they're all objects!
- Everything in Ruby is evaluated
- Ruby has the ternary operator `condition ? true : false` :D

So for example, this is legal:

```Ruby
> 3
```

That'll just print out 3

- **Comments:** Use the pound sign (#)


#### `puts`

Stands for **put s**tring. Standard ruby method to print things to console.

- Adds a new line after the printed string

You can also use `print` and it will print no newline char

#### `p`

```Ruby
p "Got it" 
```

Prints out the internal representation of an object

## Execute Ruby
Just `ruby nameoffile.rb` in the terminal. 

There is also a REPL interpreter. Run 

```Ruby
$ irb
```

### **`||`**

The double pipe operator `||` evaluates the left side

- If `true`, returns it
- Else - returns the right side
- `@x = @x || 5` will return `5` the first time and `@x` the next time

Short form:

```Ruby
@x ||= 5 # same as above
```

### Naming conventions

- Variables: lowercase or `snake_case` if multiple words
- Constants: `ALL_CAPS` or 	`FirstCap`
- Classes and modules: `FirstCap`

No need for semicolons. If you want to do multiple statements in the same line, put a semicolon between.

**`nil`** is `NULL` for Ruby

## Flow Control

- if/elsif/else
- case
- until / unless
- while / for

No parentheses or curly braces. Always end the control block with an `end` statement


```Ruby
a = 5 # declare var

if a == 3
	puts "a is 3"
elsif a == 5
	puts "a is 5"
else
	puts "a is not 3 nor 5"
end


a = 5

unless a == 6
	puts "a is not 6"
end
```

### **`while`, `until`**

```Ruby
a = 10
while a > 9
	puts a
	a -= 1
end

# >= 10 


a = 9

until a >= 10
	puts a
	a += 1
end
```

### Modifier form

`if, unless, while, until` can be used on the same line as the statement

```Ruby
# if modifier form

a = 5
b = 0

puts "One liner" if a == 5 and b == 0 
# => One liner


# while modifier form

times_2 = 2
times_2 *= 2 while times_2 < 100
puts times_2 

# => 128
```

## True & False in Ruby

**The only things that evaluate to `false` are:** `false` and `nil` literal objects

### **EVERYTHING ELSE EVALUATES TO TRUE!**

- `""` is `true` (empty string)
- `0` is `true` (!watch out!)
- `"false"` is `true`
- `"nil"` is `true`
- `nil` is `false`
- `false` is `false`

### `===`

Case equality operator. Not like JavaScript. This is not strict comparison. It is unique to Ruby.

```Ruby
if /sera/ === "coursera" # regular expression comparison
	puts "Triple Equals"
end
# => Triple Equals

if "coursera" === "coursera"
	puts "also works"
end
# => also works

if Integer === 21
	puts "21 is an Integer"
end
# => 21 is an Integer
```

### Case expressions

2 ways to do this. Not fall-through logic!

```Ruby
age = 21 

case 
	when age >= 21
		puts "You can buy a drink"
	when 1 == 0
		puts "Written by a drunk programmer"
	else
		puts "Default condition"
end
# => You can buy a drink

# Second way:

name = 'Fisher'
case name
	when /fish/i then puts "Something is fishy here"
	when 'Smith' then puts "Your name is Smith"
end

# => Something is fishy here
```

## For loops

- Hardly used
- `each` / `times` is preferred

```Ruby
for i in 0..2 # range of 0 to 2 (DATATYPE)
	puts i
end

# => 0
# => 1
# => 2
```

## Functions

In Ruby, every function/method has at least one class it belongs to. Not always written inside a class.

Parentheses are **optional** both when defining and calling a method. Used for clarity

**`return` keyword is optional**

```Ruby
def simple
	puts "no parens"
end

def simple1()
	puts "yes parens"
end

# all perfectly legal ways of calling:

simple() # => no parens
simple 	# => no parens
simple1 # => yes parens

def divide(one, two)
	return " I don't think so" if two == 0
	one/two
end

```

The last line of the function is always returned.

#### Expressive methods

Method names can end with 

- `?` - Predicate methods
- `!` - Dangerous side-effects

```Ruby
def can_divide_by?(number)
	return false if number.zero?
	true
end

puts can_divide_by? 3 # => true
puts can_divide_by? 0 # => false
```

You can define your function to have default args just like you would anywhere else.

#### Splat

This is used to pass multiple arguments as one array.

`*` prefixes parameter inside method definition. 

```Ruby
def max(one_param, *numbers, another)
	# Variable length parameters passed in
	# become an array
	numbers.max # this is returned
end

puts max("something", 7, 32, -4, "more") 
# => 32
```

## Blocks

Blocks are chunks of code that you pass into methods

- Enclosed between either curly braces (`{}`) or the keywords **`do`** and **`end`**
- Passed to methods as last parameter
- Can accept arguments in between `||` pipes

##### Convention:

- Use `{}` when block content is a single line
- Use `do` and `end` when block content spans multipl lines
- Often used as iterators

**Example:**

```Ruby
1.times { puts "Hello world!" }
# => Hello world!

2.times do |index|
	if index > 0 
		puts index
	end
end

# => 1

# Or the equivalent:

2.times { |index| puts index if index > 0}
# => 1
```

There are two ways to configure a block in your own method

- **Implicit**
	- Use `block_given?` to see if block was passed in
	- Use `yield` to "call" the block

- **Explicit**
	- Use `&` in front of the last parameter
	- Use `call` method to call the block

**Implicit:**

Need to check `block_given?` else an exception is thrown

```Ruby
def two_times_implicit
	return "No block" unless block_given?
	yield
	yield 
end

puts two_times_implicit { print "Hello" } 
# => Hello
# => Hello

puts two_times_implicit # => No block
```

**Explicit:**

Should check if the block is `nil?`

```Ruby
def two_times_explicit (&i_am_a_block)
	return "No block" if i_am_a_block.nil?
	i_am_a_block.call
	i_am_a_block.call
end

puts two_times_explicit # => No block
puts two_times_explicit { print "Hello" }
# => Hello
# => Hello
```

## Files

#### Reading and writing files

**Reading:**

```Ruby
File.foreach('test.txt') do |line|
	puts line
	p line
	p line.chomp # chops off newline character
	p line.split # array of words in line
end

# => Output:
# The first line of the file
# "The first line of the file\n"
# "The first line of the file"
# ["The", "first", "line", "of", "the", "file"]
# And the second
# "And the second\n"
# "And the second"
# ["And", "the", "second"]
# Third
# "Third"
# "Third"
# ["Third"]
```

**Writing:**

```Ruby
File.open('test1.txt', 'w') do |file|
	file.puts "One line"
	file.puts "Another"
end
```
The file is automatically closed after the block executes

#### Exceptions

If a file does not exist you will get an exception and executioin will terminate.

**`begin` & `rescue`**

However, you can handle the exception like so:

```Ruby
begin
	File.foreach('does_not_exist.txt') do |line|
		puts line.chomp
	end
end
rescue Exception => e
	puts e.message
	puts "Let's pretend this didn't happen..."
end
```

For files, specifically, you can check if the file exists:

```Ruby
if File.exist? 'test.txt'
	# do something with file...
end
```


## Strings

- **Single quote literals**
	- Allow escaping of `'` with `\`
	- Almost everything else as-is
- **Double quoted strings**
	- Interpret special characters like `\n` and `\t`
	- Allow for string interpolation

No concatenating via `+`

#### String interpolation

```Ruby
def multiply(one, two)
	"#{one} multiplied by #{two} equals #{one * two}"
end
puts multiply(5, 3)
# => 5 multiplied by 3 is 15
```

You can also create a string `%Q{long multiline string}` which has the same behaviour as a double-quoted string.

__Example:__

```Ruby
weather = %Q{It's a hot day outside
				Grab your umbrellas...}
weather.lines do |line|
	line.sub! 'hot', 'rainy' # substitute 'hot' with 'rainy'
	puts "#{line.strip}"
end
# => It's a rainy day outisde
# => Grab your umbrellas...
```

Strings are generally immutable, but you can modify it with a version of the string-modifying method that ends with `!`. 

__Example:__

```Ruby
name = " tim"
puts name.lstrip.capitalize # => Tim
p name # => " tim"
name.lstrip! # (destructive) removes the leading space
p name # => "tim"
name[0] = 'K' # replace the first character
puts name = Kim
```

## Arrays

- Collection of __object references__ (auto-expandable)
- Indexed using `[]` operator (method)
- Can be indexed with __negative numbers__ or __ranges__
- Can use `%w{str1 str2}` for string array creation

```Ruby
arr_words = %w{ what a great day today!}
pp arr_words[-3, 2] # => ["great", "day"] (go back 3 and take 2)

# Range 

p arr_words[2..4] # => ["great", "day", "today"]
```

### Modifying arrays

- Append: `arr.push()` or `<<`
- Prepend: `arr.unshift()`
- Inserting: `arr.insert(position, item)`
- Remove: `arr.pop()` or `arr.shift()`
- Set: `[] = ` method

`sort!` and `reverse!`: self-explanatory

If you insert an element at the 6th position of a 4 element array, it will add it and fill the missing spaces with `nil`

## Ranges

Data type to express natural sequences.

```Ruby
1..10
'a'..'z'

```

- __`..`__ is all-inclusive
- __`...`__ is end-exclusive

```Ruby
puts (1...10) === 5.3 # => true (the range of 1 to 9 includes 5.3)

age = 55
case age
	when 0..12 then puts "Still a baby"
	when 13..99 then puts "Teenager at heart!"
	else puts "You're getting older"
end

```

## Hashes

Basically, associative arrays in PHP, bat for Ruby

- Indexed collections of object references
- Created with either `{}` or `Hash.new`
- Also known as __associative arrays__
- Index can be anything
- Exactly like in PHP

```Ruby
editor_props = {"font" => "Arial", "size" => 12, "color" => "red"}

# Iteration

editor3_props.each_pair do |key, value|
	puts "Key: #{key} value: #{value}"
end

Hash.new(0) # pass a default value that is returned when a value does not exist

# This is a stupid line of code:
word_frequency = Hash.new(0)

sentence = "Chicka Chicka boom boom"
sentence.split.each do |word|
	word_frequency[word.downcase] += 1
end
```


## Object Oriented Programming in Ruby

Methods are **public** by default. There is a default, blank constructor.

You can change access scope using `private`, `protected`, and `public` and everything until the next access control keyword will remain of that type

Private methods are not callable from inside the class either with an explicit receiver.

You can include the class definitions like so:

```Ruby
require_relative 'nameoffile' # sans .rb extension

# some more code...
```

#### Instance variables

- Are **private** by default
- Begin with `@` (`@name`)
- Not declared
	- Spring into existence when first used
- Available to all instance methods of the class

#### Instantiation

Is done calling the **`new`** method. This calls the class's `initialize` method, which serves as a constructor.

The object state can/should be initialized inside the `initialize` method.

```Ruby
class Person
	def initialize (name, age) # Constructor
		@name = name
		@age = age
	end
	def get_info
		@additional_info = "Interesting" # declaration
		"Name: #{@name}, age: #{@age}"
	end
end

person1 = Person.new("Joe", 14)
person2 = Person.new
p person1.instance_variables # [:@name, :@age]
puts person1.get_info # => Name: Joe, age: 14
p person1.instance_variables # [:@name, :@age, :@additional_info]
```

#### Getters and Setters

You must define these methods because object attributes are private by default.

```Ruby
class Person
	def initialize (name, age) # Constructor
		@name = name
		@age = age
	end
	def get_info
		@additional_info = "Interesting" # declaration
		"Name: #{@name}, age: #{@age}"
	end


	# Getter
	def name
		@name
	end
	# Setter:
	def name= (new_name)
		@name = new_name
	end
end
```

It's a bit tedious to do this for every attribute so Ruby provides some sintactic sugar for this:

Using the `attr_*` form

```Ruby
attr_accessor # - getter and setter
attr_reader # - getter only
attr_writer # - setter only
```

So you would have:

```Ruby
class Person
	attr_accessor :name, :age # getters and setters for name and age
end
```

#### `self`

When in a method, the keyword `self` will refer to the object instance. When outside, it will refer to the class itself.

```Ruby
class Person
	attr_reader :age
	attr_accessor :name

	def initialize(name, ageVar) # constructor
		@name = name
		self.age = ageVar # calls the age= method, self is required here
		puts age
	end
	def age= (new_age)
		@age = new_age unless new_age > 120
	end
end
```

### Class methods and variables

Equivalent to Java's `static` methods. Methods called on the class, not on instances.

- Class variables begin with `@@`
- Three ways to define class methods in Ruby 

```Ruby
class MathFunctions
	def self.double(var) # 1. Using self
		times_called; var * 2;
	end
	class << self # 2. Using << self
		def times_called
			@@times_called ||= 0; @@times_called += 1
		end
	end
end

# 3. Outside of class
def MathFunctions.triple(var)
	times_called; var * 3
end

# No instance created:

puts MathFunctions.double 5 # => 10
puts MathFunctions.triple(3) # => 9
puts MathFunctions.times_called # => 3
```

### Inheritance

Every class implicitly inherits from the __`Object`__ class, which in turn inherits from `BasicObject`

Ruby does not support multiple inheritance. Instead you use mixins.

```Ruby
Child < Parent # < denotes inheritance
```

```Ruby
class Dog
	def to_s
		"Dog"
	end
	def bark
		"barks oudly"
	end
end
class SmallDog < Dog
	def bark # Override
		"barks quietly"
	end
end
```

### Modules

Container for classes, methods, and constants or other modules. It's like a class, but it cannot be instantiated. So `Class` really inherits from `Module` and adds `new`

Used as 

- namespace
- mixin

```Ruby
module Sports
	class Match
		attr_accessor :score
	end
end

module Patterns
	class Match
		attr_accessor :complete
	end
end


match1 = Sports::Match.new
match1.score = 45; puts match1.score # => 45

match2 = Patterns::MAtch.new
match2.complete = true; puts match2.complete # => true
```


#### Mixins

- Like interfaces in OO
- Contract - define what a class "could do"

Mixins provide a way to share ready code among multiple classes. 


```Ruby
module SayMyName
	attr_accessor :name
	def print-name
		puts "Name: #{@name}"
	end
end

class Person
	include SayMyName
end
class Company
	include SayMyName
end

person = Person.new
person.name = "Joe"
person.print_name # => Name: Joe
company = Company.new
company.name = "Microsoft Inc."
company.print_name # => Name: Microsoft Inc.
```

You can `include` other built-in modules like `Enumerable`, which includes the `map`, `select`, `reject`, and `detect` methods.


