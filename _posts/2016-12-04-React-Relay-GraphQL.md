---
layout: post
title: React Apps with React, GraphQL, Flux, Relay (by Samer Buna from Pluralsight) (in progress!)
---

* Samer Buna @samerbuna

# Table of Contents
  * [Chapter 1 - Setup](#chapter-1)

## Chapter 1 - Setup<a id="chapter-1"></a>

### Summary
* React with GraphQL and Relay (RGR) Stack instead of MVC
* RGR Template Code https://github.com/RGRjs/express-webpack-template

### Flux
* MVC
    * Models read/write to each other
    * Views read/write to all Models
* Flux Pattern https://github.com/facebook/flux
    * Unidirectional data flow (Action, Dispatcher, Store, View)
    * Views only read from Stores (aka Models), but NOT write directly to Stores
    * Views create Actions that are sent directly to Action Interface
    * Action Interface communicates with API/Data
    * Action Interface contains Action Creators for each Action
    * Action Creators are JS modules where organise Actions
    * Action Creators dispatch Actions (Type + Data Payload) notifying Stores of Data changes
    * Dispatcher (Single) controls unidirectional data flow 
    * Dispatcher is a register of callbacks to notify appropriate Stores in order
    * Dispatcher methods `dispatch(), register(), waitFor()`
    * Stores that depend on other Stores declare their Dependency with the Dispatcher
    `StoreA.waitFor(StoreB)`
    * Stores contain Logic for modifying Data before exposing changes.
    * Stores manage the app State
    * Stores register interest with Dispatcher for Action Types use logic to manage Action Data 
    * Stores emit change event upon Data/State change so View may update/re-render
    * Actions are JS objects with Type (identify how use Action) and Data Payload as properties
    * Controller Views listen and read data upon Store emit change event
    * Controller Views query Stores for new data upon change and re-render Views

### GraphQL Queries (instead of REST APIs)
* REST APIs Problems:
    * Client dependent on server endpoints
    * Restricted to existing endpoints
    * Potentially multiple round-trips to retrieve data
* GraphQL String is JSON, but WITHOUT the values, from server.
* GraphQL String query Declaratively models the data requirement hierarchically instead of Imperative multiple query endpoint steps
* Views are rendered using GraphQL String (JSON Keys ONLY)
* Not require multiple custom endpoints exposing data
* Client sends GraphQL String to single server endpoint for parsing and provide response
* `edges` and `nodes` in query
* Tools
    * [GraphiQL in-browser IDE for GraphQL Queries with Star Wars API swapi.co](https://graphql-swapi.parseapp.com/)
        * Autocomplete allows explore data a deep as we like
        * CTRL + SPACE (autocomplete)
        * CMD + Enter (run query)
        * Click "Docs" link (top-right)

{% highlight javascript %}
{                           // Selection Sets (groups of parenthesis)
	allFilms {              // Field Entrypoint (maps to Property of Root Query Object
    edges{
      node {
        openingCrawl        // Field (maps to Property of Node Object)
      }
    }
  }
}
{% endhighlight %}

* GraphQL Server Setup (execution engine)
    * Create data/schema.js
        * Import Facebook `graphql` implementation

### GraphQL About
* Data communication bridging language 
* GraphQL Server execution engine
* Query language on Client (declarative) with specificity of objects requested
    * JSON (Without Values)
    * Graphical Query (queries as JSON Keys Only describe shape of JSON Key/Value response)
        * Objects `node` 
        * Connections `edge`
* **Selection Sets** (groups of parenthesis in JSON)
* **Field Entrypoint** (maps to outermost Property of root Query Object in JSON)
* **Fields** (map to Property of Nested Object in JSON)
* **Fields Arguments** may be modelled after functions
* **Alias**

{% highlight javascript %}
{
    user(id: 42) {          // Field Argument (i.e. query the name and age of user id 42)
        name,
        boss(level: 1) {    // Complex Field
            name
            age
        },
        directBoss: boss(level: 1) {    // Alias
            name
            age
        }
        groupBoss: boss(level: 2 {      // Alias
            name
            age
        }
        age
    }
    __type(id: 41) {                    // Introspection 
        fields {
            name
        }
    }
}
{% endhighlight %}

* Fields map to actual functions on server (i.e. `resolve()`)
* **Complex Fields** are Properties that represents an Object itself
* Benefit: No over-fetching or under-fetching

### GraphQL Principles, GraphQL Server wrapping Server-side logic, Type System
* GraphQL may be used as a Layer integrated on top of any existing Server-side logic
even including RESTful
    * SWAPI is example of GraphQL Server wrapped over RESTful API https://github.com/graphql/swapi-graphql

* GraphQL Client expresses Data Requirements to a GraphQL Server
    * GraphQL Queries may be validated on the Client-side using the **Type System** 
    on Server-side (and the Type System's availability using introspection,
    i.e. entering Field Argument value with incorrect type shows validation error in GraphiQL)
* GraphQL Server expresses Capabilities (using a Type System) to a GraphQL Client
* Views may change Data Requirement without needing change on Server-side
* **Instrospection** allows access Type System dynamically
* **Composition** split and combine GraphQL queries
    * **Fragments** used to DRY up duplication in queries

Fragments Example

{% highlight javascript %}
fragment userInfo on User {
    name
    age
}

{
    user(id: 42) {
        name,
        boss(level: 1) {
            ...userInfo
        },
        directBoss: boss(level: 1) {
            ...userInfo
        }
        groupBoss: boss(level: 2 {
            ...userInfo
        }
        age
    }
}
{% endhighlight %}


### Relay Framework
* React allows creation of declarative Views, 
modelling State for Views (where Views are a function of the Data)
(without the transactions to render the Views)
* Relay is Data managing domain of React apps
* React makes declarative programming easier for building UIs
* Relay makes declarative programming easier for Fetching and Mutating Data
* Builds upon Flux pattern and reduces it into Single Store
* Relay co-locates Model with View. Relay data requirement directly in View to render correctly
* Data requirements from ViewModel using GraphQL String (of Keys) sent to GraphQL server that provides Values response
* Actions may trigger data requirements from Multiple ViewModels,
merge GraphQL String (of Keys) from each ViewModel, and aggregates queries into Single Request to gathers Values from GraphQL server
* Relay receives data from GraphQL server and makes them available to React Components via Props
* Relay manages updates with GraphQL Mutations

* Traditional HTTP Request/Data issues
    * Performance 
    * Optimisation
        * Throttle requests
        * Batching requests
        * Optimistic Mutation and Rollbacks
            * i.e. showing changes immediately in UI so app is responsive 
            (even when change requests still in-flight) and perform rollbacks to before
            what we assumed would work
        * Pagination
        * Caching
            * when to expire cache, where to support cache
    * Error handling
        * Retries

* Relay Design Decisions
    * Storage and Caching (within **Relay.Store**)
        * RelayStore == QueueStore + MemoryStore + CacheManager
        * Layer Hierarchy: 
            * Relay.Store is single normalised client-side in-memory Data Store
            * Relay.Store's initial load puts all Data in Memory Layer of Relay.Store
            * Relay.Store's **QueueStore**
                * Manages in-flight changes to Data
                * Allows Rollbacks (instead of managing state for a record
                simply remove a faulty object from QueueStore)
            * Relay.Store's **MemoryStore** Layer manages symbol binary State for all Records
            (i.e. fetched or not fetched, etc)
            * Relay.Store's CacheManager Layer may be any storage engine (i.e.
            Local Storage, etc)
        * Queries can read from QueueStore (Optimistic UI Updates) or MemoryStore.
    * Object Identification
        * All Relay objects have UUID (unique id) for re-fetching records and prevent duplicate records
    * Diffing algorithm in Relay provides efficient data fetching 
    (i.e. only fetches new/changed parts of object) using the **NODE Interface**
    * **Relay Connection Model** related to **Pagination**
        * Similar to After/First Model but enhanced with `edge` and `node`
        that store extra info/metadata (i.e. total number of records, or whether we
        have next page). All `edges` have built-in `cursor`
        * Example using **Offset/Limit Model**
            * i.e. Given list alphabet letters posted as comments and created in order from
            "Z" to "A" (we sorted them by created by, with most recent "A" first)
            i.e. ["A"..."Z"]
            * Step 1 - Fetch first three with **Offset 0 / Limit 3** returns ["A","B","C"]
            * Step 2 - User adds new comment "A0", which is added to start of list
            * Step 3A - **Duplicate Problem** Retrieving Next Page **Offset 3 / Limit 3** returns
            ["C","D","E"] instead of ["D","E","F"]
            * Step 3B - **Missing Problem** If user deleted comment "A", then
             **Offset 3 / Limit 3** would return ["E","F","G"] (skipping "D")
         * Example using **After/First Model** (similar to Relay Connection Model)
            * Note: Unique IDs required to reference records in After value using this model
            * Note: Fetch an extra list to check if further requests after fetching first three
            * i.e. Given same list ["A"..."Z"]
            * Step 1 - **After ? (null) / First 3** returns ["A","B","C"]
            * Step 2 - User adding "A0" or deleting "A" does not affect correct retrieval of 
            Next Page, since query is for first three after "C" **After C / First 3**
            
{% highlight javascript %}
// Relay Connection Model
{
    commentConnection(first: 3, after: "C") {
        edges {
            cursor,
            node { authorName, commentText }
        },
        totalCount,
        pageInfo { hasNextPage }
    }
}
{% endhighlight %}

* Relay and GraphQL
    * Relay transpiles GraphQL Queries and Mutations against 
    server schema prior to usage
    * Example with GraphiQL 
    `http://localhost:3000/graphql?query={dataArrayOfLinkObjects{_id}}`
    * Relay uses [Tagged Template Strings/Literals (ES2015 feature)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals) 
    to express queries in console.log
    * **Tagged Template Strings/Literals**
        * Allows Tagging any Template String with a Function Name to be invoked for Pre-Processing
        * Tagged Function receives processed list and works with interpolated values
        * Example
{% highlight javascript %}
node
let titleCase = strings => strings.join().replace(/\b\w/g, match => match.toUpperCase());
titleCase`my test string`;
--> 'My Test String'
{% endhighlight %}
* Cont'd
    * `Relay.QL` is Relay function for transpiling queries against
    schema
    * Setup Babel Transform `babel-relay-plugin` by requiring it and invoking it with our 
    Schema as argument to provide a Babel Relay Plugin variable for use with Babel
    * Add to Babel Loader so Tagged Template Strings work: `npm install --save react-relay babel-relay-plugin`
    * Queries with Relay are more strict. Always specify the selection set (there is no default)
    (i.e. `query Test { ...`)
    * Note: If get error running `webpack -wd`: `Module build failed: ReferenceError: Unknown plugin "./babelRelayPlugin" specified in "base" at 0`
    then update webpack.config.js with `.map(require.resolve)`

{% highlight javascript %}
// View what Relay uses to replace raw GraphQL query
console.log(
    Relay.QL`
        query Test {
            dataArrayOfLinkObjects {
                _id
            }
        }
    `
);
{% endhighlight %}
    * [Babel Relay Plugin](https://facebook.github.io/relay/docs/guides-babel-plugin.html)
    is used by Relay to Pre-Process GraphQL Relay-Tagged Queries and Mutations on the
    Client-Side and then Relay parses them using Server Schema (usually too large to ship to
    Client-Side). Babel Relay Plugin may be configured ([Relay Starter Kit](https://github.com/relayjs/relay-starter-kit) 
    is preconfigured) to convert the Relay-Tagged Queries 
    and Mutations into an Object containing more Info about the types of the query fields
        * Advanced Setup
            * Copy from [Advanced Usage](https://facebook.github.io/relay/docs/guides-babel-plugin.html)
            to generate Plugin instance, and include it in babelRelayPlugin.js
            * Since we only have a JavaScript representation of the Schema, but not
             a schema.json. Use the **GraphQL JS implementation builtin tools**
             to compile the JS schema into a schema.json file so the Relay Plugin works.  
            * Generate ./data/schema.json file in existing server.js (where we already have the
            JS schema imported and invoked)
            * Read from the generated ./data/schema.json file inside the babelRelayPlugin.js file
            and call `.data` on it, and export the plugin for use by Webpack
            * Use the plugin with the `babel-loader`, add the `plugins` property to the
            `query` Property in webpack.config.js, as an array that is passed the path
            for the plugin (i.e. `./babelRelayPlugin.js`
            
            
* ES2015 Features Client-Side
    * Babel enables ES2015 Final features
    * Babel enables ES2015 Earlier stages according to [TC39](https://github.com/tc39/ecma262#ecmascript)
    by enabling the `stage-0` preset in webpack.config.js (`npm install --save babel-preset-stage-0` 
    including ECMAScript TC39 debated once such as use of `static propTypes`
    * Example - Use `stage-0` for all features (beyond 2015) such as:
        * Move `propTypes` and `defaultProps` (static properties) into the class itself
        * Move the `state` to be a direct property of the class (not within the contructor)
        * Convert `onChange` function to be a property of the class instead, allowing use of arrow fn,
        which means we no longer need to manually `.bind(this)` anymore, and so the `constructor`
        function may be removed

{% highlight javascript %}
// BEFORE
class Main extends React.Component {
    constructor(props) {
        super(props)
        this.state = _getAppState();
        this.onChange = this.onChange.bind(this);
    }
    ...
    onChange() {
        this.setState(_getAppState());
    }
}
Main.propTypes = {
    limit: React.PropTypes.number
}
Main.defaultProps = {
    limit: 4
}
export default Main;
{% endhighlight %}

{% highlight javascript %}
// AFTER
class Main extends React.Component {
    // Static Properties
    static propTypes = {
        limit: React.PropTypes.number
    }
    static defaultProps = {
        limit: 4
    }
    // Instance Properties (every component has its own)
    state = _getAppState();
    //
    onChange = () => {
        this.setState(_getAppState());
    }
}
export default Main;
{% endhighlight %}

* ES2015 Features Server-Side (Node)
    * Babel enables ES2015 Earlier stages according to [TC39](https://github.com/tc39/ecma262#ecmascript)
    by enabling the `stage-0` preset in package.json in the start script of `babel-node` 
    including usage of `async` [Stage 3 Draft](https://tc39.github.io/ecmascript-asyncawait/), and 
    immediately invoke it with an IIFE if required i.e. `(async () => { ... })();`

### Container Componet
* `componentDidMount` lifecycle method to invoke API read operation

### TODO

- Go through section "Working with Data" of course again as explains Flux Pattern concise example