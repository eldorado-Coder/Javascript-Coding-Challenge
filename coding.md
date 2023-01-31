```jsx
console.log(-"6" + 9);
```

> Output 3

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

Consider the following code snippet

```jsx
for (var i = 0; i < 5; i++) {
  var btn = document.createElement('button');
  btn.appendChild(document.createTextNode('Button ' + i));
  btn.addEventListener('click', function(){ console.log(i); });
  document.body.appendChild(btn);
}
```

> What gets logged to the console when the user clicks on “Button 4” and why? => always 5
> Provide one or more alternate implementations that will work as expected.

```jsx
for (var i = 0; i < 5; i++) {
  var btn = document.createElement('button');
  btn.appendChild(document.createTextNode('Button ' + i));
  btn.addEventListener('click', (function (i) {
    return function (i) {console.log(i);};
  })(i));
  document.body.appendChild(btn);
}

Lastly, the simplest solution, if you’re in an ES6/ES2015 context, is to use let i instead of var i:
```

The following recursive code will cause a stack overflow if the array list is too large. How can you fix this and still retain the recursive pattern?

```jsx
var list = readHugeList();

var nextListItem = function() {
    var item = list.pop();

    if (item) {
        // process the list item...
        nextListItem(); 

    }
};

var list = readHugeList();

var nextListItem = function() {
    var item = list.pop();

    if (item) {
        // process the list item...
        setTimeout(nextListItem(), 0);
    }
};
```

What is the output of the following code?

```jsx
var a={},
    b={key:'b'},
    c={key:'c'};

a[b]=123;
a[c]=456;

console.log(a[b]);
```

> The output of this code will be 456 (not 123). When setting an object property, JavaScript will implicitly stringify the parameter value. In this case, since b and c are both objects, they will both be converted to "[object Object]". As a result, a[b] and a[c] are both equivalent to a["[object Object]"].

What is the output of the following code?

```jsx
var length = 10;
function fn() {
 console.log(this.length);
}

var obj = {
  length: 5,
  method: function(fn) {
    fn();
    arguments[0]();
  }
};

obj.method(fn, 1);
```

> 10, 2 When fn() is called inside method, which was passed the function as a parameter at the global level, this.length will have access to var length = 10 (declared globally) not length = 5 as defined in Object obj. Now, we know that we can access any number of arguments in a JavaScript function using the arguments[] array. Hence arguments[0]() is nothing but calling fn(). Inside fn now, the scope of this function becomes the arguments array, and logging the length of arguments[] will return 2.

Avoid Callback hell

```jsx
// The callback function for function
// is executed after two seconds with
// the result of addition
const add = function (a, b, callback) {
  setTimeout(() => {
    callback(a + b);
  }, 2000);
};
  
// Using nested callbacks to calculate the sum of first four natural numbers.
add(1, 2, (sum1) => {
  add(3, sum1, (sum2) => {
    add(4, sum2, (sum3) => {
      console.log(`Sum of first 4 natural 
      numbers using callback is ${sum3}`);
    });
  });
});
  
// This function returns a promise that will later be consumed to get the result of addition
const addPromise = function (a, b) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(a + b);
    }, 2000);
  });
};
  
// Consuming promises with the chaining of then() method and calculating the result
addPromise(1, 2)
  .then((sum1) => {
    return addPromise(3, sum1);
  })
  .then((sum2) => {
    return addPromise(4, sum2);
  })
  .then((sum3) => {
    console.log(
      `Sum of first 4 natural numbers using 
       promise and then() is ${sum3}`
    );
  });
  
// Calculation the result of addition by consuming the promise using async/await
(async () => {
  const sum1 = await addPromise(1, 2);
  const sum2 = await addPromise(3, sum1);
  const sum3 = await addPromise(4, sum2);
  
  console.log(
    `Sum of first 4 natural numbers using 
     promise and async/await is ${sum3}`
  );
})();
```

> we can use promise to avoid callback hell

```jsx
var name = "Bob";
function a() {
  var a = name;
  console.log("a --", a);
  var name = "John";
  var b = name;
  console.log("b -- ", b)
}
a();
```

> a -- undefined
  b --  John

```jsx
var name = "Bob";
function a() {
  var a = name;
  console.log("a --", a);
  let name = "John";
  var b = name;
  console.log("b -- ", b)
}
a();
```

> Cannot access 'name' before initialization
