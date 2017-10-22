# Javascript Concepts 

![concepts](http://www.quantiklab.com/wp-content/uploads/2016/05/ecmascript6.png)

##### Hello Foks ! Welcome to Javascript World 

######  I have created this Javascript concept list that everyone must check before starting with Javascript or going for an interview  and I am constantly updating it with new resources and information. Please feel free to Contribute !  
---  
#### Basics of Javascript

Whenever javascripts code runs, it runs inside the execution context so whenever you start to run the code. Javascript automatically create a global execution context and that global execution context create a window object inside it and every global variable created will be automatically attach to this window object.
So whenever you create a global variable or function in a .js file

```javascript 
var a =2 ; // global variable (Not inside any function)
function b(){
 var c = 1 ;  // local variable 
}
```

[Live Example](https://jsbin.com/zeyesiqoho/edit?js,console)

#### Phases in Javascripts 
In javascript there are two phases
 Creation phase
 Execution phase
So whenever Javascript starts (Creation Phase) global execution context is created and inside this Global execution context a global object is created which is (window object) , this object , and there is also a place for variable setup (Hoisting) where function are setup and variable are set up to undefined 
To try out hoisting 

``` javascript
console.log(a);
var a = "hello world"
console.log(a);
c();
function c(){
  console.log("hello");
}

```
Output for the above will be
```
undefined
hello world
hello
```
Creation Phase of above code will be 
``` javascript
var a = undefined;
function c(){
  console.log("hello");
}
console.log(a);
a="hello world"
console.log(a)
c();
```

So in Creation Phase all the variable will be defined as undefined and all the fucntion defination will be move at the top and after creation phase the code will execute line by line in execution phase. 


  
#### Hoisting
The interesting thing about these is that they are “hoisted” to the top of their scope, which means this code:
``` javascript
A();
function A(){
  console.log('foo');
};
``` 
Gets executed as this code:
```javascript
function A(){
  console.log('foo');
};
A();
```
Which practically means that, yes, you can call the functions before they’re written in your code. It won’t matter, because the entire function gets hoisted to the top of its containing scope. (This is contrasted with variables, which only have their declaration hoisted, not their contents, as we’ll see in the next section).
  
  Functions declarations must have names
This method doesn’t allow you to create anonymous functions, meaning that you always have to give it an identifier (in this case we’ve used “A”).

  ##### Functions 
   Funtion in Javascript can be define in 3 ways 
   * Function Defination
   * Function Expressions 
   * Immediately invoking Function epressions (IIFE)
 
##### Function Defination 


``` javascript
function A(){
 // write code
}; 
```

##### Immediately-invoked function expressions (IIFE)
 
 
```javascript
(function(){
    // all your code here
    
    // ...
})();

``` 

The first pair of parentheses (function(){...}) turns the code within (in this case, a function) into an expression, and the second pair of parentheses (function(){...})() calls the function that results from that evaluated expression.

This pattern is often used when trying to avoid polluting the global namespace, because all the variables used inside the IIFE (like in any other normal function) are not visible outside its scope.



##### Function expressions: 

 ```javascript
var B = function(){
//code
};
```
A function expression looks similar to function declarations, except that the function is assigned to a variable name. Though functions are not primitive values in JavaScript, this is the way they can be utilized to their full effect in this functional language. Functions are “first class”:

>[JavaScript] supports passing functions as arguments to other functions, returning them as the values from other functions, and assigning them to variables or storing them in data structures

Variable declaration hoisting
Variable declarations are hoisted to the top of their scope, somewhat similarly to function hoisting except the contents of the variable are not hoisted as well. This happens with all variables, and it means it’s now happening with our functions, now that we’re assigning them to variables.

This code:
```javascript
var A = function(){};
var B = function(){};
var C = function(){};
```
Will be executed as this:
``` javascript
var A, B, C;  // variable declarations are hoisted
A = function(){};
B = function(){};
C = function(){};
```


Therefore the order of setting and calling this type of function is important:
``` javascript
// this works
var B = function(){};
B();

// this doesn't work
B2();  // TypeError (B2 is undefined)
var B2 = function(){};
```
The second example gives us an error because only the variable B2’s declaration is hoisted, but not its definition, thus the “undefined” error.

##### [Try This To test Your Knowledge ](https://javascriptweblog.wordpress.com/2010/07/06/function-declarations-vs-function-expressions/)


