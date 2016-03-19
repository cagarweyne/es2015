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

In ES2015, you can shorten the definition of the object properties if the key value pairs are exactly the same. Once this code is transpiled, it will be translated into it's equivalent or close counterpart in ES2015. So, in our example above, the code will be transpiled to: 

```javascript 

var name = "Abdi"; 

var myObj = {
  name: name
};

```
