#Promises 

JavaScript, as a programming language, is single threaded. To understand what we mean by this let's take the example of an office where there is just one person that is available to serve customers. If the office worker is at the back working on a task, then this means that there is no one at the front desk to serve the other customers and this will force them to wait until the office worker finishes the task that they are currently working on. This is similar to JavaScript's single threaded nature, there aren't multiple threads available to do different tasks, so when you make an AJAX call to retrieve some data in JavaScript you will have seen this: 

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

It’s very important to understand how to work with JavaScript’s single-threaded model. Otherwise, we might accidentally freeze the entire app, to the detriment of user experience.

The way in which this is handled is to use a callback, that is create function that makes an AJAX call and takes in a callback function to be called when the data is received and we can then do something with with it. So we are typically used to seeing something like this: 

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

Because JavaScript executes code in a synchronous manner, we have to use callbacks whenever we come across something that will take some time to execute. Using callbacks is fine for most trivial use cases, however, when you are using callbacks everywhere and they are nested this is when you can run into issues wit them and you might get caught by what is called the callback hell. Let's look at an example to understand what is meant by callback hell. Say we have a simple function that we run that will simply log out a string of text to say we are waiting for someone, and it will call a callback function after a given amount of time to mimick making an AJAX call:
```javascript 
function waitingFor(name, done) {
  console.log('Wating for ' + name)

  setTimeout(()=> {
    if (name === 'Mike') {
      done('Mike is always late!')
    } else {
      done(null, name)
    }
  }, 3000)
}
```
The callback function takes two parameters, an error and the name of the person that we waited for. So let's put the function to use and say that we are waiting for three people, Abdi, Michelle and Thomas: 

```javascript 

waitingFor('Abdi', function(error, abdi) {
  
  if(error) {
    console.log(erorr);
  } else {
    waitingFor('Michelle', function(error, michelle) {
      if(error) {
        console.log(error);
      } else {
        
        waitingFor('Thomas', function(error, thomas) {
          if(error) {
            console.log(error);
          } else {
            console.log('OK good to go, we got ' +  abdi);
            console.log('OK good to go, we got ' +  michelle);
            console.log('OK good to go, we got ' +  thomas);
          }
        });
      }
    })   
  }
})

//output 
// "Wating for Abdi"
// "Wating for Michelle"
// "Wating for Thomas"
// "OK good to go, we got Abdi"
// "OK good to go, we got Michelle"
// "OK good to go, we got Thomas"

```
When our function waitingFor is invoked we call it with its parameters name and the callback function. Inside the function body we simply console log out the string "Waiting for" plus the name of the person that we are waiting for. Then we run the setTimeout function and after 3 seconds we call the given callback function and give it the name parameter that was passed in. 

Inside the callback function that is executed after the setTimeout, we first check to see if there is an error, and if there is we simply console log it. If there is no error we call the waitingFor function again with a new person's name and another callback function that will be invoked after the setTimeout inside the waitingFor function. 






