# React.js 


## JSX

You can interpolate variables in your HTML like so:

```JSX
render: function(){
	var name = "hector";
	return(
		<form className="store-selector">
			<h2>Some title</h2>
			<p>Hello {name}</p>
		</form>
	)
}
```

using the `{}` brackets.


#### `className`

**`class` is a reserved word in React** so you can't use it to apply a class to an HTML element. Use `className` instead.


#### Comments

If you try to comment like regular JavaScript when you're writing JSX, the interpreter will actually write the comments to the HTML.

```JSX
render: function(){
	var name = "hector";
	// Comments here no prob'
	return (
		<form className="store-selector">
			/* This won't work, it'll be printed */
			<h2>Some title</h2>
			<p>Hello {name}</p>
		</form>
	)
}
```

So you actually have to enclose it in brackets as well:

```JSX
render: function(){
	var name = "hector";
	// A normal JS comment
	return(
		<form className="store-selector">
			{/* This is a JSX comment */}
			<h2>Some title</h2>
			<p>Hello {name}</p>
		</form>
	)
}
```

**Only what's between the parentheses after `return` is actually JSX**, the rest is normal JavaScript.

### Props

You can give your components properties, which work exactly the same as HTML tag properties. You don't have to define them first to use them. Just add it to your component.

```JSX
var App = React.createClass({
	render: function(){
		return (
			<div className="catch-of-the-day">
				<div className="menu">
					<Header tagline="Fresh Seafood" />
				</div>
				<Order />
				<Inventory />
			</div>
		)
	}
});
```

Where `tagline` is a prop(erty) of the Header component (which is defined elsewhere)

You can access in JSX the props of the component via the `props` attribute of the `this` object and interpolating just like any variable

```
var Header = React.createClass({
	render: function(){
		return (
			<header className="top">
				<h1>Catch of the Day</h1>
				<h3 className="tagline">{this.props.tagline}</h3>
			</header>
		)
	}
});
```

### Getting values from inputs

Your `form` **must** have an `onSubmit` property and you should handle it in a function defined within the component.

```JSX
var StorePicker = React.createClass({
	goToStore: function(event){
		event.preventDefault();
		// get the data from the input
		// transition from <StorePicker /> to <App />
		console.log('Ya submitted it!');
	},

	render: function(){
		return (
			<form className="store-selector" onSubmit={this.goToStore}> {/* Whats within brackets is plain JS */}
				<h2>Please Enter A Store {name}</h2>
				<input type="text" ref="storeId" required defaultValue={h.getFunName()}/>
				<input type="submit" value="Submit" />
			</form>			
		)
	}
});
```

Notice how the `input` has a `ref` attribute. This attribute will be the one used to retrieve its value like so:

```
// ...
// get the data from the input
var submittedVal = this.refs.storeId.value; // this refers to the component
// ...
```

## State

The state of your application is a model of the current data that is held by your application. If before you used jQuery, you used to save the data in the actual DOM (via a series of `input` elements and so). But then when you wanted to update say, a price for a product, you would have to update each section where the price is used or referenced manually via a series of manipulations of the DOM.

React's main feature is precisely the abstraction of this process, in that it will automatically update the View in all the places where a property is referenced so long as you update and maintain a State.


### Implementing state 

The implementation of the State is really just an empty object composed of objects (your models) where you will be storing the data for your application.

Your main `<App />` is in charge of handling the state. Everything state related goes here. So think of it sort of like a StateController, if that makes any sense to you.

#### `getInitialState`

Your `<App />` component needs a `getInitialState` method that React will call **before** it renders the component. This is part of the React Life Cycle. 

This function will really only initalize the state of the application. It is sort of like a constructor.

```JSX
var App = React.createClass({
	getInitialState: function(){
		return {
			fishes: {},
			order: {}
		}
	},
	render: function(){
		// some code here 
	}
})
```

After this function, you can create as many handlers (methods) to mutate (change) your state. 

```JSX
var App = React.createClass({
	// ...
	addFish: function(fish){
		var timestamp = (new Date()).getTime();
		// Update the state object
		this.state.fishes['fish-' + timestamp] = fish;

		// Render the state to the view
		this.setState({fishes: this.state.fishes});
	},
	// ...
})
```

#### `setState`

When you're done changing your state object, you want to render those changes to the view. React makes use of the Virtual (Shadow) DOM to diff your changes with the current state and *only* make those changes. This optimizes and speeds up your code like you wouldn't imagine.

To render the changes call `setState` like in the function above. It receives an argument that tells it exactly what you changed. You _could_technically pass the whole `this.state` object but then it's going to have to diff absolutely everything and it'll be super inefficent and slow.


## Proptypes

Proptypes is React's method of validating that you pass it all the settings it needs to run. (CSS transition timeout, CSS animation name, a certain handler function, etc.). This is useful if you are going to share your component to another developer who does not necessarily know your code.

For each prop that your component accepts, define which ones are required and of what type they are

```JSX
var Header = React.createClass({
	render: function(){
		return (
			<header className="top">
				<h1>Catch 
				<span className="ofThe">
					<span className="of">of</span>
					<span className="the">the</span>
				</span>
				Day</h1>
				<h3 className="tagline"><span>{this.props.tagline}</span></h3>
			</header>
		)
	},
	propTypes: {
		tagline: React.PropTypes.string.isRequired
	}
});
```

Just add the proptypes object as shown above

# ES6

When working with ES6, we can use actual JavaScript classes to create our components and chunk each one into their own files:


