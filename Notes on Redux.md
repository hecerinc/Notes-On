# Notes on Redux

See result: http://jsbin.com/keqixet/7/edit?js,output

## Redux principles

- Single source of truth: All of the state of the App is maintained in a single `state` object (AKA state tree)
- The state tree is read-only
- Changes are made with pure functions (reducers)

## Folder structure

A typical folder structure for a Redux application might look like this:

```
/
--src/
----actions/
------index.js
----components/
----reducers/
------index.js
------todos.js
------visibilityFilter.js
----anotherfile.js
index.html
package.json
```

## Actions


Anytime you want to change the state, you need to dispatch an `Action`. 

An `Action` is a plain JavaScript object describing the change.

The structure of this object is completely up to you. The only requirement is that it has a `type` attribute which is not undefined and is preferably a string because it's serializable.


## Reducer function

The View layer works best when it is a pure function of the application state.

A reducer function is a function that takes two parameters: the previous application state, and an action, and returns the new application state.

It's important that this function is pure (it returns a new object), as this makes it deterministic.

If the reducer receives `undefined` as the previous sate, it _must_ return what it considers to be the initial state of the application.


## Stores

The Store binds together the 3 principles of Redux: 

1. It holds the current application state object.
2. It lets you dispatch Actions.
3. It contains the reducer function (the function that mutates states)

The Redux library has a factory for Stores. 

```JS
import {createStore} from 'redux';

const store = createStore(myReducerFunction);
```

It has 3 member functions or methosd:

1. `getState(void)`: It retrives the current state of the app/store.
2. `dispatch(Action a)`: Dispatches actions to change the state of the application
3. `subscribe(Function callback)`: Registers a callback that the Store will call anytime an action has been dispatched (to, for example, update the View)

The Store generator looks like something like this: 

```JS
function createStore(reducer) {
	let state;
	let listeners = []; // keep track of the listeners

	const getState = () => {
		return state;
	};

	const dispatch = (action) => {
		state = reducer(state, action);
		listeners.forEach(listener => listener());
	};

	const subscribe = (listener) => {
		listeners.push(listener);
		// Add some way to unregister the listener
	};

	// Initial state
	dispatch({});

	return {getState, dispatch, subscribe};
}
```

## Example

A simple example:

```JS

// Reducer
const counter = (state, action) => {
	if(typeof state === 'undefined') {
		// return initial state
		return 0;
	}
	switch(action.type) {
		case 'INCREMENT': 
			return state + 1;
		case 'DECREMENT':
			return state - 1;
		default:
			return 0;
	}
};

const {createStore} = Redux;

const counterStore = createStore(counter);

counterStore.subscribe(() => {
	document.body.innerText = counterStore.getState();
});

document.addEventListener('click', () => {
	counterStore.dispatch({type: 'INCREMENT'});
});
```

### Initial state

The `createStore` function also accepts a second parameter to specifiy an initial state other than the one your reducers provide:

```JS
createStore(todoApp, {todos: [{id: 1, text: 'Welcome back!', completed: true}]});
```

## Immutability

If you want to maintain immutability of Objects in the reducer, you can use ES6's `Object.assign`:

```JS
Object.assign({}, oldstate, {property: newvalue});
```


## Reducer Composition

Suppose you have a more complex application like a todo list:


```JS
// Reducer
const todos = (state = [], action) => { // state is represented by a single array of todos
	switch(action.type) {
		case 'ADD_TODO':
			// Add todo to state (which is an array of todos)
			return [...state, {
				id: action.id,
				text: action.text,
				completed: false
			}];
		case 'TOGGLE_TODO': 
			// Toggle `complete` field of todo
			state[action.id].complete = !state[action.id].complete;
			return state;
		default: 
			return state;
	} 
}

```

This is slightly confusing because the reducer deals both with updating the collection (array) of todos, as well as a _single_ todo. 

It's good practice to separate these domains into their own reducer. You can use the **Composition pattern** to do this: 


