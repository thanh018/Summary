I. JAVASCRIPT

2. Load javascript: defer, async, đặt cuối thẻ body khác nhau sao?
  https://stackoverflow.com/questions/10808109/script-tag-async-defer

  2.1. Defect
  - Delaying script execution until the HTML parse has finished
  - The DOM will be available for your script
  - Not every browsers supports defer yet
  - If your second script depends upon the first script (your second script uses the jQuery loading in the first script)
  - Defer script will still be executed in order
  - Until after the document has been parsed

  In `<head>` tag
  - The loading of script will be deferred until DOM has been parsed
  - It wont help you at all in older browsers
  - It isnt realy any faster than just putting the script right before `</body>` tag

  2.2. Async
  - Dont care script will be available
  - HTML parsing may be continued. The script will be execution as soon as its ready
  - Is more useful when you dont care when the script loading
  - You dont depend upon the script loading
  - Its not urgent to run soon and its stands alone so nothing else depends upon it

3. Phân biệt var, let, const
- main difference is scoping rules
- variable declared by
- var keyword are scoped to immediate function body (hence function scope)
- let keyword are scoped to enclosing block denoted by `{}` (hence block scope)
- https://stackoverflow.com/questions/762011/whats-the-difference-between-using-let-and-var
- let can be re-assigned
- const can not be re-assigned (can re-assigned value of each key in obj was declared with const)
```
  function run() {
    var foo = "Foo";
    let bar = "Bar";

    console.log(foo, bar); // Foo Bar

    {
      let baz = "Bazz";
      console.log(baz); // Bazz
    }

    console.log(baz); // ReferenceError
  }

  run();
```
```
  var funcs = [];
  // let's create 3 functions
  for (var i = 0; i < 3; i++) {
    // and store them in funcs
    funcs[i] = function() {
      // each should log its value.
      console.log("My value: " + i);
    };
  }
  for (var j = 0; j < 3; j++) {
    // and now let's run each one to see
    funcs[j]();
  }
```
- `My value: 3` was output to console each time `funcs[j]()` was invoked anonynomus function was bound to the same variable

  3.2 Hoisting
  - variables declared with var are hoisted
  - initialize with undefined before the code run
  - they are acessible in their enclosing scope even before the declared
  ```
    function run() {
      console.log(foo); // undefined
      var foo = "Foo";
      console.log(foo); // Foo
    }

    run();
  ```
  - let variables are not initialized until their definition is evaluated.
  ```
    function checkHoisting() {
      console.log(foo); // ReferenceError
      let foo = "Foo";
    }

    checkHoisting();
  ```

  3.3. global object
  - let does not create a property in global objects

  ```
    var foo = "Foo";  // globally scoped
    let bar = "Bar"; // globally scoped

    console.log(window.foo); // Foo
    console.log(window.bar); // undefined
  ```

  3.4. Redeclaration
  ```
    'use strict';
    var foo = "foo1";
    var foo = "foo2"; // No problem, 'foo' is replaced.

    let bar = "bar1";
    let bar = "bar2"; // SyntaxError: Identifier 'bar' has already been declared
  ```

9. Await vs Promise
- https://stackoverflow.com/questions/34401389/what-is-the-difference-between-javascript-promises-and-async-await

  9.1. async/await
  - give you a synchronous feel to asynchronous code
  - very elegant form of syntactic sugar
  - return value of async functions is a promise
  - gives us the possibility of writing asynchronous in a synchronous manner

  Promise chaining:
  ```
    function logFetch(url) {
      return fetch(url)
        .then(response => response.text())
        .then(text => {
          console.log(text);
        }).catch(err => {
          console.error('fetch failed', err);
        });
    }
  ```
  Async function:

  ```
    async function logFetch(url) {
      try {
        const response = await fetch(url);
        const text = await response.text();
        console.log(text);
      }
      catch (err) {
        console.log('fetch failed', err);
      }
    }
  ```

II. REACT

