---
layout: post
title: Building Apps with React and Flux (by Cory House from Pluralsight) (DONE)
---

* Cory House @housecor

# Table of Contents
  * [Chapter 1 - Topics](#chapter-1)
  * [Chapter 2 - Versions Used](#chapter-2)
  * [Chapter 3 - Setup Dev Environment](#chapter-3)
  * [Chapter 4 - Build App with Custom Routes](#chapter-4)
  * [Chapter 5 - Dynamic Components](#chapter-5)
  * [Chapter 6 - Composition](#chapter-6)
  * [Chapter 7 - Flux Development](#chapter-7)

## Chapter 1 - Topics<a id="chapter-1"></a>

### Node.js

* Node.js Usage:
    * Server-side JS using Google V8 engine

* Node.js CommonJS Pattern:
    * Pattern of encapsulating JS into reusable modules for referencing and importing (differs from AMDs RequireJS)

        {% highlight javascript %}
        var dependency = require('/path/to/file')
        var MyModule = { ... }; // declare
        module.exports = MyModule; // bundle and export module context to expose module so other modules may reference it using require statement
        {% endhighlight %}

### Gulp
* Gulp Usage:
    * Builds task runner script to easily automate builds, watch files, and automatically reload
    * Stream-based using outputs from plugins as inputs for others
    * Performs in-memory so faster

### Browserify 
* Browserify Usage:
    * Bundler to bundles NPM package (Node.js library modules) dependencies into single file (minimising HTTP requests to load app) and exposes them to the browser

### React Topics, Patterns, and Approaches
* React Usage:
    * View engine that uses Web Components library from Facebook for packaging, composing, and rendering HTML Components that ignores traditional best practices

* React Stances:
    * HTML is projection of app state
    * App is function of Props and State
    * DOM automatically reflects encapsulated State of Data, but does not store anything
    * Virtual DOM so fast as compares Old State with New State in memory when UI changes and updates DOM with least expense
    * JS and HTML belong in same file (exception to Separation of Concerns as not strong-typed interface to keep in sync)
    * Note: In Angular.js JS is used in HTML too (i.e. `<div ng-repeat="">`) by using proprietary DSL that JS uses to parse HTML 
    * Avoids traditional requirement to redraw UI when JS changes by not storing data in DOM/JS
    * Inline Styles (dynamic, deterministic, component-specific) encouraged to increase maintainability (avoid monolithic style sheets)
    * View Layer only (no opinions on data flows, routing, builds, deployments)
    * Plug into other technologies
    * React.js and Flux have preference for clarity and scalability over convention and minimising typing (framework trade-off)
* JSX
    * Optional JSX markup language (abstraction to plain JS) that compiles down to JS for interpretation by browser
    * JSX is preferred over plain JS as it appears like final HTML (easier to read/write)

{% highlight javascript linenos %}
var R = require('r');
var A = React.createClass({
    render: function() {
        return (
            // JSX (appears like HTML but compiles to JS) compiles down to:
            // React.createElement("hi", {color: "red"}, 
            //   React.createElement("span", {className: "boo"},
            //     React.createElement("i", null, {this.props.bar})
            //   )
            // )
            //
            // Note: Last element contains calls to other elements when nested markup used
            <h1 color="red"><span class="boo"><i>{this.props.bar}</i></span></hi>
        );
    }
});
module.export = A;
{% endhighlight %}

* ...Continued
    * JSX acknowledges the tight coupling of HTML and JS and allows us to compose HTML using JSX but with ease of debugging as described below
    * JSX uses opposite mindset of other approaches like in Angular.js where they use a proprietary DSL that JS uses to parse HTML
    * Instead React defines HTML markup in JS with the optional help of using JSX (instead of like other frameworks where they do it the other way around and try to enhance HTML with proprietary binding systems)
    * Note: HTML and JS should be kept in sync manually as there is no interface between them. HTML is not strictly parsed like JS so may be the cause of silent failure and hard to debug errors (if any)
    * Note: JSX is preferred (**Fail Fast Fail Loudly**) over enhanced HTML since in case of error it shows line number (since it is JS)

* Virtual DOM
    * Virtual DOM is an abstraction over the DOM that performs in-memory comparisons. React monitors the value of each component's state using fast memory
    * Efficiency is important with mobile web and to consume batteries using less CPU
    * Less expensive DOM updates for performance and efficiency. Avoids **browser thrashing** where everything updates due to small change
    * Save time by comparing current DOM state with desired future DOM state and choosing most efficient way to update it without an expensive redraw using up the CPU
    * Clear separation between Data and DOM. DOM is representation of current State so easier to understand and test
* Performance
    * Request not to update DOM at all `shouldComponentUpdate` even when certain data changes
    * Use faster immutable data structures `PureRenderMixin + immutability` (they cannot be changed, and instead a fresh copy made). React is faster with this as can do reference comparisons.
    * Synthetic Events to abstract away browser specific event problems allowing React to optimise performance of attaching event handlers behind the scenes (i.e. when attach event handlers to each row in a table, React will attach the event handler at the highest abstraction for efficiency) 
* Isomorphic Rendering
    * Enabled by the Virtual DOM
    * Components may be rendered on client and server (not DOM oriented like Angular.js)
    * DRYer code (avoids repetition across client and server
    * SEO simplified
* Components (decompose into multiple nested components)
    * Component-based app solution that is powerful and flexible
    * React Components used to build rich Web UIs through composition of small components
* Lifecycle (of features)
* Composition (parent-child components)
    * Pass data down to child components via props (similar to HTML attributes)
    * Composition involves breaking page down into Controller View Pattern
    * Simple composition of multiple components easy to scale
* React Router (client-side routing library by Facebook)
    * Split app into multiple client rendered pages with deep linking
    * Nested views map to nested routes that are declared
    * Scalable and elegant architecture for handling routing in app without reliance on ad-hoc hacked solutions
* React Router Config
    * Route Configuration declaring and maps a routes for each page in app using route components
    * Alias React Router's `DefaultRoute` Component for loading root of app
    * Alias React Router's `NotFoundRoute` Component for handling client-side 404 not found pages
    * Alias React Router's `Redirect` Component to programmatically redirect to another route so old URLs (bad references) 
    that have changed still work, or to catch typos in URL. `from` can have value `*` or subdirectory redirects `skills/*`
* Centralised Header
    * Compose the header in the top level component

{% highlight javascript %}
// Alias React Router's Redirect Component
var Redirect = Router.Redirect;

// Create new route
<Redirect from="old-path" to="new-path-name" />
{% endhighlight %}

* ...Continued
    * Centralised Routes file `routes.js`
    * Optionally display custom route URL using `path` in child for /skills-url (otherwise would just be /skills)

{% highlight javascript %}
<Route name="app" path="/" handler="{require('./component/app)}">
    ...
    <Route name="skills" path="skills-url" ...>
    ...
{% endhighlight %}

* React Router - Parameters and Querystrings
    * React Router automatically adds data to Props under Params and Query
    
    * Example with placeholder "skillId"

{% highlight javascript %}
// Route
<route path="/skill/:skillId" handler={Skill} />

// URL with Querystring of 'module' and a Value of '1'
'/skill/angular-101?module=1'

// Retrieve data from route and populate and make accessible in Component Props as follows:
var Skill = React.createClass({
    render: function() {
        this.props.params.skillId;  // "module"
        this.props.query.module;    // "1"
        this.props.path;            // "/skill/angular-101/?module=1"
    }
});
{% endhighlight %}

* React Router - Link Component
    * Link Component allow reuse of routes declared in Route Config (central location) and prevent breaking when change a link
    * Links are an Abstraction alternative to just using href anchors
    * Links normalises links by abstracting hrefs and instead allowing you to reference routes by name
    * Links allow reuse defined links to keep codebase DRY 
    * Benefit of using Link Component of React Router is it avoids full post-back and changing links no longer produce flicker
    
    * Example

{% highlight javascript %}
// Create route using placeholder for userId and attribute "name" hook to reference the route in links
<route name="skill" path="/skill/:skillId" />

/**
 *  Create link using JSX pointing to specific resource URL route: /skill/1
 *  Link Component from React Router is used where attribute "to" is set to name to link to,
 *  and attribute "params" for defining JSON object of data for the URL, where we set properties
 *  corresponding to placeholders (i.e. skillId) set on the route.
 *  Note: Below JSX is compiled into HTML: <a href="/skill/1">React.js</a>
 */
<Link to="skill" params={{skillId: 1}}>React.js</Link>
{% endhighlight %}

* React Router - Handling Transitions between Components and Pages
    * Run static functions before loading/unloading
        
{% highlight javascript %}
// Timing: Check rules and determine if should transition to page
// Usage: Check if user authenticated
willTransitionTo

// Timing: Check before user navigates away
// Usage: Validating if a form has unsafe/incomplete data and prompt user before navigate away and lose work
willTransitionFrom
{% endhighlight %}

* Client-Side Routing Approaches 
    * Hash-Based Routing Location Style
        * Works in all browsers, ugly URLs with hashbang
        * Incompatible with server-rendering
        * i.e. xyz.com/#/abc

    * HTML5 History API (Push State Routing) URL Locations
        * Only works in IE10+, not Opera, provides clean URLs
        * Server-rendering compatible
        * via HTML5 States for URLs to change history: Push (pushing into browser state), Replace, and Pop
        * i.e. xyz.com/abc
        * Enable with the following, and by configuring Server to support clean URLs 
        so all URL requests are redirected to client-side index page so React Router can handle it:

{% highlight javascript %}
Router.run(routes, Router.HistoryLocation, ...
{% endhighlight %}

* React Router - Navigation Mixin
    * Enables moving between routes programmatically
    * Examples:

{% highlight javascript %}
// Go to new route
this.transitionTo('skills')

// Replace current route
this.replaceWith('skills')

// Go back
this.goBack

// Create URL to route
makePath(routeName, params, query)
{% endhighlight %}

* Forms (adding/editing data, input changes, validations, warnings/errors)
    * Virtual DOM used for robust and responsive forms
    * Manage data for a Component by creating a file (i.e. `skillManager.js`)
    * Convert `skillManager.js` into Controller View "Smart" that marshals data, 
    validate input, passing data down to child component `skillForm.js`

    * Common Issues - Resolve using Controlled Components (using Change Event Handlers)
        * Note: **Controlled Components** require a Change Event Handler so user input registers
        * Note: **Uncontrolled Components** are those that do not set a value
        
        * Problem: Virtual DOM prevents entry into input field of form
        * Cause: React redraws UI on each request animation frame, so we need to assign a value
        to each input field and also explicitly attach change event handlers to input fields so 
        their value may change (to prevent React ignoring entry into input)
        * Solution Step 1: Define initial State in `skillManager.js` to contain object called skill
        * Solution Step 2: Pass the Initial Form State to Child Form via Props
        * Solution Step 3: Check Props in `skillForm.js` and update the form utilising this data
        * Solution Step 4: Reference Props passed down in the input value `<input value={this.props.skill.skillName}>`
        so the text input is bound to the skillName Prop (property)
        * Solution Step 5: Setup Change Handlers for Inputs in Top-Level Component `skillManager.js` that handles each key presses
        and changing the State by calling `setSkillState` function. Input Field Change Handler takes event references bubbled up from Child
        Component then Updates Field State that was passed with data value, and lastly
        call setState to update State of the Skill Object to the updated Field State. We first need to pass reference of
        State to the Child Component along with an Event Change Handler onChange.
        In the Child Component we need to use the onChange handler passed down to it from Parent Component
        and attach it to associated input fields. Upon change the event bubbles up to Parent Component,
        which calls setSkillState event handler to update State

    * Common Errors
        * Problem: Adjacent JSX elements must be wrapped in an enclosing tag

{% highlight javascript %}
var SkillManager = React.createClass({
    render: function () {
        return (
            <h1>Manage Skills</h1>
            {/* Controller View calls Child Form */}
            <SkillForm />
        );
    }
});
{% endhighlight %}

* ...Continued
    * ...Continued
        * Solution: Only single Top-Level Component function call allowed since JSX compiles to JS.
        Either wrap multiple given components in a <div> or move one elsewhere

    * Form Validation of Inputs Fields
        * Create method `skillFormIsValid()` and apply in `saveSkill` method
        * Pass list of errors from Parent State down to the Child Component using a `errors` Prop
        * Wire up inputs in Child Component to accept the string for the respective `errors` Prop passed down

    * Form Validation of Data send to Form Component
        * propTypes used to make expectations clear and overcome having to make Assumptions about data it will receive

    * Form Redirects
        * Use mixin React.Navigation to programmatically redirect the user to a page after saving the form
        * Transition to a different route using `transitionTo('skills')`

    * Form Reusable Inputs

        * Justification
            * `<input ... ref="..">` is reference to a Child Component
            that is usually passed to React.findDOMNode to get reference to Component
            * Repeated markup, especially when wrap `div`'s around input fields, and 
            include placeholder for displaying inline error message

        * Create Reusable Text Input Component (centralised handling of markup complexity)
            * Create `textInput.js` as Centralised and Reusable abstraction config that enforces conventions and consistency that saves a 
            lot of typing for handling decisions relating to Text Input. The JSX markup config is passed down for 
            Component UIs
            * Create function to dynamically create a wrapper className for the outer div using Bootstrap classes
            * When creating reusable components to be used by many people we must always define propTypes
            so developers know what data to expect to be passed down to Child Components by providing warnings as reminders

    * Form User Notifications
        * Use Toaster library

    * Form Saving and Pre-Populating upon Load
        * Create a function in `skillManage.js` to Save form fields. `saveSkill` uses the Mock API 
        and accepts event parameter passed up from Child Component and passes down this saveSkill function 
        to Child Component in render call using onSave (no DB is being hit and no AJAX calls are made to a server)
        * `skillForm.js` updated by adding an onClick Hander for the input field that references the
        `saveSkill` function that is passed down so it may be called
        * Benefit of having Separation of Concerns between Control View and Form that just handles the markup

    * Form Transitions without losing unsaved data
        * Define statics `willTransitionForm` method and check if the State is dirty 
        (i.e. not saved yet)
        * Initialise State of the form in `getInitialState`, check it in `willTransitionTo`,
        and change it in `setSkillState`

    * Form Editing data
        * Currently clicking the name of a skill in the list of skills takes user to 404
        * We need an `editSkill` Edit route
        * We want form to pre-populate with data depending on what skill is being edited.
        To hydrate form we use lifecycle method `componentWillMount` (so rendering will not happen twice) to set State before rending occurs

### Flux (actions, dispatchers, stores)

* Problem: MVC works for small apps but does not make room for new features

* Problem Definition: Client-side app requiring various interactions b/w ViewModels is tricky to debug:

[MVC] Action => Controller => Model  \   View
                          \=> Model  /   View
                          \=> Model  -   View
* Example: Two-way binding

ViewModel <=> View

* Example: Unidirectional Data Flow (Actions flow in single direction from it to the View.
 View does not directly update State (whereas in Two-way binding it does), instead
 Actions are fired that eventually update the State.
Upon Action occurrence from user interaction with UI, then Unidirectional Data Flow commences.
A Dispatcher notifies Stores that registered with it that Action occurred. When Store changes 
the React Component and UI are updated.
Additional concepts and code are required for UDF but with benefits of clarity, testability, and predictability

Action => Dispatcher => Store => React View

* Trade-off is that Two-way binding is conceptually simpler and requires less typing, whereas 
UDF is more scalable due to being more explicit and it is easier to update multiple Stores for a given Action


### Flux (unidirectional Data Flow handling)
* Definition: Facebook's Flux branding for architectural Design Pattern implementation of Unidirectional Data Flow handling is easy to track changes
* Instead of MVC it uses "Controller Views" (**React Views** that handle data concerns and compose other child components similar to Controllers in MVC) and UDF 
so large apps easier to predict and reason about by avoiding traditional complex interactions between View and View Model
because results of a given Action are easy to trace from the Action to Dispatcher to Store to React View 
* **React Views** promote Code Reuse and Separation of Concern
* Centralised dispatcher handles data flows in single direction to easily update Data Stores as app changes (not two-way binding)
* Avoids unpredictable risks of two-way binding (which is normally used to overcome boilerplate) when multiple developers without careful app design
* Flux overcomes maintainability issues caused by making data calls from React Controllers
* Unidirectional Data Flow requires use of Flux Dispatcher and a JavaScript Event Library
* Flux deals with Actions and Data Changes

### Flux Key Concepts
* Actions
    * Triggered by View calling appropriate Action due to UI interaction
    * Triggered by Server (i.e. page load, errors during server calls)
    * Actions register Action Creators with Dispatcher so it will notify call Stores that care to have registered a callback with the Dispatcher
    * Action Creators (i.e. CreatSkill, etc) are Dispatcher Helper Methods that describe all actions possible in the app
    * Action Creators are grouped in files of related Actions
    * Action Creator methods adds a Type (stored in a Constants file) that is used by Dispatcher to handle the Action
    and pass updates to related Stores
    * Actions encapsulate events describing UI interactions occurring in React Components of an app.
    * Actions update specific Stores based on the callbacks registered with the Dispatcher that 
    send the Action Payloads
* Dispatchers
    * Centralised singleton (only one per app) holds list of callbacks to all Stores that want to be notified when a given Action occurs
    and dispatches the Actions
    * Centralised hub where multiple Stores may request updates upon occurrence of given Actions (predictable data flow) 
    * Exposes method that allows Actions to triggering dispatch to Stores including the Action Payload 
    * Action Payloads have common structure of Type and Data i.e. below triggered when skill saved

{% highlight javascript %}
{
    type: SKILL_SAVED
    data: {
        skillCategory: 'Back End',
        skillName: 'Python'
    }
}
{% endhighlight %}

* ...Continued
    * Action Payload Type informs Store what Action occurred
    * Dispatcher invokes callbacks registered with it and broadcasts the Action Payload received 
    from the Action to relevant Stores
    * Dispatcher transfers messages from UI Views to Stores

* Stores (Data Layer)
    * Flux Stores only interact with Controller Views (top-level Parent Components), not Child Components
    * Flux Stores find out about Flux Actions by registering a callback with the Dispatcher
    * Flux Stores are the only part of app that should know how to update Data
    * Stores hold app State, and have app State, Logic, and Data Retrieval Methods
    * Stores register callbacks with the Dispatcher so they will be notified of data changes
    * Stores receive Action Payloads that are sent from Dispatcher
    * Stores update State with Action Payload and fire an EventEmitter to broadcast change event so React Components re-render the UI
    * Stores actually contain Models within
    * Stores have No direct Setter methods 
    (they only accept updates via callbacks registered with Dispatcher)
    * Multiple Stores may be created to group related data, i.e.
    
    Dispatcher -> Payload -> User Store
                         |
                          -> Skill Store
    
    * Only Stores may register callbacks with the Dispatcher (other React Components should not do this)
    * Stores are extended with Node's EventEmitter to Listen and Broadcast events and update
    React Components based on the events

* Dispatchers and Stores in Large Scale Apps
    * https://facebook.github.io/flux/docs/overview.html
    * Dispatcher may manage Dependencies between Stores in larger apps
    by invoking registered callbacks in specific order
    * Stores may declare to use the Flux method `Dispatcher.waitFor` and wait for other Stores to finish updating before updating themselves

* Store Structure
    * Common Interface functions that are always implemented

{% highlight javascript %}
Extend EventEmitter     // Allows Store to emit events to React Components advising data changed

addChangeListener       // Expose methods to inform React Component when Store changes
removeChangeListener

emitChange              // Emit the change that occurred to the Store
{% endhighlight %}

* React Components
    * Stores emit changes that are Broadcast using Node's EventEmitter
    * React Components Listen to Stores to find out when app State has changed
    * React Components that use the Store data that changed are re-drawn upon State changes

* Example:
    * Centralised Dispatcher handles UI interactions (singleton registry which has a list of callbacks).
    * Dispatcher makes calls to notify Stores.
    * Stores hold app state (data). Store updates are reflected in React Views (Unidirectional Flow). 
    * Note: UI never updates Store data directly (only through Stores). 
    * UI changes are rendered due to changes in the Store

### Constants File
* Centralised hub to maintain organisation with high-level view of app (similar to Enumerations)
* Instead of having to type Key/Value, pass to React "Key Mirror" library (automatically copies key to the value)

* WITHOUT using React "Key Mirror" library
{% highlight javascript %}
"use strict";

module.exports = {

    // Define list
    CREATE_SKILL: CREATE_SKILL
};
{% endhighlight %}

* WITH using React "Key Mirror" library
{% highlight javascript %}
"use strict";

var keyMirror = require('react/lib/keyMirror');

module.exports = keyMirror({

    // Define list
    CREATE_SKILL: null
});
{% endhighlight %}

### Flux Alternative Implementations (often at expensive of clarity or flexibility than Facebook Flux)
* Alt, Reflux, Flummox, Marty, Fluxxor, Delorean, Redux, Nuclear, Fluxible

### Flux API Functions
* `register` function accepts a callback we want the Dispatcher to call for a given Action
* `unregister` function accepts a string id of callback to not call when given Action occurs
* `waitFor` function accepts an array of string ids and allows updating Stores in specific order, 
by specifying callbacks that must be completed before continuing execution of current callback.
Pass Dispatch Token of callback to wait for to the `waitFor` function
* `dispatch` function accepts an Action Payload and is how Action informs Dispatcher of an occurrence 
that it should send to the Stores, where the Action Payload contains Action Type and Data properties.
It then dispatches the payload to all registered callbacks
* `isDispatching()` is boolean when Dispatcher busy dispatching callbacks

### Flux vs Pub-Sub Design Pattern Differences
* All Action Payloads are dispatched to call registered callbacks, 
so callbacks are not subscribed to specific events
* Callbacks may be deferred using `Dispatcher.waitFor` in whole or part until other callbacks finish executing
(ensures certain Stores of data updated before others)

### Jest
* Wrapper over Jasmine

## Chapter 2 - Versions Used<a id="chapter-2"></a>

* React 0.13.3
* React Router 0.13.3
* Flux 2.0.3

### TODO
* Update to later versions according to Change Logs
* Update and transpile from ES6 (ES2015) to ES5 using Babel

### CDN Bootstrap and jQuery
http://stackoverflow.com/questions/18474564/bootstrap-3-navbar-with-logo
http://stackoverflow.com/questions/12458522/bootstrap-dropdown-not-working

## Chapter 3 - Setup Dev Environment<a id="chapter-3"></a>

* Clone repo [https://github.com/coryhouse/react-flux-starter-kit](https://github.com/coryhouse/react-flux-starter-kit)
* Compile React JSX, ESLint JSX and JS, bundle and migrate JS/CSS files to dist folder, run/open web server in browser, live reload

{% highlight bash %}
node -v
nvm use 7.1
npm init
npm install --save gulp@3.9.0 gulp-connect@2.2.0 gulp-open@1.0.0
npm install --save browserify@11.0.1 reactify@1.1.1 vinyl-source-stream@1.1.0
npm install --save bootstrap@3.3.5 jquery@2.1.4 gulp-concat@2.6.0
npm install --save gulp-eslint@0.15.0
npm install --save react@0.13.3 react-router@0.13.3 flux@2.0.3
npm install --save gulp-rev-append gulp-sass gulp-sourcemaps gulp-autoprefixer
npm install --save lodash
npm install --save toastr@2.1.0
npm install --save object-assign
cat package.json
mkdir src && mkdir dist
touch gulpfile.js
gulp
{% endhighlight %}

* Use IDE with JSX syntax highlighting

## Chapter 4 - Build App with Custom Routes<a id="chapter-4"></a>

### Custom Routing for SPA

* Single Component Routes - Hard Coded

{% highlight javascript %}
var React = require('react');
var Home = require('./components/homePage');
React.render(<Home />, document.getElementById('app'));
{% endhighlight %}

* Multiple Component Routes - Abstraction that interprets URLs for Client-side Navigation (without React Router)
    
{% highlight javascript %}
"use strict";

// IIFE to overcome ESLint warnings and prevent jQuery undefined browser error
(function(win) {
    return function () { win.$ = win.jQuery = require('jquery'); };
}(window));

var React = require('react');
var Home = require('./components/homePage');
var About = require('./components/about/aboutPage');

// Track child routes and render depending on URL. Check properties and route for the app. Render appropriate markup
var App = React.createClass({
   render: function () {
       var Child;

       switch(this.props.route) {
           case 'about': Child = About; break;
           default: Child = Home;
       }

       return (
           <div>
               <Child />
           </div>
       );

   }
});

// Define render function for route change
function render() {
    console.log("New Route: ", window.location.hash);
    var route = window.location.hash.substr(1);
    React.render(
                    <App route={route} />,
                    document.getElementById('app')
                );
}

// Watch for hash change in URL when rendering (i.e. localhost:9005/#about)
window.addEventListener('hashchange', render);
render();
{% endhighlight %}

### React Naming Conventions

* AbcXyz.react.js OR abcXyz.js
    * Syntax highlighting automatic by IDEs and recognised by OS
    * `require('abcXyz')` statements expect .js files. 
    * Facebook themselves use the .js extension for files containing JSX

* AbcXyz.jsx
    * Must explicitly state extension in `require('AbcXyz.jsx')`, as they expect .js files. 

## Chapter 5 - Dynamic Components<a id="chapter-5"></a>

### Data
* Data for React Components stored in Props (properties) and State

    * Props
        * Allow for passing Data to Child Components
        * **Immutable** as owned by parent who sets the Props and passes them down to the children
        * Similar to HTML attributes
            `this.props.myVar`
            
            * Declare method to define default properties for component. Returns set of properties for component to use
            by default in case the Parent Component does not declare a value
            
            `getDefaultProps`

    * State
        * **Mutable** to hold Mutable State
        * Try to only use State on Controller Views (i.e. only on top-level Component and pass Data down to Child Components via Props)
        
        `this.state.myVar`
        
        * Declare function to define initial state in Controller View (top-level Component)
        
        `getInitialState`

    * Methods to hook into a React Component's Lifecycle
        * Note: Despite React being isomorphic, only some hooks work on both Server and Client

{% highlight javascript %}
// Usage: Set initial state
// Timing: Runs before initial render (both Client and Server)
componentWillMount

// Usage: Access DOM, integrate frameworks, set timers, AJAX requests
// Timing: After rendered component in DOM
componentDidMount

// Usage: Set State before render as runs just before new Props received
// Limitations: Not called on initial render
// Timing: Just before component receives new Props (i.e. Props change)
componentWillReceiveProps

// Usage: For performance, as can return false from this function to avoid unnecessary re-renders
(since some new Props/State changes not affect DOM so component shouldn't need re-render)
// Limitations: Not called on initial render
// Timing: Just before render when new Props or State being received by component
shouldComponentUpdate

// Usage: Prepare for updates
// Limitations: Cannot call setState from this function. Not called on initial render
// Timing: Runs just before rendering when new Props/State being received
componentWillUpdate

// Usage: Allows operate on DOM just after component updated and re-rendered in DOM
// Limitations: Not called on initial render
// Timing: Runs just after component's updates are flushed to DOM
componentDidUpdate

// Usage: Cleanup and resources or DOM elements created when DOM was mounted
// Timing: Runs just after component removed from DOM
componentWillUnmount
{% endhighlight %}

* ...Continued
    * Dynamic Child Components
        * Assigning keys when creating multiple Child Components dynamically
        * React uses the key to ensure Child Components are re-ordered or destroyed when Child Components added, modified or removed
        * Key may be primary key for database record, or just the id declared for the specific record

{% highlight javascript %}
<tr key={location.id}>
{% endhighlight %}

### Lifecycle Functions
* Hooks to initialise components and attach behaviours at points in time

### Mock API
* Create folder /src/api as centralised location to make AJAX calls
* Persists data just within browser as not using database

bit.ly/authorapi
bit.ly/authorapidata

* Create new component to display mock data

## Chapter 6 - Composition<a id="chapter-6"></a>

### Prop Types
* Validation functions that declare what Prop data to expect to be passed to child components (i.e. required, of certain data type) otherwise warning logged in browser console
* Usage: Validate that Parent passing expected Props to Child Component
* Note: Prop Types only run in Development version of React (NOT minified Production version) for validation and to enforce rules
* `getDefaultProps` should have a field for any Props that are not required

{% highlight javascript %}
propTypes: {
    skills: React.PropTypes.object.isRequired,  // Require skills object (i.e. object may be 'array', etc)
    onSave: React.PropTypes.func.isRequired,    // Require function that gets called when user clicks save
    validate: React.PropTypes.func.isRequired,  // Require function that gets called to validate input
    errors: React.PropTypes.object,             // Declare any values passed in via form should be of type object
    hasErrors: React.PropTypes.func.isRequired  // Requiring function that determines if any errors occured
},
{% endhighlight %}

### Mixins
* Handling cross-cutting concerns and functionality (when sharing code between multiple Components)
* Usage: Set the Mixins Prop in a React Component and populate it with array of mixins
* React Router ships with mixins out of the box

{% highlight javascript %}
va ManageSkillPage = React.createClass({
    mixins: [
        Router.Navigation,
        Route.State,
        ...
    ],
    ...
})
{% endhighlight %}

### Controller View
* **Definition** Refactor a Component into two separate components (Parent "Smart" Component as the Controller View, aka Top-level React Component on the page) that 
interacts with Flux Stores, sets State as Parent and passes data down to Child "Dumb" Components via Props
to all nested Child Components, delegates handling of JSX markup down to Child Components,
and controls Child Component data flows
* Not recommended to nest Controller Views (only allow nested Child Components) as may result in multiple updates as React render method called multiple times that reduces performance
and may call React render method multiple times
* Controller Views (top-level Parent Components) control data flow down to all Child Components
* Controller Views interact with Flux Stores (i.e. Controller View receives updates emitted from Stores and passes updated data to Child Components that update via Props)
* Controller Views hold Data in State, and send Data to Child Components as Props
* Single Controller View recommended for each Page or Major Page Portion with as many nested Child Components as we like
* Recommended to pass single object to Child Components via Props to save changing code in multiple places (as new Props added to objects in future)

### Refactor Page Component by separating it, making it a Parent (i.e. SkillPage) Controller View with state, and moving the JSX markup part defined in it into a Child (i.e. SkillList) Component that is composed
* Before refactoring we iterate State in page, but when move iteration to Child we instead receives Props (NOT State)
* Separation of "Smart" and "Dumb" Components. Page becomes "Smart" (Parent) Component, 
as it gets data, sets data in State, passes down data from State to Child (Dumb) Component. 
Child (Dumb) Component just defines markup, and receives data via Props (NOT State), which is originally sent from State (from Parent)
* Child may become Reusable as a result

### React Chrome Dev Tools
* React tab in Chrome DevTools shows list of rendered React Parent/Child Components
* Select Components to view/edit Props/State
* After inspecting DOM element or applying breakpoint in render phase in Elements tab switch over to React tab and corresponding React Component will be highlighted. Enabling stepping through render tree

## Chapter 7 - Flux Development<a id="chapter-7"></a>

### Dispatcher
* Create Dispatcher folder/file and `src/dispatcher/appDispatcher.js` using Facebook code https://github.com/facebook/flux/blob/master/examples/flux-todomvc/js/dispatcher/AppDispatcher.js

### Actions
* Create Actions folder/file `src/actions/actionsSkill.js`
* Action Type must remain in sync with Flux Stores so use a Constants File (like Enums) listing all Action Types used in the app

### Stores
* Create Stores folder/file `src/stores/storeSkill.js`
* Add Node.js EventEmitter to broadcast events from Stores to notify React Components
* Extend the Flux Store to have Event Emitter capabilities using object-assign library
that joins two objects and their properties together (i.e. SkillStore and EventEmitter prototype)
https://www.npmjs.com/package/object-assign
* Extend the Flux Store using object-assign library by passing empty base object, and extend utilising:
EventEmitter.prototype parameter; and a block that defines the Store parameter.
Essentially defining EventEmitter.prototype as the base class
* Define Flux Store providing three core functions for React Components to interact with it
utilising the EventEmitter.prototype interface and functionality:
addChangeListener (accepts a callback parameter); removeChangeListener; and emitChange
* Register Store with Dispatcher so notified when Actions occur (defined below the Store as private method not concerned with public API)
* Only change data in Flux Store via Public API
* Call `emitChange` method in Public API to emit the change any time Flux Store changes
to notify any React Components that registered with `addChangeListener` function of this Flux Store 
so they update the UI
* Example Flux Store Boilerplate Template:

{% highlight javascript %}
"use strict";

// Import core libraries to create new Store

var Dispatcher = require('../dispatcher/appDispatcher');
var ActionTypes = require('../../constants/flux/typesSkill.js');

// Node.js EventEmitter broadcasts events from Stores to notify React Components
var EventEmitter = require('events').EventEmitter;

/**
 *  Extend the Flux Store to have Event Emitter capabilities using object-assign library
 *  that joins two objects and their properties together (i.e. SkillStore and EventEmitter prototype)
 *  https://www.npmjs.com/package/object-assign
 */
var assign = require('object-assign');

var CHANGE_EVENT = 'change';

/**
 *  Public Store API functions.
 *  Extend the Flux Store using object-assign library by
 *  passing empty base object, and extend utilising:
 *      - EventEmitter.prototype parameter
 *      - block that defines the Store parameter
 *  Essentially this defines EventEmitter.prototype as the base class
 */
var SkillStore = assign({}, EventEmitter.prototype, {

    /**
     *  Define Flux Store providing three core helper functions for React Components to interact with it
     *  utilising the EventEmitter.prototype interface and functionality
     *      - addChangeListener (accepts a callback parameter)
     *      - removeChangeListener
     *      - emitChange
     */
    addChangeListener: function(callback) {

        // Call the callback whenever change occurs in this Store
        this.on(CHANGE_EVENT, callback);
    },

    removeChangeListener: function (callback) {
        this.removeListener(CHANGE_EVENT, callback);
    },
    
    emitChange: function () {
        this.emit(CHANGE_EVENT);
    }

});

/**
 *  Private function implementation detail not concerned with our Public Store API
 *  Register Store with Dispatcher. All Stores are notified each time any
 *  Action occurs and is dispatched. Note: Flux differs here from traditional Pub-Sub Design Patterns
 */
Dispatcher.register(function(action) {

    // Switch based on all possible Action Types that may be passed in with the Action Payload
});

module.exports = SkillStore;
{% endhighlight %}

### Debugging with Breakpoints
* Add a line `debugger;` wherever want browser to pause during execution. Press F8 to step through

### Definitions:
* Ponyfill - Polyfill that does not overwrite the native method (i.e. allows library that is native in ES6 to work in ES5)

### TODO
* Flux https://facebook.github.io/flux/docs/overview.html
* Flux Examples Docs https://facebook.github.io/flux/docs/todo-list.html
* Flux Examples Code https://github.com/facebook/flux/tree/master/examples
* React Training https://github.com/ReactTraining

* Selection box drop-down for skill Reusable Component

* Build Course Management section with ability to: Add Course, Edit Course

* List Courses page having Title, Author, Category, Length, with ability to: Watch, Delete

* Resources: API - bit.ly/courseapi; Data - bit.ly/mockcoursedata