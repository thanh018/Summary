## I. JAVASCRIPT
## 1. Load Javascript with `async` and  `defer` in <script>
- With `async`, the file gets downloaded asynchronously and then executed `as soon as it’s downloaded`.
- With `defer`, the file gets downloaded asynchronously, but executed only when the `document parsing is completed`
- [Read more](https://www.digitalocean.com/community/tutorials/html-defer-async)

## 2. Phân biệt var, let, const
- Main difference is scoping rules
- Variable declared by `var` keyword are scoped to immediate function body (hence function scope)
- Variable declared by `let` keyword are scoped to enclosing block denoted by `{}` (hence block scope)
- `let` can be re-assigned
- `const` can not be re-assigned (can re-assigned value of each key in `object` was declared with `const`)
- [Read more](https://stackoverflow.com/questions/762011/whats-the-difference-between-using-let-and-var)
```
  function run() {
    var VAR = "VAR";
    let LET = "LET";
    console.log(VAR, bar); // VAR LET
    {
      let LET_IN_BLOCK = "LET_IN_BLOCK";
      console.log(LET_IN_BLOCK); // LET_IN_BLOCK
    }
    console.log(LET_IN_BLOCK); // ReferenceError
  }
```
# 3. Hoisting
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
# 4. Implement a map function
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
# 5. Closure
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

# II. REACT
# 1. Why should we update the state directly?
  - If you try to update state directly then it `won't re-render` the component
      ```
      this.state.visible = true; // Wrong
      ```
  - Instead use `useState()` method. It schedules an update to a component's state object. When state changes, the component responds `by re-rendering`
      ```
      this.setState({ visible: true });
      ```

# 2. React Element & React Component
  [Read more](https://stackoverflow.com/questions/30971395/difference-between-react-component-and-react-element/47675471)

  React Element
  - Gets returned from Components
  - It's an object that virtually decribes the DOM nodes that Components

  React is good with these way:
  - Diff an object with previous object representation to see what has changed.
  - Can update the actual DOM specifically where the changes it detected occurred
  - CreateElement invocation returns an object

  React Components:
  - Is a function or class which optionally accepts input and returns a React Element
  - React sees a function or class as the first argument
  - It will check to see what element it renders
  - Give the corresponding props and will continue to do this until there are no more createElement invocation

  Class based component
  - Have instance
  - Using lifecycle
  - Executes after the component is render for the first time (componentDidMount)

  Why do functional components in Reactjs not have instance?
  - [Read more 1](https://stackoverflow.com/questions/44478809/why-do-functional-component-in-reactjs-not-have-instances)
  - You may not use ref attributes on functional components because they dont have instances
  - The only difference between class and functional components is that can have things like constructor and lifecycle management
  - Functional components don't have instances because they are just JS function. A function can't have instance whereas classes have instances (object) of them
  - Default React component extend React component class, so they inherit features like lifecycle and internal state management
  - Those features allow the component to keep their state between render
  - In that sense, I would call them component which have instance
  - [Read more 2](https://github.com/facebook/react/issues/4936#issuecomment-142379068)
  - Ref don't work on the stateless components

  Function Component
  - Don't have instances
  - Can be rendered mutiple times
  - But React does not associated a local instance each render
  - React uses invocation of the function to determine what DOM element to render for the function

# 3. Algorithms use to compare changes in DOM?
  - When a component's props or state change, React decides whether an `actual DOM update` is necessary by comparing the newly returned element with the previously renderd one
  - When they are `not equal`, React will update the DOM. This process is called `reconciliation`
  - [more 1](https://stackoverflow.com/questions/34990190/reconciliation-in-react-detailed-explanation)
  - [more 2](https://evilmartians.com/chronicles/optimizing-react-virtual-dom-explained)

# 4. HOCs, renderProps?
- [Read more](https://www.robinwieruch.de/react-higher-order-components)

 HOC and conditional rendering
  - With React context, takes a Component and returns another function (function stateless component or ES6 class component)
  - High-order components are reusable

# 5. How to prevent a component render?
 - Pure components defined as function will always re-render
 - Convert the component to class and prevent the re-render in `shouldComponentUpdate()` returning false
 - [Read more](https://stackoverflow.com/questions/41763031/how-to-prevent-react-from-re-rendering-the-whole-component)

 Pure Component
  - Does shallow compare on the component's props and state
  - If nothing changes, it `prevents` the re-render of the component
  - If something changes, it re-renders
  - If you want to use functional stateless components as Pure Component instead, use recompose's pure high-order-component
  - import `{ pure }` from `recompose` to wrap that component
  - [Read more](https://www.robinwieruch.de/react-prevent-rerender-component)

Class component with `shouldComponentUpdate` method
  - Has access to the `next props` and `state` before running the re-rendering component that's where you can decide to prevent the re-render by return false from this method
  - If you return true, the component re-renders
  - It can be used to prevent the component re-rendering on fine-grained levels

# 6. Tell some react-hooks you know, how to write an react-hook?
- [Read more](https://www.digitalocean.com/community/tutorials/react-hooks)
- Hooks bring statefulness and lifecycle method, previous only available from class component to function component
- Hooks can only be called from within function components and custom hooks

 Component state
  - We need to initialize the state using useState

 Component lifecycle
  - Hooks feature is the addition of useEffect which is a combination of componentDidMount, componentDidUpdate, componentWillUnmount
  - `useEffect` will fire after initial render and subsequent re-renders

# 7.	Write a countdown by using react-hook
- [Read more 1](https://github.com/do-community/react-hooks-timer/blob/master/src/App.js)
- [Read more 2](https://www.digitalocean.com/community/tutorials/react-countdown-timer-react-hooks)

#8. Do you need to usecallback?
  [Read more 1](https://stackoverflow.com/questions/53159301/what-does-usecallback-usememo-do-in-react)
  [Read more 1](https://stackoverflow.com/questions/54963248/whats-the-difference-between-usecallback-and-usememo-in-practice/54963730)
  - `useCallback` does not memoize the function result
  - Arrow function in render
  - Bind in contructor (es2015)
  [Read more 3](https://atomizedobjects.com/blog/react/what-is-the-difference-between-usememo-and-usecallback/)

 8.1. useCallback and useMemo use memoization
  - Can call thing of memoization as remembering something
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

21. Promise
- https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-promise-27fc71e77261
* Summary this post

- an object that can be returned a synchronous from asynchronous function
- Async Function standard used to make a asynchronous code look synchronous
- will be in one of 3 possiable state:
- fulfilled: `resolve()` was called;
- rejected: `reject()` was called: the reason for rejection
- pending: no yet 2 states above but transition into a fulfilled or rejected

- es6 promise constructor takes a function
- takes 2 parameters `resolve()` or `reject()`
- `resolve()`: pass a callback function attached with `.then()`
- `reject()`:  pass an Error object

- `.then()`: return a new promise
- it's possible to promise chain

- ex: promise chaining
```
    fetch(url)
      .then(process)
      .then(save)
      .catch(handleErrors)
    ;
```
- promise chain: will result in a sequence, that runs in serial
- assuming each function `fetch()`, `process()`, `save()` return a promise
- `process()` will wait for `fetch()` to complete before starting
- `save()` will wait for `process()` to complete before starting
- `handleErrors()` will only run if any of the previous promises reject

- Extras of Native JS Promise
- `Promise.resolve()` returns a resolved promise
- `Promise.reject()` returns a rejected promise
- `Promise.race()` takes an array and returns a promise, that 
  - resolves with value of the first promise that resolves OR
  - rejects with the reason of the first promise that rejects
- `Promise.all()` takes an array and returns a promise, that
  - resolves when all of the promises in arguments have resolved OR
  - rejects with the reason of the first promise that rejects


22. JSX
- React like libraries, there's no HTML and instead everything is JS
- essentially allow us write HTML Javascript
- allow also write JS within `{}` ex: `<p>{this.item.content}</p>`
- JSX is not a valid JS
- help in writing represention of real DOM
- convert into corresponding Json object (VDOM is tree), so we can eventually use it an input create Real DOM
- JSX was Babel convert to Pure JS

23. DOM
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

23. Pass by reference object
- https://codeburst.io/explaining-value-vs-reference-in-javascript-647a975e12a0
- Js has 5 data types that are passed by value: Boolean, null, undefined, String, Number (primitive types)
- JS has 3 data types that are passed by reference: Array, Function, Object
- Objects are created at some location in your computer's memory
- An address points to the location, in memory, of a value that is passed by reference
- A variable holding an object does not directly hold an object.
- what it holds is `reference to an object`
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

  const PAGING_CONFIG = {
    pageSize: 30,
    pageOrder: 0,
  };

  
  useEffect(() => {
    const payload = PAGING_CONFIG; // const payload = { ...PAGING_CONFIG };
    payload.pageOrder = 1;
    action.getFetchData(payload)
    console.log(PAGING_CONFIG) // { pageSize: 30, pageOrder: 1 };
  }, [])

  // objOne -> { x: 1, y: 2 }

  var objTwo = objOne; // when you assign that reference from one to another, you make a copy of the reference

  // objOne -> { x: 1, y: 2 } <- objTwo

  objTwo.x = 2; // Modify an object through that reference, changes it for both variables, holding a reference to that object

  // objOne -> { x: 2, y: 2 } <- objTwo (update object via objTwo variable)

  objTwo = {}; // When you assign a new value to one of the variables, you modify the value that variable holds

  // objOne -> { x: 2, y: 2 }, objTwo -> {}
```
  (https://stackoverflow.com/questions/37290747/pass-by-reference-javascript-objects)

24. Remove the same item in array

```
  let users = [1, 1, 2, 2, 3, 3, 5, 8, 10, 23, 15];

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

25. Implement function
  case 1: add(1,2); // 3
  case 2: add(1,2,3); // 6
  ```
    function add3(...x) {
      console.log(x)
      return x.reduce((count, item) => count + item, 0);
    }
  ```


26. Compare 2 object

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

27. bind(this) explain?

28. show number of element in array
  ```
    let input = ['a', 'a', 'b', 'c', 'b', 'c', 'd']; // output: {a: 2, b: 2, c: 2, d: 1}

    input.reduce((ob, chars) => {
        if (!ob[chars]) ob[chars] = 1;
        else ob[chars]++
        return ob;
    }, {})

  ```

29. copy shadow obj and deep obj

30. Server side rendering?

31. Generator function?

32. React compare obj in dependence of useEffect
- The useEffect hook runs even if one element in the dependency array has changed
- Then even if the object is modified, the hook won't re-run because it doesn't do the deep object comparison between these dependency changes for the object.
// https://stackoverflow.com/questions/54095994/react-useeffect-comparing-objects
// https://github.com/facebook/react/issues/14476
// https://dev.to/w1n5rx/ways-to-handle-deep-object-comparison-in-useeffect-hook-1elm

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

32. Clone a nested object

`https://blog.logrocket.com/4-different-techniques-for-copying-objects-in-javascript-511e422ceb1e/`

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

--- VUE

1. Method
- A method invocation will always run whenever a re-render happen
- In case where you do not want caching

2. Computed: Return cached values until dependencies change
(Computed nó như là property của component nhưng property nó đc tính toán)
- Computed properties are cached based on their reactive dependencies
- A computed will only re-evaludate when some of its dependencies have changed
- In case you want to compose a new data from existing data sources,
  to manipulate a large group of data,
  to use values directly in the template

3. Watch
(Watch thì mình theo dõi cái property đó change để khi nào nó change giá trị mình expected 
  thì mình thực hiện tính toán khi cần)
nó xay dựng tường minh hết ak anh
- Allow us to perform asynchorous operation (accessing an API),
  limit how often we perform that operation
- In case you want to listen when data property changes,
  to watch a data property until it reaches some specific value and then do something

4.
Mutation vs action khác nhau sao?
Mà sao mình không change state ở trong action mà phải change state ở mutation
- Nhiệm vụ mutation: là change state và nó thực hiện tác vụ đồng bộ
  => để mình có thể biết state đó 'được thay đổi bởi action nào'
  - mutations are essentially events: each mutation has a name and a handler.
  - mutation is the only way to modify state
  - mutation doesn't care about business logic, it just cares about "state"
  - mutating the state
- Nhiệm vụ của actions khác mutation là : nó thực hiện các tác vụ bất đồng bộ
  => nên đa phần mình sẽ call api ở chỗ này
  - Actions: Actions are just functions that dispatch mutations.
  - action is business logic
  - it just implements the business logic
  - doesn't care about data changing
  - commit mutations
  - An action takes a context object, which we can use to call commit to commit a mutation.

- Instead of mutating the state, actions commit mutations.
- Actions can contain arbitrary asynchronous operations.
- In mutations you can change the state but not it actions.
- Inside actions you can run asynchronous code but not in mutations.
- Inside actions you can access getters, state, mutations (committing them), actions (dispatching them) etc in mutations you can access only the state.

5.
React & vue khác nhau như thế nào
  react js: nó binding data one way
  vue js: binding data two way

- a/ 2 binding?
  Two Way
  - Changes made in view will reflect in Controller
  - changes made in Controller will reflect in View
- b/ 1 binding?
  One Way
  - Once you set the value it will not affect the View or Controller for further changes


6. Single-Page Application
- The simplest answer is an SPA or Single Page Application is a web app
  that loads a single HTML page and all the necessary assets (such as JavaScript and CSS)
  required for the application to run
- It then uses dynamic techniques (AJAX) to update just parts of that page,
  instead of making a round trip to the server to create new pages.
full: https://stackoverflow.com/questions/51506533/what-is-single-page-application
7. So sanh dispatch vs commit
`$dispatch` triggers an action, and `commit` triggers a mutation

ex: getUser => save vao` store

mounted / created:  dispatch("loadCurrentUser") vi du Returning a Promise
  => $dispatch triggers an action
  $dispatch sends a message to your vuex store to do some action.

commit => It is done only from within an action => commit is synchronous
  // The data is available now. Finally we can commit something
  vd : context.commit("saveCurrentUser", response.body)

```
  mutations: {
      saveCurrentUser(state, data) {
          Vue.set(state, "currentUser", data)
      },
      // More commit-handlers (mutations)
  }

```
8. mappers
- mapState is a helper that simplifies creating a computed property
  that reflects the value of a given state.
- mapGetters is a helper that simplifies creating a computed property
  that reflects the value returned by a given getter.
- mapActions is a helper that simplifies creating a method
  that would be equivalent as calling dispatch on an action
- mapMutations is a helper that simplifies creating a method 
  that would be equivalent as calling commit on an mutation

9. Life cycle hooks

* beforeCreate
- Runs at the very initialization of your component
- Data has not been made reactive, and events have not been set up yet.

* created
- access reactive data
- events that are active with the created hook.

- reading/writing the reactive data
- make an API call and then store that value

* beforeMount
- right before the initial render happens
- after the template or render functions have been compiled.
- perform your API calls before it’s unnecessary late in the process 

* mounted
- access to the reactive component, templates, and rendered DOM
- watch-compute-render cycle for your component.

* beforeUpdate
- after data changes on your component 
- before the DOM is patched and re-rendered.

* updated
- after data changes on your component and the DOM re-renders.

* destroyed
- there’s practically nothing left on your component.
- Everything that was attached to it has been destroyed.

* beforeDestroy
- clean-up events or reactive subscriptions.