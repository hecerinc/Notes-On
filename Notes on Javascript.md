# Javascript

## Objects

JavaScript supports the `in` operator

```JS
var obj = {left: 1, right: 2}
console.log("right" in obj); // true
```

And the `delete` operator

```JS
delete obj.left;
console.log("left" in obj); // => false
```


## Array methods

Another fundamental method is slice, which takes a start index and an end index and returns an array that has only the elements between those indices. The start index is inclusive, the end index exclusive.

```JS
console.log([0, 1, 2, 3, 4].slice(2, 4));
// → [2, 3]
console.log([0, 1, 2, 3, 4].slice(2));
// → [2, 3, 4]
```

#### `forEach`

Looping through an array to apply a function to each element. 

```JS
array.forEach(function(entry){
	sum += entry;
});
```

## Higher Order Functions

Functions that operate on other functions, either by taking them as arguments or by returning them, are called *higher-order functions*.

For example, you can have functions that create new functions:

```JS
function greaterThan(n) {
  return function(m) { return m > n; };
}
var greaterThan10 = greaterThan(10);
console.log(greaterThan10(11));
// -> true
```

You can even write functions that provide new types of control flow

```JS
function unless(test, then) {
	if (!test) then();
}
function repeat(times, body) {
	for (var i = 0; i < times; i++) body(i);
}

repeat(3, function(n) {
	unless(n % 2, function() {
		console.log(n, "is even");
	});
});
// → 0 is even
// → 2 is even
```

### `filter`

```JS
function filter(array, test) {
  var passed = [];
  for (var i = 0; i < array.length; i++) {
    if (test(array[i]))
      passed.push(array[i]);
  }
  return passed;
}

console.log(filter(ancestry, function(person) {
  return person.born > 1900 && person.born < 1925;
}));
```

The function above filters out the elements in an array that don't pass a test.

Note how the `filter` function, rather than deleting elements from the existing array, builds up a new array with only the elements that pass the test. This function is *pure*. It does not modify the array it is given.

Like `forEach`, `filter` is also a standard method on arrays. The example defined the function only in order to show what it does internally.


### `map`

The `map` method transforms an array by applying a function to all of its elements and building a new array from the returned values.

```JS
function map(array, transform) {
	var mapped = [];
	for (var i = 0; i < array.length; i++)
		mapped.push(transform(array[i]));
	return mapped;
}
```
`map` is also a standard method on arrays. Takes one argument.


### `reduce`

Another common pattern of computation on arrays is computing a single value from them. Our recurring example, summing a collection of numbers, is an instance of this.
When summing numbers, you’d start with the number zero and, for each element, combine it with the current sum by adding the two.

The parameters to the `reduce` function are, apart from the array, a combining function and a start value.

```JS
function reduce(array, combine, start) {
  var current = start;
  for (var i = 0; i < array.length; i++)
    current = combine(current, array[i]);
  return current;
}

console.log(reduce([1, 2, 3, 4], function(a, b) {
  return a + b;
}, 0));
// → 10
```

The standard array method reduce, which of course corresponds to this function, has an added convenience. If your array contains at least one element, you are allowed to leave off the start argument. The method will take the first element of the array as its start value and start reducing at the second element.

The `reduce` function takes the following arguments: 

```JS
[0, 1, 2, 3, 4].reduce(function(previousValue, currentValue, currentIndex, array) {
	return previousValue + currentValue;
});
```


Higher order functions really shine when you need to **compose** functions.

```JS
function average(array) {
  function plus(a, b) { return a + b; }
  return array.reduce(plus) / array.length;
}
function age(p) { return p.died - p.born; }
function male(p) { return p.sex == "m"; }
function female(p) { return p.sex == "f"; }

console.log(average(ancestry.filter(male).map(age)));
// → 61.67
console.log(average(ancestry.filter(female).map(age)));
// → 54.56
```

This comes at the cost of performance since looping several times is super costly.

