## I. JAVASCRIPT
## 1. Load Javascript with `async` and  `defer` in <script>
- With `async`, the file gets downloaded asynchronously and then executed `as soon as it’s downloaded`
- With `defer`, the file gets downloaded asynchronously, but executed only when the `document parsing is completed`

## 2. Phân biệt var, let, const
- Main difference is scoping rules
- Variable declared by `var` keyword are scoped to immediate function body (hence `function scope`)
- Variable declared by `let` keyword are scoped to `enclosing block` denoted by `{}` (hence `block scope`)
- `let` can be re-assigned
- `const` can not be re-assigned

## 3. Hoisting
  - Variables declared with `var` are hoisted
  - Initialize with `undefined` before the code run
  - They are accessible in their enclosing scope even before the declared
  ```
    function run() {
      console.log(VAR); // undefined
      var VAR = "VAR";
      console.log(VAR); // VAR
    }
  ```
  - `let` variables are not initialized until their definition is evaluated.
  ```
    function checkHoisting() {
      console.log(LET); // ReferenceError
      let LET = "LET";
    }
  ```
## 4. Implement a map function
```
  Array.prototype.mymap = function(callback) {
    const resultArray = [];
    for (let index = 0; index < this.length; index++) {
      resultArray.push(callback(this[index], index, this));
    }
    return resultArray;
  }
  [1, 2, 3].myMap(function(item) { return item + 1});
```
## 5. Closure
- A persistent scope that holds on to local variables
- Languages which support closure will allow you to keep a reference to a scope
```
  var log = function() { console.log(1) };
  var outer = function(f) {
    var isCalled = false;
    return function() {
        if (!isCalled) {
            isCalled = true;
            return f();
        }
    }
  }
  var result = outer(log);
  result(); // 1
  result(); // undefined
```
- `isCalled` persist because the function `result` persist as long as the function continue exist
- `result` variable point `f()`
- `isCalled` persist as long as `result` persist. `isCalled` is within a closure.

## 6. Promise
- An object that can be returned a synchronous from asynchronous function
- Async Function standard used to make a asynchronous code look synchronous
- Will be in one of 3 possiable state:
    - fulfilled: `resolve()` was called;
    - rejected: `reject()` was called: the reason for rejection
    - pending: no yet 2 states above but transition into a fulfilled or rejected

## 7. Clone a nested object
```
function deep(value) {
 if (typeof value !== 'object' || value === null) {
   return value
 }
 if (Array.isArray(value)) {
   return deepArray(value)
 }
 return deepObject(value)
}
```
```
function deepObject(ob) {
 const result = {}
 Object.keys(ob).forEach((key) => {
   const value = ob[key]
   result[key] = deep(value)
 }, {})
 return result;
}
```
```
function deepArray(arr) {
 return arr.map((value) => {
   return deep(value)
 })
}
```
```
const clone = JSON.parse(JSON.stringify(object))
```
```
Object.assign(objectA, objectB)
```

## 8. Compare 2 object

```
  function isObject(ob) {
    return ob !== null && typeof ob === 'object';
  }

  function deepEqual(a, b) {
    const keysA = Object.keys(a);
    const keysB = Object.keys(b);
    if (keysA.length !== keysB.length) return false;
    
    for (let key of keysA) {
        const val1 = a[key];
        const val2 = b[key];
        const areObjects = isObject(val1) && isObject(val2);
        // a recursive call starts to verify whether the nested objects are equal too.
        if (areObjects && !deepEqual(val1, val2) || !areObjects && val1 !== val2) return false;
    }
    return true;
  }
```
## 9. Pass by reference object
- Js has 5 data types that are passed by value: Boolean, null, undefined, String, Number (primitive types)
- JS has 3 data types that are passed by reference: Array, Function, Object
- Objects are created at some location in your computer's memory
- An address points to the location, in memory, of a value that is passed by reference
- A variable holding an object does not directly hold an object.
- What it holds is `reference to an object`
- when you assign that reference from one to another, you make a copy of the reference
- Now both variables hold a reference to an object 
- Modify an object through that reference, changes it for both variables, holding a reference to that object
- When you assign a new value to one of the variables, you modify the value that variable holds
- The variable now stop holding a reference to objects and instead of holding something else
- The other variable is still holding its reference original object, the assignment didn't influence it at all.
```
  var objOne = {
    x: 1,
    y: 2
  };

  // objOne -> { x: 1, y: 2 }

  var objTwo = objOne; // when you assign that reference from one to another, you make a copy of the reference

  objTwo.x = 2; // Modify an object through that reference, changes it for both variables, holding a reference to that object

  // objOne -> { x: 2, y: 2 }

  objTwo = {}; // When you assign a new value to one of the variables, you modify the value that variable holds

  // objOne -> { x: 2, y: 2 }, objTwo -> {}
```

