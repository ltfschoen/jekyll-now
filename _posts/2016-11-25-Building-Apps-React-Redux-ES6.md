---
layout: post
title: Building Apps with React, Redux, and ES6 (by Cory House from Pluralsight) (in progress!)
---

* Cory House @housecor
* My Code from the course [https://github.com/ltfschoen/react-redux](https://github.com/ltfschoen/react-redux)

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
* Update skills Component with a `constructor` to initialise `state` for form
and add markup with `input` fields
* Add Event functions for form input change events (i.e. `onTitleChange`),
that accepts `event` parameter, extracts the target value and sets State `setState`
* Test Event functionality in browser and check console for errors
* Add Binding for ES6 Classes (since NOT done automatically) to overcome errors like
`Cannot read property 'state' of undefined` by ensuring the `this` context is 
correct in the `onTiteChange` handler, as the function inherits the `this` context
of the caller (i.e. passing the input field `this` context to the `onTiteChange`
change handler) instead of `this` being bound to the instance of the Component. 
**Bind to the `this` context of the Component instance in the `constructor` for all
Event handling functions**. Note: Alternative is to implement the binding in the
`render` function, but binding on each render causes a new function to be run on
each render, which negatively impacts performance
(i.e. DO NOT `<input onChange={this.onTitleChange.bind(this)}`)
* Setup Actions for Redux
    * Create Action Creator for skill in new folder/file of /src/actions/skillActions.js
    * Define Actions (define convenience function that returns Action plain object that must have Type property)
    * Refactor to use a `const` for the value of Type
    
Note: Using "short-hand property names", the below are equivalent in ES6 (i.e. do not need to show value if key/value are the same)

{% highlight javascript %}
  return { type: 'CREATE_SKILL', skill: skill }
  
  return { type: 'CREATE_SKILL', skill }
{% endhighlight %}

* Setup Reducer 
    * Create Reducer for skill in new folder/file of /src/reducers/skillReducer.js, and set initial state 
    in default parameters `state = []` (explicit name `skillReducer.js` so easily identify the file in 'tabs' of IDE)
    * In the Reducer, each Reducer processes specific slice of State. 
    Fork logic based on the Type of Action being processed using `switch`
    (alternatively use `if`, a lookup table of functions
    * DO NOT mutate State in the Reducer. Avoid mutating state by using ES6 spread operator
    to return new instance of State array, then create deep copy of the passed in action.skill and return a 
    brand new State containing extra value of new action.skill

{% highlight javascript %}
case 'CREATE_SKILL':
    return [...state,
    Object.assign({}, action.skill)
    ];
{% endhighlight %}

* Cont'd
    * Note: In Flux we handled Actions in the Store. In Redux we handle Actions in Reducers (accepts State/Action and returns new State)

* Setup Root Reducer (to support using Multiple Reducers in Redux)
    * Create folder/file at /src/reducers/index.js
    * Alias specific Reducer name for use throughout app (when `import`ing)

* Setup Redux Store
    * Create folder/file at /src/store/configureStore.js
    * Define a function to configure the Store and initialise it using `initialState` with some State (good for server-side 
    rendering), to be called at app entry point, so runs when start app
    * Pass parameters to `createStore`
    * Pass optional 3rd parameter as Middleware to enhance the Store using Redux function `applyMiddleware`
    and pass it any middleware to use in app (`reduxImmutableStateInvariant`) and invoke it
        * Other middleware options are support for hot reloading, dev tools in chrome, etc (see react-slingshot repo)
    * Update app entrypoint src/index.js to create instance of the Store Configuration.
        * Optionally pass initial State to Reducers to override default State defined there
        Use Cases for passing initial State: To **Rehydrate** Store using separate State passed down from server-side or from local storage

* Wrap app with Redux `Provider` from react-redux Library to makes use of Redux Store Instance across React Container Components
    * Note: `Provider` is a higher-order Component that attaches Redux Store with React Container Components
    by wrapping over entire app so the Store may be accessed from React Container Components.
    It takes the `store` as a Property
    * In src/index.js, wrap the Router Component with the Provider Component

* Update React Container Components by Decorating them with the React Redux `Connect` Component function
    * Note: `Connect` is a higher-order Component function that connects React Container Components with Redux
    * Note: `Connect` takes two functions as params `mapStateToProps` and `mapDispatchToProps` (may be named differently if desired)
    * Declare `mapStateToProps` - return Properties we want exposed/inject on the React Container Component
        * Declare Props to expose on Component so access with `this.props.__`. 
        Obtain the data for Prop from within Redux Store using the `state` parameter
        (i.e. `state.skills` would access the Alias `skills` for the skills Reducer defined in the Root Reducer)
        * `ownProps` allows access to Props attached to the React Component.
    * Declare `mapDispatchToProps` (OPTIONAL) - Actions to expose on React Container Component as Props 
    (i.e. `createSkill` injected in Component by Connect function)
        * **Approach Option #1 (Dispatch to Props)** If we DELETE this optional `connect` parameter, doing so causes the Component to automatically get a 
        `dispatch` Prop attached to it that is injected by `connect`, so now in the `render` function can use
        `this.props.dispatch`
        * Use `dispatch` by importing reference to Action Creators, and then dispatching actions in the 
        Event handlers of the Component by calling `this.props.dispatch(skillActions.createSkill(this.state.skill))` 
        and passing it an Action and Data
        for handling by Redux
        * Note: `dispatch` is a function that allows firing off Actions that are defined in the Action Creator
    * Example: Chain Functions.
    
{% highlight javascript %}
const connectedStateAndProps = connect(mapStateToProps, mapDispatchToProps);
export default connectedStateAndProps(SkillsPage);

// Decorate with React Redux Connect function
export default connect(mapStateToProps, mapDispatchToProps)(SkillsPage);
{% endhighlight %}

* Cont'd
    * **Approach Option #2 (Dispatch to Props)** - Use `mapDispatchToProps` instead of verbose way to dispatch an action (i.e. `this.props.dispatch..`)
     by using `mapDispatchToProps` to wrap the Action in a call to `dispatch` so easy to use in function (i.e. 
     onClickSave()), and triggers the flow through Redux. `dispatch` is no longer injected by Connect as a Prop
     on our Component, as we are using `mapDispatchToProps` now so `createSkill` is injected instead
    
{% highlight javascript %}
// BEFORE
onClickSave() {
    this.props.dispatch(skillActions.createSkill(this.state.skill));
}

SkillsPage.propTypes = {
  dispatch: PropTypes.func.isRequired
};

export default connect(mapStateToProps)(SkillsPage);

// AFTER
onClickSave() {
    this.props.createSkill(this.state.skill);
}

SkillsPage.propTypes = {
  createSkill: PropTypes.func.isRequired
};

function mapDispatchToProps(dispatch) {
  return {
    createSkill: (skill) => dispatch(skillActions.createSkill(skill))
  }
}

export default connect(mapStateToProps, mapDispatchToProps)(SkillsPage);
{% endhighlight %}

* Cont'd
    * **Approach Option #3 (Dispatch to Props)** - Using Helper function is less verbose than having to wrap
    Action Creator in a `dispatch` call using `bindActionCreators`, which goes through **all** 
    `skillActions` and then wraps them in a call to `dispatch`
    (now we map to **all** Actions and expect Actions at `this.props.actions` to help separate 
    Actions from Skills)

{% highlight javascript %}
// BEFORE
onClickSave() {
    this.props.createSkill(this.state.skill);
}

SkillsPage.propTypes = {
  skills: PropTypes.array.isRequired,
  createSkill: PropTypes.func.isRequired
};

function mapDispatchToProps(dispatch) {
  return {
    createSkill: (skill) => dispatch(skillActions.createSkill(skill))
  };
}

// AFTER
import {bindActionCreators} from 'redux';

onClickSave() {
    this.props.actions.createSkill(this.state.skill);
}

SkillsPage.propTypes = {
  skills: PropTypes.array.isRequired,
  actions: PropTypes.object.isRequired
};

function mapDispatchToProps(dispatch) {
  return {
    actions: bindActionCreators(skillActions, dispatch)
  };
}

{% endhighlight %}

* Create Mock API to simulate delay calling API 
(copy from https://github.com/coryhouse/pluralsight-redux-starter/tree/master/src/api)
    * Create /src/api/mockSkillApi.js
    * Create /src/api/delay.js (manages simulated delay of each Mock API call)

* Add Thunk Middleware to handle making Async calls
    * Import and apply Redux Thunk Library as Middleware in store/configureStore.js
    * Create Thunk to load skills when app loads initially (at bottom of src/actions/skillActions.js)
    * Note: Action Creator Naming Conventions - postfix 'Success' as only called upon successful promise
    * Create Success/Error Action Types called after API response (i.e. `LOAD_SKILLS_SUCCESS`)

{% highlight javascript %}
import * as types from './actionTypes';
import skillApi from '../api/mockSkillApi';

// Action Creators (postfix 'Success' as only called upon successful promise)
export function loadSkillsSuccess(skills) {
  debugger;
  console.log("action creators:loadSkillsSuccess - Called Action LOAD_SKILLS_SUCCESS");
  return { type: types.LOAD_COURSES_SUCCESS, skill: skill };
}

// Thunk async call to Mock API, wait for Promise resolve then dispatch Action
export function loadSkills() {
  // Thunk Wrapper function accepts dispatcher and are always returned by Thunks
  return function(dispatch) {
    /**
     *  Thunk Body makes call to API that returns a Promise
     *  Handle Promise in anonymous function that expects a list of skills
     *  Dispatch an ActionCreator and pass it the list of skills returned.
     */
    return skillApi.getAllSkills().then((skills) => {
      dispatch(loadSkillsSuccess(skills));
    }).catch(error => {
      throw(error);
    });
  }
}
{% endhighlight %}

* Cont'd
    * Create handler in Reducers file corresponding to Action Thunk to update Redux State
    with response from Mock API
    * Configure app entrypoint `index.js` so Fetches Skill data from Mock API when app initially loads.
    After `configureStore()` we may start Dispatching Actions against the Store
     (alternative in server-rendering is to inject JSON data into header of index.html)

* Restructure app to achieve Separation of Concerns
    * Create SkillsPage, SkillList, SkillListRow
    * Create common/SelectInput and common/TextInput
    * Container vs Child Components 
        * Container to list data (existing skills shown on page load after retrieval from 
        Mock API). Skills loaded by displaying Action with Redux using Redux Thunks
        * Child to manage add/edit data 
    * Move to Child the JSX markup, etc currently contained in SkillsPage
    * Separate Pages for Adding Skills and Editing Skills (ManageSkills Component)
    * Create Container Component using IntelliJ's Final Template Feature to create 
    Redux Container Component with boilerplate already setup
        * New > React Class Component
        * New > React Redux Container Component
        * New > React Stateless Component
    * Populate form using `mapStateToProps` and `componentWillReceiveProps`

* Centralise declarations of initial State by creating reducers/initialState.js
    * Reducers handle slices of the Store but provides centralised picture of how initialise object wrapped in Store

* Manage and display Status/Errors of Async Calls using Redux using dedicated Actions and Reducers
    * Loading indicator when app first loads during API call to retrieve data
    * Track and Display on progress of async call status when save form data 
    (avoid redirecting before save complete and updates not immediately shown)
    * **Solution** Use Loading Dots library as Preloader to display when AJAX call in progress.
    Add Toastr to show notifications
    * Handle errors of rejected Promises elegantly (instead of allowing API calls to silently fail)
    * **Optimistic Updates** are when want to immediately update UI with changes
    even before AJAX call has completed without showing Preloader (i.e. deleting a user)
    AJAX finishes behind the scenes.
    
### Redirect URL using React Router Context Types
* Context Types are global variables that Library authors use (avoids boilerplate
 plumbing code) but that other app developer consumers should avoid

* Declare Context Types (Static Property) to import into Component
    * Declare after Class Definition (since Context Types is Static Property)

{% highlight javascript %}
// React Router context pulled in so router available with this.context.router
ManageSkillPage.contextTypes = {
  router: PropTypes.object
};

...
// Change URL
this.context.router.push('/skills');
{% endhighlight %}

### Populate Form on page load (using Lifecycle Methods)
* `mapStateToProps` - initially populate form when mounted
* `componentWillReceiveProps` - update local State when Props change 

### Lifecycle Method componentWillReceiveProps updates State when Props change
* Situation is initialising Container Component State based on Props. 
After Container Component page initialises any changes to the Props
are not reflected in the Container Component State. 
Lifecycle method `componentWillReceiveProps` used to update Container Component State when
Props change, as it is called anytime the Props are changed.
* Warning: `componentWillReceiveProps` may run even when Props have not changed

### Container Component Structure Summary (using Redux)
* File Structure of Container Component
    * Constructor
        * Initialise State
        * Call `bind` on functions to be bound to `this` context
    * Child Functions (called by `render` function)
    * `render` function
        * Calls Child Component with JSX markup (Containers shouldn't renders JSX inline)
    * `propType` validations
    * Redux Connect and related functions (`mapStateToProps` and `mapDispatchToProps`)

### Destructuring 

{% highlight javascript %}
  render() {
    const {skills} = this.props; // Destructuring to achieve shorter calls (instead of having to write `this.props.skills` later)
    return (
       <SkillList skills={skills} />
{% endhighlight %}

### Automated Testing Stack using React and Redux
* Testing Stack: 
    * Testing frameworks
        * Mocha - highly configurable
            * Add Chai or Expect assertion library
                * i.e. Chai (to.equal, etc, with No Spy built-in)
                * i.e. Expect (toBe, etc, with Spy built-in)
        * Jasmine - less configurable
        * Jest - wrapper over Jasmine (from Facebook) with unique features
        * Tape - minimalistic
        * AVA - robust like Mocha, but NEW unproven.
            * Concurrent (not serial)
            * Built-in assertions
            * No globals
            * Built-in ES6
            * Runs only impacted tests on save
            * Specific errors with marker
    * Assertion libraries
    * Helper libraries
        * [React Test Utils API](https://facebook.github.io/react/docs/test-utils.html) (built by Facebook, verbose API)
            * `shallowRender` - render in Isolation a single component (without Child-level Components), no DOM required
            * `renderIntoDocument` - render Set of components with their Children,
             actually renders component into DOM using JSDom to simulate browser interaction.
             Supports use of refs and interactions (not supported yet in `shallowRender`
                * `findRenderedDOMComponentWithTag` - query specific DOM element by Tag
                * `scryRenderedDOMComponentWithTag` - find DOM elements by Tag
                * Simulate event interactions on component and assert based on outcomes 
        * [Enzyme](https://github.com/airbnb/enzyme) (PREFERRED)
            * Abstraction that calls React Test Utils API behind the scenes to provide nicer API
            * JSDOM used to provide in-memory DOM in Node.js
            * Cheerio used for fast elegant jQuery-like selectors
            * DOM interaction similar to jQuery https://github.com/airbnb/enzyme/blob/master/docs/api/shallow.md
            * `shallow` - renders one level deep
            * `mount` - renders set of components (including Child Components)
            with full DOM loaded in memory using JSDOM
* Testing Approaches
    * Browser (i.e. Karma test runner)
    * Headless Browser (i.e. PhantomJS)
    * In-memory DOM (i.e. JSDOM)
        * Simulates actual browser in-memory to interact with via Node.js

* Test Dumb Presentation Components
    * Provide Props and expect Markup
        
* Automated Unit and Integration Tests using Redux
    * Test Scope
        * Test Components connected to Redux
            * Test `mapStateToProps` by extracting out data selection or manipulation into 
            separate pure function in src/selectors that are easy to test
        * Test Redux Action Creators
            * Simple as just checking return 
        * Test Thunks
            * Requires Mock Store (using **redux-mock-store**) and HTTP Calls (using **nock**) with fake responses for testing
        * Test Reducers
            * Automate the creation of Reducer tests using "Redux Test Recorder" library 
            https://github.com/conorhastings/redux-test-recorder
        * Test Stores (and integration)
            * **Integration Test** to ensure Actions, Store, Reducers are interacting as expected
    * Test Sections
        * Test Markup
        * Test Behaviour (i.e. Components connected to Redux)

### Setup Production Build
* Goals
    * Lint and run tests
    * Bundle and minify JS and CSS
    * Generate JS and CSS SourceMaps
    * Exclude dev concerns
        * Store
            * src/store/configureStore does not require `reduxImmutableStateInvariant()` in prod
        * Webpack
            * Define global env variable `process.env.NODE_ENV`
            * Use `devtool: 'source-map'` (slower than other)
            * Remove Webpack Hot Middleware in Prod
            * Serving content from /dist folder instead of /src in Prod
            * Use different Plugins
            * Change CSS Loader
            * Create tools/build.js file to run Webpack Prod build
            * Create tools/buildHtml.js to copy src/index.html to dist/index.html
            * Serve Prod build on local machine with tools/distServer.js (including GZIP compression library)
            * Update package.json with Prod scripts
            
    * Build React in prod mode
    * Open prod build in browser
    * Reduce size of bundle.js (4.9 MB is far too large) to 120 kB
    
    * Run prod build locally with `npm run build` and go to localhost:3000

    * Further optimisations:
        * Remove babel-polyfill, toastr, jQuery, Bootstrap, and use custom React Router to reduce to ~50 kB

### Debugging

* Insert `debugger;` in code and then run in browser to trigger breakpoints (using `devtool: inline-source-map` of Webpack) 
    * Hover over variables to display info
    * Test values in console window

### IDE
* Atom - Add packages react, and terminal-plus
* IntelliJ - Configur Languages & Frameworks > JavaScript > Change to ES6 or JSX Harmony/React JSX (for React)

### IntelliJ's File and Code Template Feature
* Preferences > Editor > File and Code Templates > Files 

**React Class Component**
{% highlight javascript %}
import React, {PropTypes} from 'react';

class $NAME extends React.Component {
    render() {
    
    }
}

$NAME .propTypes = {
  // myProp: PropTypes.string.isRequired
};

export default $NAME;
{% endhighlight %}

**React Stateless Component**
{% highlight javascript %}
import React, {PropTypes} from 'react';

const $NAME = (props) => {
    return (
      //
    );
}

$NAME .propTypes = {
  // myProp: PropTypes.string.isRequired
};

export default $NAME;
{% endhighlight %}

**React Redux Container Component**
{% highlight javascript %}
import React, {PropTypes} from 'react';
import {connect} from 'react-redux';
import {bindActionCreators} from 'redux';

class $NAME extends React.Component {
  constructor(props, context) {
    super(props, context);
  }
  
  render() {
    return (
      //
    );
  }
}

$NAME .propTypes = {
  // myProp: PropTypes.string.isRequired
};


function mapStateToProps(state, ownProps) {
  // debugger;
  return {
    state: state
  };
}

function mapDispatchToProps(dispatch) {
  return {
    actions: bindActionCreators(actions, dispatch)
  };
}

export default connect(mapStateToProps, mapDispatchToProps)($NAME);
{% endhighlight %}

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

### Reducers Handling of Data changes
* Store changes by Dispatch an Action handled by Reducer
* Pure Functions
    * Return same value when called with same set of arguments
    * Note: Return value depends only on value of parameters with no side-effects
* Forbidden in Reducers
    * Mutation of arguments
    * Perform API calls and routing transitions
    * Call non-Pure Functions (i.e. date.now, Math.random)
* Single Store
    * Single Reducer slices State changes into multiple smaller Reducers
    (for separate Data Domains)
    * Avoids multiple Stores where have to wait for others
* All Reducers are called on each Dispatch from Single Store.
Switch statements inside each Reducer function checks if the passed slice of State should
be handled by that specific Reducer
(so important that Reducers return untouched State as default if no switch
case matches, as those that do not match simply return the state passed to them)
* Multiple Reducers allows handling different pieces of the Store in isolation
* "Reducer Composition" is where each Action may be handled by Multiple Reducers responsible for updating specific slices of State

{% highlight javascript %}
// WRONG
function myReducer(state, action) {
  // return new State based on given existing State and Action
  switch(action.type) {
    case 'INCREMENT_COUNTER':
      state.counter++; // WRONG - DO NOT MUTATE STATE OR PRODUCE SIDE-EFFECTS!!
      return state;
  }
}
{% endhighlight %}

{% highlight javascript %}
// RIGHT
// Deep clones State object with counter incremented by one
function myReducer(state, action) {
  // return new State based on given existing State and Action
  switch(action.type) {
    case 'INCREMENT_COUNTER':         // RIGHT - DEEP CLONE WITH PURE FUNCTION
      return (Object.assign(
        {},                           // Target object
        state,                        // Mix new object with existing State
        {counter: state.counter + 1}  // Change counter property
      ));
  }
}
{% endhighlight %}

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
    * Action Types (constants may be used to prevent typos)
        * Store constants in src/actions/actionTypes.js
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

### React-Redux Library
* react-redux Library connects React Components to Redux
* react-redux Library is companion library for Redux 
* react-redux Library connects React Router Container Components to Redux
* react-redux Library used between Store/Reducers and React Components
* React-Redux generates Container Components
(React Components that use `store.subscribe` to read part of
Redux state tree and supply Props to Child Components)
* React-Redux library comprises:
    * Provider Component - Attaches React App to Store
        * Wraps entire app at root top-level Component, only used once when root renders.
        Makes Redux Store available to all Container Components automatically
        (using React context)
        Attaches app to Redux Store
        (otherwise would have to explicitly pass Store to all Components requiring it individually)
    * Note: React context is a feature for Library authors (not developers)
{% highlight javascript %}
<Provider store={this.props.store}>
  <App/>
</Provider>
{% endhighlight %}

* Cont'd
    * Connect function - Creates Container Component
        * Wraps Component so connected to Redux Store 
        (alternatively in Flux we manually add `addChangeListener/removeChangeListener(this.onChange` 
        and `onChange() { this.setState({ skills: SkillStore.getAll...` to each Store inside `componentWillMount`) 
        * Declare part of Redux Store to attach to Component as Props
        * Declare Actions to expose on Props
        * Creates container Components automatically
        * Redux `connect` Benefits:
            * Not need manual subscribe/unsubscribe to Store, as `connect` does it automatically
            * Not need Lifecycle Methods to subscribe/unsubscribe to Store in Redux, 
            as Redux allows all to be Stateless "functional" Components (avoiding `this` keyword confusion), 
            whereas Flux requires use of `componentWillMount` and `componentWillUnmount` so cannot use React Stateless 
            "functional" Components for Container Components when working in Flux (since Stateless ones do 
            not have Lifecycle Methods)
            * Redux allows explicitly exposing subsets of State to Container Component (performance improvements
            as only renders when specific data that is connected changes, saving unnecessary renders or manual
            suppression of them using `shouldComponentUpdate`),
            whereas in Flux after wire `onChange` listener to Store then entire Store data is exposed
        * `mapStateToProps` function passed to `connect` function specifies State to expose to Component as Props,
        causing Component to subscribe to Redux Store and gets called when Store is updated. This function returns
        and object whose properties become available as Props of Container Component
            * Filter/transform/sort State here ready for use in Container Component
            * Prepare data (transform) to prepopulate forms
        * `mapDispatchToProps` function is passed to `connect` function the Actions to expose to Component.
        * `mapStateToProps` function is called every time Component is called
            * Use **Reselect** Library to increase performance if any expensive operations (i.e.
            change list, calculations, etc)
            being performed in `mapStateToProps`.
            It memoizes (caching for function calls) to keep track of results of each function call
            to increase performance, so if function called again with same parameters it just returns 
            memoized value and but doesn't need call function again
        * `mapDispatchToProps` function allows specify what Actions to expose at Props.
        See section "3x Ways to Handle passing Actions to Props in Redux Container Components"  
            
{% highlight javascript %}
function mapStateToProps(state, ownProps) {
    return {appState: state.skillReducer};
}

// Optional parameters
export default connect(
  mapStateToProps,
  mapDispatchToProps
)(SkillPage);
{% endhighlight %}

* Cont'd
    * Cont'd
        * Example 1: Simple app w 1x Container Component and 1x Reducer
            * Pass down all State to Presenter Component via Props
            * Expose only part of Store's State by specifying within return block
             
{% highlight javascript %}
function mapStateToProps(state) {
    return {
        // Expose only part of Store's State by specifying here
        appState: state
    };
}

// In React Container Component to access any State handled by appState Reducer
this.props.appState
{% endhighlight %}         
       
* Cont'd
    * Cont'd
        * Example 2: Complex
            * Create Multiple Container Components (to manage sections of app)
            * Create different Reducers to handle diff slices of Stores

### 3x Ways to Handle passing Actions to Props in Redux Container Components (i.e. for use with `mapDispatchToProps`)
* 1) Use Dispatch Directly (not use `mapDispatchToProps`). 
`dispatch` Prop may be manually called and passed an ActionCreator `this.props.dispatch(loadSkills());`
so the `dispatch` Prop calls the ActionCreators.

Ignore adding `mapDispatchToProps` (Optional Param) as param of `connect`
that automatically causes `dispatch` function to attach itself to React Container Component.

Disadvantages: Extra boilerplate explicitly calling `dispatch` each time that has been passed
an Action to fire off; Child Components become tied to Redux concepts (i.e. dispatch function
and ActionCreators)

* 2) Manually wrap within `mapDispatchToProps` our calls to `ActionCreator` and `dispatch`
(results in more code within `mapDispatchToProps` than in the React Container Component).
Explicitly specify Actions to expose to React Container Component (each ActionCreator wrapped
in a `dispatch` call)
{% highlight javascript %}
function mapDispatchToProps(dispatch) {
    return {
        loadSkills: () => {
            dispatch(loadSkills());     // i.e. loadSkills() ActionCreator wrapped in function that calls dispatch
        }
    };
}
{% endhighlight %}  

Example:

{% highlight javascript %}
function mapDispatchToProps(dispatch) {
    return {
        loadSkills: () => {
            dispatch(loadSkills());
        },
        createSkill: (skill) => {
            dispatch(createSkill(skill));
        },
        updateSkill: (skill) => {
            dispatch(updateSkill(skill));
        }
    };
}

// In Component to access any State handled by loadSkills Reducer
this.props.loadSkills();
{% endhighlight %}

* 3) Use `bindActionCreators` convenience function that wraps ActionCreators passed to it
in dispatch calls for us (saving us having to do it manually like in Option 2) so easy to pass
down to Child Componets
(does Option 2 approach to expose Actions to React Container Components, but automatically)

{% highlight javascript %}
// Parameter accepted is dispatch
// Returns callback Props we want to pass down using Redux's bindActionCreators function
function mapDispatchToProps(dispatch) {
    return {
        actions: bindActionCreators(actions, dispatch)
    };
}

// In Component to access any State handled by actions Reducer
this.props.actions.loadSkills();
{% endhighlight %} 


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

### Container Component vs Presentation Component (two types of React Components)

* Container (Parent/Smart/Stateful/Controller View) (back-end of the front-end)
    * Container Components are easily generated by react-redux Library
    * Container Components use React Store.subscribe to read part of Redux State tree
    and supply Props to Child Components (when generated with react-redux Library additional 
    performance optimisations are included)
    * Behaviour 
    * Marshalling Data and passing to Child Components
    * Actions passed to Child Components
    * Stateful (manage State)
    * Created using Redux connect
    * Subscribe to Redux State
    * Dispatch Redux Actions to Store (and connecting to it with connect)
    * Aware of Redux
    * No Markup
    * Note: May have multiple Container Component levels

* Presentational (Child/Dumb/Stateless/View)
    * Dumb (no logic)
    * Receive and read Data and Actions functions via Props to display UI
    using render function that defines markup
    * Fire Actions by invoking callbacks passed down to them via Props 
    (so not tied to specific Behaviour)
    * Stateless functional components (written by hand)
    * No dependency/awareness of Redux, Actions, or Stores (easier to reuse)
    * Not specify how Data loaded or mutated
    * Markup

### Async in Redux
* i.e. AJAX calls to server to load data via an API on page load

* Mock API Pattern for client-side apps
    * Rule of coding to Interface over an Implementation
    * Benefits
        * Not reliant on other devs or finalised API to build UI
        * RAD with fast responses (may mask performance issues but we 
        test against actual API before deployment)
        * Test how UI performs when calls to API are slow/fast
        by using `setTimeout` in Mock API to delay responses

* Flux vs Redux
    * Flux handles Async API calls in Action 
    * Redux handles Async API calls using Redux Async Middleware Library such as:
    Note: Redux Actions are Synchronous and return an object
        * **redux-thunk**
            * Return Functions (instead of Objects) from Action Creators
            * **Thunk** wraps async operation in a function
                * a function that returns a function
                * a function that wraps an expression to delay its 
                evaluation
                * a thunk always returns a function that accepts dispatch as parameter
            * Difficult Testing as must use Mock API without easy hooks
            to observe individual steps in async flow
            * Easy to learn
            

{% highlight javascript %}
// i.e. deleteSkill wraps dispatch function so it runs later
export function deleteSkill(skillId) {
    return dispatch => {
        return SkillApi.deleteSkill(skillId).then(() => {
            // Call Action Creator (deleteSkill)
            dispatch(deleteSkill(skillId));
        }).catch(handleError);
    }
}
{% endhighlight %}
            
* Cont'd
     * Cont'd
        * **redux-saga**
            * ES6 Generators used along with an async DSL
            * Generators handle async operations
            * Generators are functions that may be paused/resumed later
            and contain multiple `yield` statements (where generator 
            pauses)
            * Testing easy as can `assert` on effects that return data
            (no Mocking required)
            * Hard to learn and large API and need to understand possible errors
        * redux-promise (less popular)
            * Flux standard Actions/promises used to provide clear conventions
            to async calls
* Async Libraries for async flows using Redux

* Thunk to implement Redux flows

### TODO

* [Learn ES2015(ES6)](https://babeljs.io/docs/learn-es2015/)
* [ES6 Features](https://github.com/lukehoban/es6features#readme)
