#Promises 

JavaScript, as a programming language, is single threaded. To understand this quickly, let's take the example of an office where there is just one person that is available to serve customers. If the office worker is at the back working on a task, then this means that there is no one at the front desk to serve the other customers and this will force them to wait until the office worker finishes the task that they are currently working on. This is similar to JavaScript's single threaded nature, there aren't multiple threads available to do different tasks, so when you make an AJAX call to retrieve some data in JavaScript you will have seen this: 

```javascript 

let url = 'http://data-api/data'; 

let data = getdata(url);

console.log(data);

console.log('I am running after Ajax call');

//when executed - our output 

undefined 

I am running after Ajax call
```
