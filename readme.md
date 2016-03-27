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
      console.log(moreThanFive); //moreThanFive - causes reference error
      alert(lessThanFive);
      //other
    }
    
    //both let variables don't exist here either
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
By using the arrow function instead of a normal syntax with the `function` keyword, we get the behavior that is desired. This is because the `this` keyword binding in arrow functions is lexical. This means that arrow functions do not have a stand-alone `this` value binding, rather the value is decided by the closest containing non-arrow function and in our example that is the getFullName function. So, essentially the fat arrow `=>` replaces the `var self  this` or `bind(this)`. This creates its own set of quirks that yo uhave to keep in mind when using arrow functions.  

####Arrow functions also have their own quirks with the `this` keyword

####Other notable features of arrow functions 

-No `this`, 
-No `arguments` object
-No Prototype
