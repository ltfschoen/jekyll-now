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
* Builds upon Flux pattern and reduces it into Single Store
* Relay co-locates Model with View. Relay data requirement directly in View to render correctly
* Data requirements from ViewModel using GraphQL String (of Keys) sent to GraphQL server that provides Values response
* Actions may trigger data requirements from Multiple ViewModels,
merge GraphQL String (of Keys) from each ViewModel, and aggregates queries into Single Request to gathers Values from GraphQL server
* Relay receives data from GraphQL server and makes them available to React Components via Props
* Relay manages updates with GraphQL Mutations

### Container Componet
* `componentDidMount` lifecycle method to invoke API read operation

### TODO

- Go through section "Working with Data" of course again as explains Flux Pattern concise example