1. React Element & React Component
  https://stackoverflow.com/questions/30971395/difference-between-react-component-and-react-element/47675471

  1.1. React Element
  - gets returned from Components
  - it's an object
  - that virtually decribes the DOM nodes that Components 
  - With function component, this element is object that function returned
  - with function component, this element is the object that component render function returned
  - React elements are not what we can see in the browsers.
  - They are just objects in memory and we can't change anything about them
  - decribes what we want to see on the screen
  - is a object represention of a DOM nodes
  - is not the actual thing we see on the screen rather the object represention is what is returned

  1.2. React is good with these way:
  - diff an object with previous object representation to see what has changed.
  - can update the actual DOM specifically where the changes it detected occurred
  - createElement invocation returns an object

  1.3. React Components:
  - is a function or class which optionally accepts input and returns a React Component
  - is a template. This can be either a function or a class
  - React sees a function or class as the first argument
  - it will check to see what element it renders
  - give the corresponding props and 
  - will continue to do this until there are no more createElement invocation
  - which have a function or a class as their first argument

  1.4. Class based component
  - class syntax is one of the most common ways to define a React component
  - while more verbose than the function syntax
  - it offers more control in the form of lifecycle hooks
  - have instance
  - is this keyword that is used inside the class-based component
  - creates a class component
  - use it in any other component
  - use props
  - props can be accessed with this.props
  - using state
  - using lifecycle hooks
  - executes after the component is render for the first time (componentDidMount)

  1.5. why do functional components in Reactjs not have instance?
    https://stackoverflow.com/questions/44478809/why-do-functional-component-in-reactjs-not-have-instances
  - you may not use ref attributes on functional components because they dont have instances
  - the only difference between class and functional components is that can have things like
  - constructor and lifecycle management
  - functional components dont have instances
  - because they are just JS function. A function cant have instance
  - whereas, classes have instances (object) of them
  - Default React component extend React component class, so they inherit features
  - like lifecycle hooks and internal state management
  - those features allow the component to keep their state between render and
  - to behave accordingly
  - In that sense, I would call them component which have instance
    https://github.com/facebook/react/issues/4936#issuecomment-142379068
  - Ref dont work on the stateless components

  1.6. Function Component
  - dont have instances
  - can be rendered mutiple times
  - but React does not accociated a local instance each render
  - React uses invocation of the function to determine what DOM element to render for the function

2. algorithms use to compare changes in DOM? reconciliation
  https://stackoverflow.com/questions/34990190/reconciliation-in-react-detailed-explanation
  https://evilmartians.com/chronicles/optimizing-react-virtual-dom-explained

4. HOCs, renderProps?
- https://www.robinwieruch.de/react-higher-order-components

  4.1. HOC and conditional rendering
  - take a component and optional arguments as input (conditonal function) and return enhanced component of input component
  - takes an input and return another function
  - with React context, takes a Component and returns another function (function stateless component or ES6 class component)
  - High-order components are reusable
  - higher-order components take an input component and an optional payload.
  - this optional payloads are often used for configuration
  - the payload function that return true of false to decide the condition rendering
  - you could high-order components but with a function that determines the condition rendering

  4.2. compose is HOC library
  - pass your input component through all High-order component functions

5. How to prevent a component render?
- Pure components defined as function will always re-render
- convert the component to class and prevent the re-render in `shouldComponentUpdate()` returning false
- https://stackoverflow.com/questions/41763031/how-to-prevent-react-from-re-rendering-the-whole-component

  5.1. Pure Component
  - does shallow compare on the component's props and state
  - if nothing changes, it prevents the re-render of the component
  - if something changes, it re-renders
  - if you want to use functional stateless components as Pure Component instead,
  - use recompose's pure high-order-component
  - import `{ pure }` from `recompose` to wrap that component
  - https://www.robinwieruch.de/react-prevent-rerender-component

  5.2. class component with shouldComponentUpdate method
  - has access to the next props and state before running the re-rendering component
  - that's where you can decide to prevent the re-render by return false from this method
  - if you return true, the component re-renders
  - it can be used to prevent the component re-rendering on fine-grained levels

6. Tell some react-hooks you know, how to write an react-hook?
- https://www.digitalocean.com/community/tutorials/react-hooks
- Hooks bring statefulness and lifecycle method, previous only available from class component to function component
- Hooks can only be called from within function components and custom hooks

  6.1. Component state
  - we need to initialize the state using useState

  6.2. Component lifecycle
  - Hooks feature is the addition of useEffect which is a combination of
  - componentDidMount, componentDidUpdate, componentWillUnmount
  - useEffect will fire after initial render and subsequent re-renders

7.	Write a countdown by using react-hook
- https://github.com/do-community/react-hooks-timer/blob/master/src/App.js
- https://www.digitalocean.com/community/tutorials/react-countdown-timer-react-hooks

8. do you need to usecallback
  https://stackoverflow.com/questions/53159301/what-does-usecallback-usememo-do-in-react
  https://stackoverflow.com/questions/54963248/whats-the-difference-between-usecallback-and-usememo-in-practice/54963730
  - useCallback does not memoize the function result
  - Arrow function in render
  - Bind in contructor (es2015)
  https://atomizedobjects.com/blog/react/what-is-the-difference-between-usememo-and-usecallback/

 8.1. useCallback and useMemo use memoization
  - I like to thing of memoization as remembering something
  - both useMemo and useCallback remember something between renders until
  - dependencies changes
  - the difference is just what they remember
  - these two hooks are primarily based arround performance between renders
  - useMemo will remember the rendered value from function
  - useCallback will remember the actual function, the memoized version of the callback, memoized callback
  - useful when passing callbacks to optimized child component that rely on reference equality to prevent unnecessary renders

  8.2. useMemo
  - A component can re-render even if its props don’t change
  - due to a parent component re-rendering causing the child component re-render
  - to void this, we can wrap a child component in React.memo()
  - to ensure it only re-renders if props have changed
  - the memoized version of the component above will compare new props and only
  - re-render if they have changed
  - it's worth noting that comparison is done on a shallow basis.
  - if you need fine-grained control you can supply a custom comparison function as the second argument