## 10. Remove the same item in array

```
  let users = [1, 1, 2, 2, 3, 3, 5, 8, 10, 23, 15];
  // users = new Set(users);

  function isExistsArray(arr, x) {
      for(let item of arr) {
          if(item === x) return true;
      }
      return false;
  }

  // [1, 2, 3, 5, 8, 10, 23, 15]
  users.reduce((arr, item) => {
    if (!isExistsArray(arr, item)) {
        arr.push(item);
    }
    return arr;
  }, []);

  // [1, 2, 3, 5, 8, 10, 23, 15]
  for(let i = 0; i < users.length; i++){
    for(let j = i+1; j < users.length; j++){
        if (users[i] === users[j]) {
            users.splice(j, 1);
        }
    }
  }
```

# 11. Implement function
  case 1: add(1,2); // 3
  case 2: add(1,2,3); // 6
  ```
    function add3(...x) {
      console.log(x)
      return x.reduce((count, item) => count + item, 0);
    }
  ```

## 12. bind(this) explain?

## 13. Count the same element in array
  ```
    let input = ['a', 'a', 'b', 'c', 'b', 'c', 'd']; // output: {a: 2, b: 2, c: 2, d: 1}

    input.reduce((ob, chars) => {
        if (!ob[chars]) ob[chars] = 1;
        else ob[chars]++
        return ob;
    }, {})
  ```

## 14. Copy shadow obj and deep obj

## 15. Server side rendering?

## 16. Generator function?

## II. REACT
## 1. Why should we update the state directly?
  - If you try to update state directly then it `won't re-render` the component
      ```
      this.state.visible = true; // Wrong
      ```
  - Instead use `useState()` method. It schedules an update to a component's state object. When state changes, the component responds `by re-rendering`
      ```
      this.setState({ visible: true });
      ```

# 2. React Element & React Component
React Element
  - Gets returned from Components
  - It's an object that virtually decribes the DOM nodes that Components

React Components:
  - Is a function or class which optionally accepts input and returns a React Element
  - React sees a function or class as the first argument
  - It will check to see what element it renders

Class based component
  - Have instance
  - Using lifecycle
  - Executes after the component is render for the first time (componentDidMount)

Why do functional components in Reactjs not have instance?
  - The only difference between class and functional components is that can have things like constructor and lifecycle management
  - Functional components don't have instances because they are just JS function. A function can't have instance whereas classes have instances (object) of them
  - Default React component extend React component class, so they inherit features like lifecycle and internal state management
  - In that sense, I would call them component which have instance

  Function Component
  - Don't have instances
  - Can be rendered mutiple times

## 3. Algorithms use to compare changes in DOM?
  - When a component's props or state change, React decides whether an `actual DOM update` is necessary by comparing the newly returned element with the previously renderd one
  - When they are `not equal`, React will update the DOM. This process is called `reconciliation`

## 4. HOCs, renderProps?
 HOC and conditional rendering
  - With React context, takes a Component and returns another function (function stateless component or ES6 class component)
  - High-order components are reusable

## 5. How to prevent a component render?
 - Pure components defined as function will always re-render
 - In component class and prevent the re-render in `shouldComponentUpdate()` returning false

 Pure Component
  - Does shallow compare on the component's props and state
  - If nothing changes, it `prevents` the re-render of the component
  - If something changes, it re-renders

Class component with `shouldComponentUpdate` method
  - Has access to the `next props` and `state` before running the re-rendering component that's where you can decide to prevent the re-render by return false from this method
  - If you return true, the component re-renders

