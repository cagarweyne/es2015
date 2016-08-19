#ES2015 JavaScript reloaded

JavaScript has undergone some major changes in the latest update to the language, which was finalized in June 2015. You might have heard it by the name ES6 but this is the old name. The committee decided to change from version number to year of release to better reflect their plans to release a new version every year which is great news for JavaScript developers. In case you're not familiar, JavaScript is also called ECMAScript, and the current version which is supported by all modern browsers is ECMAScript 5.1, which was finalized in 2011. The name is also abbreviated to ES5 and as such you will see that the latest version being called ES6. 

The name of the standard versioning of ES2015 was agreed, because the committe realized that they didn't want to limit the release of a standard update to once every few years, rather, suggestions were made that versioning will be based on year of release. For example the current version was finalized in June 2015, so the version will be called ES2015 to refer to the year in which the standard was finalized. All future versioning will be referred to in this way, ES20xx. 

#How to run ES2015

Running the latest and greatest version of JavaScript can pose a problem for the JS developers that want to make use of the new features made available in the new standard, whilst at the same making making sure that their websites and apps support older browsers who have yet to implement the new features. Fortunately, there is a workaround that will enable developers to make use of new features and still run their sites and apps on older browsers, transpiling. 

Transpiling is the process of taking one language and translating it to another similar language, in this case it's going from ES2015 (ES6) to ES5. This is the perfect answer to run the latest and greatest version of JavaScript on browsers that haven't implemented the standard yet. The process of transpiling is basically taking your ES2015 code and translating it to its equivalent in ES5. One prime example is the way property definitions can be done in ES2015, for example: 

``` javascript
var name = "Abdi"; 

var myObj = {
  name
};

```

In ES2015, you can shorten the definition of the object properties if the key value pairs names are exactly the same. Once this code is transpiled, it will be translated into it's equivalent or close counterpart in ES2015. So, in our example above, the code will be transpiled to: 

```javascript 

var name = "Abdi"; 

var myObj = {
  name: name
};

```

