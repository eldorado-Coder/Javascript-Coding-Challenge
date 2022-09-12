```jsx
var hero = {
    _name: 'John Doe',
    getSecretIdentity: function (){
        return this._name;
    }
};

var stoleSecretIdentity = hero.getSecretIdentity;

console.log(stoleSecretIdentity());
console.log(hero.getSecretIdentity());    
```
> The code will output: undefined John Doe. The first console.log prints undefined because we are extracting the method from the hero object, so stoleSecretIdentity() is being invoked in the global context (i.e., the window object) where the _name property does not exist.


```jsx
var myObject = {
    foo: "bar",
    func: function() {
        var self = this;
        console.log("outer func:  this.foo = " + this.foo);
        console.log("outer func:  self.foo = " + self.foo); 
        (function() {
            console.log("inner func:  this.foo = " + this.foo);
            console.log("inner func:  self.foo = " + self.foo);
        }());
    }
};
myObject.func();
```

> outer func:  this.foo = bar
outer func:  self.foo = bar
inner func:  this.foo = undefined
inner func:  self.foo = bar

```jsx
(function(x) {
    return (function(y) {
        console.log(x);
    })(2)
})(1);
```

> The output will be 1, even though the value of x is never set in the inner function. Here’s why:
As explained in our JavaScript Hiring Guide, a closure is a function, along with all variables or functions that were in-scope at the time that the closure was created. In JavaScript, a closure is implemented as an “inner function”; i.e., a function defined within the body of another function. An important feature of closures is that an inner function still has access to the outer function’s variables.

Therefore, in this example, since x is not defined in the inner function, the scope of the outer function is searched for a defined variable x, which is found to have a value of 1.

```jsx
var x = 0;
function f() {
  var x = y = 1; // Declares x locally; declares y globally.
}
f();

console.log(x, y); // 0 1

'use strict';

var x = 0;
function f() {
  var x = y = 1; // Throws a ReferenceError in strict mode.
}
f();

console.log(x, y);
```

```jsx
var x = 21;
var girl = function () {
    console.log(x);
    var x = 20;
};
girl ();
```

> x = undefined

```jsx
for (let i = 0; i < 5; i++) {
  setTimeout(function() { console.log(i); }, i * 1000 );
}
```

> It will print 0 1 2 3 4, because we use let instead of var here. The variable i is only seen in the for loop’s block scope.

```jsx
for (var i = 0; i < 5; i++) {
	setTimeout(function() { console.log(i); }, i * 1000 );
}

for (var i = 0; i < 5; i++) {
    (function(x) {
        setTimeout(function() { console.log(x); }, x * 1000 );
    })(i);
}
```

> The code sample shown will not display the values 0, 1, 2, 3, and 4 as might be expected; rather, it will display 5, 5, 5, 5, and 5.

The reason for this is that each function executed within the loop will be executed after the entire loop has completed and all will therefore reference the last value stored in i, which was 5.
This will produce the presumably desired result of logging 0, 1, 2, 3, and 4 to the console.



```jsx
console.log(1 < 2 < 3);
console.log(3 > 2 > 1);
```

> true < 3 => true; true > 1 => false;


```jsx
console.log(typeof NaN === "number");  // logs "true"
console.log(NaN === NaN);  // logs "false"
```
> NaN compared to anything – even itself! – is false:

Discuss possible ways to write a function isInteger(x) that determines if x is an integer

```jsx
function isInteger (x) {return (x ^ 0) === x; }
```

Shallow copy and deep copy

```jsx
let obj = {
    a: 1,
    b: 2,
    c: {
        age: 30
    }
};

var objclone = Object.assign({},obj);
console.log('objclone: ', objclone);

obj.c.age = 45;
console.log('After Change - obj: ', obj);           // 45 - This also changes
console.log('After Change - objclone: ', objclone); // 45
```
> Note the potential pitfall, though: Object.assign() will just do a shallow copy, not a deep copy. This means that nested objects aren’t copied. They still refer to the same nested objects as the original:

```jsx
const _ = require('lodash');
   
var obj = {
    x: 23
};
   
// Deep copy
var deepCopy = _.cloneDeep(obj);
```
Write a sum method which will work properly when invoked using either syntax below.

console.log(sum(2,3));   // Outputs 5
console.log(sum(2)(3));  // Outputs 5

```jsx
function sum (x) {
    if (arguments.length === 2) return arguments[0] + arguments[1];
    else return function (y) {
        return x + y;
    }

}
```