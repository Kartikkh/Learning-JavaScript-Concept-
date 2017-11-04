# Node.js Concepts 

![concepts](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2015/07/1436439824nodejs-logo.png)

##### Hello Foks ! Welcome to Javascript World 

######  I have created this Node.js concept list that everyone must check before starting with Node.js or going for an interview  and I am constantly updating it with new resources and information. Please feel free to Contribute !  
---  
  
##### Callbacks
* Every function is a variable in javascript . We can pass it as we would pass a normal variable.
* So the concepts of callbacks emerged from closures or the first class functions in javascript.
* There can be two types of callbacks
&nbsp;&nbsp;&nbsp;&nbsp; i. Where functions are passed as variables
&nbsp;&nbsp;&nbsp;&nbsp; ii. Where we define Anonymous Functions as callbacks
* The concept of First Class Functions helps us to treat javascript functions as any other variable and the concept of closure helps inner function to access the variables defined inside outer function ( even if the outer function has returned. )
* So example on a callback function which is passed as a variable is
``` javascript
var myCallback  = (a,b,c,d,error) => {
// Callback logic
};
var myAJAXFunction = (http) => {
//do http call or long running task
//call callback on success or error
 .success(myCallback(data))
.error(myCallback(error));
} 
``` 
* Now anonymous callback; no need to define myCallback if any lengthy logic has to be implemented 
``` javascript
.success( (data)=>{
//Success Callback
} )
.error( (error)=>{
//Error Callback
})
```
---
#### Events and the Event Emitter
##### Self Made Events and Emitter Implementation 
_**app.js**_
``` javascript
var Emitter = require('./emitter');

var emtr = new Emitter();

emtr.on('greet', function() {
	console.log('Somewhere, someone said hello.');
});

emtr.on('greet', function() {
	console.log('A greeting occurred!');
});

console.log('Hello!');
emtr.emit('greet');
```
_**emitter.js**_
``` javascript
function Emitter() {
	this.events = {};
}

Emitter.prototype.on = function(type, listener) {
	this.events[type] = this.events[type] || [];
	this.events[type].push(listener);
}

Emitter.prototype.emit = function(type) {
	if (this.events[type]) {
		this.events[type].forEach(function(listener) {
			listener();
		});
	}
}

module.exports = Emitter;
```


###### Object.create(): is used to set up the prototype chain
For Example,
``` javascript
var Person = {
firstName: 'John',
lastName: 'Doe',
greet: function(){
return this.firstName+' ' +this.lastName;
}
};

//Use Object.create() to create a new person Object which has Person on its prototype
var Abhishek = Object.create(Person);
Abhishek.firstName = 'Abhishek'; //This hides John
Abhishek.lastName = 'Srivastava'; // This hides Doe
Abhishek.greet(); // prints Abhishek Srivastava
```
Since Person object is on the prototype of Abhishek object and Abhishek object does not have greet(), so js engine goes down the prototype chain to find greet().

##### Another Prototypal Inhertence Example using Node.js's util module

``` javascript
var EventEmitter = require('events');
var util = require('util');

function Greetr() {
	this.greeting = 'Hello world!';
}
//util.inherits adds the second object on the prototype of the first object
util.inherits(Greetr, EventEmitter); 
//EventEmitter is on Greetr's prototype

Greetr.prototype.greet = function(data) {
	console.log(this.greeting + ': ' + data);
	this.emit('greet', data);
}

var greeter1 = new Greetr();
//Now greetr1 has access to all properties of EventEmitter as well as Greetr
greeter1.on('greet', function(data) {
	console.log('Someone greeted!: ' + data);
});

greeter1.greet('Tony');
```

##### .call() and .apply()

