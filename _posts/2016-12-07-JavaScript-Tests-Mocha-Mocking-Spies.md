---
layout: post
title: JavaScript Tests Mocha, Mocking, Sinon, Spies (by Joe Eames from Pluralsight) (in progress!)
---

* Joe Eames @josepheames

* [Mocha](https://mochajs.org/)

# Table of Contents
  * [Chapter 1 - Mocha and Jasmine Spies](#chapter-1)
  * [Chapter 2 - SinonJS](#chapter-2)
  
## Chapter 1 - Mocha<a id="chapter-1"></a>

* TDD Style Mocha
    
{% highlight javascript %}
suite('my suite', function() {
    setup(function() { ... });      // Called before every test
    teardown(function() { ... });   // Called before other tests are run but only once
    before(function() { ... });
    after(function() { ... });
    test('test 1', function() {
        expect(1).to.equal(1);
    });
    suite('inner nested suite', function() {
        test('test 2', function() {
            expect(1).to.equal(1);
        });
    });
});
{% endhighlight %}

* BDD Style Mocha

{% highlight javascript %}
var assert = require("assert"); // build-in Node.js assert library

describe('a feature/my suite', function() {
    setup(function() { ... });     // Called before every test
    teardown(function() { ... });
    before(function() { ... });    // Called before other tests are run but only once
    after(function() { ... });
    it('test 1', function() {
        expect(1).to.equal(1);
    });
    describe('a scenario/inner nested suite', function() {
        it('should be test 2', function() {
            assert(true);
        });
    });
});
{% endhighlight %}

* Exclusive Run `describe` block of tests with `describe.only`
* Exclusive Run `it` test with `it.only`
* Skip `describe` block of tests with `describe.skip`
* Skip test with `it.skip`
* Pending tests by excluding the callback i.e. `it('...');`
* Ignore specific global variables

{% highlight javascript %}
describe('...', function() {
    mocha.setup({globals: ['1']});
});
{% endhighlight %}

* Ignore all global variables (not throw errors when found)

{% highlight javascript %}
describe('...', function() {
    mocha.setup({ignoreLeaks: true});
});
{% endhighlight %}

* Asynchronous Tests using BDD Style Mocha 
    * Use with `setTimeout` and `setInterval` calls)
    * Use to test UI effects
    * Do not use unit tests for AJAX calls, instead use Mocking

{% highlight javascript %}
describe('my suite', function() {
    it('test 1 should be asynchronous', function(done) {
        // Asynchronous call with callback
        setTimeout(function() {
            expect(1).to.equal(1);
            done();
        }, 10);
        ...
        done();
    });
});
{% endhighlight %}

* Asychronous Tests 
    * Globally override Mocha default timeout is 2 seconds.

{% highlight javascript %}
mocha.setup({timeout: 3000});
describe('...', function() {
    ...
});
{% endhighlight %}

* Cont'd
    * Locally override Mocha default timeout is 2 seconds.

{% highlight javascript %}
describe('...', function() {
    this.timeout(3000);
    it('test 1 should be asynchronous', function(done) {
        // Asynchronous call with callback
        setTimeout(function() {
            expect(1).to.equal(1);
            done();
        }, 10);
        ...
        done();
    });
});
{% endhighlight %}

* Integration with CI using Mocha, TeamCity and [PhantomJS](https://code.google.com/archive/p/phantomjs/downloads) executable (capture output)
    * Download PhantomJS headless Webkit `phantomjs` executable and copy to project
    * Multiple TeamCity Reporters setup for HTML test runners, i.e.
    
    {% highlight javascript %}
    <script>
    mocha.setup({ui:'bdd',
        reporter: function(runner) {
            new Mocha.reporters.HTML(runner);
            new Mocha.reporters.TeamCity(runner);
        }
    });
    </script>
    {% endhighlight %}

    * Create custom `run-mocha.js` file
        * Note: WebKit (engine behind Chrome)
        * Note: PhantomJS (JS runtime environment that wraps around a version of WebKit engine without browser overhead)
        * Note: PhantomJS is insufficient for cross-browser compatibility testing, as it
        wraps around WebKit version that does not verify JS will run on multiple browsers and versions 
        * Note: Cross-browser testing with CI using 3rd parties
    * PhantomJS translates JS console.log into format to expected by CI server to determine if tests passing/failing

* Failing test that completes before assert runs, unless stop running tests 
until notified `stop();` and instruct to `start();` so it does not run code
after the `setTimeout` first

{% highlight javascript %}
suite('my suite', function() {
    test('test 1 should be broken asynchronous timing test', function(done) {
        // Notify what asynch calls to wait for before continue
        stop(); // Shorthand for multiple is `stop(2)`
        stop();
        // Asynchronous call with callback
        setTimeout(function() {
            ok(true);
            start();
        }, 2000);
        setTimeout(function() {
            ok(true);
            start();
        }, 10);
    });
});
{% endhighlight %}

* Definitions: 
    * System Under Test (SUT)
    
* Mock Object Types ([Test Doubles](http://martinfowler.com/bliki/TestDouble.html))
    * Dummy objects - passed around but not actually used other than to fill parameter lists.
    * Fake objects - simplified working implementation so not suitable for production
    * Stubs - return canned/predefined answers to test calls
    * Spies (enhanded Stub) - spies hold/record info on how method called and with what parameters. 
    (i.e. email service records quantity of messages received)
    * Mock - pre-programmed specification of calls expected to receive, 
    throwing exceptions if a method receives unexpected calls,
    verification checks that mocks receive all expected calls

* Mocking Benefits
    * Test component isolated from SUT 
    (i.e. remove costs of interaction with external or complex systems with associated versions)
    * Test interactions between Components
    * Flow control simple (i.e. avoid declaring state to return true/false, instead 
    create Mock objects to so)

* Methodology
    * No interface of class for mocking against in JS
    * Freestanding functions outside JS classes
        
* Mocking Functions - replace function with Test Double
* Mock Object - Options (2 OFF) for creating a Mock Interface from which to create a Mocking Object:
    * Create Fake object (Test Double) manually with same interface as real object being mocked
        * Disadvantage: requires updating Test Double to match changes to Object
    
        * Example: Given prototypical inheritance class defined using constructor function and prototype modification.
        Mock the class by creating a new object with the same interface as the class
    
        {% highlight javascript %}
        function MyClass(myAttr) {
            this.myAttr = myAttr;
        }
        
        MyClass.prototype = function() {
            var myFunc = function(param) {
                return param;
            };
        }();
        
        module.exports = MyClass;

        var interfaceMyClass = { myFunc: function(param) {} };
        
        // Create mock of interfaceMyClass for testing
        {% endhighlight %}
    
    * Create Real object instance using existing object and mock it (to defined an interface of it)
        * Disadvantage: Object constructor may have side-effects we do not want in our test

        * Example: Create live instance of class and use as interface definition to be mocked
    
        {% highlight javascript %}
        var myClass = new MyClass("abc");
        
        // Create mock of MyClass for testing
        {% endhighlight %}

* Spy Options in Mocking Implementation (allows testing how SUT calls its collaborators to check performed correctly)
    * Replacing Methods with Spies
        * Spy methods replace all methods of the Mocked class
    * Intercepting Actual Methods with Spies
        * Insert Spy method between class method and real implementation 
    * Note: Both Spy implementations return predefined response, whilst tracking quantity of times 
    called and parameters provided

* Example 1 Mocking Library Implementation (Manual Mocking)

{% highlight javascript %}
var MyClassA = function() {};
MyClassA.prototype = function() { var myFunc1 = function(param) { 
    console.log("inside myFunc1");
    return param; 
}; }();

var MySubclassA = function() {};
MySubclassA.prototype = new MyClassA();
MySubclassA.prototype = function() { var myFunc2 = function(param) { 
    console.log("inside myFunc2");
    return param; 
}; }();

// Create Spy for each class passed as key
function SpyOn(classToSpyOn) {
    for(var key in classToSpyOn) {
        classToSpyOn[key] = CreateSpyPassThrough(key);
    }
}

// Interceptor
function CreateSpyPassThrough(key) {
    return function() { console.log("spying on class: " + key);
}

var mySubclassA = new MySubclassA();
SpyOn(mySubclassA);

// Call methods
mySubclassA.myFunc1();
mySubclassA.myFunc2();
{% endhighlight %}

* Example 2 Mocking Library Implementation (Pass-Through Mocking)
    * Passes context and original function through as parameters

{% highlight javascript %}
var MyClassA = function() {};
MyClassA.prototype = function() { var myFunc1 = function(param) { 
    console.log("inside myFunc1");
    return param; 
}; }();

var MySubclassA = function() {};
MySubclassA.prototype = new MyClassA();
MySubclassA.prototype = function() { var myFunc2 = function(param) { 
    console.log("inside myFunc2");
    return param; 
}; }();

// Create Spy for each class passed as key
function SpyOn(classToSpyOn) {
    for(var key in classToSpyOn) {
        classToSpyOn[key] = CreateSpyPassThrough(key, classToSpyOn, classToSpyOn[key]);
    }
}

// Interceptor
function CreateSpyPassThrough(key, context, origFunc) {
    // return function() { console.log("spying on class: " + key);
    var passThroughSpy = function() {
       console.log("passthrough spy for: " + key); 
       return origFunc.apply(context, arguments);
    };
    return passThroughSpy;
}

var mySubclassA = new MySubclassA();
SpyOn(mySubclassA);

// Call methods
mySubclassA.myFunc1();
mySubclassA.myFunc2();
{% endhighlight %}

* Spies
    * Jasmine Spies
        * Jasmine Spies are subset of Sinon.JS
        * Jasmine Spies are built for mocking single functions
            * Spying on freestanding functions (i.e. callbacks)

{% highlight javascript %}
/** 
 *  Given function that takes callback parameter
 */
function callMyCallback(callback) {
    callback();
}

/**
 *  Use Jasmine Spy to create a Freestanding Spy used as a callback and test
 *  the callback is called in expected circumstances
 */
it('...'), function() {
    var spyCallback = jasmine.createSpy('mySpy');
    callMyCallback(spyCallback);
    expect(spyCallback).toHaveBeenCalled();
}
{% endhighlight %}

* Cont'd
    * Cont'd
        * `spyOn` global function of Jasmine Spies (attached to window object)
            * Used to spy on method of object
            * Create a spy on a dependency's functions that is used 
            in a class being tested (replacing the old function)
            * Use `spyOn(objectWithMethodToFake, "methodNameAsString")` with `andReturn`
            * Use `andCallFake` to call an actual fake implementation for the spy
            * Use a spy that dispatches call to the real implementation using
            `andCallThrough`, and monitor how many times it is called, 
            and monitor arguments passed
            * Cause spy to throw error `andThrow` to test class exception handling
            
{% highlight javascript %}
/** 
 *  Spy on existing function
 */
var myObj = {
    save: function() {
        //
    },
    getQuantity: function() {
        return 5;
    }
}

/**
 *  Create spy that is pointer to myObj function. This spy replaces the actual implementation.
 *  If the actual implementation returned a value we can force a return value out of the spy
 */
describe("Spies", function() {
    it('should spy on save', function() {
        var spy = spyOn(myObj, 'save');
        myObj.save();
        expect(spy).toHaveBeenCalled();
    });
    it('should spy on getQuantity where a value is returned', function() {
        var spy = spyOn(myObj, 'getQuantity').andReturn(10);
        expect(myObj.getQuantity()).toEqual(10);
    });
    it('should spy on getQuantity using fake implementation', function() {
        var spy = spyOn(myObj, 'getQuantity').andCallFake(function() {
            console.log('returning 20');
            return 20;
        });
        expect(myObj.getQuantity()).toEqual(20);
    });
    it('should spy on getQuantity callThrough using original implementation', function() {
        var spy = spyOn(myObj, 'getQuantity').andCallThrough();
        expect(myObj.getQuantity()).toEqual(5);
        expect(spy).toHaveBeenCalled();
    });
    it('should spy on getQuantity and throw exception', function() {
        var spy = spyOn(myObj, 'getQuantity').andThrow(new Error('problem'));
        var qty;
        try {
            qty = myObj.getQuantity();
        } catch(err) {
            qty = 100;
        }
        expect(qty).toEqual(100);
    });
}
{% endhighlight %}

* Cont'd
    * Cont'd
        * `createSpyObj` function of Jasmine Spies
            * Creates new object with only spy methods (not need hijack existing object)
            * Usage is when object under test has dependency with many 
            functions we may be calling (instead of having to repeatedly call
            `spyOn` we just call `createSpyObj` and all spies already set up)

{% highlight javascript %}
/** 
 *  The createSpyObj method takes name of spy as first param,
 *  and an array of elements representing names of spy methods
 *  for the object 
 */
var spy = jasmine.createSpyObj('spyName', ['method1',
    'method2', ...]);
{% endhighlight %}

{% highlight javascript %}
/** 
 *  Example of creating a full spy object and methods with fake implementations
 */
describe("Spies", function() {
    it('should create a spy object', function() {
        var spy = jasmine.createSpyObj('mySpy', ['getName', 'save']);
        spy.getName.andReturn('bob');
        spy.save.andCallFake(function() { console.log('save called'); });
        expect(spy.getName()).toEqual('bob');
        spy.save();
        expect(spy.save).toHaveBeenCalled();
    });
}
{% endhighlight %}

* Jasmine Spy supported Matchers
    * `toHaveBeenCalled` - asserts spy called at least once
    * `not.toHaveBeenCalled` - asserts spy never called
    * `toHaveBeenCalledWith(arg1, arg2...)` - asserts spy called at least once 
    with all specific args provided (no guarantee about multiple calls)

{% highlight javascript %}
describe("Spies", function() {
    it('should verify arguments', function() {
        var spy = jasmine.createSpy('mySpy');
        // spy(1);
        spy(2);
        spy(1, 1);
        expect(spy).toHaveBeenCalledWith(1);
    });
}
{% endhighlight %}

* Cont'd
    * `not.toHaveBeenCalledWith(arg1, arg2...)` - asserts spy never called with
    specific set of args

{% highlight javascript %}
describe("Spies", function() {
    it('should verify that given args not called', function() {
        var spy = jasmine.createSpy('mySpy');
        spy(1);
        spy(4, 1);
        spy(4); // causes fail
        expect(spy).not.toHaveBeenCalledWith(1);
    });
}
{% endhighlight %}

* Jasmine Spy supported Metadata in assert calls
    * `calls[]` - array of objects representing each time spy called.
    each object has 2 OFF properties with info about specific invocation
        * `calls[].args[]` property is array of each arg passed for invocation
        * `calls[].object` property is object that was context of the call
        (i.e. object that was `this` then spy was invoked), and is useful
        for callbacks to determine object they were called on
        * `callCount` convenience method instead of `calls.length`
        * `mostRecentCall` gets last call
        * `argsForCall[]`

{% highlight javascript %}
describe("Spies", function() {
    it('should work with metadata', function() {
        // create object
        var myObj = { method: function() {} }
        // spy on the object
        var spy = spyOn(myObj, "method");
        myObj.method(1);
        myObj.method(2);
        myObj.method(3);
        expect(spy.calls[0].args[0]).toEqual(1);
        expect(spy.calls[0].object).toEqual(myObj);
        expect(spy.calls.length).toEqual(3);
        expect(spy.callCount).toEqual(3);
        expect(spy.mostRecentCall.args[0]).toEqual(3);
        expect(spy.argsForCall[1][0]).toEqual(2);
    });
}
{% endhighlight %}

* Jasmine Spy supported Utility methods
    * `jasmine.isSpy` - check if object is a spy or not
    * `reset` - method on spies to reset them
    
{% highlight javascript %}
describe("Spies", function() {
    it('should work with utility methods', function() {
        // create object
        var spy = jasmine.createSpy('a spy');
        expect(jasmine.isSpy(spy)).toEqual(true);
        spy();
        spy.reset(); // reset spy call count
        expect(spy.callCount).toEqual(0);
    });
}
{% endhighlight %}

## Chapter 2 - SinonJS<a id="chapter-2"></a>

* [SinonJS](http://sinonjs.org/) (mocking library, not a test framework)
    * Configure to use Sinon for spies in HTML spec runner

* Sinon Spies Usages
    * Option 1 - test double for single function (i.e. taking callback) 
    * Option 2 - watch object's existing methods (i.e. using original implementation)
    similar to `andCallThrough`
    
* Create Sinon Spies by either:
    * `sinon.spy` method without params creates anon function without return value
    suitable when callback required for testing
    
{% highlight javascript %}
/**
 *  mySUT should not have implicit dependency on global variable myDep (BAD PRACTICE). 
 *  Instead pass the dependent object explicitly into the function
 */
var mySUT = {
    /**
     *  Example System Under Test (SUT) Object with single method
     *  that takes a callback and calls it.
     */
    callCallback: function(cb) {
        cb();
    },
    callCallbackWithReturnValue: function(cb) {
        return cb();
    },
    // // Call someMethod method on the myDep object and return the value
    // callDependency: function() {
    //     return myDep.someMethod();
    // }
    callDependencyBetter: function(dep) {
        return dep.someMethod();
    }
}

var myDep = {
    someMethod: function() {
        return 10;
    }
}

function realCallback() {
    return 4;
}

// Test to call callback
describe("Spies", function() {
    it('should spy on a callback', function() {
        // Spy on existing object using first way of calling it 
        // (without params to create fn with no return value
        var spy = sinon.spy();
        mySUT.callCallback(spy);
        expect(spy.called).toBe(true);
    });
    it('should call real implementation if given a real freestanding fn to spy on', function() {
        // Spy on existing object using second way of calling it 
        // (with single param to create fn with a return value)
        var spy = sinon.spy(realCallback);
        var returnValue = mySUT.callCallbackWithReturnValue(spy);
        expect(spy.called).toBe(true);
        expect(returnValue).toBe(4);
    });
    it('should spy on a method of an object', function() {
        // Use third parameter set of passing in the actual object and method to spy on in that object
        var spy = sinon.spy(myDep, 'someMethod');
        // var returnValue = mySUT.callDependency();
        var returnValue = mySUT.callDependencyBetter(myDep);
        expect(spy.called).toBe(true);
        expect(returnValue).toBe(10);
    });
}
{% endhighlight %} 

* Sinon Spy API
    * Supports Sinon Stubs and Sinon Mocks

{% highlight javascript %}
// Check if spy called at least x times
called
calledOnce
calledTwice
calledThrice

// Retrieve spy object from first time it was called (convenience methods)
// IMPORTANT NOTE: The returned objects use the Sinon Call API
firstCall
secondCall
thirdCall
lastCall

// Checks if a spy was called before another spy passed as parameter
calledBefore(spy)
calledAfter(spy)

// Checks that spy was called with particular object as context `this`
calledOn(obj)
alwaysCalledOn(obj)

// Checks that spy was called at least once with given set of arguments
calledWith(args...)
alwaysCalledWith(args...)
calledWithExactly(args...)
alwaysCalledWithExactly(args...)
notCalledWith(args...)
neverCalledWith(args...)

// Takes a set of matchers
calledWithMatch(args...)
alwaysCalledWithMatch(args...)
notCalledWithMatch(args...)
neverCalledWithMatch(args...)

// Check that spy was called with `new` operator and uses constructor
calledWithNew

// Check that spy threw an exception at least once
threw
threw("string")
threw(obj)
alwaysThrew
alwaysThrew("string")
alwaysThrew(obj)

// Check that spy returned a given object
returned(obj)

/**
 *  Methods that give metadata back about each call the spies made
 */
 
// Returns metadata about a specific call (use with convenience methods
// i.e. firstCall, secondCall, etc)
getCall(n)

// Returns array of all objects that were used as context for each call to that spy
thisValues

// Returns array of args / exceptions that were thrown
args
exceptions

// Returns array of return values that were given for each call to a spy
returnValues

// Reset spy
reset

// Debugging
printf

{% endhighlight %} 

* Sinon Call API

{% highlight javascript %}
calledOn(obj)
calledWith(args...)
calledWithExactly(args...)
calledWithMatch(args...)
notCalledWith(args...)
notCalledWithMatch(args...)
threw
threw("string")
threw(obj)
thisValue
args
exceptions
returnValue
{% endhighlight %} 

* Sinon Assertions
    * Benefit of usage: Use optionally instead of assertions that come with testing framework
    since more explicit/expressive/readable and error messages clearer upon
    assertion failure
    * Example with deliberate failure for each test as they both create the
    Sinon Spy but neither of them actually call it before asserting it was called

{% highlight javascript %}
describe("Spies", function() {
    it('should use testing framework built-in assert', function() {
        var spy = sinon.spy();
        // Note: Error message in console is just 'Expected false to be true'
        expect(spy.called).toBe(true); 
    });
    it('should use a sinon assert', function() {
        var spy = sinon.spy();
        // Note: Error message in console is clearer 'AssertError: expected spy to have
        been called at least once but was never called'
        sinon.assert.called(spy);
    });
}
{% endhighlight %} 

* Sinon Assertion API - assertions built-in to Sinon
    * Available by using `sinon.assert.___`
        * `.called`
        * `.notCalled` - spy was never called
        * `.calledOnce` - takes spy as single parameter 
        * `.calledTwice` - as above
        * `.calledThrice` - as above
        * `.callCount(spy, num` - takes the spy and a number of times we expect spy was 
        called, as parameters
        * `.callOrder(spy1, spy2...)` - checks certain set of spies called in specific order
        * `.calledOn(spy, obj)` - checks a spy was called with a given context
        * `.alwaysCalledOn(spy, obj)` - checks a spy was always called with a given context
        * `.calledWith(spy, args...)` - checks a spy was called with specific set of args
        * `.alwaysCalledWith(spy, args...)` - checks every call on a spy
        * `.neverCalledWith(spy, args...)` - checks spy was never called with given args
        * `.calledWithExact(spy, args...)` - checks spy was called with exact set of args
        * `.calledWithMatch(spy, matches...)`
        * `.alwaysCalledWithMatch(spy, matches...)`
        * `.neverCalledWithMatch(spy, matches...)`
        * `.threw(spy)` - checks spy threw an exception
        * `.threw(spy, exception)` - checks spy threw a specific exception
        * `.alwaysThrew(spy, exception)` - checks spy always threw a specific exception

* Sinon Stubs
    * Definition: 
        * Sinon Stubs are the second form of Test Double provided by SinonJS
        * Sinon Stubs are Sinon Spies that have pre-programmed behaviour
        * Sinon Stubs support Sinon Spy API + additional methods to control behaviour
        * Sinon Stubs may be anonymous or wrap existing methods
        * Sinon Stubs DO NOT call the original function, whereas Sinon Spies DO
        * Sinon Stub usage purposes:
            * Control behaviour
            * Suppress existing behaviour (since actual underlying implementation is not called)
            * Verification of behaviour (i.e. check Sinon Stubs were called in specific manner)
     
* Sinon Stub Creation (3 OFF ways)

{% highlight javascript %}
// Option 1: Create an Anonymous Function that acts as a Stub
var stub = sinon.stub();

// Option 2: Replace existing implementation instead using Stub implementation
by stubbing a given Method on a given Object
var stub = sinon.stub();

// Option 3: Replace existing implementation instead using Stub implementation
by stubbing a given Method on a given Object. Providing the stubbed implementation 
as a third param. Use this when behaviour too difficult to sufficiently specify using the
Sinon Stub API
var stub = sinon.stub();

// Option 4: // Option 1: Passing in an Object and Sinon will Stub all its Methods.
Useful when have large object with many methods we need to stub.
This simple takes a function and stubs the function (without an Overload)
since Stubs (unlike Spies) suppress the underlying implementation (so there is no point in 
providing the actual implementation as a parameter to the constructor)
var stub = sinon.stub(object);
{% endhighlight %}

* [Sinon Stub API](http://sinonjs.org/docs/#stubs)
    * `.returns(obj)` - specify that whenever call Stub it will return the Object passed as param
    * `.throws` - tells Sinon to throw general exception whenever given Stub is called
    * `.throws("type")` - tells Sinon to throw a particular type of exception whenever given Stub is called
    * `.throws(obj)` - tells Sinon to throw particular object whenever given Stub is called
    * `.withArgs(args...)` - allows customise particular Sinon Stub on a per-invocation basis
    (i.e. tell it to act a certain way when called with certain set of args)
    * `.returnsArg(index)` - tells Sinon to return the arg at index in array of provided args
    * `.callsArg(index)` - instruct Sinon to call arg at index in array of provided args
    (implies that the arg at that index is a Function)
    * `.callsArgOn(index, context)` - same as above, but can pass in context for Sinon to 
    call the Function arg at given index
    * `.callsArgWith(index, args...)` - same as above, but can pass a set of args to the
    Function arg at given index that gets called
    * `.callsArgOnWith(index, context, args...)` - same as above, but can pass in context for Sinon to 
    call the Function arg at given index, passing a given set of args to it
    * Stub method call one or more Callbacks that are passed to it (i.e. more complex than
    `callsArg`
    * See docs for other more advanced API methods

* Sinon Stub Example
    * Objective is to test using Stubs whether the Boxing class `.punch` 
    method works correctly
     * Note that implementations of methods irrelevant to testing the 
     `.punch` method are commented out as they don't matter

{% highlight javascript %}
// Use the Boxing class as the SUT
var Boxing = function() {};
Boxing.prototype.punch = function(attacker, defender) {
    // Call method to check if attacker hit defender
    if(attacker.calcHit(defender)) {
        defender.takeDamage(attacker.damage);
    }
}

// The Player class has no implementation for its methods
since we will Stub out any players we create
var Player = function() {};
Player.prototype.calcHit = function() {
    // Calculation to determine if attacker hits defender
};
Player.prototype.takeDamage = function() {};

// Test using Stubs that checks that defender should be damaged
if hit was successful
describe("boxing punch", function() {
    it('should damage defender if hit is successful', function() {
        // Setup Objects - "CREATE OBJECTS / SINON STUBS" by Create
        objects of combat and defender
        var boxing = new Boxing();
        var defender = sinon.stub(new Player());
        var attacker = sinon.stub(new Player());
        //
        // Assign damage to attacker
        attacker.damage = 5;
        //
        // Setup Behaviour - "CONTROL BEHAVIOUR of Stub" by telling it to return true whenever
        method `calcHit` is called, we do this using the `returns` method
        from Sinon Spy API, which is now attached to the `calcHit` method
        attacker.calcHit.returns(true);
        boxing.punch(attacker, defender);
        // 
        // Setup Expectation - "VERIFY BEHAVIOUR of Stub" occurs as 
        expected using Sinon Assertion API's `called` method
        expect(defender.takeDamage.called).toBe(true);
        //
        // Setup Expectation - "VERIFY BEHAVIOUR of Stub" occurs as 
        expected using Sinon Spy API's `getCall(n)` method
        and Sinon Assertion API's `calledWith(spy)`
        expect(defender.takeDamage.getCall(0).calledWith(5)).toBe(true);
    };
});
{% endhighlight %}

* Sinon Mocks
    * Definition: 
        * Sinon Mocks are the third form of Test Double provided by SinonJS
        * Sinon Mocks are like Sinon Stubs but have pre-programmed expectations
        * Sinon Mocks fail tests if pre-programmed expectations not met
        * Sinon Mocks state assertions before/upfront (instead of afterwards)
    * Guidelines
        * Try to Only use ONE Sinon Mock Object per test
        * Try to Only use 1-2 Expectations per test
        * Try to Only use 1-2 Assertions that are External to the mock
        (try to only use the Sinon Mock Object with the test, not others)

* Different Architectures between Spies, Stubs, and Mocks
    * Sinon Spy Archi - Wrap old fn with new fn and use new fn in place of old one
        * WITHOUT SPY - MyFn ----> Orig Fn
        * WITH SPY    - MyFn ----> Spy Wrap Fn ( Orig Fn + Spy API )
    * Sinon Stub Archi - Sinon takes original method on existing object, 
    and replaces reference to the original method with a brand new method,
    then set expectations (AFTER actual action takes place)
        * WITHOUT STUB - MyObj ----> Orig Fn
        * WITH STUB    - MyObj ----> Stub Fn ( + Spy API + Stub API )
    * Sinon Mock Archi - Create a mock of an object, and then create expectations
    (BEFORE actual action takes place)
    on the methods of the mocked object, such that each expectation causes the orig
    fn to be replaced with a mock fn (discarding orig implementation)
    (instead of wrapping orig fns or replacing orig fns)
            * WITHOUT STUB - MyObj ----> Orig Fn 1
                                  |
                                  '----> Orig Fn 2
            * WITH STUB    - MyObj ----> Orig Fn 1   <----------------------------------,
                              |   |                                                     |
                              |   `----> Orig Fn 2   <-------------------------------,  |
                              |                                                      |  |
                            Mock -- Orig Fn 1 Expectation ( + Spy API + Stub API )------'
                                |                                                    |
                                |__ Orig Fn 2 Expectation ( + Spy API + Stub API ) __|

* Sinon Mock Usage

{% highlight javascript %}
// Create mock object
var myMock = sinon.mock(object);

// Create expectations by calling `myMock.expects` and passing a method name
var myExpectation1 = myMock.expects("method1");
// Set expectations on myExpectation1

{% endhighlight %}

* Sinon Mock API
    * Note: Each method returns the expectation to allow chaining into compound expressions
    * `expectation.atLeast(number)` - tell mock to fail test if specific mock fn
    is not invoked at least n times
    * `expectation.atMost(number)`
    * `expectation.never` - tell mock to fail test if mock method is never called
    * `expectation.once` - tell mock to fail test if an expectation isn't called
    exactly once
    * `expectation.twice`
    * `expectation.thrice`
    * `expectation.exactly(number)`
    * `expectation.withArgs(args...)` - tell mock to fail test if mock method is
    not called at least once with given args
    * `expectation.withExactArgs(args...)`
    * `expectation.on(obj)` - tell mock to fail test if mock method is
    not called at least once with given object as the context
    * `expectation.verify` - checks that all expectations have been met, and if any
    not met then the test fails


* Sinon Mock Example
    * Objective is to test using Mock instead of Stub to assert
    whether the Boxing class `.punch` method works correctly
    * Note that either Stubs or Mocks may be used to VERIFY BEHAVIOR
    * Note that implementations of methods irrelevant to testing the 
     `.punch` method are commented out as they don't matter
    * Note: Using Stubs, Functions are replaced with Stubs in-place.
     Using Mocks however, we have a separate Mock Object that we talk to

{% highlight javascript %}
// Use the Boxing class as the SUT
var Boxing = function() {};
Boxing.prototype.punch = function(attacker, defender) {
    // Call method to check if attacker hit defender
    if(attacker.calcHit(defender)) {
        defender.takeDamage(attacker.damage);
    }
}

// The Player class has no implementation for its methods
since we will Stub out any players we create
var Player = function() {};
Player.prototype.calcHit = function() {
    // Calculation to determine if attacker hits defender
};
Player.prototype.takeDamage = function() {};

// Test using Stubs that checks that defender should be damaged
if hit was successful
describe("boxing punch", function() {
    it('STUB VERSION - should damage defender if hit is successful', function() {
        // Setup Objects - "CREATE OBJECTS / SINON STUBS" by Create
        objects of combat and defender
        var boxing = new Boxing();
        var defender = sinon.stub(new Player());
        var attacker = sinon.stub(new Player());
        //
        // Assign damage to attacker
        attacker.damage = 5;
        //
        // Setup Behaviour - "CONTROL BEHAVIOUR of Stub" by telling it to return true whenever
        method `calcHit` is called, we do this using the `returns` method
        from Sinon Spy API, which is now attached to the `calcHit` method
        attacker.calcHit.returns(true);
        boxing.punch(attacker, defender);
        // 
        // Setup Expectation - "VERIFY BEHAVIOUR of Stub" occurs as 
        expected using Sinon Assertion API's `called` method
        expect(defender.takeDamage.called).toBe(true);
        //
        // Setup Expectation - "VERIFY BEHAVIOUR of Stub" occurs as 
        expected using Sinon Spy API's `getCall(n)` method
        and Sinon Assertion API's `calledWith(spy)`
        expect(defender.takeDamage.getCall(0).calledWith(5)).toBe(true);
    };
    //
    it('MOCK VERSION - should damage defender if hit is successful', function() {
        var boxing = new Boxing();
        var defender = new Player();
        var mockDefender = sinon.mock(defender);
        var expectation = mockDefender.expects("takeDamage").once().withArgs(5);
        var attacker = sinon.stub(new Player());
        attacker.damage = 5;
        attacker.calcHit.returns(true);
        // Note: Passing in the actual attacker (not a mock)
        boxing.punch(attacker, defender); 
        // Verify that all expectations set (i.e. `.once` and `withArgs` were met
        expectation.verify();
    };
});
{% endhighlight %}

* Sinon Matchers
    * Definition: Fuzzy matching of calls to a Test Double based on args used 
     (without exactly specifying the args). Use them in place of an arg to check 
     if Test Double called correctly
        * i.e. matcher to check spy called with any number as an arg
        * i.e. matcher to check spy called with a function as an arg
    * Example usage: Create spy, call the spy, and call `calledWithMatch` method
    on the spy (passing in one or more Matchers)

{% highlight javascript %}
var spy = sinon.spy();
spy(3); // Call the spy with an arg (in this case the arg is a Number)
spy.calledWithMatch(sinon.match.number); // Number Matcher returns true if arg was a number
{% endhighlight %}

* [Sinon Matchers API](http://sinonjs.org/docs/#matchers-api)
    * `sinon.match(number)` - verifies arg matches the given number exactly
    * `sinon.match(string)` - verifies arg matches the given string or a
     substring to match the expectation provided. **SEE EXAMPLE BELOW**
    * `sinon.match(regexp)` - matches arg if was string matching the given regex
    * `sinon.match(object)` - matches arg if was object matching the given object
    * `sinon.match(function)` - Creates a **Custom Matcher**
    * `sinon.match.any` - matches any arg given
    * `sinon.match.defined` - matches any arg was anything but not undefined
    * `sinon.match.truthy` - matches any arg that is truthy
    * `sinon.match.falsy`
    * `sinon.match.bool`
    * `sinon.match.number`
    * `sinon.match.string`
    * `sinon.match.object` - matches any arg that is an object (not a primitive value)
    * `sinon.match.func` - match any arg that is a function
    * `sinon.match.array`
    * `sinon.match.regexp`
    * `sinon.match.date`
    * `sinon.match.same(ref)` - matches only if object that was passed as arg is the 
    same as the arg given to the `same` matcher. **SEE EXAMPLE BELOW**
    * `sinon.match.typeOf(type)` - matches when arg is of given type. type can be either:
        * `"undefined"`, `"null"`, `"boolean"`, `"number"`, `"string"`,
        `"object"`, `"function"`, `"array"`, `"regex"`, `"date"`
    * `sinon.match.instanceOf(type)` - requires value to be an instance of given type
    * `sinon.match.has(property[,expectation])` - matches when object of arg has
    at least one of the properties given are the same as given to `has`. 
    **SEE EXAMPLE BELOW**
    another parameter may be given as the 'value' of the property (to be more exact)
    * `sinon.match.hasOwn(property[,expectation])` -  same as `has` but property
    must be defined on the object itself, and cannot be inherited

{% highlight javascript %}
describe("matchers", function() {
    it('should work with matchers', function() {
        var spy = sinon.spy();
        spy('1234');
        spy.calledWithMatch(sinon.match('1')); // Returns true since '1' is substring of '1234'
        var o = { myProp: 1 };
        spy(o);
        spy.calledWithMatch(sinon.match.same(o));
        // Call spy with an object as arg
        spy({myProp:42, myProp2: 35});
        spy.calledWithMatch(sinon.match.has("myProp",  42)); // Returns true since has at least one of properties given to spy as arg
    }
}
{% endhighlight %}

* Cont'd
    * Custom Sinon Matcher
        * Purpose: create for flexibility when Sinon Matchers API insufficient for use case
        * Example Usage: Create custom matcher that passes "successfully" if given value is
        within a range (for example.
        * Note: second param is just a readable version of the name that is used when
        given arg fails to match against the Matcher
{% highlight javascript %}
var lessThan100 = sinon.match(function(value) {
    return value < 100;
}, "less than 100");
{% endhighlight %}

* Cont'd
    * Combining Multiple Sinon Matchers
        * Example: Support passing either a 'string' or a 'function' as first param
        of a method by using the `and` and `or` functions (available on every matcher)
        `sinon.match.func.or(sinon.match.string)`

* Mock Timers with Sinon Fake Timers
    * Similar to Jasmine's mock clock
    * Clock Object allows control time during tests with `setTimeout` and `setInterval` calls
    * Allows control of Dates (i.e. `Date.now`)
    * Integrates with jQuery animations (i.e. move forward with time to when jQuery animation callback fires)
    * Note: Import `sinon-ie` (Custom Sinon Library) when working with IE

{% highlight javascript %}
var myClass = {
    // Takes callback as param and calls it after 1s
    doTimeout: function(cb) {
        setTimeout(cb, 1000);
    },
    hide: function() {
        $("#hideMe").fadeOut();
    }
}

describe("timers", function() {
    var spy;
    // Callback function
    var cb = function() {
        console.log('cb called');
    }
    //
    beforeEach(function() {
        $("#hideMe").show();
        // Initialise to Sinon Spy that wraps around callback
        spy = sinon.spy(cb);
    });
    //
    // Handling timeouts
    it('should be able to handle timeouts', function() {
        // Initialise a Sinon Fake Timer
        var clock = sinon.useFakeTimers();
        myClass.doTimeout(spy);
        // Fire clock tick just after 1s using `tick` function on clock
        clock.tick(1010);
        expect(spy.called).toBe(true);
        // Restore clock to avoid hijacking afterwards and clean state between tests
        clock.restore();
    });
    //
    it('should be able to handle ui animations', function() {
        var clock = sinon.useFakeTimers();
        myClass.hide();
        clock.tick(1010);
        expect($("#hideMe:visible").length).toBe(0);
        clock.restore();
    }
    //
    it('should be able to fake dates', function() {
        var initialDate = 1237423755011;
        var clock = sinon.useFakeTimers(initialDate);
        var date1 = Date.now();
        console.log(date1);
        clock.tick(1000);
        var date2 = Date.now();
        console.log(date2);
        clock.restore(); // Move to `afterEach` function
    }
});
{% endhighlight %}

* Sinon Fake XMLHttpRequest (Test Double)
    * Sinon controls XHR Object in browser but not all requests will receive a response
    * Fake implementation of XMLHttpRequest (XHR) Object
    * Tests that verify/inspect each request from SUT making AJAX calls and associated responses
    * Tests that alternatively use XHR Object for canned responses
    * Create Fake Request
        * **Option 1** `useFakeXmlHttpRequest` API low-level method
        * **Option 2** `fakeServer` API higher-level method for hijacking XHR object in browser
        that sets up a pattern for responses to allow inspection of specific requests/responses

* Option 1 `useFakeXmlHttpRequest`

{% highlight javascript %}
var requests = [];
// Create Fake XHR Object
var xhr = sinon.useFakeXMLHttpRequest();
// Register Fake XHR Object to listen to when requests are made and inject them in requests array
xhr.onCreate = function(req) {
    requests.push(req);
    // AJAX Call
};
...
xhr.restore();
{% endhighlight %}

* Cont'd
    * Purpose of Test and what is being tested
        * Inspect request object (Fake XHR Object) to test code is correctly sending requests/data
        * Testing original objects but want Fake XHR Object to respond the way a server would

* FakeXMLHttpRequest API
    * Use Fake XHR Object properties to inspect the request object and asset made correctly
        * `request.url` - url that made request
        * `request.method` - http method made
        * `request.requestHeaders` - http request headers
        * `request.requestBody`
        * `request.status` - status of Response to http request depending on how Sinon Fake XHR Object setup
        * `request.statusText`
        * `request.username`
        * `request.password`
        * `request.responseXml` - may have a value based on http request headers
        * `request.responseText` - text of response if not an XML based response
        * `request.getResponseHeader(header)` - inspect specific response header
        * `request.getAllResponseHeaders()` - all response headers as string

* FakeXMLHttpRequest Response API
    * `request.respond(status, headers, body)` // status (int), headers (obj), body/data (string)
    Sinon responds when call on Fake XHR `request` object causing callbacks to fire or promises to complete
    * `request.setResponseHeaders(object)` // set request header separately
    * `request.setResponseBody(body)`

{% highlight javascript %}
describe("useFakeXMLHttpRequest", function() {
    //
    // Declare Fake XHR Object, and Requests
    var xhr, requests;
    //
    beforeEach(function() {
        // Initialise Fake XHR Object and Requests
        xhr = sinon.useFakeXMLHttpRequest();
        requests = [];
        // 
        // Event Listener to onCreate
        xhr.onCreate = function(request) {
            requests.push(request);
        }
    });
    //
    afterEach(function() {
        xhr.restore();
    });
    //
    it('should be able to handle responses', function() {
        // Create response data variable
        var responseData = '{"myData":3}';
        //
        // Make Request
        $.getJSON('some/url', function(data) { console.log(data); });
        //
        // Tell Fake XHR Request to Respond to request
        requests[0].respond(200, {"Content-Type":"application/json"}, responseData);
        //
        // Assertion
        expect(requests[0].url).toBe('some/url');
    });
});
{% endhighlight %}

* Option 2 `fakeServer`
    * Example
    
{% highlight javascript %}
// Create Fake Server
var server = sinon.fakeServer.create();
// Define how Fake Server should respond to requests
server.respondWith(response);
// Make requests
// Restore server so original XHR object is available to browser again
server.restore();
{% endhighlight %}

* Fake Server API
    * `server.respondWith(response)`
    * `server.respondWith(url, response)` - with overload for custom response to url
    * `server.respondWith(method, url, response)` - with overload for custom response to url for specific methods
    * `server.respondWith(urlRegExp, response)` - with overload, so vague as to which urls to respond to
    * `server.respondWith(method, urlRegExp, response)`
    * `server.respond()` - causes all requests to be responded to

{% highlight javascript %}
describe("fakeServer", function() {
    // Declare Fake Server
    var server;
    beforeEach(function() {
        // Initialise Fake Server
        server = sinon.fakeServer.create();
        // 
        // Tell Fake Server how to respond to requests
        server.respondWith([200, {"Content-Type":"application/json"}, '{"myData":35}']);
    });
    afterEach(function() {
        server.restore();
    });
    it('should respond with data', function() {
        // Create blank Spy
        var spy = sinon.spy();
        //
        // Make Request passing in the Spy
        $.getJSON('some/url', spy);
        //
        // Tell Fake Server to Respond to AJAX request
        server.respond();
        //
        // Assertion to check response is correct
        sinon.assert.calledWith(spy, {"myData":35});
    });
});
{% endhighlight %}

* Sandboxing
    * Post-testing ensure global objects restored to original state, including:
        * Spies, Stubs, Mocks, Fake Timers, Fake XHRs
    * Implement Sandboxing with either
        * Option 1: `sinon.sandbox.create`
        * Option 2: `sinon.test()` - use this method as wrapper to test callback

{% highlight javascript %}
// Underlying implementation
var myXhrWrapper = {
    // Show timing using console.log
    get: function() { console.log('--get function'); },
    save: function() { console.log('--save function'); }
};

describe("sandbox", function() {
    var server;
    beforeEach(function() {
    });
    afterEach(function() {
        console.log('sandbox restored');
        myXhrWrapper.get(); // Not sandboxed so has access to underlying implementation
        myXhrWrapper.save();
    });
    it('should be able to sandbox using Option 1 sinon.sandbox.create', function() {
        // Create sandbox
        var sandbox = sinon.sandbox.create(); 
        //
        // Stub out XHR Wrapper and call both its methods
        console.log('in sandboxed test');
        // Manually create Sandbox and Link the Stub to particular Sandbox
        sandbox.stub(myXhrWrapper);
        myXhrWrapper.get(); // Stubs do not use underlying implementation
        myXhrWrapper.save();
        sandbox.restore(); // Unstub the stubbed methods used in the Sandbox
    });
    //
    it('should be able to sandbox using Option 2 sinon.test wrapper fn', function() {
        console.log('in sandbox test');
        // Call `this.stub` when using sinon.test() Wrapper function
        this.stub(myXhrWrapper);
        myXhrWrapper.get();
        myXhrWrapper.save();
    });
});
{% endhighlight %}


## Links

* [TDD JS book](http://www.tddjs.com/)