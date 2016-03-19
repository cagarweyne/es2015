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

#let 

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