## 6. Tell some react-hooks you know, how to write an react-hook?
- [Read more](https://www.digitalocean.com/community/tutorials/react-hooks)
- Hooks bring statefulness and lifecycle method, previous only available from class component to function component
- Hooks can only be called from within function components and custom hooks

 State
  - We need to initialize the state using useState

 Lifecycle
  - Hooks feature is the addition of useEffect which is a combination of componentDidMount, componentDidUpdate, componentWillUnmount
  - `useEffect` will fire after initial render and subsequent re-renders


## 7. Do you need to usecallback?
  - `useCallback` does not memoize the function result

## 8 `useCallback` and `useMemo` use memoization
  - Memoization as remembering something
  - Both useMemo and useCallback remember something between renders until dependencies changes
  - The difference is just what they remember
  - These two hooks are primarily based arround performance between renders
  - useMemo will remember the rendered value from function
  - useCallback will remember the actual function, the memoized version of the callback, memoized callback
  - Useful when passing callbacks to optimized child component that rely on reference equality to prevent unnecessary renders

## 9. Middleware
  - Redux middleware function provides a medium to interact with dispatched action before they reach the reducer
  - Providing a third-party extention point between dispatching action and the moment it reaches the reducer

## 10. Saga:
  - Create the task will perform the asynchronous action
  - Launch the above task on each action
  - `takeEvery` allows mutiple fetchData instances to be started concurrently. At a give moment, we can start a new fetchData task while there are still one or move previous fetchData task which have not yet terminated
  - `takeLatest` allows only one fetchData task to run at any moment. It is latest started task. If a previous task is still running when another fetchData task is started, the previous task will be automatically cancelled

## 11. Redux
- Redux is a pattern and library for managing and updating application state

STORE
  - The center of redux application is the Store
  - A store is a container that holds your application global's state
  - Create a plain action object to decribes something that happens in the application, and then
  - dispatch / execute the action to the store
  - When a action is dispatched, the store runs the root reducer 
  - Lets it calculate the new state based on the old state and the payload of the action
  - The store will notify to the subscribers that the state has been updated so UI can be updated with the new data

STATE, ACTION, REDUCER
  - State value describes the application
  - Reducer receive two arguments, the current state and an action object describing what happened
  - Action object always have a type field

 UI
  - The user interface will show existing state on screen
  - User does something, the app will update its data
  - and redraw the UI with those values

Details:
  - When the store state changes, update UI by reading the latest store state and show new data
  - Subscribers redraw whenever the data changes in the future

FLOW
  - Create a store instance by Redux library createStore API
  - Pass reducer to createStore generate initial state and to calculate future updates
  - A user does something, Redux application need to repond to input, create action object describes what happened
  - And dispatching to the store
  - When we call dispatch store.dispatch(action), the store run reducer, calculate the updated state
  - And run subscribers to update UI

## 12. Ref
- React's virtual DOM
- There are some cases, you need to interact with the actual elements
- Ror these occasions, React provides a ref system
- Using Refs to get value of an input
- Using Refs to focus the input
- To interact with directly DOM element, using React's createRef method allows you just do that
- React provides a way to get references to DOM nodes by using React.createRef()
- It's really equivalent `document.getElementById('foo-id');`
- Controlling HTML media elements
- Refs with React Hooks Using useRef

`useRef`
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

## 13. JSX
- React like libraries, there's no HTML and instead everything is JS
- Essentially allow us write HTML Javascript
- Allow also write JS within `{}` ex: `<p>{this.item.content}</p>`
- JSX is not a valid JS
- Help in writing represention of real DOM
- Convert into corresponding Json object (VDOM is tree), so we can eventually use it an input create Real DOM
- JSX was Babel convert to Pure JS

## 14. DOM
- React creates a tree of custom objects representing a part of DOM
- Instead of creating a actual `div` element
- Creates a React.div object
- It can manipulate these object every quickly without actually touching the real DOM
- When it renders a component, it uses the virtual DOM to figure out what it needs to do
  with the real DOM to match the 2 trees to match
- You can think of virtual DOM like a blueprint
- It contains the details needed to construct the DOM
- but because it doesn't require all the heavyweight part that go into the real DOM
- It can be create and change much more easily
- Is an in-memory represention of the real DOM element generated by React component before any changes are made to the page
- Render the function being called, displaying of elements on the screen
- A component renders some markup, but it's not the final HTML
- It's the in-memory representation of what will become real elements
- Then that output will be transformed into real DOM that is what gets displayed in the browser
- So why go through all this to generate a virtual DOM?
- Simple answer
- This is what allow react to be fast
- It does this by means of virtual DOM diffing. Comparing two virtual trees — old and new — and make only the necessary changes into the real DOM.


## 15. React compare obj in dependence of useEffect
- The useEffect hook runs even if one element in the dependency array has changed
- Then even if the object is modified, the hook won't re-run because it doesn't do the deep object comparison between these dependency changes for the object.

```
  const prevObj = usePrevious(obj); // hold prev ver

  useEffect(()=>{
    if (prevObj && !_.isEqual(prevObj,obj)) { // lodash
      // ...execute your code
    }
  },[obj, prevObj])
```
```
const initialRender = useRef(true); // https://stackoverflow.com/a/53351556/5644090
  const prevBook = usePrevious(book);
  useEffect(() => {
    if (initialRender.current) {
      initialRender.current = false;
      return;
    }

    if (!_.isEqual(prevBook, book)) {
      // do something
    }
  }, [book, prevBook]);
```

```
  import useDeepCompareEffect from 'use-deep-compare-effect'

  useDeepCompareEffect(()=>{
      // ...execute your code
  }, [obj])
```

```
import isDeepEqual from 'fast-deep-equal/react'
const bookRef = useRef(book)

  if (!isDeepEqual(bookRef.current, book)) {
    bookRef.current = book
  }

  useEffect(() => {
    // do something
  }, [bookRef.current])
```
