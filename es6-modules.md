## ES6 Modules 
Modules are an integral part of your code, and it's what makes your app readable and maintainable in the long run. In fact, it is good practice to break up your code into modules where possible to make your code more readable and scalable in the future. Before the latest update to JavaScript there wasn't any native way of creating and using modules, all of the modules approaches that are currenty in use today such as CommonJS and AMD are not native to JavaScript. They are just ways in which we can emulate a module system using either CommonJS or AMD library. 

With this in mind, the committee responsible for defining the ECMAScript specification decided that it was time to add native support for modules in JavaScript. The JavaScript community has really embraced the approaches of CommonJS and AMD, and these approaches have been implemented by NodeJS and RequireJS respectively. So, with this in mind, the committee decided that they wanted to create a native module system that would appease users of both camps. 

###Quick recap of the need for modules in the first place 

Just like good authors divide their books into chapters you should always take the opportunity to divide your code into modules! There are a host of arguments for the need to modularize code, so we will only mention a few: 

####Maintainability

The fact that you have extracted out certain functionality into its own module means that it is self contained and not dependent on the rest of you code. This means that you can improve on this module in the future without affecting the rest of your code base. Imagine you had a massive codebase and you did not modularize your code, then you decided to change a part of your code, but since the code is interdependent on each other it will affect other parts of the code. This means that you have to go through your entire code base to check and see which parts of other code will be affected by the change. This is a nightmare scenario, as a small change is likely to break your whole application. This in itself is probably enough of an argument as to why you should use modules in your code!

####Avoiding namespace 'pollution'
As you are probably aware, variables outside the scope of a top-level function are global, meaning, everyone can access them. This leads to what is called 'namespace pollution' or collisions, where unrelated code share variables and overwrite each other's data. 

####Reusability 

It is more than likely that you have copied and pasted code that you have written in a project to another one that you are working on. Instead of repeating your code between projects, you can create a module that you can reuse again and again. You also have the flexibility to improve and update the module as and when you please without needing to go back to your code update it. You only need to do this once and all of the projects that you used the module on will have the updated code in their copy of the module.  

###Creating modules in ES6 

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
###Named exports - multiple
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

###How to import named export modules in ES6 
To import modules we use the new `import` keyword in ES6, so to import all of the functions in the calculator.js file we will write: 

```javascript 
//main.js 

import { add, subtract, multiply, square } from './calculator'; 

//we can now use any of the functions 
console.log(divide(27, 3));//9

```
It is important to note that when you import named modules, you need to make sure that the variable in which you assign the return of the import matches the name of the exported item. So for example, if you wanted to import subtract and you assigned it the name sub instead of subtract, then this would not work since the import will use the name to reference the correct exported module and will return undefined. Also you can have as many named exports per file as you want. 

You might be thinking is there a way that I can import all of the functions without having to assign them to individual variables as above? And the answer is yes! you can import all named exports as an object and call each function as a property of this object. So to demonstrate what we mean we will refactor the above import statement   

###The 'default' export in ES6 and what this means 

###Importing named exports 

###Importing a modules an object 

###Simpler way to export multiple items at once

###Class modules can also be exported using default export

###Exporting class modules with named export 

