# ES6 Modules 
Modules are an integral part of your code, and it's what makes your app readable and maintainable in the long run. In fact, it is good practice to break up your code into modules where possible to make your code more readable and scalable in the future. Before the latest update to JavaScript there wasn't any native way of creating and using modules, all of the modules approaches that are currenty in use today such as CommonJS and AMD are not native to JavaScript. They are just ways in which we can emulate a module system using either CommonJS or AMD library. 

With this in mind, the committee responsible for defining the ECMAScript specification decided that it was time to add native support for modules in JavaScript. The JavaScript community has really embraced the approaches of CommonJS and AMD, and these approaches have been implemented by NodeJS and RequireJS respectively. So, with this in mind, the committee decided that they wanted to create a native module system that would appease users of both camps. 

<iframe width="560" height="315" src="https://www.youtube.com/embed/5P04OK6KlXA" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Quick recap of the need for modules in the first place 

Just like good authors divide their books into chapters you should always take the opportunity to divide your code into modules! There are a host of arguments for the need to modularize code, so we will only mention a few: 

#### Maintainability

The fact that you have extracted out certain functionality into its own module means that it is self contained and not dependent on the rest of you code. This means that you can improve on this module in the future without affecting the rest of your code base. Imagine you had a massive codebase and you did not modularize your code, then you decided to change a part of your code, but since the code is interdependent on each other it will affect other parts of the code. This means that you have to go through your entire code base to check and see which parts of other code will be affected by the change. This is a nightmare scenario, as a small change is likely to break your whole application. This in itself is probably enough of an argument as to why you should use modules in your code!

####Avoiding namespace 'pollution'
As you are probably aware, variables outside the scope of a top-level function are global, meaning, everyone can access them. This leads to what is called 'namespace pollution' or collisions, where unrelated code share variables and overwrite each other's data. 

#### Reusability 

It is more than likely that you have copied and pasted code that you have written in a project to another one that you are working on. Instead of repeating your code between projects, you can create a module that you can reuse again and again. You also have the flexibility to improve and update the module as and when you please without needing to go back to your code update it. You only need to do this once and all of the projects that you used the module on will have the updated code in their copy of the module.  

### Creating modules in ES6 

In following the general trend of simplifying things, ES6 introduces two kinds of exports; named exports and default exports. Let's say we are writing a helper module that helps us calculate. We'll call it the module calculator. In the calculator module we have methods that allow is to add, subtract and divide and square.   

```javascript 
//calculator.js

function add(a, b) {
  return a + b; 
}

function subtract(a, b) {
  return a - b; 
}

function multiply(a, b) {
  return a / b; 
}

function square(x) {
  return multiply(x, x); 
}
```
### Named exports - multiple
Now that we have the functions written in our calculator.js file, we want to be able to use them elsewhere, say like in a main.js file. In order for to to be able to use the functions we need to 'export' them first, and in ES6 you can export more than one function or variable using the `export` keyword. So all we have to do is to prepend the keyword export on each of the functions that we want to export: 

```javascript 
//calculator.js

export function add(a, b) {
  return a + b; 
}

export function subtract(a, b) {
  return a - b; 
}

export function multiply(a, b) {
  return a / b; 
}

export function square(x) {
  return multiply(x, x); 
}
```
Now we can use import in any of the functions in the calculator.js file. 

### How to import named export modules in ES6 
To import modules we use the new `import` keyword in ES6, so to import all of the functions in the calculator.js file we will write: 

```javascript 
//main.js 

import { add, subtract, multiply, square } from './calculator'; 

//we can now use any of the functions 
console.log(divide(27, 3));//9

```
It is important to note that when you import named modules, you need to make sure that the variable in which you assign the return of the import matches the name of the exported item. So for example, if you wanted to import subtract and you assigned it the name sub instead of subtract, then this would not work since the import will use the name to reference the correct exported module and will return undefined. Also you can have as many named exports per file as you want. 

### Importing modules as an object 

You might be thinking is there a way that I can import all of the functions without having to assign them to individual variables as above? And the answer is yes! you can import all named exports as an object and call each function as a property of this object. So to demonstrate what we mean we will refactor the above import statement to use a single imported object and we can each of the function as a property of the imported object: 

```javascript 
//main.js 

import * as calculator from './calculator'; 

//we can now use any of the functions 
console.log(calculator.divide(27, 3));//9

console.log(calculator.add(5, 7));//12
```
As you can see from the refactor, we have now imported all of the functions in calculator.js as an object and assigned this to the variable calculator. And just like you would access an object, we can use the functions as property of the object and call them with the normal dot notation method and pass in the required function parameters. 

Now if we want to compare how this is different from the ES5 way, we can refactor this to see how we might do the same thing but in ES5 using the CommonJS approach: 

```javascript 
//calculator.js - in ES5 with CommonJS module pattern

function add(a, b) {
  return a + b; 
}

function subtract(a, b) {
  return a - b; 
}

function multiply(a, b) {
  return a / b; 
}

function square(x) {
  return multiply(x, x); 
}

module.exports = {
  add: add, 
  subtract: subtract, 
  multiply: multiply, 
  square: square
}

//main.js - in ES5 with CommonJS module pattern

var square = require('calculator').square;
var add = require('calculator').add;
console.log(square(11)); // 121
console.log(add(4, 3)); // 7

```
### Simpler way to export multiple items at once