``` javascript
var obj = {
name: 'ABhishek Srivastava',
greet: function(greeMessage){
console.log(greetMessage+' '+this.name);
}
};
obj.greet();
obj.greet.call({name:'Surbhi Srivastava'},'Study');
obj.greet.apply({name:'Surbhi Srivastava'},['Study']);
````
* ##### Difference between obj.greet.call() and obj.greet() is that using call we can pass an object and change the reference of this inside the obj.

* ##### That is this will point to {name:'Surbhi Srivastava'}. 
* ##### difference between apply and call is only that we pass params using apply in an array while in call using a comma seperated list


## Using Java Construct in Javascript and Use of .call() (which is like super())

* _**Remember in java when the class inherits another class then in the constructor of the child, first statement is always super() or super(params)**_
* Likewise in javascript when using prototypal inheritence, when we inherit we first call the super Function Constructor

For Instance
``` javascript
function Person(){
this.fname = 'John',
this.lname = 'Doe'
}

Person.prototype.greet = function(){
console.log('Hello, '+this.fname+' '+this.lname);
}

function Policeman(){
Person.call(this);
this.badgeNo = '';
}
uitl.inherit(Policeman,Person);                  // Util is Node module

var officer = new Policeman();

officer.greet();
```
The output of this code is Hello Undefined Undefined.
_Recall that when we use new to create an object from Function Constructor, this inside the Function Constructor points to {} i.e. an empty object._

The above case happens because Person never gets called so this of Person is Undefined. So to define this inside Person Function Constructor, we call the Person Function Constructor explicitely. 'this' in Person.call(this); is the empty object of Policeman which is passed to Person. So any changes done to this inside Person Function Constructor will be reflected back in Policeman Function Constructor.

---
# **First Class Functions**: We can treat Functions as Objects(or Variables). Pass them around like variables.
		Functional Expressions : `var func1 = function(var a,var b){  };`
		Functional Statements : `function(var a,var b){  };`

# **IIFEs** : Immediately Invoked Functional Expressions
``` javascript
	               (function(fname) {
                          var firstname = 'Abhishek';
                          console.log(firstname);
                    })();
                var firstname = 'Surbhi';
                cosnole.log(firstname);
```
The Output would be: 
Abhishek
Surbhi

Thus we have protected the code inside using IIFE.

# **Modules**: Separate Javascript Code.
Export the content of a module on the _**'exports'**_ property on the _**'module'**_ Object available to us. For instance 
````
	`module.exports = func1;`
    `module.exports.var1 = var1; module.exports.var2 = var2;`
````
#### Use require to get the module.exports object
_ie. when we do: var express = require('express'); then express varibale = module.exports_
## NOTE
* 1. Modules and require use PASS BY REFERENCE since js uses pass_by_reference to pass objects. So if any changes to received object object are done it shows up in the original object.
* 2. Code inside the modules is thus has to be protected against unwanted changes. For this we use IIFEs(Imediately Invoked Functional Expressions). 

## Function Constructors: a js function used to construct a js Object.
Example 
``` javascript
		var Person = function(fname,lname){
		//Initially 'this' is an empty Object
		//Now we can assign properties on 'this' empty object
		this.fname = fname;
		this.lname = lname;
		}
		
		// Create new Person objects from Person object constructor
		var Person1 = new Person('Abhishek','Srivastava'); // new creates an empty object and 'this' points to that empty object.
```
## Prototypal Inheritence : Every js Object has a prototype property on it.
For above example 
``` javascript
		Person.prototype.greetPerson = function(){
		console.log(`Hello, ${this.fname} ${this.lname}`);
		};
