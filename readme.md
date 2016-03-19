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

In order to make use of new features that cannot be achieved with transpiling, we make user of shims to mimic the same behavior in older versions of JavaScript. There are many new features introduced that require a shim in order to work properly, and these are included within the tooling that we can use to run ES2015 in older browsers. Just in case, you're wondering what a shim is, it is basically a small piece of code that imeplements the feature which is missing from an older environment. for example, in ES2015 it introduces new API's to check strict equality of two values called `Object.assign()` 