```JS
// Reducer for a single todo
const todo = (state = {}, action) => {
	switch(action.type) {
		case 'ADD_TODO':
			return {
				id: action.id,
				text: action.text,
				completed: false
			};
		case 'TOGGLE_TODO': 
			if(state.id  !== action.id)
				return state;
			return {
				...state,
				completed: !state.completed
			};
		default:
			return state;
	}
};

// Reducer for the collection of todos (refactored)
const todos = (state = [], action) => {
	switch(action.type) {
		case 'ADD_TODO':
			return [...state, todo(undefined, action)];
		case 'TOGGLE_TODO': 
			return state.map(t => todo(t, action))
		default:
			return state;
	}
};
```

It's a good idea to use this pattern to modify a single property of the state tree with its own reducer.

```JS
	
... // previous reducers 

// Add a visibility filter property to the state
const visibilityFilter = (state = 'SHOW_ALL', action) => {
	switch(action.type) {
		case 'SET_VISIBILITY_FILTER':
			return action.filter;
		default:
			return state;
	}
};

// Main Reducer (use this one to create the store)
const todoApp = (state = {}, action) => {
	return {
		todos: todos(state.todos, action),
		visibilityFilter: visibilityFilter(state.visibilityFilter, action)
	};
}

const todoStore = createStore(todoApp);

```

This allows you to have different reducers controlling different part of the state tree, which facilitates collaboration and easier debugging.

### `combineReducers`

This pattern is **so common** across Redux that the library provides a function called **`combineReducers`**, that accepts one argument:

```JS
const MainReducer = combineReducers({
	// mapping of state field names and the reducers that manage them
});
```

For the above todo example, this would like so:

```JS
const todoApp = combineReducers({
	todos: todos,
	visibilityFilter: visibilityFilter
});

// This can be further simplified in ES6 syntax like so:
// (so long as the reducer has the same name as the state keys it manages)
// (this is a useful convention to follow)
const todoApp = combineReducers({todos, visibilityFilter });

```

## Presentational Components

Components that don't define behaviour at all, only presentation (how they render). (So, for example, click handlers would be passed as props).

To turn a normal component into a Presentational component, pass the behaviour in through props. 

So for example if your TodoListItem looks like this:

```JS
const TodoListItem = ({}) {
	return(
		<li 
			onClick={() => {this.toggleTodo(todo.id)}}
			style={{textDecoration: todo.completed ? 'line-through' : 'none'}}
		>
			{todo.text}
		</li>
	);
};
```

You can see that there is behaviour defined on this component (the `onClick` action). To turn it into a presentational component, pass in the action as a prop:

```JS
const TodoListItem = ({onClick}) { // automatic extraction syntax 
	return(
		<li 
			onClick={onClick}
			style={{textDecoration: todo.completed ? 'line-through' : 'none'}}
		>
			{todo.text}
		</li>
	);
};
```

## Container Components

> Their job is to connect a Presentational Component to the Redux Store and specify the data and the behaviour that it needs


> **Tip #1:** all data mutations (e.g. calls to `store.dispatch`) and readings (e.g. `store.getState`) should go ino these components

> **Tip #2:** no markup should go into these components


Container components are the components that 

- actually pass the data to the presentational components from the Store and 
- specify the behaviour (<small>The definition of `onClick` would go into these components</small>).
- **delegate the rendering to Presentational components**

A Container component will tipically look like this:

```JS
class FilterLink extends React.Component {
	componentDidMount() {
		this.unsubscribe = store.subscribe(() => { // Redux returns the unsubscribe closure for this particular subscription
			this.forceUpdate();
		});
	}
	// Make sure to unsubscribe when we're done
	componentWillUnmount() {
		this.unsubscribe();
	}
	render() {
		// There is an issue with this implementation: 
		// This FilterLink component reads from the state, but it is not _subscribed_ to its changes
		// This means that if the state changes, but the parent component does not rerender, 
		// this FilterLink will show a stale value.
		// Solution: move subscription to the store to the life cycle methods of the container component (Filterlink)
		// (see componentDidMount)

		const state = store.getState();
		const props = this.props;

		// <Link /> is the presentational component here

		return (
			<Link 
				active={this.props.filter === state.visibilityFilter}
				onClick={() => {
					store.dispatch({
						type: 'SET_VISIBILITY_FILTER',
						filter: this.props.filter
					});
				}}
			>
				{this.props.children}
			</Link>  
		);
	}

}
```

### Multiple containers

If you go try to have a single Container Component for your whole application, you'll soon see that you have to pass state down your main container to all of your children components. (This same problem exists in barebones React). 

