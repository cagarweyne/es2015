# Iterators and Generators -

Now on to one of the more interesting additions to JavaScript, and these are Iterators and Generators. But before diving into what they are and how to use them, let's first examine how we would typically iterate over data using the well known for loop method. This will give us a basis for understanding the need for iterators and how powerful they can be. 

Anyone who has programmed in JavaScript will have most likely written code that involves a for loop. Let's say we have an array of friends that contains their names and we want to loop over each friend's name and print it to the console. Using a for loop your code would typically look like this: 

```javascript 

var friends = ['John', 'Rhoda', 'Yasmin', 'Abdi']; 

for(var i = 0; i<friends.length; i++) {
  console.log(friends[i])
}

// "John"
// "Rhoda"
// "Yasmin"
// "Abdi"

```
OK, so this is easy enough - it loops through the friends array and prints out each name one at a time. The `for` loop tracks the index of the `friends` array with the `i` variable and the value of the `i` increments each time the loop executes provided that `i` is not larger than the length of the `friends` array. 

This is a really simple example, but loops can grow in complexity when you nest them, which means that you have to keep track of multiple variables. More often than not, this additional complexity leads to errors and bugs in your code as result because the way in which the `for` loop works means that you might write similar code in multiple places. Iterators were designed to solve this problem. 

In fact, many programming languages have shifted from iterating over data with `for` loops to using iterator objects that programmatically return the next item in a collection. Iterators greatly improve data processing and when they are coupled with the new array methods, the new types such as sets and maps, they become an integral part of the language. 

So far we have painted a rosy picture about iterators in ES6 without actually really understanding what they are and how we can use them. It's time for us now to take a deep dive into the world of Iterators and Generators in ES6. 

##What are Iterators? 
Iterators are nothing special, in fact they are just good old JavaScript objects with a `next` method on them. The `next` methods was designed for iteration, and this method returns what's called a 'result object'. This result object has two properties, `value` and `done`. 

Things will be become a lot clearer when we write some code, so let's create an Iterator function that will take a collection, i.e. an object that is iterable, and will return an object, when we call its `next()` method, with the two properties that we mentioned already, `value` and `done`. I'm going to write it in the most simplest way, so that we can understand the underlying concept of an Iterator in ES6: 

```javascript 
function createIterator(items) {
    let i = 0;
    
    return {
        next() {
            let done = (i >= items.length);
            let value; 
          
            if(!done) {
              value = items[i]; 
              i++; 
            } else {
              value = undefined; 
            }
            //var value = !done ? items[i++] : undefined;

            return {
                done,
                value
            };

        }
    };
}

var iterator = createIterator([1, 2, 3]);

console.log(iterator.next());           // "{ value: 1, done: false }"
console.log(iterator.next());           // "{ value: 2, done: false }"
console.log(iterator.next());           // "{ value: 3, done: false }"
console.log(iterator.next());           // "{ value: undefined, done: true }"

// for all further calls
console.log(iterator.next());           // "{ value: undefined, done: true }"

```
In our function `createIterator`, we first declare a variable `i` that we will use to keep track of the index of the items that we are iterating over, `items` is passed in as a parameter when the function is called. Then we simply return an object that contains a single item, the `next` function. All the `next` function does is also return an object that contains two properties, `value` and `done`. 

Inside the `next` method, we delcare two variables, `done` and `value`. `done` is a boolean and all we are doing is checking to see if our tracker variable `i`, is greater than or equal to the length of the passed in iterable - we did the same thing with the for loop.

We then do a check to see if `done` is false: `if(!done)` if it is, we assign value to contain the item at the current index using `i` and then increment `i` so that it can contains the next index. Finally, the `next` method returns an object containing two properties and their values are `done` and `value` respectively.

Each time the `next()` method is called, the next value in the items array is returned as `value`. When `i` is 3, `done` becomes true and this means that we execute the code inside of else in the `next` method, which simply assigns `undefined` as the value since we have iterated over all of the items of the passed in array. 

Our code inside the `next` method could do with a bit of refactoring, we can get rid ofthe `if` `else` and use a ternary operator to make an assignment to our `value` variable. Here's the refactored code: 

