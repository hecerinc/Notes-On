# Python Notes


Base types are the same. 

Booleans are `True` and `False` (capitalised). In Python, booleans `True` and `False` can be operated on as if they were the integers `1` and `0`;

`i++` and `i--` do not work :(

Comparisons can be chained. For example, `a < b == c` tests whether `a` is less than `b` and moreover `b` equals `c`.

This shit is valid:
```Python
>>> string1, string2, string3 = '', 'Trondheim', 'Hammer Dance'
```

Note that in Python, unlike C, assignment cannot occur inside expressions.

`NoneType` keyword `None`

`2**2 == 2^2 // exponent operator` 

`2.2 * 3.0`

For the last problem (10), typing the expression into your Python interpreter may give a result that is not exact. For example, 6.6000000000000005 instead of 6.6. This is because computers have difficulty storing fractions exactly in binary. So when performing calculations, numbers first get converted to binary then the calculation is performed and the result converted back into decimal. However, fractions cannot be stored exactly in binary (for example, there is no way to store 0.33333 repeating exactly) so we introduce these small rounding errors that propagate until the final answer. 



## Importing

You can import modules into the symbol table of the interpreter or a Python script via the **`import`** keyword. If you create a file called `fibo.py` then you can import it via
```Python
import fibo
```

This is not directly imported into the symbol table of the interpreter, but rather the whole namespace is imported and you can access it via the dot notation
```Python
fib(1000) # this will not work
fibo.fib(100) # this will
```
There is a variant of the `import` statement that imports names from a moduule directly into the importing module's symbol table

```Python
>>> from fibo import fib, fib2
>>> fib(500) # this will now work

1 1 2 3 5 8 13 21 34 55 89 144 233 377
>>> from fibo import * # this imports all the names from the module
```
This does not introduce the module name from which the imports are taken in the local symbol table (so in the example, fibo is not defined).


## String manipulation

**Strings are immutable.**

If you don’t want characters prefaced by \ to be interpreted as special characters, you can use raw strings by adding an r before the first quote:
```Python
>>> print 'C:\some\name'  # here \n means newline!
C:\some
ame
>>> print r'C:\some\name'  # note the r before the quote
C:\some\name
```


```Python
>> 3 * 'a'
'aaa'

>> str(123)
'123'

>> len('abc')
3

>> 'abc'[0]
'a'
```


**`lower()`**
Returns a lower case version of the string. Inverse is **`upper()`**.
```Python
str = 'HELLO'
str = str.lower() 
print str // prints 'hello'
```

Other string methods

- `str.capitalize()`
- `str.center(width [,fillchar])`
- `str.count(sub[, start[, end]])` // sub is substring
- `str.find(substring [, start [,end]])` // returns index of first occurrence from front to back
- `str.rfind(sub[, start[, end]])` // returns index of first occurrence from back to front
- `'Py' in 'Python'` // returns true or false if the substr is found
- `str.replace(old, new [,count])` 
- `str.split(separator)` // returns a list
- `str.strip([chars])` // equivalent to `trim` in PHP


#### Slicing
```Python
>> 'abc'[1:3]
'bc'
```

Slicing is done s[i:j] From index i to j-1. If you omit the starting index, Python will assume that you wish to start your slice at index 0. If you omit the ending index, Python will assume you wish to end your slice at the end of the string.

```Python
>>> s = 'Python is Fun!'
>>> s[1:5]
'ytho'
>>> s[:5]
'Pytho'
>>> s[1:]
'ython is Fun!'
>>> s[:]
'Python is Fun!'
```

There's one other cool thing you can do with string slicing. You can add a third parameter, k, like this: `s[i:j:k]`. This gives a slice of the string s from index `i` to index `j-1`, with step size `k`.

```Python
>>> s = 'Python is Fun!'
>>> s[1:12:2]
'yhni u'
>>> s[1:12:3]
'yoiF'
>>> s[::2]
'Pto sFn'
```

## `in`

The operators `in` and `not in` test for collection membership (a 'collection' refers to a string, list, tuple or dictionary). The expression 

```python
element in coll
```

evaluates to `True` if `element` is a member of collection `coll`, and `False` otherwise.

You can also do 

```python
element not in coll
```


## Input/Output

`print` for output
`raw_input('Prompt to console')`


## Comments

Comments are done via the pound `#` sign
```Python
# this is a python comment
```

## Conditions

Indentation sensitive. No parentheses.

```Python
if x%2 == 0:
	print('')
	print('Even')
else:
	print('')
	print('Odd')
print('Done')
```

`elif` is equivalent to `else if`


## Loops
```Python
while(condition):
	// Indented code
// other lines of code out of the loop

for <identifier> in <sequence>:
	<code block>

```

**`range`**
`range(n)` Built in function that will give the sequence of numbers from 0 to `n`, non-inclusive (`n-1)`
`range(m,n)` Gives the sequence of numbers from `m` to `n-1`

To specify a step size, you must specify a start value - the call is `range(start, stop, stepSize)` 

## Floating point approximations

If there is no integer p such that `x*(2**p)` is a whole number, then internal representation is always an approximation

Suggest that testing equality of floats is not exact

You should use `abs(x-y) < 0.0001` rather than `x==y`


## Functions

```Python
def <fuction_name> (<formal parameters>):
	<function_body>
```

Expressions are evaluated until 
- Run out of expressions, in which case the special value `None` is returned
- Or until special keyword `return` is found, in which case the subsequent expression is evaluated and that value is returned as value of the function call

In Python it *is* legal to compare functions.

#### Default arguments

**Note:** The default value is evaluated only once

```Python
def ask_ok(prompt, retries = 4, complaint ='Yes or no, please!'):
	# some code here
```

#### Keyword arguments

If you define a function with some arguments, you don't necessarily have to call it wih the arguments in the same order.
You can call the function any number of ways so long as you name the arguments you are passing to the function.
```Python
def parrot(voltage, state='a stiff', action='voom', type='Norwegian Blue'):
	# some code

# You can call this function in any of these ways:
parrot(1000)                                          # 1 positional argument
parrot(voltage=1000)                                  # 1 keyword argument
parrot(voltage=1000000, action='VOOOOOM')             # 2 keyword arguments
parrot(action='VOOOOOM', voltage=1000000)             # 2 keyword arguments
parrot('a million', 'bereft of life', 'jump')         # 3 positional arguments
parrot('a thousand', state='pushing up the daisies')  # 1 positional, 1 keyword

# But all of these are invalid:
parrot()                     # required argument missing
parrot(voltage=5.0, 'dead')  # non-keyword argument after a keyword argument
parrot(110, voltage=220)     # duplicate value for the same argument
parrot(actor='John Cleese')  # unknown keyword argument 
```

**In a function call, keyword arguments must follow positional arguments.**

### Docstrings

```Python
def a(x):
	'''
	x: int or float.
	'''
	return x+1
```

The three single quotes after the `def` line (`'''`) are called the *docstring* of the function. A docstring is a special type of comment that is used to document what your function is doing. Typically, docstrings will explain what the function expects the type(s) of the argument(s) to be, and what the function is returning. In Python, docstrings appear immediately after the def line of a function, before the body. Docstrings start and end with triple quotes - this can be triple single quotes or triple double quotes, it doesn't matter as long as they match. 


## Environment

There is a table of variables and functions that is called an environment where Python will check for values when you need it. 


## Compound data

1. Tuples
2. Lists
3. Dictionaries

### Tuples

Ordered sequence of elements. It's like an array in any other language. Type of element does not matter. **Tuples are immutable**. You cannot change an element.

```Python
t1 = (1, 'two', 3)
t2 = (t1, 'four')
```

#### Operations

- **Concatenation:** `print t1+t2`
- **Indexing:** `(t1+t2)[3]`
- **Slicing:** `(t1+t2)[2:5]`
- **Singleton:** A tuple with one element. `(3,)`. **The comma is necessary**
- **Empty typle:** `divisors = ()`

```Python
def findDivisors(n1, n2):
	'''
	assumes n1 and n2 positive ints
	returns tuple containing common divisors for n1 and n2
	'''
	divisors = () # the empty tuple
	for i in range(1, min(n1, n2) +1):
		if n1%i ==0 and n2%i ==0:
			divisors = divisors + (i,)
	return
```

### Lists

Exactly like tuples, except uses brackets ([]) instead of () parens. 

There is one **main** difference: **lists are mutable**. Tuples are not. 

#### Operations:
- `list.append(X)`
- `list.extend(anotherList)` // append all items of anotherList to list
- `list.insert(pos, item)` pos = index of element before which to insert
- `list.remove(x)`
- `list.pop([index])` // index is optional, if no index passed, removes and returns the last item in the list
- `list.index(x)` Returns the index in the list of the element x
- `list.reverse()` Reverse the elements of the list, same list.


#### List comprehensions

List comprehensions provide a concise way to create lists.

```Python
>>> squares = []
>>> for x in range(10):
...		squares.append(x**2)
...
>>> squares
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

Is **equivalent** to 

```Python
squares = [x**2 for x in range(10)]
```

**Example 2**
```Python
>>> from math import pi
>>> [str(round(pi, i)) for i in range(1, 6)]
['3.1', '3.14', '3.142', '3.1416', '3.14159']
```

#### `del` statement
There is a way to remove an item from a list given its index instead of its value: the `del` statement. It can also be used to remove slices from a list or clear the entire list.

```Python
>>> a = [-1, 1, 66.25, 333, 333, 1234.5]
>>> del a[0]
>>> a
[1, 66.25, 333, 333, 1234.5]
>>> del a[2:4]
>>> a
[1, 66.25, 1234.5]
>>> del a[:]
>>> a
[]
```


## Dictionaries

Associative arrays in PHP. 	

```Python
{} // empty dictionary
tel = {'jack': 8293824, 'sophie': 284928}
tel['john'] = 38293
```

#### Operations
- `del` to delete a key:value pair
- `dict.keys()` get a list of keys arbitrarily ordered

Dict comprehensions can also be used to create dictionaries

```Python
>>> {x: x**2 for x in (2, 4, 6)}
{2: 4, 4: 16, 6: 36}
>>> dict(sape=4139, guido=4127, jack=4098)
{'sape': 4139, 'jack': 4098, 'guido': 4127}
```

#### Iteration

When looping through dictionaries, the key and corresponding value can be retrieved at the same time using the `iteritems()` method.
```Python
>>> knights = {'gallahad': 'the pure', 'robin': 'the brave'}
>>> for k, v in knights.iteritems():
...     print k, v
...
gallahad the pure
robin the brave
```

## Assertions

If you know you are expecting input in a special format, you can `assert` that in your code so that if that condition is not met, Python will raise an `AssertionError` which is generally good defensive programming. You cannot control response as in an Exception though, but you can make it easier to locate bugs.

```Python
def avg(grades, weights):
	assert not len(grades) == 0, 'no grades data'
	newgr = [convertLetterGrade(elt) for elt in grades]
	return dotProduct(newgr,weights)/len(newgr)
```

This code will raise an `AssertionError` if it is given an empty list of grades, but otherwise will run properly. Error will print out information `no grades data` as part of process.

While preconditions are normally checked, you can also `assert` a postcondition before proceeding to next stage.

```Python
def avg(grades, weights):
	assert not len(grades) == 0, 'no grades data'
	assert len(grades) == len(weights), 'wrong number grades'
	newgr = [convertLetterGrade(elt) for elt in grades]
	result =  dotProduct(newgr,weights)/len(newgr)
	assert 0.0 <= result <= 100.0
	return result
```

There is a slight loss of efficiency. Not to be used as a replacement, but as a supplement of testing

Should probably rely on exceptions if users supplies bad data input, and use assertions for:
- Checking types of arguments or values
- Checking that invariants on data structures are met
- Checking constraints on values
- Checking for violations of contsraints on procedure (e.g. no duplicates in a list)


## Black Box Testing

Black-box testing is a method of software testing that tests the functionality of an application. Recall from the lecture that a way to think about black-box testing is to look at both:

- The possible paths through the specification.
- The possible boundary cases.

Undoubtably many - if not all - of the listed tests look like they would be pretty good for testing the function size. However, we want you to think critically about the way size is specified - including possible boundary cases - and pick a set of tests that adequately and fully tests all paths and boundary conditions. Be sure the set of tests you pick does not have extraneous, useless, or repetitive tests. 

## Glass Box Testing

Recall from the lecture that a path-complete glass box test suite would find test cases that go through every possible path in the code. This is different from black-box testing, because in black-box testing you only have the function specification. For glass-box testing, you actually know how the function you are testing is defined. Thus you can use this definition to figure out how many different paths through the code exist, and then pick a test suite based on that knowledge.

Undoubtably many - if not all - of the listed tests look like they would be pretty good for testing the function maxOfThree. However, we want you to think critically about the way maxOfThree is defined - including possible boundary cases - and pick a set of tests that adequetly and fully tests all paths and boundary conditions. A good first step will be to look at the function definition and figure out what paths through the code exist. Then, look through the suggested test suites one test at a time and see if the suite tests every single path.

In glass box testing, we try to sample as many paths through the code as we can. In the case of loops, we want to sample three general cases:

1. Not executing the loop at all.
2. Executing the loop exactly once.
3. Executing the loop multiple times.

Test Suite B has cases that explores all three loop cases.

Test Suite A only explores paths that execute the loop 0 or 1 times.

Test Suite C only explores paths that execute the loop more than 1 time.