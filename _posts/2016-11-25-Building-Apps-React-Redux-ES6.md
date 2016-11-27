---
layout: post
title: Building Apps with React, Redux, and ES6 (by Cory House from Pluralsight) (in progress!)
---

* Cory House @housecor

# Table of Contents
  * [Chapter 1 - Setup](#chapter-1)

## Chapter 1 - Setup<a id="chapter-1"></a>

### Links to Code Starter Kits
* [Pluralsight Redux Starter - Simplified version of React Slingshot](https://github.com/coryhouse/pluralsight-redux-starter)
* [React Slingshot - Redux, React, Babel, ES6, Hot Reload starter kit](https://github.com/coryhouse/react-slingshot)

### Steps to Create Build (NPM, Webpack, ESLint, Babel, ES6, Express, Mocha)
* Added package.json using template from pluralsight-redux-starter
* Added index.html using template from pluralsight-redux-starter
* Added index.js
* Added webpack.config.dev.js using template from pluralsight-redux-starter
* Added .editorconfig using template above http://editorconfig.org/
* Added .babelrc using template above for Babel transpiler with ES6 to support all recent browsers
* Added .eslintrc using template above
* Add tools/srcServer.js to serve up files in src directory using Express
* Use NPM to create script to start Express server. Use babel-node to compile ES6 in srcServer
`"start": "babel-node tools/srcServer.js"` and run with `npm start` and go to localhost:3000,
and check Console for console.log's and that bundle.js compiled
* Add NPM `prestart` script in package.json. Disable ESLint use of console `/* eslint-disable no-console */`
* Add NPM `lint` script to run ESLint. ESLint does not support watch, so ESLint Watch `esw` is used.
* Add NPM `lint:watch` script to run ESLint Watch
* Add NPM `test:watch` script to run Mocha Watch
* Use `npm run all` to run multiple NPM scripts in sequence or parallel and output to command line

### Steps to Create App (with React and React Router)
* Create pages using templates from react-slingshot
    * Create folders for each Component within src/components
    * Create HomePage.js and AboutPage.js Components (use convention of __Page.js suffix for top-level Components)
* Create layout
    * Create Common Parent AppComponent src/components/App.js for markup shown on all pages and to wrap other components
* Configure routing
    * Configure Entrypoint to use React Router in index.js
    * Setup CSS
* Setup navigation
    * Refactor Header into common Header Component
* Create skills Component and update routes and header

### IDE
* Atom - Add packages react, and terminal-plus
* IntelliJ - Configur Languages & Frameworks > JavaScript > Change to ES6 or JSX Harmony/React JSX (for React)

### Stack
* Redux, ES6 and Babel, React Router, Webpack, NPM, ESLint, Mocha, React Test Utils, Enzyme
* Note:
    * NPM scripts used directly instead of using Gulp plugin abstraction and less points of potential failure
    http://bit.ly/npmvsgulp

### JS Mutable vs Immutable

* Immutable (when change value of a type a new copy is created). High Performance
    * Number, String, Boolean, Undefined

{% highlight javascript %}
// Current State
state = { skill: 'JavaScript' }
// Return New Object (Not Mutate State)
return state = { skill: 'React' }
{% endhighlight %}

* Cont'd
    * Change State Approaches (Non-Manually create copies of existing object)
        * Object.assign(target, ...sources)
            * Allows specify existing object as template
{% highlight javascript %}
/**
 *  Create deep copy clone of existing state object with role property changed to admin.
 *  Create new empty object, mixing target object {} with existing state, and  *  changing role property to value of admin.
 *  Note: First parameter must be empty object otherwise state is mutated
 *  instead of a new deep copy object being created.
 */
Object.assign({}, state, {role: 'admin'});
{% endhighlight %}

* Mutable
    * Objects, Arrays, Functions
    * `Object.assign` (ES6 but requires babel-polyfill to transpile)
    is used in Reducers to return deep copy of existing State with
    requested changes to update State
    * Example Mutate State:

{% highlight javascript %}
// Current State
state = { skill: 'JavaScript' }
// Mutate State
state.skill = 'React';
return state;
{% endhighlight %}

### Handling Immutable State
* ES6 Reducers with:
    * `Object.assign`
    * Spread operator
* ES5
    * Lodash merge, extend
    * Object-assign (NPM)

* Libraries
    * react-addons-update
    * Immutable.js

### Enforce Immutability (not mutate Redux state)
* Trust team
* Safety net (redux-immutable-state-invariant) - only in Dev Mode
* Programmatic enforcement (Facebook Immutable.js) - creates immutable JS data structures

### Immutability Benefits
* Clarity
    * Centralised immutable Store ease of identifying where and how State change occurred
    (i.e in a Reducer). So more predictable and easier to reason about.
* Performance
    * Mutable State would require expensive operation checking every single property
    to see if State actually changed (for large State object with many properties)
    * Immutable State only requires a Reference Comparison (checking if old State not
    referencing same object in memory to know if State changed)
    i.e. `if (prevStoreState !== storeState)`.
    React-Redux library uses Reference Comparison to determine when to notify React
    Component of State changes. If NO changes then `shouldComponentUpdate` method is called.
* Time-Travel Debugging Experience
    * Redux DevTools Extension allow travel through time whilst debugging
    and see each specific State change occur (and even undo/redo State changes to
    see how affect final state). Turn off specific Actions to see final state without
    certain Actions occurring. Playback at desired speed.


### Data Management Tools
* React setState, Flux/Redux, Relay

### Hot Reloading
* `babel-preset-react-hmre` wraps Components in custom Proxy (with hooks to inject
new implementations such as immediate apply changes without reload upon save) using Babel
* Limitations:
    * Only reloads functional Components where a class is up the hierarchy tree
    * Does not reload all container functions (i.e. `mapStateToProps`)

### Redux
* Manages app data flows
* Single Centralised Object Graph - avoids interactions handling multiple Stores
* Redux uses less boilerplate code than Flux and more power
    * Components (top-level) subscribe to Redux Store
* Server-rendering of Components is possible (Isomorphic JS)
* Immutable Store offers Hot Reloading (live changes retains current client-side State),
performance, Time Travel Debugging (step fwd/back and replay State)
* API is 2kB minified

### Redux 3x Principles
* All app State in single immutable Store (State cannot be changed)
    * Note: In contrast to Flux's multi-Store model, single Store helps debugging,
    server rendering, and undo/redo
* Actions (describing user intent) emitted are the only way to trigger State mutations
* State mutations/updates performed by pure functions (Reducers)
    * Reducers are pure functions that accept current State and Action and return new State

### React + Redux Use Cases
* Basic app use Vanilla JS
* Mid-complex app use jQuery for changing DOM, AJAX calls
* Complex app use React and Redux for complex data flows, same data multiple places,
  many state changes (ideally managed in single location for consistency and testability),
    * Redux not useful for just displaying static data
    * Redux useful for handling interactions between two Components without Parent/Child relationship
    or when they both use the same data by allowing them to communicate to ensure the data
    stays in synch by connecting them both to a Redux centralised Store (like local client-side database) that
    and each Component may dispatch an Action that updates the single Store. All Components
    connected to Store are immediately notified of data changes
    * Redux useful when two Components manipulating same data (when non-hierarchical data)
    * Redux useful for many Actions by offering structure and scalability
    * High Setup time for React and Redux to be scalable, testable, maintainable, with rapid feedback for complex apps

### Redux vs Flux
* Common
    * Unidirectional data flow - Data flows down, Actions flow up
    * Finite set of Actions utilised that define how State may be mutated
    * Action Creators may be defined to generate Actions
    * Action Types (constants) may be used
    * Store that holds State
* Redux Only
    * Single Store only allowed (single source of truth avoids storing
    same data in multiple places, and complexity of handling interactions between Stores)
    * Reducers (pure functions) accept current State and Action and return new State
    * Reducers
    * Container Components (React Components) with logic for marshalling Data and Actions
    passed down to separate Dumb Components that use pure functions to receive the data via Props
    (ease of testing and reuse)
    * Redux Store is immutable and need approaches to deal with immutable data in Reducers
    * Redux does NOT have a Dispatcher, instead easily composes Reducers (pure functions)
    * Actions are each handled by one or more Reducers that update Single Store
    * Reducer updates Store with new updated copy of State (since State is immutable)
    * Reducers specify how State changes for given Action
    * Redux SEPARATES State and Logic of changing state using Single Responsibility Principle:
        * Reducers handle Logic for returning new updated copy of state (since State is immutable in Redux Store)
        * Single Store contains only State
    * Single Store passes Actions down to defined Root Reducers
    * Multiple Reducers (optionally Nested via Functional Composition like how
    React Components may be Nested) to handle complex Stores with multiple
    potential Actions
    * React-Redux library used that contains a Connect method that generates a top-level
    React Component connected to Actions and Single Redux Store automatically.
    All changes to Store State causes a function to be called triggering a
    React Component re-render
    * Dispatching Actions affects the data in the Store

    * Example Action Creator:

{% highlight javascript %}

// Action Creator (convenience function with same name as Action Type)
rateSkill(rating) {
  return
    // Action
    {
        type: SKILL_RATE,   // Action Type
        rating: 2           // Action Data may be any value serialisable to JSON (i.e. not functions or Promises)
    }
}
{% endhighlight %}    

* Cont'd
    * Example Action:

{% highlight javascript %}
{ type: SKILL_RATE, rating: 2 }
{% endhighlight %}

* Cont'd
    * Example Store:

{% highlight javascript %}
// Call createStore in app entry point.
// Pass createStore function to Reducer function to handle Logic to return new state
let store = createStore(reducer);
{% endhighlight %}

* Cont'd
    * Redux Store API

{% highlight javascript %}
store.dispatch(action)      // Dispatch Action
store.subscribe(listener)   // Subscribe to Listener
store.getState()            // Return current State
replaceReducer(nextReducer) // Replace a Reducer (i.e. Hot Reloading)

NO API to change data in Store (immutable), as may only change by dispatching an Action to Reducer for new object response
{% endhighlight %}

* Cont'd
    * Example Reducer:

{% highlight javascript %}
// Reducer is a pure function that accepts current State and Action
// and returns new object State to update Store and then React-Redux library
// notifies React Components connected to the Store that re-render
// using the new data

 function appReducer(state = defaultState, action) {
   switch(action.type) {
     case SKILL_RATE:
     // return new copy of State
   }
 }
{% endhighlight %}

  -* Action     - describes user intent contains type/data
 |     |          where data portion may be multiple objects and metadata  
 |     |          (i.e. action for rating skill { type: SKILL_RATE, rating: 2 } )
 |     |
 |     *
 |   Store *----- Reducers
 |     |   -----*
 |     *
  - React

* Flux Only
    * Multiple Stores allowed
    * Concepts are Actions, Dispatchers, Stores
    * Actions triggered causes Singleton Dispatcher to notify Stores
    * Stores explicitly connect to Dispatcher using EventEmitter to know about Actions
    * Stores contain BOTH State and also Logic to change the state
    * Different Stores are disconnected but interact via WaitFor function
    * Stores are flat (NOT Nested)
    * Flux Store is mutable
    * Explicitly Subscribe the React Views to Stores using OnChange handlers and EventEmitter

### Setup Environment
* Dependencies
    * `babel-polyfill` - Covers ES6 features that do not simply transpile from ES6 to ES5
    (i.e. array.from, set, map, promise, generators, etc) listed as "Additional polyfill needed" at
     [Babel Learn ES2015](https://babeljs.io/docs/learn-es2015/)
    Note: `babel-polyfill` is large (50kB) so consider injecting only subsets of polyfill dependencies

### React Component Approaches
* React Component Creation Approaches
    * ES5 `.createClass` (original React approach)
        * Autobinded functions to Components' `this` context
        * PropTypes declared inside class definition
        * Initialise State in `setInitialState` method
    * ES5 stateless function style (simpler syntax where React assumes that `return` is the `render` function
    (i.e. `var Hi = function(props) { return (<h1></h1>); });`
        * Use when receive data only from Props (immutable), and where Component NOT manage State, utilise Component lifecycle methods, or performance optimisations
    * ES6 `.class`
        * No autobinding of functions to Components' `this` context that changes depending on the caller.
            * Solution options (setup ESLint rules to ensure comply):
                * Inline `.bind` in Render function - Explicit bind is required (i.e. `<div onClick={this.handleClick.bind(this)}></div>`
                * Constructor binding (preferred for performance) - (i.e.
                `class ___ extends React.Component { constructor(props) { super(props); this.handleClick = this.handleClick.bind(this) } ...`)
        * PropTypes and Default Props declared BELOW class definition (unless enable experimental Stage 1 JS proposal in Babel for class fields and static properties inside class)
        * Initialise State in Component `constructor` method
    * ES6 stateless function style
    (i.e. `const Hi = (props) => { return (<h1></h1>); });` (`const` prevents accidental reassignment of Component)
    * [Others](bit.ly/react-define-component) including:
    `Object.create`, Mixins, Parasitic Components, StampIt
* Non-React Class Creation Approaches
    * [Reflect APIs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Reflect/construct) `Reflect.construct`
* Container vs Presentational Components for reusability, maintainability, and scalability

### Class Component Style vs Stateless Component (Functional) Style
* Benefits of Stateless Functional Components (pure functions that receive Props and output HTML)
    * No Classes required (simplifieds to plain functions, removes `extends` and `constructor`,
    avoid the `this` keyword and having to use `bind(this)` to pass context around)
    * No support for State or lifecycle methods (protects/prevents developer from lazily hacking in local State)
    * Less code (noise/bloat, code smells) required. Simple assertions with isolated tests (less Mocking, state changes, etc)
    * Destructure Props in ES6 so all data specified in single function argument. Clear Component dependencies
    * Usage: Dumb Presentational Components (focussed on UI rather than State/behaviour)
    * Note: State management belong in high-level Container Components and Flux/Redux

* Example Class Style
    * Usage: Require State, require Refs to underlying DOM, require lifecycle methods,
    require Nested Components (Child functions where each render creates new instance)
    * Use class for top-level Components to prevent issue with Hot Reloading

{% highlight javascript %}
import React from 'react';

class Hi extends React.Component {
  constructor(props) { super(props) }

  sayHi(event) { alert(`Hi $(this.props.name}`); }

  render() {
    return ( <a href="#" onClick={this.sayHi.bind(this)}>Say Hi</a> );
  }
};
Hi.protoTypes = {
  name: React.PropTypes.string.isRequired;
};
export default Hi;
{% endhighlight %}

* Example Stateless Functional Component Style (equivalent)
    * Usage: No State, No Refs (as does not create Component instance so Ref is null),
    No lifecycle methods, No nesting (as performance problem)


{% highlight javascript %}
import React from 'react';

const Hi = (props) => {
  const sayHi = (event) => { alert(`Hi $(this.props.name}`); }

  return ( <a href="#" onClick={sayHi}>Say Hi</a> );
};
Hi.protoTypes = {
  name: React.PropTypes.string.isRequired;
};
export default Hi;
{% endhighlight %}

### Organising Components
* Separate folders for Container/Presentation or by Feature

### Container Component vs Presentation Component

* Container (Parent/Smart/Stateful/Controller View) (back-end of the front-end)
    * Behaviour
    * Marshalling Data and passing to Child Components
    * Actions passed to Child Components
    * Stateful (manage State)
    * Created using Redux connect
    * Redux-aware to Dispatch Actions to Store (and connecting to it with connect)
    * No Markup
    * Note: May have multiple Container Component levels

* Presentational (Child/Dumb/Stateless/View)
    * Dumb (no logic)
    * Receive Data and functions via Props to display UI
    * Stateless
    * No dependency on Redux, Actions, or Stores (easier to reuse)
    * Not specify how Data loaded or mutated
    * Markup

### TODO

* [Learn ES2015(ES6)](https://babeljs.io/docs/learn-es2015/)
* [ES6 Features](https://github.com/lukehoban/es6features#readme)
