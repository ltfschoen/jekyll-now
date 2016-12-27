---
layout: post
title: React Fluxible Testing (in progress!)
---

# Table of Contents
  * [Chapter 1 - React Fluxible Testing](#chapter-1)
  
## Chapter 1 - React Fluxible Testing<a id="chapter-1"></a>

* Install Fluxible. Create/Run Fluxible app at http://localhost:3000

{% highlight bash %}
npm install -g yo generator-fluxible
mkdir react-fluxible-test && cd react-fluxible-test
yo fluxible
npm run dev
open http://localhost:3000
{% endhighlight %}

* Fluxible Stores API
    * Dfn
        * Store app state
        * Handle business logic
        * React to data events
        * Classes that follow simple interface
        * Stores are full decoupled from Fluxible (hence default exports exclude any Store implementation)
    * Creation of Flux Stores (using Helper Utilities `BaseStore` and `createStore`)
        * `createStore` - Create a [createStore](http://fluxible.io/addons/createStore.html) class that extends `BaseStore` (extendable Store class)
        * `BaseStore` of dispatchr library is an extendable Store class that extends EventEmitter [eventemitter3](https://www.npmjs.com/package/eventemitter3)
        (Library dispatchr/addons/BaseStore.js) providing methods
            * `BaseStore.addChangeListener`, `BaseStore.removeChangeListener`, and `BaseStore.emitChange`
            * Note: Library fluxible/addons uses classes from Library dispatcher/addons
        * [Fluxible BaseStore](http://fluxible.io/addons/BaseStore.html) is extendable base class (reduces boilerplate when creating stores)
            * Built-in methods
                * `emitChange()` - emits change event
                * `getContext()` - returns [Store Context](http://fluxible.io/api/stores.html#helper-utilities)
                * `addChangeListener(callback)` - adds change listener
                * `removeChangeListener(callback)` - removes change listener
                * `shouldDehydrate()` - default returns true when change event emitted
            
    * Instantiation of Flux Stores
        * `new Store(dispatcher)` - Instantiates Store using constructor fn
            * `dispatcher` - Parameter is object with access to methods
                * `dispatcher.getContext()` - Retrieve Store Context
                * `dispatcher.getStore(storeClass)`
                * `dispatcher.waitFor(storeClass[], callback)`
    * [Store Context](http://fluxible.io/api/stores.html#helper-utilities) has no methods (by default), but may be modified with Plugins
                

{% highlight javascript %}
import BaseStore from 'fluxible/addons/BaseStore';

class ApplicationStore extends BaseStore {
    // Static property for Store name
    static storeName = 'ApplicationStore';
    /**
     *  Static property mapping Action names to Handler functions when
     *  the Dispatchr instance dispatches an Action.
     *  Note: `defaultHandler` fn called for any Action without an Handler
     */
    static handlers = {
        'RECEIVE_PAGE': 'handleReceivePage'
        'default': 'defaultHandler'
    };
    /**
     *  Pass `dispatcher` (dispatcher interface) to constructor fn of stores
     *  to only allow access to `waitFor` and `getStore` methods.
     *  This enforces Flux unidirectional flow via Action Creators
     *  (and preventing Stores from dispatching new Actions directly)
     */
    constructor (dispatcher) {
        super(dispatcher);
        this.dispatcher = dispatcher;
        this.currentPageName = null;
        if (this.initialize) {
            this.initialize();
        }
    }
    // Set initial state of Store
    initialize() {
        this.currentPageName = '';
    }
    /**
     *  Hander fn accepts `payload` param (objec with Action info) and `actionName` param (name of Action)
     *  Handler fn notifies clients subscribing to the Store by listening for any state changes using
     *  `emitChange` method of the EventEmitter interface (from which BaseStore extends).
     *  Clients may subscribe to store updates by adding listener with `on('change', handler)` or `addChangeListener`
     *  (method declared in BaseStore)
     */
    handleReceivePage (payload, actionName) {
        this.currentPageName = payload.pageName;
        this.emit('change'); // or this.emitChange()
    }
    getState () {
        return this.currentPageName;
    }
    /**
     *  `dehydrate` returns serialisable data of Store State to send to client when 
     *  `BaseStore.shouldDehydrate` is true when Store emits an update event 
     *  (using `BaseStore.emitChange`) (i.e. when server completes execution)
     */
    dehydrate () {
        return { 
            currentPageName: this.getState();
        };
    }
    /**
     *  `rehydrate` takes serialisable data for restoring store on server to 
     *  original state passed in when store shared b/w client and server
     */
    rehydrate (state) {
        this.currentPageName = state.currentPageName;
    }
    /**
     *  Optionally return boolean that defines if Store State should be dehydrated by dispatcher.
     *  If returns undefined(default)/true then Store will always be dehydrated
     */
    shouldDehydrate () {
        return true;
    }
}

/**
 *  Alternatively provide a static fn (private method handler) that is bound 
 *  to the Store instance, instead of providing a method name to be called. 
 */
// ApplicationStore.handlers = {
//   'RECEIVE_PAGE': function handleReceivePage(payload, actionName) { ... }
//   ... 
// }       
 
export default ApplicationStore;
{% endhighlight %}

* Fluxible [Actions API](http://fluxible.io/api/actions.html)
    * Dfn
        * Actions (aka "Action Creators" in Flux) are stateless async fns receiving 3x params
            * `actionContext` - access to Flux methods
            * `payload` - Action payload
            * `done` - `executeAction` waits for `done()` callback fn that signifies Action completed.
            If Action returns promise then `executeAction` waits for Action to resolved/rejected.
        * Actions called with either
            * `FluxibleContext.executeAction(myAction, payload, [done])`
            * OR from Other Actions
            * OR from a Component
                * Note: **Fire-and-forget** Enforces that Actions are fire-forget with state changes only handled using Flux flow.
                Hence components running `executeAction` cannot return a promise or pass a callback, the callback
                becomes high level (app level) `componentActionErrorHandler` fn provided to Fluxible constructor
                to allow handling errors at high level that spawn from components firing actions
        * Action Context (i.e. access of Action to Flux Context)
            * `dispatch(eventName, payload)` - dispatch data event and call Store handlers
            * `executeAction(action, payload, [done])` - execute action, wait for promise to be resolved/rejected, 
            or `done` callback to be called
            * `getStore(storeConstructor)` - get Store instance to reading from it only
            * `rootId` - generated for each root level Action executed and persisted to all subsequent Actions called under it
            * `stack` - array of Action names that were called (for debugging)

{% highlight javascript %}
// Action (with less than 3x params and without promise)
export function myAction(actionContext, payload) {
    return actionContext.dispatch('RECEIVE_PAGE', payload); // or MY_ACTION
}
{% endhighlight %}

{% highlight javascript %}
impact myAction from './mySyncAction';
export default class MyComponent extends React.Component {
    static contextTypes = {
        executeAction: React.PropTypes.func.isRequired
    };
    constructor(props) {
        super(props);
    }
    onClick = (e) => {
        this.context.executeAction(mySyncAction, {});
    }
    render() {
        return <button onClick={this.onClick}>Click Me</a>
    }
};
{% endhighlight %}

* Testing Fluxible [Actions API](http://fluxible.io/api/actions.html)
    * `createMockActionContext` lib is passed Action instance and records methods called by the Action on the context
    * `dispatch` when called pushes an object to `dispatchCalls` array (each object in array contains keys `name` and `payload`)
    * `executeAction` when called pushes an object to `executeActionCalls` array (each object in array contains key `action` and `payload`)
    * `getStore` calls are proxied to dispatcher instance (upon which Stores may be registered when instantiated)

{% highlight javascript %}
import {createMockActionContext} from 'fluxible/utils';
import assert from 'assert';

// Actual Store is overridden with MockStore in test
import {BaseStore} from 'fluxible/addons';
class FooStore extends BaseStore {
    static storeName = 'FooStore';
    // ...
}

// Actions being tested
let myAction = function (actionContext, payload, done) {
    let foo = actionContext.getStore(FooStore).getFoo() + payload;
    actionContext.dispatch('FOO', foo);
    actionContext.executeAction(otherAction, foo, done);
};

let otherAction = function (actionContext, payload, done) {
    done();
};

// Mock of FooStore
class MockFooStore extends BaseStore {
    static storeName = 'FooStore'; // matches the actual FooStore.storeName
    static handlers = {
        'FOO': 'handleFoo'
    };
    constructor (dispatcher) {
        super(dispatcher);
        this.foo = 'foo';
    }
    handleFoo (payload) {
        this.foo = payload;
        this.emitChange();
    }
    getFoo () {
        return this.foo;
    }
}

// Tests
describe('myAction', function () {
    let actionContext;
    beforeEach(function () {
        actionContext = createMockActionContext({
            stores: [MockFooStore]
        });
    });
    it('should dispatch foo', function (done) {
        myAction(actionContext, 'bar', function () {
            assert.equal(1, actionContext.dispatchCalls.length);
            assert.equal('FOO', actionContext.dispatchCalls[0].name);
            assert.equal('foobar', actionContext.dispatchCalls[0].payload);
            assert.equal(1, actionContext.executeActionCalls.length);
            assert.equal(otherAction, actionContext.executeActionCalls[0].action);
            assert.equal('foobar', actionContext.executeActionCalls[0].payload);
            done();
        });
    });
});
{% endhighlight %}

* Fluxible [Components API](http://fluxible.io/api/components.html)
    * Dfn
        * Components must access State of app held in Stores
        * Components must be able to Execute Actions that Stores react to as a result of events
    * Access to React Component Context from Components
        * `ComponentContext` of current request is passed to Components for access
            * `ComponentContext` receives limited access to FluxibleContext (prevents avoidance of Flux flow and dispatching directly)
            * `ComponentContext` contains methods:
                * `executeAction(action, payload, [done])` - execute action, wait for promise to be resolved/rejected, 
                or `done` callback to be called. Note: **Fire-and-forget** (see earlier comments)
                * `getStore(storeConstructor)` - get Store instance to reading from it only
        * `ComponentContext` of current request is passed as a Prop to top-level container Component for access, then
        either: 
            * Implicitly propagated via React Context to controller views (i.e. implicit handling of propagation 
            to controller views that have registered its `contextTypes` and use `provideContext` [provideContext Helper](http://fluxible.io/addons/provideContext.html) 
            (wraps a Component with a higher-order component that declares/specifies child context with `childContextTypes`
            and allows the React context `ComponentContext` to propagate to all children that specify their `contextTypes`
            so the Components have access to listen to Store instances, execute Actions, and access methods added to the 
            ComponentContext by Plugins), and FluxibleComponent (wrapper component that imperatively declares 
            child context) (RECOMMENDED) **OR** 
            * Manually by passed `ComponentContext` manually to all Child Components as Props
    * Accessing Stores from Components
        * Components access Store instance state via `this.context.getStore(StoreConstructor)`
    * Accessing Changes to Stores from Components
        * Component adds/removes listeners to listen to a Store for state changes (with or without Helpers) so it may re-render 
        itself using `connectToStores` Helper [connectToStores](http://fluxible.io/addons/connectToStores.html) 
        (higher-order component) and store latest state locally in the component
    * Executing Actions from Components
        * `this.context.executeAction(action, { payload });`
 
* Testing Fluxible [Component API](http://fluxible.io/api/components.html)
    * `createMockComponentContext` lib
    * `dispatch` when called pushes an object to `dispatchCalls` array (each object in array contains keys `name` and `payload`)
    * `executeAction` when called pushes an object to `executeActionCalls` array (each object in array contains key `action` and `payload`)
    * `getStore` calls are proxied to dispatcher instance (upon which Stores may be registered when instantiated)

* Fluxible [Fluxible API](http://fluxible.io/api/fluxible.html)
    * Dfn
        * Fluxible instance instantiated once for app (i.e. such as in [app.js](https://github.com/ltfschoen/react-fluxible-test/blob/master/app.js))
        * Holds Settings and Interfaces that are used across Requests (i.e. in [server.js](https://github.com/ltfschoen/react-fluxible-test/blob/master/server.js))
        * Server exposes dehydrated server-side context state (serialised object of Fluxible instance and
        FluxibleContext) to by used to rehydrate the client-side state with the same state 
        [client.js](https://github.com/ltfschoen/react-fluxible-test/blob/master/client.js)

* Fluxible [FluxibleContext API](http://fluxible.io/api/fluxible-context.html)
    * Dfn
        * FluxibleContext instance instantiated once per Request/Session by calling `createContext(contextOptions)` on the 
        Fluxible instance
        * FluxibleContext provides server-side **isolation** of Stores, Dispatches, and other data between requests
    * **SubContexts**
        * SubContexts are subsets of methods from FluxibleContext are provided to each Component of app to prevent them breaking Flux flow.
        * SubContexts are provided a `getComponentContext()` getter to access the FluxibleContext
        * SubContexts (see table at FluxibleContext API link):
            * Action Context - passed as first param to all Actions (with access to most Fluxible methods)
            * ComponentContext - passed as prop to top-level React Component and then propagated to Child Components requiring access to it
            * Store Context - passed as first param to all Store Constructors (no methods/properties by default)
        * **Plugins** 
            * Plugins allow modification of SubContexts
    * SubContext Methods
        * `executeAction(actionContext, payload, [done])`
            * `executeAction` is entrypoint to app execution starting the Flux flow of:
                * **Action/Dispatcher** - Action dispatches events to Stores
                * **Stores** - Stores updating their Data structures
                * **Server-side** - Wait for initial Action to finish before React rendering
                * **Client-side** - Already immediately render Components and wait for Store change events

Callback example (i.e. in [server.js](https://github.com/ltfschoen/react-fluxible-test/blob/master/server.js))
{% highlight javascript %}
let myActionCreator = function(actionContext, payload, done) {
    // do stuff
    done();
};
context.executeAction(myActionCreator, {}, function (err) {
    // action has completed
});
{% endhighlight %}

Promise example
{% highlight javascript %}
var myActionCreator = function(actionContext, payload, done) {
    // do stuff
    done();
};
context.executeAction(myActionCreator, {})
    .then(function (result) {
        // action completed
    })
    .catch(function (err) {
        // action had an error
    });
{% endhighlight %}

* Cont'd
    * Cont'd
        * `plug(plugin)` - allows custom context settings to be shared between server and client, 
        and allows dynamically plugging ActionContext, ComponentContext, and StoreContext with extra methods
        * `getActionContext()` - returns ActionContext with access to functions that should only
        be called from Actions (i.e. by default `dispatch`, `executeAction`, `getStore`). 
        Note: ActionContext object is passed as first parameter to the Action
        each time `executeAction` is called 
        * `getComponentContext()` - returns ActionContext with access to functions that should only
        be called from Components (i.e. by default `executeAction`,  `getStore`).
        Note: `executeAction` enforces Actions to be send and forget by not allowing a callback to be passed from Components.
        * `getStoreContext()` - returns StoreContext with access to only functions that should be called from Stores
        (empty with no methods by default, but modifiable with Plugins)
        * `dehydrate()` (i.e. use with `context.dehydrate()`) returns serializable object containing state of 
        `FluxibleContext` and its `Dispatchr` instance, and calls any plugins whose `plugContext` method returns an object
        containing a `dehydrate` method
        * `rehydrate(state)` - takes object representing state of `FluxibleContext` and `Dispatchr` instances (i.e. usually
        retrieved from dehydrate) to rehydrate them to same state as on the server, and calls any plugins whose
         `plugContext` method returns an object containing a `dehydrate` method
         
 * Fluxible [Plugins API](http://fluxible.io/api/plugins.html)
     * Dfn
         * Fluxible Plugins allow us to extend the interface of each context type (i.e. provide extra properties to 
         respective `contextOptions` object)
     * Implementation:
        * See my [Pull Request](https://github.com/ltfschoen/react-fluxible-test/commit/a1607e0eaef3bdf352586e5c6f1483852cfaeaf5)
     * TODO
        * Unable to console.log the following in Store as mentioned as being possible at the link (only able to do in Component)
        
// Retrieve Plugin state value of 'bar' from actions, stores or components
// console.log("context.getActionContext().getFoo()", context.getActionContext().getFoo());
// console.log("context.getStoreContext().getFoo()", context.getStoreContext().getFoo());
// console.log("context.getComponentContext().getFoo()", context.getComponentContext().getFoo());


        

## Interpreting Errors

* `Invariant Violation: Element type is invalid: expected a string (for built-in components) or a class/function (for composite components) but got: undefined.`
    * Solution: If importing from `export default function About() { return (<div><h2>About</h2></div>); };` into 
    unit test with `import {About} from './About'` then change to `import About from './About'`

## Testing DRAFT Templates

{% highlight javascript %}
// '../../components/About'
import React from 'react';

export default function About() {
    return (<div><h2>About</h2></div>);
};

import React from 'react';
import { assert } from 'chai'; // or { assert, expect }
import expect from 'expect';
// Enzyme `shallow` is wrapper of React Test Utils `shallowRender`
import {mount, shallow} from 'enzyme';
import About from '../../components/About';
import TestUtils from 'react-addons-test-utils';

const props = {};
const context = {};

describe ('About', () => {
    it('should exist', () => {
        assert.isDefined(About)
    });
    it('should have correct title', () => {
        // Shallow - https://github.com/airbnb/enzyme/blob/master/docs/api/shallow.md#shallownode-options--shallowwrapper
        const wrapper = shallow(<About {...props} />, context);
        // const wrapper = mount(<About {...props}/>);
        expect(wrapper.find('h2').text()).toEqual('About');
    });
});
{% endhighlight %}
   
## TODO
* [provideContext](http://fluxible.io/addons/provideContext.html)
* [FluxibleComponent](http://fluxible.io/addons/FluxibleComponent.html)
* [connectToStores](http://fluxible.io/addons/connectToStores.html)
* [Facebook Flow](https://github.com/facebook/flow)
* [React Testing Demo](https://github.com/ruanyf/react-testing-demo)

## Links

* [dispatchr](https://www.npmjs.com/package/dispatchr) - Flux dispatcher for apps on server/client
(i.e. node_modules/dispatchr/addons/BaseStore.js)
* [Generator Fluxible @ Fluxible Quickstart](http://fluxible.io/quick-start.html)
* [Fluxible BaseStore](http://fluxible.io/addons/BaseStore.html)
* [Fluxible createStore](http://fluxible.io/addons/createStore.html)
* [Store Context](http://fluxible.io/api/stores.html#helper-utilities)

## Other Links
* [Stubbed Router Context](https://preact.gitbooks.io/react-book/content/testing/Notes.html)
* [React Testing Examples](http://brandonokert.com/2015/08/04/TestingInReact/)
* [Examples 1](http://thereignn.ghost.io/a-step-by-step-tdd-approach-on-testing-react-components-using-enzyme/)