```JS
// App.js

class App extends React.Component{
	method(param){
		// some code here
	}
	render(){
		// some JSX code here
	}
}
// If you need to export it:
export default App

// Or right from the declaration

export default class App extends React.Component{
	// ...
}
```

However, there are a few gotchas to doing this:


### State intialization

This is no longer done via the `getInitialState` method as in ES5, ES6 recommends doing this in the constructor:

```JSX
class App extends React.Component{
	constructor(){
		super()
		this.state = {
			fishes: {},
			order: {}
		}
	}
}
```

Notice the call to `super()` which is very important. Always put that in the constructor. It is the call to the constructor of the `React.Component` class.


#### ES7

ES7 (experimental) goes a bit further and allows you to use its own propery intializers:

```JSX
export default class App extends React.Component {
    // .. some code here
    state = {
        qty: this.props.initialQty,
        total: 0
    };
    constructor(){
    	// ... some code here
	}
	// ...
}
```

This declaration of state must go **before** the constructor.


### No autobinding

Reasons for this are outlined [in this blog post](http://facebook.github.io/react/blog/2015/01/27/react-v0.13.0-beta-1.html#autobinding)

When using `React.createClass...`, React can automatically bind the keyword `this` to the current class. However, when using JS classes this is not done automatically so on each call to `this.someFunction` in your component you would have to manually bind the call to the context (via the `this` word):

```JSX
export default class StorePicker extends React.Component{

	goToStore(event) {
		event.preventDefault();
		// get the data from the input
		var storeId = this.refs.storeId.value;
		// transition from <StorePicker /> to <App />
		var path = `/store/${storeId}`;
		browserHistory.push(path);
	}

	render() {
		return (
			<form className="store-selector" onSubmit={this.goToStore.bind(this)}> /* ADDED THIS BIT */
				<h2>Please Enter A Store</h2>
				<input type="text" ref="storeId" required defaultValue={h.getFunName()}/>
				<input type="submit" value="Submit" />
			</form>			
		)
	}
}
```

`this.function.bind(this)` would do it for you.

However, you'd need to do this for each and every reference to the context (the current class). 

You could technically do this in the constructor:

```JSX
// class signature{
	constructor(){
		super(props)
		this.goToStore = this.goToStore.bind(this)
	}
}
```

And avoid having to add `.bind(this)` everywhere in your code, but this would increase your constructor code.

Another way to do this is via ES6 fat arrow constructors:

```JS
// class signature ...
	constructor(){
		super(props)
		this.goToStore = () => this.goToStore();
	}
// some more code...
```

All of these are viable options, but you'd need to do this for each method in your class. Since that's messy, someone created a cool package called [autobind-decorator](https://github.com/andreypopp/autobind-decorator) which makes use of ES7 (still in stage 0) decorator tags (like those in Java) to do this process automatically for you.

`npm install autobind-decorator --save-dev`

And add `@autobind` to the method that needs auto-binding:

 ```JSX
export default class StorePicker extends React.Component{
	@autobind
	goToStore(event) {
		event.preventDefault();
		// get the data from the input
		var storeId = this.refs.storeId.value;
		// transition from <StorePicker /> to <App />
		var path = `/store/${storeId}`;
		browserHistory.push(path);
	}

	render() {
		return (
			<form className="store-selector" onSubmit={this.goToStore.bind(this)}> /* ADDED THIS BIT */
				<h2>Please Enter A Store</h2>
				<input type="text" ref="storeId" required defaultValue={h.getFunName()}/>
				<input type="submit" value="Submit" />
			</form>			
		)
	}
}
```

Autobind can also do this automatically for all the class if you put it before the class declaration:

```JS
@autobind
export default class StorePicker extends React.Component{
	// ...
```

**Note:** Since decorators are ES7 features, Babel (if you're using that) does not currently support them by default. You have to enable these "experimental features" via passing `{stage: 0}` to the configuration on the babel(ify) call. See [https://babeljs.io/docs/plugins/](https://babeljs.io/docs/plugins/)

### No mixins

You cannot use mixins with ES6 classes, but there's another package for that! 

The appropriately called [react-mixin](https://github.com/brigand/react-mixin) allows you to use Mixins with ES6 classes.

**1. Remove the mixin from the class**

**2. Import the mixin library:**
```
import reactMixin from 'react-mixin'
```
**3. Add the following line _after_ the closing bracket the class:**

```
reactMixin.onClass(className, NameOfTheMixin)
```

That's it!


### Proptypes

Proptypes won't work *inside* of ES6 classes either, so just pull them out:

```JSX
// Before:

var Order = React.createClass({
	// methods here
	propTypes: {
		item1: React.Proptypes.object.isRequired,
		item2: React.Proptypes.object.isRequired
	}
});

// After:

@autobind
export default class Order extends React.Component {
	// ...
}

Order.propTypes =  {
	fishes: React.PropTypes.object.isRequired,
	order: React.PropTypes.object.isRequired,
	removeFromOrder: React.PropTypes.func.isRequired
}

```

However, the ES7 experimental features of Babel allow you to use ES7 or this purpose. This way, you can keep the propTypes object *inside* the class if you add the `static` keyword and put them **before** the constructor:

```JSX
class Order extends React.Component{
	static propTypes = {
		item1: React.Proptypes.object.isRequired,
		item2: React.Proptypes.object.isRequired	
	};
	constructor(){
		// ...
	}
	// ...
}
```

