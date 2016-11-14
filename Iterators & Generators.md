#Iterators & Generators 

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

##What are Generators? 
Simply put, a Generator is a function that returns an iterator. They behave in pretty much the same way as normal function with a few differences. Generator functions contain the * star character when they are declared after the `function` keyword. 
