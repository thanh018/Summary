## I. REACT
1. Why should we update the state directly?
    - If you try to update state directly then it `won't re-render` the component
        ```
        this.state.visible = true; // Wrong
        ```
    - Instead use `useState()` method. It schedules an update to a component's state object. When state changes, the component responds `by re-rendering`
        ```
        this.setState({ visible: true });
        ```
2. What is React Element?
    - A React Element is what gets returned from components. It’s an object that virtually describes the DOM nodes that a component represents.
    - With a function component, this element is the object that the function returns.
    - With a class component, the element is the object that the component’s render function returns
    - React elements are not what we see in the browser. They are just objects in memory and we can’t change anything about them.
    - React elements can have other type properties other than native HTML elements.
    We know the following:
    - A react element describes what we want to see on the screen.
    A more complex way of saying that is:
    - A React element is an object representation of a DOM node.
      [Read more](https://stackoverflow.com/questions/30971395/difference-between-react-component-and-react-element/47675471)

 3. React is good with these way:
     - React can create and destroy these element without much overhead. The JS objects are lightweight and low-cost.
    - React can diff an object with the previous object representation to see what has changed.
    - React can update the actual DOM specifically where the changes it detected occurred. This has some performance upsides.

4. React Components:
      - A component is a function or a Class which optionally accepts input and returns a React element.
      - A React Component is a template. A blueprint. A global definition. This can be either a function or a class (with a render function).
      - If react sees a class or a function as the first argument, it will check to see what element it renders, given the corresponding props and will continue to do this until there are no more createElement invocations which have a class or a function as their first argument.
      - When React sees an element with a function or class type, it will consult with that component to know which element it should return, given the corresponding props.
      - At the end of this processes, React will have a full object representation of the DOM tree. This whole process is called reconciliation in React and is triggered each time setState or ReactDOM.render is called.

 5. Class based component
      - Class syntax is one of the most common ways to define a React component. While more verbose than the functional syntax, it offers more control in the form of lifecycle hooks.
      - We can render many instances of the same component.
      -The instance is the "this" keyword that is used inside the class-based component.
      - Is not created manually and is somewhere inside React's memory.
      - Executes after the component is render for the first time (componentDidMount)

 6. Why do functional components in Reactjs not have instance?
      - You may not use ref attributes on functional components because they dont have instances
      - The only difference between class and functional components is that can have things like constructor and lifecycle management
      - Functional components dont have instances because they are just JS function. A function cant have instance whereas, classes have instances (object) of them. Default React component extend React component class, so they inherit features like lifecycle hooks and internal state management. Those features allow the component to keep their state between render and to behave accordingly. In that sense, I would call them component which have instance
      - Ref don't work on the stateless components
        [Read more 1](https://github.com/facebook/react/issues/4936#issuecomment-142379068)
        [Read more 2]( https://stackoverflow.com/questions/44478809/why-do-functional-component-in-reactjs-not-have-instances)

7. Function Component
    - Do not have instances.
    - Can be rendered multiple times but React does not associate a local instance with each render.
    - React uses the invocation of the function to determine what DOM element to render for the function.

8. Algorithms use to compare changes in DOM? Reconciliation
    - When a component's props or state change, React decides whether an `actual DOM update` is necessary by comparing the newly returned element with the previously renderd one
    - When they are `not equal`, React will update the DOM. This process is called `reconciliation`
      [Read more](https://stackoverflow.com/questions/34990190/reconciliation-in-react-detailed-explanation)
      [Read more](https://evilmartians.com/chronicles/optimizing-react-virtual-dom-explained)

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
  - Redux middleware function provides a medium to interact with dispatched action before they reach the reducer
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
20.1.
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

20.2. useRef
- https://stackoverflow.com/questions/53351517/react-hooks-skip-first-run-in-useeffect/53351556#53351556
- The useRef hook can be used to store any mutable value, so you could store a boolean indicating if it's the first time the effect is being run.
```
  const isFirstRun = useRef(true);
  useEffect (() => {
    if (isFirstRun.current) {
      isFirstRun.current = false;
      return;
    }

    console.log("Effect was run");
  });
```