### `bind`

The `bind` method, which all functions have, creates a new function that will call the original function but with some of the arguments already fixed.


## The arguments object

Whenever a function is called, a special variable named `arguments` is added to the environment in which the function body runs. This variable refers to an object that holds all of the arguments passed to the function. **Remember that in JavaScript you are allowed to pass more (or fewer) arguments to a function than the number of parameters the function itself declares.**


### Variable \# of arguments 

Some functions can take any number of arguments, like `console.log`. These typically loop over the values in their arguments object. They can be used to create very pleasant interfaces.



## Objects


### Prototype

Most objects in JavaScript have a _prototype_, an object that acts as a fallback source of properties. When an object is asked for a property that it does not have, it will search its prototype, then the prototype's prototype and so on. The prototype of the empty object (and from which all other obejcts inherit is `Object.prototype`);

```JS
console.log(Object.getPrototypeOf(Object.prototype))
// -> null
```

Functions derive from `Function.prototype` and arrays derive from `Array.prototype`.

You can use `Object.create(prototype)` to create an object with a given prototype;

### Constructors

A more convenient way to create objects that derive from some shared prototype is to use a constructor. 

In JavaScript, calling a function with the `new` keyword in front of it causes it to be treated as a constructor. 
The constructor will have its this variable bound to a fresh object, and unless it explicitly returns another object value, this new object will be returned from the call.


```JS
function Rabbit(type) {
  this.type = type;
}

var killerRabbit = new Rabbit("killer");
var blackRabbit = new Rabbit("black");
console.log(blackRabbit.type);
// → black
```

Constructors (in fact, all functions) automatically get a property named `prototype`, which by default holds a plain, empty object that derives from `Object.prototype`. 

As mentioned before, every instance created with this constructor will have this object as its prototype. So to add a `speak` method to rabbits created with the Rabbit constructor, we can simply do this:


```JS
Rabbit.prototype.speak = function(line) {
    console.log("The " + this.type + " rabbit says '" + line + "'");
};
blackRabbit.speak("Doom...");
// → The black rabbit says 'Doom...'
```

It is important to note the distinction between the way a prototype is associated with a constructor (through its `prototype` property) and the way objects _have_ a prototype (which can be retrieved with `Object.getPrototypeOf`). The actual prototype of a constructor is `Function.prototype` since constructors are functions. Its `prototype` _property_ will be the prototype of instances created through it but is not its _own_ prototype.

```JS
console.log(Object.getPrototypeOf(Rabbit) ==
            Function.prototype);
// → true
console.log(Object.getPrototypeOf(weirdRabbit) ==
            Rabbit.prototype);
// → true
```


### Enumberable vs non-enumerable

Oddly, `toString` does not show up in the `for`/`in` loop, but the `in` operator does return `true` for it. This is because JavaScript distinguishes between **enumerable** and **nonenumerable** properties.

It is possible to define our own nonenumerable properties by using the `Object.defineProperty` function, which allows us to control the type of property we are creating.

```JS
Object.defineProperty(Object.prototype, "hiddenNonsense",
                      {enumerable: false, value: "hi"});
for (var name in map)
  console.log(name);
// → pizza
// → touched tree
console.log(map.hiddenNonsense);
// → hi
```

### `hasOwnProperty`

But we still have the problem with the regular `in` operator claiming that the `Object.prototype` properties exist in our object. For that, we can use the object’s `hasOwnProperty` method.

```JS
console.log(map.hasOwnProperty("toString"));
// → false
```

This method tells us whether the object _itself_ has the property, without looking at its prototypes. This is often a more useful piece of information than what the `in` operator gives us.

### Prototypeless

You can also create prototype-less objects via the `Object.create` function, passing `null` as the first argument.

```JS
var map = Object.create(null);
map["pizza"] = 0.069;
console.log("toString" in map);
// → false
console.log("pizza" in map);
// → true
```