```
Now if we create a new Person object and call greet(), js engine will go down and search greet() inside the Person object first and if not found then go down the prototype chain.

* We can access the prototype object on person object using
			`var person2 = new Person('Abhishek','Srivastava');`
			`console.log(person2.__proto__); `
## Working of _**require()**_ and _**modules**_
``` javascript
(function(exports,require,module,__filename,__dirname){
        var greet = function(){
                console.log('Hello!');
        }
    module.exports = greet;
})
```
### The Above function is called by require() as
`fn(module.exports, require, module, filename, dirname);`
 and require returns
`return module.exports`

## Working with many modules
For Instance we can greet in english and Hindi,
so we create english.js and hindi.js inside a folder 'Languages'
and we create greet.js

### english.js
``` javascript
var english = ()=>{
console.log('Hello!);
}
module.exports = english;
```

### hindi.js
``` javascript
var hindi = ()=>{
console.log('Namastey!);
}
module.exports = hindi;
```

### greet.js
``` javascript
var english = require('./Languages/english.js');
var hindi = require('./Languages/hindi.js');
// Export the Languages
module.exports = {
english: english,
hindi: hindi
}
```

Now in the code we can do something like this
`var greet = require('greet.js');`
So, 
~~~~
greet = module.exports =  {
english: english,  // module.exports from ./Languages/english.js
hindi: hindi  // module.exports from ./Languages/hindi.js
}
~~~~
`greet.english` or `greet.hindi`

### Using JSON with _**require()**_
`var greetings = require('./greetings.json');`
The above code will convert the JSON to Js Object.
* This can be helpful if we want to bring in some data and test.

### Various Usage of module.exports
1. ##### The First Way
 _**greet.js**_
``` javascript
module.exports = function(){
    console.log('Hello from greet.js');
}
```
Use in code
`var greet = require('./greet.js');`
##### NOTE: _**greet is now a function called exports on module object and not module.exports**_

2. ##### The Second Way
 _**greet2.js**_
``` javascript
module.exports.greet = function(){
    console.log('Hello from greet2.js');
}
``` 
Use in code
`var greet2 = require('./greet2.js');`
`greet2.greet()`
OR
`require('./greet2.js').greet();`
##### NOTE: _**greet is now a new property on module.exports**_

3. ##### The Third Way - Expose the Function Constructor
 _**greet3.js**_
``` javascript
function Greet(){
    this.greeting = 'Hey There, Hello!';
    this.greet = function(message){
    console.log(message);
    }
}
module.exports = Greet;
```
Use in Code
`var Greet = require('./greet3.js')`
`new Greet.greet('Voila!')`

4. ##### The Fourth Way - Expose the new Object genrated using Function Constructor
 _**greet4.js**_
``` javascript
function Greet(){
    this.greeting = 'Hey There, Hello!';
    this.greet = function(message){
    console.log(message);
    }
}
module.exports = new Greet();
```
Use in Code
`var Greet = require('./greet3.js')`
`Greet.greet('Voila!')`

###### NOTE : Unlike Case 3 where a fresh copy of module.exports is obtained every time we do new Greet(), in this case Node CACHEs the module.exports object received by require() and every time require is called in the entire program it returns the same module.exports object even if it has changed. (See Tony Alicea Module.23 12:00)

5. ##### The Fourth Way - The Revealing Module Pattern :To expose only the methods and properties we want to
 _**greet5.js**_
``` javascript
var greeting = 'Hi!';
function greet(){
    console.log(greeting);
    }
module.exports = {
greet: greet 
};
```
Use in Code
`var greet = require('./greet5.js').greet;`
###### NOTE: here only greet method is accessible but greeting is accessible.


## exports vs module.exports
Remember that exports is the first parameter to the function inside which all the code inside of module is wrapped and this exports is same as module.exports since module.exports is passed as a parameter for exports.

When we do `exports = something` then exports no longer points to the module.exports property of module object but is a separate entity.

Since require function returns module.exports we do get what we intended.
For instance,
_**greet.js**_
``` javascript
exports = function(){
console.log('Hello!);
}
console.log(exports); // = [Function]
console.log(module.exports); // = {}
```
So, when we use
`var greeting = require('./greet.js');`
`greeting();` 
**_Gives error since greeting = module.exports = {} and not exports (or the function)_**

##### LESSON: We can use exports if we dont use '=' instead we can mutate the exports object by adding property on it.
For instance
``` javascript
exports.greet = function(){
console.log
}
```
INSTEAD OF
``` javascript
exports = function(){
console.log
}
```

---






