```javascript 

function createIterator(items) {
    let i = 0;
    
    return {
        next() {
            let done = (i >= items.length);
            let value = !done ? items[i++] : undefined;//refactored from the if else code block

            return {
                done,
                value
            };

        }
    };
}

var iterator = createIterator([1, 2, 3]);

console.log(iterator.next());           // "{ value: 1, done: false }"
console.log(iterator.next());           // "{ value: 2, done: false }"
console.log(iterator.next());           // "{ value: 3, done: false }"
console.log(iterator.next());           // "{ value: undefined, done: true }"

// for all further calls
console.log(iterator.next());           // "{ value: undefined, done: true }"

```

Our implementation of an iterator is a really simple one, and you can see from the example that creating an Iterator function which meets all of the rules laid out in ES6 can be quite a challenge. This is where Generators come into the equation. 

## What are Generators? 
Simply put, a Generator is a function that returns an iterator. They behave in pretty much the same way as normal functions with a few differences. Generator functions contain the * star character when they are declared after the `function` keyword. Generator functions that allow you to use the `yield` keyword to return iterator objects. 

In terms of declaring a Generator function, it doesn't matter where we place the star, as long as it's the first thing after the `function` keyword. So, the following Generator function declarations are all valid: 

```javascript 

function *nameList() {};

function* nameList() {};

function * nameList() {}; 

```
Let's create an Generator function so that we can better understand how it works: 


```javascript 

function *createIterator() {
    yield 1; 
    yield 2; 
    yield 3; 
  }

let iterator = createIterator();

console.log(iterator.next());// { value: 1, done: false }
console.log(iterator.next());// { value: 2, done: false }
console.log(iterator.next());// { value: 3, done: false }

```

Let's take it from the top, we declared a Genrerator function using the star character, and we are using the the `yield` keyword, which is new in ES6, and specifies values the resulting itreator should return when `next()` is called and the order they should be returned in. As you can see from the code example, we are calling the `createIterator` function and assigning the return value, which is an object, to the `iterator` variable. 

At this moment we have an object available to us with a `next()` method that we can call. Once we call the `next` method, we get back an object with two properties `value` and `done` - just like we did previously when we created our own iterator. The iterator generated in this example has three different values to return on successive calls to the next() method: first 1, then 2, and finally 3. A generator gets called like any other function, when we call the `next` method on that is available to us on the `iterator` variable. 

Each call to `next()` returns the hard coded value that we put in the Generator function, in our example, use the `yield` keyword to return 3 times - and remember the value is only returned after you call `next()` and we will see why in just a moment.  

I mentioned previously that Generator functions behaved similarly to normal functions, but that they have some unique behavior. We have already seen that they are delcared in a different way to normal functions, so when we are creating a Generator function we need to include the star charater after the `function` keyword. 

Generator functions behave differently to normal functions when they are called. Generator functions stop execution after the `yield` statement. So in the other words, when the `yield` statement is run, execution is halted until `next()` is called again. When `next()` is called again `yield 2` is executed and once again execution is stopped until `next()` is called again, then `yield 3` is run and so on.

All subsequent calls to `next()` would return `value` as `undefined` and `done` would contain `true`. The resulting behavior is very similar to the one we got from our iterator function that we implemented previously. Both returned an object once we called the function initially, and when we called the `next` method they returned each value and wether we were done iterating over all available items. 

The main difference is that we didn't have to write the functionality ourselves, rather all we had to do was create the special Generator function and simply use the `yield` keyword to return a value when `next()` was called. 

The `yield` keyword can be used with any value or expression, so this means that you can create Generator functions that add items to iterators without just listing them one by one, like we did in our example. You can even use `yield` inside a `for` loop: 

```javscript 

function *createIterator(items) {
    for (let i = 0; i < items.length; i++) {
        yield items[i];
    }
}

let iterator = createIterator([1, 2, 3]);

console.log(iterator.next());           // "{ value: 1, done: false }"
console.log(iterator.next());           // "{ value: 2, done: false }"
console.log(iterator.next());           // "{ value: 3, done: false }"
console.log(iterator.next());           // "{ value: undefined, done: true }"

// for all further calls
console.log(iterator.next());           // "{ value: undefined, done: true }"

```
The above example has simply refactored the use of the keyword `yield` to be computed from a passed in array. When we call our Generator function `createIterator` we pass it an array of items as a parameter and this array is then iterated over using a `for` loop. Inside the body of the loop we yield the elements from the array into the iterator as the loop progresses. Just as before, each time `yield` is encountered the loop stops and picks up from the same position when `next()` is called on the iterator.  

### `yield` can only be used inside of Generator functions 