As a result, lots of intermediate components will receive props that they don't use, but mereley pass further down the tree.

To avoid this, it's recommended to have __multiple__ Container components so that the state passing is essentially short-circuited.

> "Separating the container and the presentational components, is often a good idea. But you shouldn't take it as a dogma. Only do this when it truly reduces the complexity of your codebase. In general, I suggest first trying to extract the presentational components, and if there is too much boilerplate passed in the props to them, then you can create the containers around them that load the data and specify the behaviour." - Dan Abramov

## Passing the Store down

Currently, the `store` is a global variable. This approach works if you have your whole app in a single file, but not for large applications. 

- It makes container components harder to test, because they reference a specific store, but you might want to supply a different store for testing
- It makes it hard to develop "universal" apps that are rendered on the server

There are two ways to solve this:

1. Pass the `store` as a prop to the top-level component, and then pass it down to every container component as a prop. 
	This approach is super inconvenient because the store gets passed down via props to **every single component**, and that's a lot of boilerplate.
2. Via React's `Context`
	Redux uses React's `Context` to store the `store` into the context and make it available to any component inside it. This is essentially equivalent to having a global variable but constraining it to a single namespace (the `Provider`, where every component (including the top-level `App` component) is a child of this `Provider` component).

The `Provider` is so called because it **provides** the Context to the rest of the application.

A small note on the gains of Context vs globals:

> Yeah they are like global variables. The gains are not marginal when an application scales however. Consider components that are very deep in the tree. What happens when you want to move a component to another part of the tree? Without this Provider/Connect pattern it would require a lot of refactoring... This pattern makes things less brittle in that regard.

https://github.com/reactjs/redux/issues/776


## Using Contexts

- You have **one** `Provider` component from which all other components inherit (this will be your root component that React will render)
- This `<Provider></Provider>` component defines a method `getChildContext` which returns an object that is the definition of the context.
- The `Provider` **must also** deifne a **`childContextTypes`** property, which is an object that defines the Proptypes for the context variables.
- The Context is opt-in for the descendant ocmponents, so for each component that will use the Context you **must define** a **`contextTypes`** property that defines which PropTypes for the properties of the Context that the component wants to receive.
- Once the proptypes have been defined, you can access the context variables in a property `this.context` inside the components.

### `<Provider>`

The `Provider` component looks like this:

```JS
class Provider extends React.Component {
	// This is necessary to define the context
	getChildContext() {
		return {
			store: this.props.store
		};
	}

	render() {
		// Render everything that's passed to it as-is
		return this.props.children;
	}
}
```

And the React `render` method now looks like this:

```JS
ReactDOM.render(
	<Provider store={createStore(todoApp)}>
		<TodoApp />
	</Provider>,
	document.getElementById('root')
);
```

The Provider will **not work** if `childContextTypes` is not defined, so we define it after the class:

```JS
Provider.childContextTypes = {
	store: React.PropTypes.object
};
```

Each of the Container components that want to receive the Context will have to define `contextTypes`:

```JS
FilterLink.contextTypes = {
	store: React.PropTypes.object
};

// And to use them:

class FilterLink extends React.Component {
	componentDidMount() {
		const { store } = this.context;
		// this is equivalent: const store = this.context.store;
	}
	render() {
		const { store } = this.context;
		// return ... some code here ..
	}
}
```

#### `react-redux`

