# Flux


**Cascade of updates:** problem that Flux tries to resolve

Store: Fat models. Contains all of the data pertaining to a logical domain. Domain models.

For example, in a chat application, one Store would be the **messages** store, which contains _all of the messages_ that we know about on the client, not just a single one.

It also includes all of the **application logic** for that logical domain.


Setup: register with the dispatcher.

Only accepts actions from the Dispatcher (pub/sub system) and **only has getters**, no setters. 

Stores: a package of data and a caretaker for that data.

**It updates itself. No one else can.**

The caretaker observes a stream of actions coming in from either the user or the server.

And it picks out actions from there that it's interested in. It has the opportunity to change its data as a result of that.

## Actions

Actions are the **only** entry point into the system (via server interaction or view events).

- Initialization code
- Call to API and getting data back
- User interaction
- Router change ?

Actions are just objects with a `type` property and new data. The `type` property is especially important because it is what the application uses to respond to the action.

Action Creators: methods that create actions and hands them to the Dispatcher.

## Data Flow

**UNIDIRECTIONAL**

1. An Action initiates the whole cycle
	- This is something that happens in the outside world (e.g. a user clicked on something, the server responded)
2. The Action gets handed to the Dispatcher
3. The Dispatcher distributes that action to **ALL** the Stores (!important)
4. Stores updates themselves
5. Stores handles the new data to the View
6. View updates itself


## Dispatcher


Flux dispatcher is a **singleton**.

Only handles Actions. Registers callbacks (stores them in an object) and when an Action is fired, it will sequentially call all of the callbacks with the action passed.


## Stores

- Each store is a singleton
- Manages application state for a logical domain
- Registers a callback with the Dispatcher
- Private variables hold application data
- Callbacks is the only way to get data into the store

## Views

- Controller Views are near the root (they usually manage one section of the page, so for example, an <App /> element or <MessageSection /> element)
- They listen for change events
- On change, the controller views query the Stores for new data
- With new data, they re-render themselves and children (start the cycle via `setState`)


## Initialization of the App

- Get initial data with an Action

```JavaScript
function init(initialData) {
	AppDispatcher.dispatch({
		type: AppConstants.ActionTypes.INITIALIZE,
		initialData
	});
	React.render(<AppRoot />, document.getElementById('react'));
}
```

## Getting data from/to the server

Handled entirely in `WebAPIUtils.js` module.

- encapsulates XHR (Ajax) work
- Can start requests directly in the ActionCreators, or in the Stores.
- Important: if you do it in the Store, **create a new action on success/error**.
- Data **must** enter the system through an action


## More FLUX patterns

- LoggingStore (it receives all of the actions! For free!)

## Anti-Patterns

- Getters in `render()`
- Public setters in stores & the setter mindset trap
- Application state/logic **in** React **components** (whenever stuff starts to get shared across components, abstract it out onto a Store)
- Props in `getInitialState()`