As you might have gathered the `yield` keyword can only be used inside of Generator functions. If you use this keyword anywhere else beside a generator function, then this will create a syntax error. This also applies to functions that are inside of Generator functions themselves. For example, the following will generate a syntax error: 

```javascript 

function *createIterator(items) {

    items.forEach(function(item) {

        // syntax error
        yield item + 1;
    });
}

```
In this example, `yield` is actually insidethe Generator function but still create a syntax error. This is because `yield` in the inner function, cannot cross function boundaries - just like a nested function cannot return a value for its containing function.  

##Generator Function Expressions

Just like creating normal function expressions, you can also create Generator function expressions. To create a Generator function expression, you simply add the star charatcter between the function keyword and the openning parenthesis. Let's refactor the previous example of the generator function and turn into a Generator function expression: 

```javascript 


let createIterator = function *(items) {
    for (let i = 0; i < items.length; i++) {
        yield items[i];
    }
}

let iterator = createIterator([1, 2, 3]);

console.log(iterator.next());           // "{ value: 1, done: false }"
console.log(iterator.next());           // "{ value: 2, done: false }"
console.log(iterator.next());           // "{ value: 3, done: false }"
console.log(iterator.next());           // "{ value: undefined, done: true }"

// for all further calls
console.log(iterator.next());           // "{ value: undefined, done: true }"

```

We simply refactored the previous Generator function declaration and turned it into a function expression. Because the function is anonymous, the asterisk needs to go the `functio` keyword and openning parenthesis. It's also worth noting that you cannot create Generator function using the fat arrow syntax. 

## Generator Object Methods
As I mentioned previously, Generator functions behave just like normal functions with some differences. So, just like you can add add a function to an object as a method, you can also add a Generator function as a method on an object - here is how we would add a function as a Generator in ES6: 

```javascript 

let myObj = {

    *createIterator(items) {
        for (let i = 0; i < items.length; i++) {
            yield items[i];
        }
    }
};

let iterator = myObj.createIterator([1, 2, 3]);

```
In the shorthand version of adding methods to objects in ES6 you ommit the function keyword, this means that when creating a Generator function you place the star immediately before the method name. You can leave blank space between the method name and the star, as the whitespace will simply be ignored. 

## Iterables and for-of & accessing the Default Iterator
As we've already seen in the section on Arrays, Sets, and Maps, we can use the for..of loop to iterate over these objects. An iterable is an object with a `Symbol.iterator` property. So in other words, all iterable objects in ES6 have this property on them. This is actually a method that returns an iterator for the iterable object.As we've already seen in previous sections, we can use the for loop with an iterable. 

When using a normal for loop, we have to keep track of the index, however when we used iterators we no longer had this issue. Now when we use for of on an iterable it removes the need for us to track an index when iterating over an iterable all together. So this means that we don't need to keep track of anything with the for of loop, and we simply have available to us the contents of each item in a collection. 

Now that we have a better understanding of how iterators work, we can now look at how the for of loop works under the hood and how it uses an iterator when iterating over a collection. At its core, the for of loop uses the `Symbol.iterator` property on the iterable object. In other words, it simply calls this function and gets the iterable object which has the `next()` method available on it. 

Say we have an array of fruits, we can loop over this array using the for of loop since arrays are iterables: 

```javascript 
let fruits = ['apple', 'grapes', 'banana'];

for(let fruit of fruits) {
  console.log(fruit);
}}

//apple
//grapes
//banana
```
The for of loop reads each element directly from the array and assigns it to the named variable, which is `fruit`. But how does this actually work? As I mentioned the for loop simply makes use of the iterator returned from calling the arrays' `Symbol.iterator` function and this returns an iterator that the for of loop can use. 

Since the iterator know how to access items from a collection one at a time, all the for loop needs to do is call the `next()` method then save the value into a variable - in our example we specified `fruit`.  So what is really happening behind the scenes is that the for of loop simply calls the `Symboliterator` function: 

```javascript 
let iterator = fruits[Symbol.iterator]();
```
It then calls `iterator.next().value`  and assigns this to the variable `fruit`, it keeps on doing this until `next().done` is equal to true. 

```javascript 
let iterator = fruits[Symbol.iterator]();

let fruit = iterator.next().value; 

```
So essentially it's calling `next().value` and then assigning this to the fruit variable that we provided to it. We can actually loop over the array using a `while` loop by using the iterator returned by calling `fruits[Symbol.iterator]()`: 