Finally, the `Provider` will really not change at all regardless of the application, so there is a package [`react-redux`](https://cdnjs.cloudflare.com/ajax/libs/react-redux/5.0.6/react-redux.min.js) that provides the Component already for you. You can replace the above implementation exactly by this package like so:

```JS
const { Provider } = ReactRedux;
// or, equivalently, 
// import { Provider } from 'react-redux'
```

without any further change.



## Containers: a repeating pattern

You will soon notice all container components are very similar in structure and functionality, as they share some basic functions:

- They need to rerender when the store state changes
- They need to unsubscribe from the store when they unmount
- They take the current state of the Redux store and use it to render the Presentational components with some props that they calculate from the state of the store
- They need to specify `contextTypes` to get the store from the context

For this reason, we can abstract them into a factory to avoid repeating code.

Consider the `VisibleTodoList` container component (and its corresponding presentational component `TodoList`):

```JS
class VisibileTodoList extends React.Component {
	componentDidMount() {
		const {store} = this.context;
		this.unsubscribe = store.subscribe(() => {
			this.forceUpdate();
		});
	}
	componentWillUnmount() {
		this.unsubscribe();
	}

	render() {
		const props = this.props;
		const {store} = this.context;
		const state = store.getState();
		return (
			<TodoList 
				todos={getVisibleTodos(state.todos, state.visibilityFilter)} 
				onTodoClick={id => 
					store.dispatch({
						type: 'TOGGLE_TODO',
						id
					})
				}
			/>
		);
	}
}
VisibileTodoList.contextTypes = {
	store: React.PropTypes.object
};


const TodoList = ({todos}) => {
	return(
		<ul>
			{todos.map(todo => 
				<TodoListItem key={todo.id}  {...todo} toggleTodo={this.props.onTodoClick} />
			)}
		</ul>
	);
};
```
### `mapStateToProps`

Let's define a function `mapStateToProps`. 

This function will take the `state` and return a map of the properties in the component that depend on the store state. 

In the case of `VisibleTodoList`, the only prop that gets passed down to the presentational component that depends upon the state is `todos`:

```JS
const mapStateToProps = (state, props) => {
	return {
		todos: getVisibleTodos(state.todos, state.visibilityFilter)
	};
};
```

Notice `mapStateToProps` also receives the Container props as a second argument.

### `mapDispatchToProps`

Now let's define a function `mapDispatchToProps` that will take the `dispatch` function of the store and return the props that should be passed to the presentational component which depend on the `dispatch` method.

In the case of `VisibleTodoList`, the only prop that gets passed down to the presentational component that depends on the `dispatch` method is `onTodoClick`:

```JS
const mapDispatchToProps = (dispatch, props) => {
	return {
		onTodoClick: (id) => {
			dispatch({
				type: 'TOGGLE_TODO',
				id
			});
		}
	};
};
```

Notice `mapDispatchToProps` also receives the Container props as a second argument.

### `connect`

These two functions, along with the Presentational component (`TodoList`), are sufficient to describe the Container component in its entirety. Every other Container component will follow this pattern, so `react-redux` defines a function **`connect`** that will take these three paramaters and generate the Container component for you:

```JS
const { connect } = ReactRedux;

// This is the whole container component:
const VisibileTodoList = connect(mapStateToProps, mapDispatchToProps)(TodoList);

// Note that this is a curried function so you have to pass the presentational component as an argument to the resulting call to connect.
```

**Note:** some containers will only need the dispatch method, but have no properties to pass down the tree. Because this injecton of `dispatch` is so common, if any falsy value (preferably `null`) is passed to `connect` for the `mapDispatchToProps`, then it is equivalent to passing a function like so:

```JS
connect(null, dispatch => {
	return { dispatch };
})(Component);

// === to:
connect(null, null)(Component);
```

Not that the first `null` for `mapStateToProps` will **avoid subscribing to `state`** if it's not used in the component.


Finally, because `connect(null, null)(Component)` is also a common pattern and it's a lot of code, the preferred format of this call is:

```JS
connect()(Component);
```

## Action Creators

Redux also likes to extract the action objects (`{type: 'TOGGLE_TODO', ...}`) into functions that receive the properties of the action and return the actual Action.

So for the AddTodo component we define a function `addTodo` which will return the `ADD_TODO` action:

```JS
let nextTodoId = 0;
const addTodo = (text) => {
	return {
		type: 'ADD_TODO',
		id: nextTodoId++,
		text
	};
};
```

And use that in the component:


```JS
class AddTodo extends React.Component {

	// .. more code here ..

	render() {
		{/* .. more code here .. */}
		<button onClick={() => {
			dispatch(addTodo(input.value));
		}}>Add todo</button>
		{/* .. more code here .. */}
	}	
}
```

It is common for ActionCreators to be placed **separately** from Components and Reducers. 

> "You might think that this kind of code is boilerplate and you'd rather dispatch the action inline inside the component. However, don't understimate how ActionCreators document your software, because they tell your team what kind of Actions your components can dispatch and this kind of information can be invaluable in large applications." - Dan Abramov

Of course, this only works if all your ActionCreators are in the same place.

