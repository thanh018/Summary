JAVASCRIPT

1. Load javascript: defer, async, đặt cuối thẻ body khác nhau sao?
  https://stackoverflow.com/questions/10808109/script-tag-async-defer

  Defect
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

  Async
  - Dont care script will be available
  - HTML parsing may be continued. The script will be execution as soon as its ready
  - Is more useful when you dont care when the script loading
  - You dont depend upon the script loading
  - Its not urgent to run soon and its stands alone so nothing else depends upon it

REACT

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

8. do you need to usecallback
  https://stackoverflow.com/questions/53159301/what-does-usecallback-usememo-do-in-react
  https://stackoverflow.com/questions/54963248/whats-the-difference-between-usecallback-and-usememo-in-practice/54963730
  - useCallback does not memoize the function result
  - Arrow function in render
  - Bind in contructor (es2015)
  https://atomizedobjects.com/blog/react/what-is-the-difference-between-usememo-and-usecallback/

* useCallback and useMemo use memoization
  - I like to thing of memoization as remembering something
  - both useMemo and useCallback remember something between renders until
  - dependencies changes
  - the difference is just what they remember
  - these two hooks are primarily based arround performance between renders

* useMemo will remember the rendered value from function
* useCallback will remember the actual function, the memoized version of the callback, memoized callback
  - useful when passing callbacks to optimized child component that rely on reference equality to prevent unnecessary renders

* useMemo
 - A component can re-render even if its props don’t change
 - due to a parent component re-rendering causing the child component re-render
 - to void this, we can wrap a child component in React.memo()
 - to ensure it only re-renders if props have changed
 - the memoized version of the component above will compare new props and only
 - re-render if they have changed
 - it's worth noting that comparison is done on a shallow basis.
 - if you need fine-grained control you can supply a custom comparison function as the second argument

11. Middleware
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
 * another intresting approach is Redux Saga which let you take actions as they come, transform, perform
    - request before outputing actions
    - running request in parallel
    - the useage of takeLatest permit to express that you are only interested to get the data of the LAST usernames clicked
    - handle concurrency problems in case the user click very fast on a lot of usernames
    - this is kind of stuck with thunks

    - Thunks are called by the action creator on each new action
    - Actions are continually pushed to thunks and thunks have no control on when to stop those actions
    - In saga, generator pull the next action. they have control when to listen for some action and when to not

* saga:
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

  STORE
    - the center of redux application is the Store
    - A store is a container that holds your application global's state
    - Create a plain action object to decribes something that happens in the application, and then
    - dispatch / execute the action to the store
    - when a action is dispatched, the store runs the root reducer 
    - Lets it calculate the new state based on the old state and the payload of the action
    - the store will notify to the subscribers that the state has been updated so UI can be updated with the new data

  STATE, ACTION, REDUCER
    - state value describes the application
    - reducer receive two arguments, the current state and an action object describing what happened
    - action object always have a type field

  UI
    - The user interface will show existing state on screen
    - User does something, the app will update its data
    - and redraw the UI with those values

  Details:
    - When the store state changes, update UI by reading the latest store state and show new data
    - and subscribers redraw whenever the data changes in the future

  store
    - create a store instance by Redux library createStore API
    - pass reducer to createStore generate initial state and to calculate future updates
    - A user does something, Redux application need to repond to input, create action object describes what happened
    - and dispatching to the store
    - when we call dispatch store.dispatch(action), the store run reducer, calculate the updated state
    - and run subscribers to update UI