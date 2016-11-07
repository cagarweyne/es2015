#Promises 

JavaScript, as a programming language, is single threaded. To understand this quickly, let's take the example of an office where there is just one person that is available to serve customers. If the office worker is at the back working on a task, then this means that there is no one at the front desk to serve the other customers and this will force them to wait until the office worker finishes the task that they are currently working on. This is similar to JavaScript's single threaded nature, there aren't multiple threads available to do different tasks, so when you make an AJAX call to retrieve some data in JavaScript you will have seen this: 

```javascript 

let url = 'http://data-api/data'; 

let data = getdata(url);

console.log('data:', data);

console.log('I am running after Ajax call');

//when executed - our output 

data: undefined 

I am running after Ajax call
```

In JavaScript code is executed one after the other by a single thread, so the output that we see from our code above is that we have `undefined` as the value on data and the string that we logged inside the console function. Data is undefined because the AJAX call takes some time to retrieve the data from the server, and the code is immediately executed one after the other - hence there is no waiting around for the response. We could wait until we have a response from the server, but this would mean everything will freeze, the user will not be able to interact with the app until we have received the response. 

The way in which this is handled is to use a callback, that is create function that makes an AJAX call and takes in a callback function to be called when the data is received and we can then do something with with it. So this is what we are typically used to seeing something like this: 

```javascript 
let url = 'http://data-api/data'; 

let data = getdata(url, function() {
console.log('data:', data);
}); //takes some time to get data

console.log('I am running after Ajax call');

//when executed - our output 

I am running after Ajax call

data: Hello World 
```