A great tool that will help you to transpile your code into ES5 is [Babel](https://babeljs.io/), which, as it's motto says, will let you use next generation JavaScript today. However, not all the new features in JavaScript can be simply transpiled into its equivalent in ES5, because there are some features that aren't simply availabel, even if you transpile the code. The solution to this problem is: Shims. 

#Shims/Polyfills

In order to make use of new features that cannot be achieved with transpiling, we make user of shims to mimic the same behavior in older versions of JavaScript. There are many new features introduced that require a shim in order to work properly, and these are included within the tooling that we can use to run ES2015 in older browsers. Just in case, you're wondering what a shim is, it is basically a small piece of code that imeplements the feature which is missing from an older environment. for example, in ES2015 it introduces new API's to check strict equality of two values called `Object.is()`. This is a feature that is not available in all browsers, so to get around this feature, we can use a shim to make use of this feature in older browsers, the code for this shim is pretty straight forward and you can get more details at the [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is) documentation: 

```javascript
if (!Object.is) {
  Object.is = function(x, y) {
    if (x === y) { 
      return x !== 0 || 1 / x === 1 / y;
    } else {
      return x !== x && y !== y;
    }
  };
}
```

The outer `if` statement checks to see whether the API is available, and this will only return true if it isn't implemented in the browser's JavaScript engine. There are many shims that you can use as a fallback behavior for older evironments and fortunately most of these shims are available in Babel. With the combination of transpiling and Shims/Polyfills, we can make use of the newesta and best features of JavaScript without worrying about backwards compatibility with older versions of JavaScript. This truly spells an exciting time for JS developers!

#Block scope declarations with `let` 

One of the key features of the new JavaScript update is the ability to bind your variables to a scope. Before ES2015, you had to create an immediatelt invoked function expression to create "private" variables that weren't available outside the block in which they were declared. In ES2015 we can now create variables that are bound to blocks using `let`. The wonderful thing about using this feature is that all you need is a block scoping, so in other words all you need are a pair of curly braces `{  }`. So let's have a look at an example: 

```javascript 
  function showNamesLength(names) {
    if(names.length > 5) {
      let moreThanFive = 'there are more than 5 names';
      alert(moreThanFive);
      //other code......
    } else {
      let lessThanFive = 'there are less than 5 names';
      console.log(moreThanFive); //moreThanFive - causes reference error
      alert(lessThanFive);
      //other
    }
    
    //both let variables don't exist here either
  }
  
  showNamesLength(["Abdi", "Cagarweyne", "John", "Nick", "Kieran"]);// throws reference error on line 66
```
The `let` varaiables are scoped to the block in which they were declared and cannot be accessed any where else. If you are familiar with other programming languages, then this might not mean much to you, but in the previous version all variable declaration were hoisted to the top by the JS engine.Let's look at an example to see how `let` behaves differently to using `var`. If we use the same function, but this time use `var` instead of `let`: 

```javascript 
  function showNamesLength(names) {
    if(names.length > 5) {
      var moreThanFive = 'there are more than 5 names';
      alert(moreThanFive);
      //other code......
    } else {
      var lessThanFive = 'there are less than 5 names';
      console.log(moreThanFive); //moreThanFive - returns undefined
      alert(lessThanFive);
      //other
    }
    
    //both var variables available here
  }
  
  showNamesLength(["Abdi", "Cagarweyne", "John", "Nick", "Kieran"]);// console logs undefined
```

If we run this function, you will see that it doesn't throw a reference error, and instead writes to the console `undefined`. This is because when we use `var` instead of `let` all of the variables get hoisted up to the top of the function before the code is executed line by line. All variables get assigned undefined until they are explicitly assigned a value in the code. Since the code inside the `if` block doesn't get executed the variable moreThanFive doesn't get assigned the string, it stll contains undefined and this is what is printed out to the console when the line of code: `console.log(moreThanfive)` is executed.  

##Using `let` in for loops

One very useful feature of using `let` is when you use it with for loops. To demonstrate this, we'll use an example that you have probably seen when concering closures in JS: 

```javascript 
var funcs = []; 

for(var i=0;i<5;i++) {
  funcs.push(function() { console.log(i) });
}

funcs[0](); //logs 5
funcs[1](); //logs 5
funcs[4](); //logs 5

```

When we use `var` in a for loop like this in a closure, each function will close over the last instance of the value in `i` since there is only one `i` available in the scope. Hence the reason why when we invoke any of the functions in the array, 5 is logged to the console, since this is the last variable in which the functions closed over after the test for `i<5` returned false.

Using `let` instead of `var` in this case solves the problem nicely and the functions behave as you would expect. Let's change the `var` to `let` in the same code above and see what happens when we invoke any of the functions in the array: 

```javascript 
var funcs = []; 

for(let i=0;i<5;i++) {
  funcs.push(function() { console.log(i) });
}

funcs[0](); //this logs 0 to the console.
funcs[2](); //this logs 2 to the console.
funcs[4](); //this logs 4 to the console.

```
Using `let` solves this problem of shared variable in the scope, `i` is redclared for each iteration of the loop. this means that each function that is pushed to the funcs array will close over that instance of `i` and not the last value that is assigned before the iteration comes to an end. 

##A few gotchas with `let` that are worth noting

As we have seen `let` can be very useful in some instances, but there are a few things that we need to be aware of when using it: 

####You cannot access a `let` variable earlier than its declaration 

Coming from ES5, we are used to the fact that `var` declared variables are hoisted to the top and are attached to the entire enclosing function. Let's take a look at what we mean in code: 

```javascript 

console.log(foo); //logs undefined
console.log(bar); //this is a ReferenceError

var foo = "hello";

let bar = "world"; 

```
Accessing a `let` variable earlier than it's declaration generates a ReferenceError, this is also referred to as a Temporal Dead Zone (TDZ). TDZ is not an offical term form the ECMASCript specificaton, but it is a name given by the JavaScript community to describe the behavior of non-hoisting variables of `let` and also, as we will come to see next, `const`.


####You cannot redclare `let` twice in the same scope or sharing the same identifier 

declaring a `let` variable twice in the same scope will generate an error, let' see what we mean: 

```javascript 

let foo = "hello"; 

let foo = "world"; //generates an error: SyntaxError: Identifier 'foo' has already been declared

```

Likewise, if we try to use the same identifier but for a `let` variable it throws an error: 

```javascript

var foo = "hello";

let foo = "world"; //generates error: SyntaxError: Identifier 'foo' has already been declared

```

#`const` 

Another great addition to the JS language is the variable `const` declaration. `const` creates constants and this means that value cannot be changed once it has been set. This is something that was badly needed in JS, as developers had to bec careful that they didn't reset a constant further down their program code. When using `const` you must initialize it on declaration, so if you just declare a `const` like this: 

```javascript
const API_KEY; //error: SyntaxError: Missing initializer in const declaration
```
This causes a syntax, and in the error message it tells exactly why the error occured. So the correct way to use a `const` is to initialize it on declaration like so: 

```javascript 
const API_KEY = '123'; //no error this time
```
One of the key uses of `const` is that the value cannot be reassigned after its declaration, and this is something that is desriable  if you wanted a variable to never be reassigned inadvertently later on. So if you create `const` and then try to reassign it with another value it will throw an error: 

```javascript
const name = 'Daniel';

name = 'Brian'; //throws a TypeError: Assignment to constant variable
```
Howver, if you created a `const` that holds an object, like an array or object, you can change the object can be changed without a problem: 

```javascript 

const names = ['Liam', 'Daniel'];

names.push('Abdi');//works as expected without throwing error

console.log(names[2]); //prints 'Abdi'
```
`const`s share a few similarites with `let` in that they are also block scoped, so this means that they are not accessible outside their block scope. Also, you cannot use the same identifier that has eithe been used with a `var` or `let` variable. 

#Functions 

Functions are one of the foundations of the JS language, and it is no surprise that functions have been updated in the latest edition of JS. One of the cool features of functions in ES2015 is the addition of default parameters. 

##Default parameters 

To make functions more robust and not break when they are called without their expected paramters, we would normally add in default values in the body of the function to make sure that our function didn't break. Let's take a simple function that will take 3 parameters and do some computation based on the passed in values or the default fall back ones set in the function body: 

```javascript 
function box(height, color, url) {
  var height = height || 50, 
      color = color || 'blue'
      url = url || 'google.com';
      
  console.log('height is:', height, 'color is:', color, 'url is:', url);     
}

box(75, 'green', 'fullstackstudent.com')// logs: height is: 75 color is: green url is: fullstackstudent.com

box(); //logs the default values: height is: 50 color is: blue url is: google.com

```
The above function works fine most circumstances but requires some brain power in order to undertsand what is happening at the top of the function with the default assignments. Also, your function could run into problems when a value that is deemed falsy is passed in as the parameter to the function, for example if we pass in 0 as the height: 

```javascript 
box(0, 'yellow', 'wikipedia.org'); // this logs: height is: 50 color is: yellow url is: wikipedia.org
```
When passing in 0 as the height, the value is evalluated to be falsy, so the right side of the || operation is executed and the height value is assigned to be 50 instead of 0, which is what we intended. We can account for this scenario by doing further checks in the code, but this means more work and more code that has to be written. For example: 

```javascript 
function box(height, color, url) {
  var height = (height !==undefined) ? height : 50, 
      color = color || 'blue'
      url = url || 'google.com';
      
  console.log('height is:', height, 'color is:', color, 'url is:', url);     
}

box(0, 'yellow', 'wikipedia.org'); //behaves as expected and logs: height is: 0 color is: yellow url is: wikipedia.org
```

In ES2015 you can create default paramaters on function declaration using a simple syntax and that will also resolve our issue when falsy values are passed in as parameters: 

```javascript 
function box(height = 50, color = 'blue', url = 'google.com') {
  console.log('height is:', height, 'color is:', color, 'url is:', url);     
}

box(0); //logs: height is: 0 color is: blue url is: google.com
```
You will notice that the above function declaration is terse, in that the default parameters are actually assigned when setting the parameters themselves and you will also notice that when we pass in 0 as the first argument it isn't taken as falsy value and the default parameter for height is not used. The default assignment for the height in ES2015 equivalent to the explicit checking of the parameter to see if it is undefined, this is something that you don't have to do yourself anymore. 

###Expressions as default values 

In ES2015 you can use any valid expression as the default values, including function calls. Let's create a function call as  default value for our height parameter: 

```javascript 

function setHeight(value) {
  return 10 * value; 
}


function box(height = setHeight(10), color = 'blue', url = 'google.com') {
  console.log('height is:', height, 'color is:', color, 'url is:', url);     
}

box();//logs: height is: 100 color is: blue url is: google.com
```

This is very interesting, because it means that you can invoke other functions that return values when you declare the function that will use that return value. This makes for a powerful default parameter structure in ES2015. Also, you can use a previous parameter as the default paramter for any subsequent parameters: 

```javascript 
function multiply(num1, num2 = num1) {
  return num1 * num2; 
}

console.lo(multiply(2));//returns 4
```
The above multiply function takes two arguments and sets the second parameters default value to be the first. Note that this will only work for parameters that come after the first one. For example, you can't make the default value for the first parameter to be the second parameter: 

```javascript 
function multiply(num1 = num2, num2) {
    return num1 * num2;
}

console.log(multiply(3, 3));     // 9
console.log(multiply(1));        // throws error
```
###Rest

So far we have seen some really cool additions to the language in ES2015 and the spread and rest operators are another great feature added to JS. Let's start with the rest operator first, to understand why it's useful we need to look at the arguments object that is available inside all functions. You can examine all of the parameters passed to function in the `arguments` object, this is an object that behaves very much like an array, but is actually an object. Let's take a look at an example of using the `atguments` object in a function in ES5: 

```javascript 
function sum() {
  //convert arguments object into an array using the the array slice method
   var numbers = Array.prototype.slice.call(arguments);
   var result = 0;
   
   //loop over the numebrs array and add each index value to result on each iteration
   numbers.forEach(function (number) {
       result += number;
   });
   return result;
}
console.log(sum(1));             // 1
console.log(sum(1, 2, 3, 4, 5)); // 15
```
In the sum function above it’s not at all obvious that the function can handle more than one parameter. You could define several more parameters, but you would still not know that this function can take any number of parameters and is a variadic function. If there are complex multiple parameters such as objects and arrays, you have to keep track of where to start your index when looping over the arguments object. As you can see this is a little cumbersome and the function can break if we pass it multiple parameters that aren't the correct type. 

ES2015 introduced a new operator called rest parameters to help us access the parameters passed to a function. Let's look at it in action to get a better understading: 

```javascript 
function sum(…numbers) {
  let result = 0;
  numbers.forEach(function (number) {
    result += number;
  });
  return result;
}
console.log(sum(1)); // 1
console.log(sum(1, 2, 3, 4, 5)); // 15
```
 The 3 dots before the parameter indicate that all of the arguments that are passed into the function when it's call should be gathered in ann array called numbers. This is a real array and you can use all of the normal array methods on it, in this case we are using the `forEach` array method to access each index value and add it to the result variable. This is a much easier and cleaner to access parameters in a function, and from the function declaration we can see that this function accepts any number of arguments and that these arguments will be gathered into an array called numbers. 

There is a key restriction when using rest parameters in your functions, and that it no other named arguments can follow in the function declaration. This means that you can't have something like this: 

```javascript 
function sum(…numbers, last) { // causes a syntax error
  var result = 0;
  numbers.forEach(function (number) {
    result += number;
  });
  return result;
}
```

However, if the rest parameter is the last argument to be passed in, then this will work fine and will gather all of the arguments into an array: 

```javascript 
function sum(first, second, ...numbers) { 
  var result = 0;
  numbers.forEach(function (number) {
    result += number;
  });
  return result;
}
\
sum('first', 'second', 2,2,3,4,5); //works just fine
```

###Spread 

The spread operator uses the exact same syntax, and you could think of it as acting completely oppositely to the rest operator. Whereas the rest operator gathers all of the parameters passed in to the function into an array, the spread operator spreads out the contents of an array into individual values. The spread operator uses the same 3 dots to achieve this: 

```javascript 
let numbers = [1, 2, 3]; 

function sum(num1, num2, num3) {
  return num1 + num2 + num3; 
}

sum(...numbers); 

```
The spread operator uses exactly the same syntax as the rest operator, however, it behaves differently depending on where it is used. Here we are calling the sum function and passing it our arguments using the spread operator. The operator spreads out each of the contents into individual parts and then the function merely adds up these individual values. You could think of it as acting like the `apply` method on arrays. So, if we used `apply` instead we would get the same effect: 

```javascript 
let numbers = [1, 2, 3]; 

function sum(num1, num2, num3) {
  return num1 + num2 + num3; 
}

sum.apply(null, numbers); 
```
You can also use the spread operator in place of the `concat` method: 

```javascript 
let a = [2,3,4];
let b = [ 1, ...a, 5 ];

console.log( b ); // [1,2,3,4,5]
```

###Arrow functions

Another fantastic addition to the language are arrow functions. If you are familiar with CoffeeScript the this is a great addition for you. Not only do fat arrows make your code more terse, but they also help address one of the most confusing parts of JS, the `this` context. Arrow functions are incfedibly easy to create and at first glance you would have to look carefully to understand that it is in fact a function: 

```javascript 
var foo = (a, b) => a * b; 
```
Yes, the above is a function, and you can even make it more terse, by removing the parenthesis if you only have one parameter: 

```javascript 
var bar = a => a * 5; 
```
The above are examples of valid function ES2015, the arrow definition for function consists of the parameters if there are any and then followed by the curly braces {  }. The curly braces are required when there is more than one expression. For example: 

```javascript 
var foo = (a, b) => {
  let counter = 0; 
  
  for(let i =0;i<b;i++) {
    counter += a; 
  }
  
  return counter; 
}
```

####Arrow functions are always expressions

A seasoned JS developer will know the difference between a function declaration and a function expression. Just to recap, a function declaration is a name function variable that isn't assigned to a variable: 

```javascript 
//function declaration
function foo() {
  console.log('foo'); 
}
```

And a function expression uses a variable assignment: 

```javascript
//anonymous function expression
var foo = function() {
  console.log('foo');
}
```
It is worth keeping in mind that arrow functions will always be function expressions and are also anonymous function expressions. 

####Binding of `this` with arrrow functions

If you have done any noteworthy coding with JS, then you will have come across the issue with the `this` keyword. This was a particular point of confusion when it comes to programming wit the `this` keyword and has generated a lot of complaints from the developer community. As a result, the ES2015 implementation addressed this issue with the introduction of the arrow functions. To understand what the problem with the `this` keyword was in ES5, let's have a look at an example: 

```javascript
var person = {
	firstname: "Abdi", 
	lastname: "Cagarweyene",
	getFullName: function() {
		console.log(this.firstname, this.lastname);
	}
}

person.getFullName();//prints Abdi Cagarweyne
```
The above object gas has two properties and a method, which simply logs to the console the `firstname` and `lastname` propertis. This is all well and good and the `this` context works just fine. However, you will run into problems when you change contexts or closures come into the equation, for example: 

```javascript 
var person = {
  firstname: "Abdi", 
  lastname: "Cagarweyene",
    getFullName: function() {
       var name = function() {
       console.log(this.firstname, this.lastname);
       }
		
       return name(); 
		
  }
}

person.getFullName();//undefined undefined
```
When we call the getFullName method, we get `undefined undefined`, this is the not the behavior that we expected and this is one of the quirks of JS. To fix this we have two options, we can use the `var self = this` hack: 

```javascript 
var person = {
  firstname: "Abdi", 
  lastname: "Cagarweyene",
    getFullName: function() {
       var self = this; 
       var name = function() {
       console.log(self.firstname, self.lastname);
       }
		
       return name(); 
		
  }
}

person.getFullName();//Abdi Cagarweyne
```

When we reference the context of this by assigning it to a variable inside the outer function, we will have access to inside the inner functions via a closure. So, in order to keep the context we simply use the `self` instead of the function's `this` keyword, which points to the global window object in the example above. 

We can also fix the problem with `this` using the `bind` method that is available on functions:

```javascript 
var person = {
  firstname: "Abdi", 
  lastname: "Cagarweyene",
    getFullName: function() {
       var name = function() {
       console.log(self.firstname, self.lastname);
       }.bind(this)
		
       return name(); 
		
  }
}

person.getFullName();//Abdi Cagarweyne
```

ES2015 enables us to eliminate much of the frustrations with the `this` keyword in JS by using fat arrow functions. So, we can rewrite the above examples using arrow functions and also address the issue of the value of `this`: 

```javascript 

var person = {
  firstname: "Abdi", 
  lastname: "Cagarweyene",
  getFullName: function() {
	var name = () => {
	  console.log(this.firstname, this.lastname);
	}
		
	return name(); 
		
  }
}

person.getFullName();//Abdi Cagarweyne 
```
By using the arrow function instead of a normal syntax with the `function` keyword, we get the behavior that is desired. This is because the `this` keyword binding in arrow functions is lexical. This means that arrow functions do not have a stand-alone `this` value binding, rather the value is decided by the closest containing non-arrow function and in our example that is the getFullName function. So, essentially the fat arrow `=>` replaces the `var self  this` or `bind(this)`.

#Objects, Strings and Object.assign 

ES2015 has added a shorter syntax for creating object literals, in ES5 you would typicaly see something like this: 

```javascript 
function createUser(first, last){
  let fullName = first + " " + last;
  return { first: first, last: last, fullName: fullName }
}

//calling the build user fuction 
let user = createUser("Abdi", "Cagarweyne");
console.log( user.first );//Abdi
console.log( user.last );//Abdi Ahmed
console.log( user.fullName );//Abdi Ahmed
```
However, as you can see returning objects with keys and variables with the same name looks repetitive and in ES2015 you can use the shorthand to initialize objects: 

```javascript 
function createUser(first, last){
  let fullName = first + " " + last;
  return { first, last, fullName }//this is equivalent to: return { first: first, last: last, fullName: fullName }
}

//calling the build user fuction 
let user = createUser("Abdi", "Cagarweyne");
console.log( user.first );//Abdi
console.log( user.last );//Abdi Ahmed
console.log( user.fullName );//Abdi Ahmed
```
As you can see this is much cleaner and means that you have less writing to do when initialize objects, if the key and value variables both have the same name, then you can use the shorthand method to make your code more terse and less repetitive. The object initializer shorthand works anywhere a new object is returned for example: 

```javascript 
let name = "David";
let age = 45;
let colleagues = ["George","Michelle"];
let user = { name, age, colleagues };//this is the same as: let user = { name: name, age: age, friends: friends };

console.log( user.name );//Davide
console.log( user.age );//45
console.log( user.colleagues );//["George","Michelle"]
```

##Object Destructuring 

ES2015 introduces anothe really cool feature when it comes to assigning values attached to an object or in an array. If we had an array that contained values which we wanted to assign to individual variables we would do the following in ES5: 

```javascript 
let nums = [1, 2, 3]; 

let a = nums[0], b = nums[1], c = nums[2]; 

console.log(a, b, c); //prints 1 2 3 to the console 
```
To get the values into individual we had to access each value in the index and assign it to a variable. However, we can achieve the same result by using destructuring in ES2015: 

```javascript 
let nums = [1, 2, 3]; 
let [a, b, c] = nums; 

console.log(a, b, c)
```
This looks much cleaner than the previous version and means that you have less code to write. The same can also be done with objects, for example: 

```javascript 
let obj = {
  x: 7, 
  y: 8, 
  z: 9
}

let x = obj.x, y = obj.y, z = obj.z; 
console.log(x, y, z);//prinst 7 8 9 to the console
```

This becomes:

```javascript 
let obj = {
  x: 7, 
  y: 8, 
  z: 9
}

let { x, y, z } = obj; 
console.log(x, y, z);//prinst 7 8 9 to the console
```

This destructuring process might seem confusing at first, as you are used to seeing syntax like `[ a, b, c]` or `{ x, y, z }` on the right hand side instead of the left. However, what is happeining here is that the pattern has been flipped, so that when you have `[ a, b, c]` or `{ x, y, z }` on the left hand side of the assignment it means that detach all of the values corresponding on the left handside from the right. Obvisouly, for this to work in object destructuring the variables that you are assigning the values to must match the keys of the object that you are destructuring. For example, the following will not work: 

```javascript 
let obj = {
  a: 7, 
  b: 8, 
  c: 9
}

let { x, y, z } = obj; 
console.log(x, y, z);//prints: undefined undefined undefined
```
Also for array destructuring, this will only work if the values on the right are the same number or less than the length of the array, for example: 

```javascript 
let nums = [1, 2, 3]; 
let [a] = nums; //only the first element will be assigned to the variable a

console.log(a)//1
```
This results in undefined for variables `b` and `c`: 

```javascript 
let nums = [1]; 
let [a, b, c] = nums; //b and c are assigned undefined since there isn't a corressponding value in array

console.log(a, b, c)//1 undefined undefined
```

##Adding functions to an Object 

Adding functions to an object is something that is done all of the time, and in ES6 it has been made simpler to add a function to an object. Previously, we would declare the property of the object and then use th full function declaration to add a method: 

```javascript 

let myObj = {
  prop1: 'Hello', 
  prop2: 'world', 
  fullName: function(firstname, lastname) {
  	let fullName = firstname + ' ' + lastname; 
  	return fullName; 
  }
}

```
In ES6 the syntax is shortand and made simpler by just declaring the object property followed by the parenthesis and the function body: 

```javascript 

let myObj = {
  prop1: 'Hello', 
  prop2: 'world', 
  fullName(firstname, lastname) {
  	let fullName = firstname + ' ' + lastname; 
  	return fullName; 
  }
}

```
##Template strings

Another great feature added to ES6 is the use of ttenplate strings. Template strings allow embedded expressions and you can use multi-line strings and string interpolation features with them. Let's have look at some examples to understand better. Say you have a url where you are posting some data and you want to add interpolation with a service id, in ES5 this would be done like this:

```javascript 

let url = "/service/"+servid

```
In ES6, you can just use backticks to surround your whole string and add interpolation using the dollar sign and curly braces: 

```javascript 

let url = `/service/${servid}`

```

This is much cleaner and means that you can actually create complex strings with interpolation without having to use plus signs, line breaks, multiple double quotes etc. If you have ever needed to create multi-line strings before you would have to use line breaks to achieve this. However, in ES6 you simply use the backticks and continue writing you string on the new line without the need for a line break. Let's look at how this is done in ES5 first and then using template strings in ES6: 

```javascript 

//ES5 
console.log("string text line 1\n"+
"string text line 2");
// "string text line 1
// string text line 2"


//ES6 

console.log(`string text line 1
string text line 2`);
// "string text line 1
// string text line 2"

```

##Object.assign

As a developer, writng flexible and reusabel functions is something that we all must strive to do and the new feature in ES6 object.assign helps in that regard. Let's look at an example scenario where using object.assign will be useful for us. Say we're creating a function that accepts an options parameter, in that it can take 0 or more options as properties, and depending on the options supplied the function will return a different value: 

```javascript 

function person(name, options = {}) {
    let age = options.age || 'at least 18';
    let address = options.address || 'Shared accommodation'; 
    let occupation = options.occupation || 'Student'; 
    
  return `${name} is ${age} and is currently at ${address} 
  and their occupation is ${occupation}`;
}

```
When calling this function some options might not be specified, so this means that we need to account for this using default values. As you can see from the function, we assign each property of the options object to a variable and then use the double pipe to check for the presence of a value that has been passed in. If there is no value it will return undefined and then we fall back to the default values.

the code above is fine as it is, but it requires a bit of brain power to understand and in the long term may be difficult to maintain. let's fix this function using a defaults object and the new feature object.assign: 

```javascript 

function person(name, options = {}) {
  let defaults = {
    age: 'at least 18', 
    address: 'Shared accommodation', 
    occupation: 'Student'
  }
    
  return `${name} is ${defaults.age} and is currently at ${defaults.address} 
  and their occupation is ${defaults.occupation}`;
}

```
Now that we have improved upon our function, the next step is to merge options and defaults, and where there are duplicate, those from options must override the properties in the defaults object. This is where the object.assign feature in ES6 comes in really useful to help us. 

The object.assign feature helps us to copy one or more source objects into a target object and retunrs the target object. The way in which the function works is that it takes a target object as the first argument and then takes any number of subsequent arguments as the source object from which to copy the properties. So the function looks like this: `Object.assign(target, source_1, ..., source_n)`. 

If Object.assign encounters duplicate properties on source objects, the value from the last source object will always be returned. So, if a property foo was already in the target object and then in the last source there is also another property foo, then this last property will overwrite the last one and this is the one that gets returned. 

Let's now use the Object.assign function to merge the defaults object with the passed in options object and see the Object.assign in action. 

```javascript 

function person(name, options = {}) {
  let defaults = {
    age: 'at least 18', 
    address: 'Shared accommodation', 
    occupation: 'Student'
  }
  
  let finalOptions = Object.assign({}, defaults, options); 
    
  return `${name} is ${finalOptions.age} and is currently at ${finalOptions.address} 
  and their occupation is ${finalOptions.occupation}`;
}

person('abdi', {age: 30});

```

The way in which you use Object.assign is very straight forward, you create a variable that will hold the return value of the target object and you pass an empty object as the first argument then followed by the source objects that you wish to copy from: 

`let finalOptions = Object.assign({}, defaults, options)`.

One thing to note is that we haven't used the defaults object as the target object, because if we did we would be mutating the defaults object and we would  not have access to the original values anymore. It's important that we do not mutate the defaults object because if we wanted to compare the defaults value with the passed in options value then we would have nothing to compare with. 

Also, notice how there is a variable declared to hold the return  value from the Object.assign function, we could have written it like this: 

```javascript 
let finalOptions; 
Object.assign(finalOptions, defaults, options); 
```
If we wrote it like this then we would not be using the return value from the function, and even though it would still work, the correct way is to assign the return value directly to the `finalOptions` variable. 

##Arrays

###Assigning with Array desctructuring 

Arrays are an important data type that is used extensively, and it is not uncommon to access elements by their index. For example, say we have an array of fruits: 

```javascript 
let fruits = ['apple', 'grapes', 'banana']; 

let a = fruits[0]; 
let b = fruits[1]; 
let c = fruits[2]; 

console.log(a, b, c);

```

As you can see from the code above, we can assign each fruit to its own variable using their indices 0, 1, and 2. This is perfectly fine and works, but it is more code than we actually need and if we had more elements in the array we would need to know the index fo each element in order to assign it to a variable, which means that this doesn't scale very well. 

In ES6, we can assign each fruit to a variable using what's called array destructuring. Array destructuring allows us to write the code in a much better way, similar to the way in which we destructure an object, we can destructure an array. So let's rewrite the example above using array destructuring: 

```javascript 
let fruits = ['apple', 'grapes', 'banana']; 

let [a, b, c ] = fruits; 

console.log(a, b, c);//apple grapes banana
```
Instead of accessing elements by their index, we declare a single line of code between square brackets and assign them to the variables on the left. The JS engine will try to match the number of variables on the left to the number of elements in the array. As you can see from the code above, we assigned the variables a, b and c the values apples, grapes and banana respectively. This code is actually easier to undertand and requires less code. 

If there are any values that we aren't interested in, we can discard them during the assignment operation: 

```javascipt 
let fruits = ['apple', 'grapes', 'banana']; 

let [a, , b ] = fruits; 
console.log(a, b) //apple banana
```
In the exampe above we only store apple and banana into the variables and we have left out grapes. We achieved this using a blank space after the first variable to indicate that we don't want the second element assigned to any variable. When we run the code we only get apples and banana assigned to variables a and b respectively. 

###Destructuring and rest parameters

We've already learned some cool things that we can do in ES6, and we can combine array destructuring with rest paramaters group values into other arrays. Let's look at an example to see what we mean: 

```javascript 
let fruits = ['apple', 'grapes', 'banana']; 
let [first. ...rest] = fruits; 

console.log(first, rest);//apple, ['grapes', 'banana']
```
The example above shows array destructuring and the rest parameters in use, we assigned the first element apple to the variable first and then we used the rest parameter with the three dots `...` to group all remaining elements into a new array called rest 

###Destructuring from return values 

There will always be opportunities to use array destructuring in your JS code, and another place where we can use them is when we return values from functions. We can use them to assign to multiple variables at once. Let's see what we mean by looking at some code: 

```javascript 

function myFruits() {
   let fruits = ['apple', 'grapes', 'banana'];
   return fruits; 
}

```
As you would expect in JS, we can assign the return value to variable: 

```javascript 
function myFruits() {
   let fruits = ['apple', 'grapes', 'banana'];
   return fruits; 
}

let allFruits = myFruits(); 

```
Nothing new in the example above, however, using array destructuring we can assign multiple variables at once, just as we did before, from the return value of the function: 

```javascript 

function myFruits() {
   let fruits = ['apple', 'grapes', 'banana'];
   return fruits; 
}

let [a, b, c] = myFruits(); 

console.log(a, b, c);//apple, grapes, banana

```
###The for...of loop 

The for of loop is a new feature added in ES6, which is a better way of looping over arrays and other iterables. Let's look at an example to understand further. Once gain, say we have an array of fruits: 

```javascript 
let fruits = ['apple', 'grapes', 'banana'];
```

To loop over the array we can use the for in loop:

```javascript 
let fruits = ['apple', 'grapes', 'banana'];

for(let i in fruits) {
  console.log(fruits[i]);
}

```
The for in loop returns the index for each element, and it is assigned to the `i` variable in the loop. We can then use this index variable to access each element of the array. So, there are two steps here, first assigning each index to the `i` variable and then accessing each element using the index in the variable on the array. 

Using for of we don't need to use the index to access an element in an array: 

```javascript 
let fruits = ['apple', 'grapes', 'banana'];
```

To loop over the array we can use the for in loop:

```javascript 
let fruits = ['apple', 'grapes', 'banana'];

for(let fruit of fruits) {
  console.log(fruit);
}

```
The for of loop reads each element directly from the array and assigns it to the named variable, which is `fruit`. This is only one step when compared to the for in loop, and this means that we can loop over arrays and other iterables writing less code. 

###Objects and the for...of loop 

The for of loop cannot be used to iterate over properties of a plain javascript object. So the following will not work: 

```javascript 

let person = {
  name: "Abdi", 
  address: "123 JS street Node Avenue", 
  occupation: "JS Developer"
}

for(let prop of person) {
  console.log("Property", prop);
}

```

If you try to run the code above you will run into a type error: `TypeError: person[Symbol.iterator] is not a function`. So you migt be asking: when can I use for of without running into errors? We can check to see if the for of loop will work by looking to see if there is a function assigned to the Symbol.iterator property. For the array if we log the type that is assigned to the Symbol.iterator property, we can see that this returns a function: 

```javascript 
let fruits = ['apple', 'grapes', 'banana'];
console.log(typeof fruits[Symbol.iterator]);//function

let person = {
  name: "Abdi", 
  address: "123 JS street Node Avenue", 
  occupation: "JS Developer"
}

console.log(typeof person[Symbol.iterator]);//undefined

```
If we run the same check on the plain JS object, you will notice that it returns undefined. This means that there is nothing assigned to the property and the obpject will not work with for of loop. 

###Array.find() 

ES6 also adds new array function that we can use to find a specific element in an array. The array.find() function takes a testing function to return an element that meets this criteria: 

```javascript 

let services = [
  {name: 'nails', activated: false},
  {name: 'haircut', activated: true},
  {name: 'feet therapy', activated: true}
]

```
Lets say that we wante to find the first service object that was activated, we can use the array.find function to ohelp us get the first object which has activated set to true: 

```javascript 
let services = [
  {name: 'nails', activated: false},
  {name: 'haircut', activated: true},
  {name: 'feet therapy', activated: true}
]

let activated = services.find(service => {
  return service.activated
}); 

console.log(activated);//{name: 'haircut', activated: true}

```
The find method will return the first object that has activated set to true

##Maps

ES6 introduces a new data structure called maps. Maps are a data structure composed of a collection of key value pairs, which makes them very useful to store simple data. Maps are actually present in other programming languages and are useful to store property values. A Map stores collections of unique keys mapped to values each key is associated with one, and only one value, in order to find a specific value in map, you give it it key and you receive its value in return. 

###Issues with Objects as maps 

JS developers are first exposed to Maps through objects, you can use objects as key values stores, there are some limitations with this. The main limitation is that you cannot use a non string value as a key. The JS engine always converts object keys to strings and this causes unexpected behavior when you use objects as keys. Let's look at an example to better understand this: 

```javascript 

let carOne = { make: 'Audi' };
let carTwo = { make: 'Ford' }; 

```
lets add a new object to the scene `carAge' that will also be an object but will use the two car objects as keys: 

```javascript

let carOne = { make: 'Audi' };
let carTwo = { make: 'Ford' }; 

let carAge = {}; 

carAge[carOne] = 3; 
carAge[carTwo] = 5; 

console.log(carAge); //{ '[object Object]': 5 }

```

When you look at the console log of the carAge object you will see that it only contains one key which is `object Object` and a value of 5. Both keys have been converted to strings, and since they were objects, the string that they were converted to was `object object', and this means that only that key is being set in the carAge object. So in otherwords, the last value this set in the object will overwrite all previous values and so on. 

###The Map data structure 

To overcome this limitation in using objects as keys, ES6 introduced Map as a new data structure. The Map object is similar to the JS objects that we are used to, it is a simple key => value data structure. If you want to access the value of a particular key, you just provide that key and in return you get the value. The main difference with Maps is that you can use ANY value as a key or a value andmore importantly, objects are not converted to strings. 

To see Maps in action, let's make carAge a Map instead of a normal JS object, and use the set method to add keys to the Map. This is different to simply assigning the key in plain JS objects with dot notation or a using the brackets: 

```javascript 

let carOne = { make: 'Audi' };
let carTwo = { make: 'Ford' }; 

let carAge = new Map(); 

carAge.set(carOne, 3); 
carAge.set(CarTwo, 5); 

console.log(carAge); //{ '[object Object]': 5 }

```
The `set` method takes two arguments, a key and a value. As we did before we are using the objects as keys and assiging them their respective ages. To read the values of a map we can't simply use the dot or bracket notation, again we need to use one of its methods that it comes with to manipulate the `Map`, which is the `get` method that only takes a key as its only argument. Here's how we read keys off of a map: 

```javascript 

console.log(carAge.get(carOne));// 3
console.log(carAge.get(carTwo));//5

```

As you can see when you log the keys, the two values are assigned to different keys in the Map and nothing is converted to string or overwritten. Therefore, in majority of the cases we should not use JS objects as maps, because of their limitations when it comes to using objects as keys. 

You should use Maps when the keys are unknown until runtime, for example after loading in data from an AJAX call. However, when we are using predefined keys and we know their values before runtime, it is perfectly fine to use normal JS objects. We should also aim to use keys when all the keys and their values are of the same type. This will help keep the maps consistent and easier to work with, as you know what to expect.  

###Iterating Maps with for...of

The Map data structure are iterable, this means that we can use the for.. of loop and each run of the loop will return a [key, value] pair for each entry in the Map. Let's create a new Map of cars and add some entries: 

```javascript 

let cars = new Map(); 

cars.set("CarOne", "Audi"); 
cars.set("CarTwo", "Ford"); 
cars.set("CarThree", "GM"); 
cars.set("CarFour", "BMW"); 
```

We can easily loop through this Map of car using for..of loop and in each loop it will return a key value pair: 

```javascript 

let cars = new Map(); 

cars.set("CarOne", "Audi"); 
cars.set("CarTwo", "Ford"); 
cars.set("CarThree", "GM"); 
cars.set("CarFour", "BMW"); 

for(let [key, value] of cars) {
  console.log(`${key} = ${value}`); 
}

```

As you can see from the for of loop, we have used array destructuring to assign the key to a key and value to value respectively and we are accessing these using template strings when we log them out. When we run the code we can see that it prints each entry of the Map to the console successfully. 

###WeakMaps 

ES6 has also inttroduced another type of data set that is a variation of the Map called WeakMaps. The WeakMap is a special type of Map, and the main difference is that you can only use objects as keys. This means that you can't use primitive data types such as strings, numbers and booleans as the keys in a WeakMap. Let's look at an example where WeakMaps are used: 

```javascript 

let personOne = {}; 
let personTwo = {};

let people = new WeakMap(); 
people.set (personOne, "Abdi"); 
people.set(personTwo, "David"); 

console.log(people.get(personOne));//Abdi
console.log(people.get(personTwo));//David

```
As you can see from the code above, we can use the same `set` and `get` methods as we did with `Map`. However, if you tried using a string as a key, you will run into an error, which says `Invalid value used as weak map key`. Besides only allowing objects as keys, WeakMaps are not iterable, you cannot use the for..of loop to iterate over the keys in a WeakMap. You will run into the same error when trying to iterate over objects with a for...of loop. 

###Why do need WeakMaps? 

The main use for WeakMaps is that they make efficient use of memory, this means that individual entries can be garbage collected while the WeakMap is still in use. They are called 'Weak' because they hold a weak reference to the object that is used as reference for the keys. As long as an object is no longer referenced after it is used, WeakMaps will not prevent the garbage collector from colecting objects that are being used a keys in a WeakMap. This makes efficient use of memory and frees more of it up that can be used else where. 

##Sets


Like Maps and WeakMaps, Sets are a new data structure introduced in ES6, to understand why we need Sets in the first place, let's first go back to JS Arrays and see some of the limitations that lead to Sets being added to ES6. 

###Limitations with Array 

As you know Arrays in JS are simple and easy to use, however one thing that they don't do is enforce uniqueness in the elments that they hold. This means that you can have duplicate entries in an array in JS. So the following array in JS is perfectly fine: 

```javascript 
let cars = ['Audi', 'Ford', 'Audi', 'Mercedes', 'VW']; 

console.log(cars.length)//5
```

If we print the length property of the array we will see that it has a size of 5 items, even though we have duplicate item - audi. So in ES6 if you want to prevent duplicate entries in an array you can use Sets. Sets can store unique values of any type, be it primitive values or object references. You can create Sets in the same way that you create Maps, using the new keyword: 

```javascript 
let cars = new Set(); 
```
Now if you want to add items to a set you use the add method that is available on all instances of a set, insted of array push method: 
```javascript 
let cars = new Set(); 

cars.add('Audi');
cars.add('Ford');
cars.add('Mercedes');
cars.add({driver: 'Abdi'}); 
cars.add('VW');
cars.add('Audi');

console.log('Total no. cars', cars.size);//5

```
To get the number of items in a set you use the `.size` propery instead of the `.length`. You will notice that the duplicate entry of Audi is ignored and the total size is 5 not 6. 

###Sets and for...of 

As you would expect, Set objects are iterable and we can use the for...of loop and destructuring. Let's see an example of iterating over a set object: 

```javascript 
let cars = new Set(); 

cars.add('Audi');
cars.add('Ford');
cars.add('Mercedes');
cars.add({driver: 'Abdi'}); 
cars.add('VW');
cars.add('Audi');

for(let car of cars) {
  console.log(car);
}
```
###Sets and destructuring 

We can also use destructuring with sets just like we can with normal JS arrays: 

```javascript 
let cars = new Set(); 

cars.add('Audi');
cars.add('Ford');
cars.add('Mercedes');
cars.add({driver: 'Abdi'}); 
cars.add('VW');
cars.add('Audi');

let [a, b, c] = cars; 
console.log(a, b, c);//Audi, Ford, Mercedes

```

###WeakSets 

Similar to WeakMaps we have WeakSets, and if you recall these are the memory efficient version for Sets. Let's look at an example to see how WeakSets work: 

```javascript 

let weakCars = new WeakSet(); 

weakCars.add('Audi'); //error: Invalid value used in weak set

```
If you try to add a string to a WeakSet you will get an error: Invalid value used in weak set, just like WeakMaps, WeakSets only accept objects and nothing else can be stored. So let's add an object instead: 

```javascript 
let weakCars = new WeakSet(); 

weakCars.add({driver: 'Abdi'});
let passenger = { name: 'Sarah' }; 
weakCars.add(passenger);

```

To see if a particular object is in a WeakSet you can use the `has()` method which returns a boolean, to see whether a WeakSet contains a given object. 

```javascript 
let weakCars = new WeakSet(); 

weakCars.add({driver: 'Abdi'});
let passenger = { name: 'Sarah' }; 
weakCars.add(passenger);

console.log(weakCars.has(passenger))// true 

```
If you wanted to delete a particular entry in a WeakSet you can use the `delete` method: 

```javascript 

let weakCars = new WeakSet(); 

weakCars.add({driver: 'Abdi'});
let passenger = { name: 'Sarah' }; 
weakCars.add(passenger);
weakCars.delete(passenger); 
console.log(weakCars.has(passenger))// false 

```

WeakSets are different from Sets in a few different ways, first they are not iterable and they offer no methods for reading values from them. 



###When should we use WeakSets 

There are a limited use cases when WeakSets are actually useful, even though we can't iterate over them or even read values from them. One obvious example is efficient memory usage and to prevent memory leaks. Another instance where WeakSets can be used is when you want to make sure that you do not muatate any data in your app. For example say you have a function that is called when ever a particular link is clicked, and when it is called the function will set a property in an object to true: 

```javascript 

let carSlides = [
{ car: 'Audi', seen: false, image: 'url' },
{ car: 'Ford', seen: false, image: 'url' },
{ car: 'Mercedes', seen: false, image: 'url' },
{ car: 'VW', seen: false, image: 'url' }
];



function clicked(carSlides) {
  carSlides.forEach(car => {
    //mutates each object in the carSlides array
    car.seen = true; 
  } )
}

//lets say this is set true when user clicks on a link somewhere
let linkClicked = true; 
  if(linkClicked) {
  clicked(carSlides); 
}

console.log(carSlides)

```

The above is fine, but let's say that you do not want to mutate your data, having immutable object in your code is something that you should try and implement in your code where possible. We can refactor the code above to make use of WeakSets and not make any mutations to the carSlides array: 

```javascript 

let carSlides = [
{ car: 'Audi', seen: false, image: 'url' },
{ car: 'Ford', seen: false, image: 'url' },
{ car: 'Mercedes', seen: false, image: 'url' },
{ car: 'VW', seen: false, image: 'url' }
];

let carsViewed = new WeakSet();

function clicked(carSlides) {
  carSlides.forEach(car => {
    //instead of mutating we add the object to the carsViewed WeakSet
    carsViewed.add(car);
  } )
}

//lets say this is set true when user clicks on a link somewhere
let linkClicked = true; 
  if(linkClicked) {
  clicked(carSlides); 
}

//console.log(carSlides)//still have our object intact without mutations

//we can then check to see that we have each car as an object in the WeakSets array
for(let car of carSlides) {
 //check each individiual object is present in the WeakSet
 console.log(carsViewed.has(car)); //true 
 //other code.......
}

```
Even though it seems that we are doing extra work, this is actually making sure that we do not mutate our carSlides array data, and in essence achieve some form of data immutability. What you have to also remember is that the WeakSet does not preven the garbage collector from collecting objects that are no longer being referenced, and in turn creating an efficient use of memory. 


##Classes

If you've written any amount of substantial code you will have come across the need to encapsulate your code, and using JS constructor functions is the way this has been achieved. Let's say that we want to create an object that we can use to create instances of cars from. To do this we will typically create a constructor function: 

```javascript 

function Car(carSpec) {
  this.name = carSpec.name; 
  this.model = carSpec.model; 
  this.description = carSpec.description; 
} 

let car = new Car({name: 'Ford', model: 'Galaxy', description: 'A large family car'}); 

console.log(car.description);//A large family car 

```
Now if we want to add functions (methods) that we want to attach to the constructor function we add this to its to the functions prototype property like this: 

```javascript 
function Car(carSpec) {
  this.name = carSpec.name; 
  this.model = carSpec.model; 
  this.description = carSpec.description; 
}  

//add the function to the prototype property 
Car.prototype.drive = function() {
  console.log("Driving.....");
}

let car = new Car({name: 'Ford', model: 'Galaxy', description: 'A large family car'}); 

console.log(car.description);//A large family car 

//Now our car instance has the drive method available to it
car.drive(); //Driving.....
```

Now we've created a generic car object model, what if we wanted to create another car model, say Audi, that inherits from this generic car object, but also has unique properties? We can achieve with this with  the prototypal inheritance of JS. To implement this we'll need to create another constructor function and inherit from the Car object: 

```javascript 

//our main generic car object 
function Car(carSpec) {
  this.name = carSpec.name; 
  this.model = carSpec.model; 
  this.description = carSpec.description; 
}  

//add the function to the prototype property 
Car.prototype.drive = function() {
  console.log("Driving.....");
} 

function Audi(carSpec) {
  //call the generic car function constructor an pass it the context and the carSpec params
  Car.call(this, carSpec)
  this.engine = carSpec.engine;
  this.audiDrive = function() {
    console.log('vroom vrooom');
  }
}

//Then we need to use the Object.create to construct the subclass prototype so that we don't have to call the base constructor
Audi.prototype = Object.create(Car.prototype); 
//set the constructor of our Audi back to the Audi constructor again
Audi.prototype.constructor = Audi;

const myAudi = new Audi({name: 'My Audi', model: 'Audi', description: 'great German car', engine: 'A313'}); 

console.log(myAudi.description); 

```

Using this approach works but it involves a writing quite a bit of code, especially when you want to inherit from another object. There are quite a lot of things that we have to write just to get things wired up and working - this is one of main points that can make working with OO code in JS confusing. 

In ES6 we can rewrite our constructor functions using classes. Now in terms of the actual implementation of our ES6 classes it will actually just be converted to a constructor function under the hood, it is worth remembering that JS has a prototype system that is different from classical OOP languages such as Java and as such the JS remain a prototype based language. When we create classes in ES6 it doesn't mean that we are using new object model, rather using classes is just another way to create objects - but with a lot less code and lot less confusion surrounding it! 

With that out of the way, let's actually write an ES6 class to see how it's done. We'll use the car model again as an example but this time we will write it in ES6 using classes. Creating Object Oriented code using classes will actually assimilate it with other popular programming languages, and the syntax will look familiar to other developers coming from a non JS background. 

```javascript 

class Car {
  constructor(name, model, description) {
    this.name = name; 
    this.model = model; 
    this.description = description; 
  }
}

let car = new Car('Ford', 'Galaxy', 'A large family car' )
console.log(car.description);

```

You might argue that we have merely replaced the `function` keyword with `class` and everything else is still the same. Well, let's see how we can add methods to the class and then you'll realise that it's actually a lot different: 

```javascript 

class Car {
  constructor(name, model, description) {
    this.name = name; 
    this.model = model; 
    this.description = description; 
  }
  
  drive() {
    console.log("Driving.....");
  }
}

let car = new Car('Ford', 'Galaxy', 'A large family car' )
console.log(car.description);
car.drive(); 

```