Let's say that you are using named exports and that in a single file you want to export several items, just like we did in the calculator.js file - but you want a shorter and simpler way to simply adding the keyord export to each item that you wish to export. You can achieve by passing multiple functions to the export keyword using curly braces. To see how this works let's refactor the export statemets in our calculator.js file: 

```javascript 
//calculator.js

function add(a, b) {
  return a + b; 
}

function subtract(a, b) {
  return a - b; 
}

function multiply(a, b) {
  return a / b; 
}

function square(x) {
  return multiply(x, x); 
}

export { add, subtract, multiply, square }; 

//main.js
import * as calculator from './calculator';
//works the same as previously 
console.log(calculator.divide(27, 3));//9
console.log(calculator.add(5, 7));//12

```

### The 'default' export in ES6 and what this means 
The second way in which we can export modules is using the export default keyword. In NodeJs community and also on the Front end, there is a tendency to have modules that export only a single value. ES6 plays along really nicely with this concept, as it provides the option to have a main module exported as the 'default'. To use the export default approach you simply prepend `export default` to the value that you want to export as the default. To demonstrate this let's export a simple function as a default export: 

```javascript 
//lib.js 
export default function greet(name) {
  return `hello ${name}`;  
}

//this function is not available outside this module
function anotherFunc() {
  //some computations...
}

```
Now for us to use the default exported value, we need to assign to a variable. Note that this variable name can be called anything since the value that we are importing is a default export: 

```javascript 

import myFunc from './lib'; 
myFunc('Abdi');//hello Abdi
```
It is worth noting that you cannot default export multiple values from a module, the default type export limits the number of values that you can default export so the following will not work: 

```javascript 
//lib.js 
export default function greet(name) {
  return `hello ${name}`;  
}

//this function is not available outside this module
export default function anotherFunc() {
  //some computations...
  
  //this will create an error - only one default export allowed per module
```

### Class modules can also be exported using default export
Classes can also be exported from modules using the same syntax as functions. Instead of several individual functions, we can have a single class that has multiple functions. If we refactor the calcultator.js module to be class and export it as a default: 

```javascript 
//calculator.js 
export default class Calculator {
  function add(a, b) {
  return a + b; 
}

  subtract(a, b) {
    return a - b; 
  }

  multiply(a, b) {
    return a / b; 
  }

  square(x) {
    return multiply(x, x); 
  }
}
```
We now have a single class that contains all of our functions as methods and this class been exported as the default module. Imported classes are assigned to a variable using import and can then be used to create new instances: 

```javascript 
import Calculator from './calculator';

let calculator = new Calculator(); 
//works the same as previously 
console.log(calculator.divide(27, 3));//9
console.log(calculator.add(5, 7));//12

```
### Exporting class modules with named export 

Just as we can export classes using the `export default` method, we can export classes as named exports: 

```javascript 
//calculator.js 
class Calculator {
  function add(a, b) {
  return a + b; 
}

  subtract(a, b) {
    return a - b; 
  }

  multiply(a, b) {
    return a / b; 
  }

  square(x) {
    return multiply(x, x); 
  }
}

export { Calculator }
```
## Some points to note on ES6 Modules
As simple as ES6 Modules are to implement, there are some things that we need to know about how they work. 

###You cannot conditionally import or export ES6 Modules

When importing or exporting ES6 Modules you cannot do these conditionally, so for example the following will not work and will generate a syntax error: 

```javascript 
if (someCondition) {
    import myMdoule from './lib'; // SyntaxError
}

// You can’t  `import` and `export`
// inside a simple block:
{
    import React from 'react'; // SyntaxError
}
```
### Imports are hoisted 
Just like the `var` ES6 import are hoisted to the top of the current scope, so no matter where the imports are declared in a module, they will always be hoisted up to the beginning of the current scope. So the following works just fine: 

```javascript 
someFunction();

import { myModule } from 'my_module';
```
### Imported variables are read-only 
When you import variables from a module they are read-only and you cannot change their values when once imported. So for example, if we had a count variable and we imported this in we would be able to read the value but cannot change it inisde our code: 

```javascript 
// my-module.js
export let count = 0;
 
// main.js
import { count } from './my-module';
 
count++; // read-only error
```
However, if we had imported a function along with the variable from the module and then this function can change the value of the count variable. The following works just fine: 

```javascript 
// my-module.js
export let count = 0;

export function increment() {
  count++;
}

// main.js
import { count, increment } from './my-module';
increment(); 

console.log(count); //1 
```

This means that the module we import is not a copy, but rather there is a connection to the variables declared inside module bodies and these are in essence 'live'. 

Even though imported variables are read-only, we can change the property of an imported object and this works just fine: 

```javascript
// my-module.js
export const myObj = {
  name: 'Robert De Niro'
}

//main.js 
import { myObj } from './my-module';

myObj.name = 'Al Pacino';//

console.log(myObj.name);//Al Pacino 

```