```javascript 

let fruits = ['apple', 'grapes', 'banana'];

let iterator = fruits[Symbol.iterator]();

let counter = 0; 

while (counter < fruits.length) {
  let fruit = iterator.next().value; 
  console.log(fruit); 
  counter++; 
}

//apple
//grapes
//banana
```
Using the for of loop is should be used where possible, as it's less error prone and yo don't have to keep track of the index in a sequence. We shouldn't abandon the for loop altogether, but it's use should be reserved for complex scenarios where you need more control. The for of loop only works on iterable objects, so this means that when you try to use the for loop with a non iterable object such as plain JavaScript object `{}`, `null` or `undefined` it will throw an error: `TypeError: [Symbol.iterator] is not a function`. 

## Creating Iterables
As I mentioned, calling the for of loop on a plain JavaScript object will throw a type error, as there is no `Symbol.iterator` property on the object. Since this is not built-in, we can actually add a the `Symbol.iterator` property ourselves. Let's say we have a simple object that contains an array of items as a single property: 

```javascript 

let simpleObj = {
    items: ['Milk', 'ES6', 'Gym']
};

```
So this is a typical object nothing special. However, we can't call the for of on it. We simply add the `Symbol.iterator` as a property method: 

```javascript 

let simpleObj = {
    items: ['Milk', 'ES6', 'Gym'],
    *[Symbol.iterator]() {
        for (let item of this.items) {
            yield item;
        }
    }

};

for (let item of simpleObj) {
    console.log(item);
}

//output
//Milk
//ES6
//Gym

```
All we've done is add another property, which is actually a Generator function. It is now possible to use the for of loop on our object since it has the `Symbol.iterator` property. The for of will use this as the default operator and when it calls the function it gets back the iterator. From here, all it does is call the `iterator.next()` and assigns the value to our variable `prop` and then this logged on to the console. This is repeated until `iterator.next().done` is equal to true - hence our output Milk, ES6, Gym.

The Generator that we created uses a for of loop on each of the elements of the items array, remember that arrays are iterable by default and this means we can use the for of loop on them. Then it uses the `yield` keyword to return each element in the items array. 

The iterator that we added to our object merely returns each item in the items array, using the arrays default iterator. But what if we want our object to behave just like the array, so when we call for of loop on it, all of the objects properties values are retireived one at a time using the for of loop. We can easily do that all we have to do is refactor the Generator function to yield each property on every call to `next()`: 

```javascript 

let simpleObj = {
    items: ['Milk', 'ES6', 'Gym'],
    anotherProp: 'Hello World!',
    Name: 'Abdi', 
    *[Symbol.iterator]() {
        let keys = Object.keys(this)
           for(let i =0;i<keys.length;i++) {
                 yield this[keys[i]]
            }
    }

};

for (let item of simpleObj) {
    console.log(item);
}

//output: 
// ["Milk", "ES6", "Gym"]
// Hello World!
// Abdi

```

Now our object behaves just like an iterable object, all we did is use a for loop to return each property in the object via the yield statement. In most cases you won't need to create your own iterators, as ES6 comes with many built-in iterators that you can use to efficiently work with collections in JavaScript 

## Built-in Iterators
Only in certain special use cases would you need to create your own iterators, ES6 comes with iterators by default and we have already seen some these in action. In ES6 there are 3 types of collection objects: Arrays, Maps and Sets and by default all three types of collections have the following built-in iterators to help you retriev their content: 

1. entries()
2. values()
3. keys()

### Entries iterator 

The `entries()` iterator returns a two item as the value every time `next()` is called. The two items represent the key and the value for each item in the collection. For arrays, the first item in the pair is the numeric index and the second item second item is the actual element on that index. For Sets the first item is also the numeric index, but since the values double up as the numeric index this means that it will return both key and value as the same. And finally, for Maps the first item is the key and the second is the value. 

Let's look at an example of each type of collection with the entries iterator: 

```javascript 

let colors = [ "red", "green", "blue" ];
let tracking = new Set(['Milk', 'ES6', 'Gym']);
let data = new Map();

data.set("Name", "Abdi Cagarweyne");
data.set("Occupation", "Web Developer");

for (let entry of colors.entries()) {
    console.log(entry);
}

for (let entry of tracking.entries()) {
    console.log(entry);
}

for (let entry of data.entries()) {
    console.log(entry);
}

// output
// [0, "red"]
// [1, "green"]
// [2, "blue"]
// ["Milk", "Milk"]
// ["ES6", "ES6"]
// ["Gym", "Gym"]
// ["Name", "Abdi Cagarweyne"]
// ["Occupation", "Web Developer"]

```

