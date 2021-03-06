# Middleware

[Redux Middleware Documentation](https://redux.js.org/docs/advanced/Middleware.html)

If you have ever used any node server side applications you may be familiar with the concept of **middleware** -- which is
essentially code you put between the framework and its execution completion, and is usually chainable -- so you can assign multiple
middlewares that will execute in the order assigned.

**Redux Middleware** <cite>It provides a third-party extension point between dispatching an action, and the moment it reaches the reducer. </cite>

The main usages for middleware are:
1. Logging
2. Asynchronous API calls
3. Crash Reporting


## Logging

The first practical example of a **redux middleware** will be a logger that will log out the actions dispatched and the
state before and after each action. This is greatly useful when debugging your application flow.

An already existing libary for this is [Redux Logger](https://github.com/evgenyrodionov/redux-logger).

It can be installed with yarn or npm:

`yarn add redux-logger`

### Applying Middleware

**Middleware** can be added to the store upon creation using the `applyMiddleware` function from **redux** which takes in 
the middleware as a parameter is and is passed as the second parameter to `createStore`:

```javascript 1.8
import { applyMiddleware, createStore } from 'redux';
import logger from 'redux-logger'

const store = createStore(
  reducer,
  applyMiddleware(logger)
)
```

With this middleware if you monitor the debug console anytime an action occurs you can see the messages show the previous state
action dispatched and next state.
```javascript 1.8
redux-logger.js:1 prev state {user: {…}, profile: {…}}
redux-logger.js:1  action     {type: "ADD_TODO"}
redux-logger.js:1  next state {user: {…}, profile: {…}}
```

## Asynchronous API Calls

In single page applications most of the interactions occur through asynchronous calls, up until now everything in the redux
flow has been synchronous and there has not been any obvious place to put these async calls. There are a few **middleware** solutions
out there that aim to solve this issue.

1. [Redux-Thunk](https://github.com/gaearon/redux-thunk)
2. [Redux-Saga](https://github.com/redux-saga/redux-saga)
3. [Redux-Promise](https://github.com/acdlite/redux-promise)

Each of these libraries are considered **side-effect** libraries in Redux and there is much debate on which should be used and
if **side effects** should be avoided.

#### Redux Thunk

**Redux Thunk** allows you to write action creators that **function** instead of an action object. This allows you to delay
the dispatch of an action until a certain condition is met (example asynchronous call is completed with results).

#### Redux Saga

**Redux Saga** uses an ES6 feature called [Generators](https://davidwalsh.name/es6-generators) which allows the application
flow to be started, paused, and cancelled making the asynchronous calls function more synchronously.

At a very high level `Generators` are functions that can have their executions **paused** and **resumed**.

From MDN :

__A generator is a special type of function that works as a factory for iterators. A function becomes a generator if it contains one or more yield expressions and if it uses the function* syntax.__

Example of generator being defined (noticed the * after the function)

```javascript 1.8
function* generator () {
  yield 'f'
  yield 'o'
  yield 'o'
}
```

An instance of this generator can be created an each time `next` is invoked on the generator the code will execute
until the next `yield` is encountered

```javascript 1.8
gen = generator()
gen.next()   // { value: 'f', done: false }
gen.next()   // { value: 'o', done: false }
gen.next()   // { value: 'o', done: false }
getn.next()  //{ value: undefined, done: true }
```

This alone may not seem very useful but it can be combined with promises and async very powerfully (see a 
[Asyc Generation Exanple](https://davidwalsh.name/async-generators))

#### Redux Promise 

**Redux Promise** allows action creators to return a **promise** - when the promise is resolved the action will be dispatched
if the promise is rejected nothing will be dispatched.


### Quick Summary

Normally the decision comes down to **redux-thunk** vs **redux-saga** (**reduce-promise** has more limited functionality
compared to these two libraries) and there is not always a clear winner between them.

**Thunk** provides some simplicity over **Saga** as it doesn't require the additional understanding of `generators`. With
the addition of `asyn/await` (see [Promises](../../../material/1_es6/5_promises/readme.md)) it is very easy to synchronize
asynchronous code within the **thunk** functions.

**Sagas** do allow for more complex orchestration of the side effects and allow for very easy unit testing compared to **Thunks**.

Both libraries are very powerful and people may have a preference of one over the other.

If you are newer to some `ES6` features or working on a simple application **Redux-Thunk** is probably a good side effect 
middleware to start with.

If you are more familiar with `ES6` features and are working on a more complex application **Redux Saga** may be a good option
due to ease of testability and more advanced orchestration.

## Redux Thunk

`yarn add redux-thunk`

or

`npm install --save redux-thunk`

Next you need to apply the `thunk` middleware in `createStore`

```javascript 1.8
import { createStore, applyMiddleware } from 'redux';
import thunk from 'redux-thunk';
import rootReducer from './reducers/index';

const store = createStore(
  rootReducer,
  applyMiddleware(thunk)
);
```


A [thunk](https://en.wikipedia.org/wiki/Thunk) is a function that returns a function delaying the execution of the logic.


```
function fetchSomething(id) {
  return (dispatch, getState, api) => {
    // you can use api here
    
    return dispatch(/*action*/)
  }
}
```

Example:

```javascript 1.8
function setCats(cats) {
     return {
         type: 'SET_CATS',
         cats
}
    
function loadCats(catType) {
    return async function (dispatch) {
        const cats = await loadCatsofType(type)
        return dispatch(setCats(cats))
    }
}
```

Rewritten with arrow functions:

```javascript 1.8
const loadCats = catType => async dispatch => {
    const cats = await loadCatsofType(type)
    return dispatch(setCats(cats))
}
```


The thunk can be dispatched to the store like any other action:

````javascript 1.8
store.dispatch(loadCats('Siamese'))
````

The returned function takes the first parameter of the `dispatch` action. Once any asynchronous logic is completed
the dispatch can be called with the action to be dispatched to the store. `dispatch` can also be called within a **thunk** more than once (this is one main advantage of **redux-thunk** over **redux-promise**).

A second optional parameter to the function is `getState` which gives the thunk the ability to read the current state of the store.
This parameter should be used sparingly but it has uses such as if you want to check if there is data already cached in the store.

The third optional parameter is an injected `api`, to use this you need to inject extra arguments when registering the 
thunk middleware:

```javascript 1.8
const store = createStore(
  reducer,
  applyMiddleware(thunk.withExtraArgument(api))
)
```

More than one argument can be passed in as an object:

```javascript 1.8
const store = createStore(
  reducer,
  applyMiddleware(thunk.withExtraArgument({ api, more }))
)
```