11. Middleware
  11.1.
  - providing a third-party extention point
  - between dispatching action and the moment it reaches the reducer

  https://redux.js.org/tutorials/fundamentals/part-6-async-logic
  - The thunk middleware allow us to write function that get dispatch and getState as arguments
  - thunk can have any async logic we want inside
  - with the plain Redux store, you can do simple synchronous update by dispatching an action
  - Middleware extends the store's abilities and lets you write async logic that interact with the store
  - thunk is a Function that can be dispatched to perform async activity and can dispatch and read state
  - A action creator that returns a thunk
  - thunk middleware lets me dispatch thunk async actions as if they were actions
  - this is useful for server side rendering because you can wait data is available then synchronous render the app
  https://github.com/reduxjs/redux-thunk
  https://stackoverflow.com/questions/34570758/why-do-we-need-middleware-for-async-flow-in-redux/34599594#34599594
  - one thing I like about Thunk approach is that the component doesn't care that action creator is async
  - it just call dispatch normally
  - The benefit of using middleware like Redux Thunk is that component aren't aware of
  - how action creators are implemented and whether they care about Redux state
  - whether they are synchronous or asynchronous
  - whether or not they call other action creator
  - Redux thunk and friends is just one possible approach to asynchronous request in Redux app 

  https://stackoverflow.com/questions/34570758/why-do-we-need-middleware-for-async-flow-in-redux/34599594#34599594

  11.2.
  - another intresting approach is Redux Saga which let you take actions as they come, transform, perform
  - request before outputing actions
  - running request in parallel
  - the useage of takeLatest permit to express that you are only interested to get the data of the LAST usernames clicked
  - handle concurrency problems in case the user click very fast on a lot of usernames
  - this is kind of stuck with thunks
  - Thunks are called by the action creator on each new action
  - Actions are continually pushed to thunks and thunks have no control on when to stop those actions
  - In saga, generator pull the next action. they have control when to listen for some action and when to not

  11.3. saga:
  - create the task will perform the asynchronous action
  - launch the above task on each action
  - takeEvery allows mutiple fetchData instances to be started concurrently
  - at a give moment, we can start a new fetchData task while there are still one or move previous fetchData task
  - which have not yet terminated
  - takeLatest allows only one fetchData task to run at any moment
  - it is latest started task
  - if a previous task is still running when another fetchData task is started,
  - the previous task will be automatically cancelled

10. Redux
- Redux is a pattern and library for managing and updating application state, using events called "actions".
- it serves as a centralized store for state need to be used across entire application
- with rule ensure that the state can be updated

- the patterns and tools provided by redux make it easy to understand when, where, why and how the state
- in your application is being updated

  10.1. STORE
  - the center of redux application is the Store
  - A store is a container that holds your application global's state
  - Create a plain action object to decribes something that happens in the application, and then
  - dispatch / execute the action to the store
  - when a action is dispatched, the store runs the root reducer 
  - Lets it calculate the new state based on the old state and the payload of the action
  - the store will notify to the subscribers that the state has been updated so UI can be updated with the new data

  10.2. STATE, ACTION, REDUCER
  - state value describes the application
  - reducer receive two arguments, the current state and an action object describing what happened
  - action object always have a type field

  10.3. UI
  - The user interface will show existing state on screen
  - User does something, the app will update its data
  - and redraw the UI with those values

  10.4. Details:
  - When the store state changes, update UI by reading the latest store state and show new data
  - and subscribers redraw whenever the data changes in the future

  10.5. store
  - create a store instance by Redux library createStore API
  - pass reducer to createStore generate initial state and to calculate future updates
  - A user does something, Redux application need to repond to input, create action object describes what happened
  - and dispatching to the store
  - when we call dispatch store.dispatch(action), the store run reducer, calculate the updated state
  - and run subscribers to update UI

20. Ref
- React's virtual DOM
- There are some cases, you need to interact with the actual elements
- for these occasions, React provides a ref system
- using Refs to get value of an input
- using Refs to focus the input
- To interact with directly DOM element, using React's createRef method allows you just do that
- React provides a way to get references to DOM nodes by using React.createRef()
- it's really equivalent `document.getElementById('foo-id');`
- controlling HTML media elements
- Refs with React Hooks Using useRef
- https://www.digitalocean.com/community/tutorials/react-createref
- https://www.digitalocean.com/community/tutorials/react-refs