### Values iterator 
The `values()` iterator returns each value associated with an item in a collection. We will simply use the `values()` iterator in each for loop: 

```javascript 

let colors = [ "red", "green", "blue" ];
let tracking = new Set(['Milk', 'ES6', 'Gym']);
let data = new Map();

data.set("Name", "Abdi Cagarweyne");
data.set("Occupation", "Web Developer");

for (let value of colors.values()) {
    console.log(entry);
}

for (let value of tracking.values()) {
    console.log(entry);
}

for (let value of data.values()) {
    console.log(entry);
}

// output 
// "red"
// "green"
// "blue"
// "Milk"
// "ES6"
// "Gym"
// "Abdi Cagarweyne"
// "Web Developer"

```

### Keys iterator 

The keys iterator returns each key associated with a collection: 

```javascript 

let colors = [ "red", "green", "blue" ];
let tracking = new Set(['Milk', 'ES6', 'Gym']);
let data = new Map();

data.set("Name", "Abdi Cagarweyne");
data.set("Occupation", "Web Developer");

for (let key of colors.keys()) {
    console.log(key);
}

for (let key of tracking.keys()) {
    console.log(key);
}

for (let key of data.keys()) {
    console.log(key);
}

// output 
// 0
// 1
// 2
// Milk
// ES6
// Gym
// Name
// Occupation

```

## The Spread Operator and Non-Array Iterables

The spread operator, as we've already seen, works by spreading out the contents of an array into individual values. So, you will recall this example from the spread section previously: 

```javascript 

let numbers = [1, 2, 3]; 

function sum(num1, num2, num3) {
  return num1 + num2 + num3; 
}

sum(...numbers); 

// ouput 
//6 

```
What I didn't mentioned was that the spread operator also works on any iterable object, and works by using the default iterator to determine which values to include. Let's look at an example where we spread out the values from a Set into an array: 

```javascript 

let set = new Set([1, 2, 3, 3, 3, 4, 5];
let array = [...set];

console.log(array);             

// output
// [1,2,3,4,5]

```
This works on Sets because the spread operator uses the default iterator and in the case of Arrays and Sets their default iterator is the `values()` iterator. This means that what is returned on each call to next using this iterator is the value for the Array and the Sets. As I mentioned the spread operator works on any iterable, so this means that it will also work on Maps, here is an example that demonstrates that: 

```javascript 

let person = new Map([['name', 'Abdi'], ['job title', 'JS Developer']]);

let array = [...person]

console.log(array)

// output 
// [["name", "Abdi"], ["job title", "JS Developer"]]

```
In the example above we create a new Map object and initialize it with two properies, `name` and `job title`. We can initialize the Map when we call the constructor initially and pass in an array of key value pairs that we want the Map to contain. So, in this example we pass in an array that contains two arrays, the first array contains two elements: name and abdi, this will be used as the key value pair. The we have another array that contains: job title and JS Developer, similarly these will be used to populate the second key value pair in our map. 

The default iterator for the Map object is the `entries()` iterator, and this means that it returns key-value pairs on each call to `next()`. This is why we have an array whose first and second elements are arrays. The really cool thing about the spread operator is you can use it in an array literal as many times as you want, and you can use it wherever you want to insert multiple items from an iterable.

For example let's say that you have three arrays and each contain single string elements and you want to combine them to create one array that contains all of the contents aof the previous three arrays, you can do this using the spread operator easily: 

```javascript 

let smallNumbers  = [1, 2, 3];

let mediumNumbers = [4, 5, 6];

let largeNumbers  = [7, 8, 9];

let allNumbers = [0, ...smallNumbers, ...mediumNumbers, ...largeNumbers, 10]; 

console.log(allNumbers); 

// output 
// [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

```
We've used the spread operator to create `allNumbers`, the first element in the array we've specified as 0 and the last 10. In between these two elements the spread operator was used to read the values from each array in the order that they appear in their respective arrays. The original values are changed all that has happened is that their values have been copied into the allNumbers array using the spread operator. Using the spread operator is the easiest way convert an iterable oject into an array. 
