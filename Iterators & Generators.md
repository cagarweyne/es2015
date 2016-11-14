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

