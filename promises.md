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

  setTimeout(function() {
    if (name === 'Mike') {
      done('Mike is always late!')
    } else {
      done(null, name)
    }
  }, 3000)
}
```
The callback function takes two parameters, an error and the name of the person that we waited for. So let's put the function to use and say that we are waiting for three people, Abdi, Michelle, Thomas and John: 

```javascript 
waitingFor('Abdi', function(error, abdi) {
  if (error) {
    console.log(error)
  } else {
    waitingFor('Michelle', function(error, michelle) {
      if (error) {
        console.log(error)
      } else {
        waitingFor('Thomas', function(error, thomas) {
          if (error) {
            console.log(error)
          } else {
            waitingFor('John', function(error, john) {
              if (error) {
                console.log(error)
              } else {
                console.log('Got ' + abdi)
                console.log('Got ' + michelle)
                console.log('Got ' + thomas)
                console.log('Got ' + john)
                console.log("Ok, let's go!")
              }
            })
          }
        })
      }
    })
  }
})

//output 
// "Wating for Abdi"
// "Wating for Michelle"
// "Wating for Thomas"
// "Wating for John"
// "Got Abdi"
// "Got Michelle"
// "Got Thomas"
// "Got John"
// "Ok, let's go!"

```
When our function waitingFor is invoked we call it with its parameters name and the callback function. Inside the function body we simply console log out the string "Waiting for" plus the name of the person that we are waiting for. Then we run the setTimeout function and after 3 seconds we call the given callback function and give it the name parameter that was passed in. 

Inside the callback function that is executed after the setTimeout, we first check to see if there is an error, and if there is we simply console log it. If there is no error we call the waitingFor function again with a new person's name and another callback function that will be invoked after the setTimeout inside the waitingFor function. 

This process is repeated three times, so hence the output of three sentences that say who we are waiting for and then after each has run its duration with no errors, then in the final callback function's else block we have access to all outer functions parameters and here we simply console log out the text to sau that we have finally got each person that we were waiting for. 

If you have worked with Node.js previously, then this pattern will be very familiar. Again, this is a very simple example without any errors, so if we had an error after the first call to waitingFor, this would happen if the person we are waiting for is Mike, then the execution of the code would stop and it can get very messy: 

```javascript 
waitingFor('Mike', function(error, mike) {
  if (error) {
    console.log(error)
  } else {
    waitingFor('Michelle', function(error, michelle) {
      if (error) {
        console.log(error)
      } else {
        waitingFor('Thomas', function(error, thomas) {
          if (error) {
            console.log(error)
          } else {
            waitingFor('John', function(error, john) {
              if (error) {
                console.log(error)
              } else {
                console.log('Got ' + mike)
                console.log('Got ' + michelle)
                console.log('Got ' + thomas)
                console.log('Got ' + john)
                console.log("Ok, let's go!")
              }
            })
          }
        })
      }
    })
  }
})

//output 
// "Wating for Mike"
// "Mike is always late!"
```
You can see from this simple example that we need to think very carefully when using callbacks, becuase it can get very messy quickly and it can be very difficult to read and to trace any bugs in your code. In these cases, you’d need to track multiple callbacks and cleanup operations which requires a lot of brain power and is easy prone to introducing bugs into your code. 

Now that we have an understanding about the problems we can run into when using callbacks in asynchronous tasks, we need better way that will enable us to write async code and make it less complex than using callbacks. 

Well, the answer is Promises. And I promise it's not going to be difficult to understand! You might already by using promises if you have used the fetch library in any of our projects previously. Promises help you to write cleaner code that is easier to comprehend and also provide mechanisms for error handling. 

###Promise concepts 
Promises have actually been around for a number of yours, and they have been implemented as third party libraries. To look at it simply, promises are objects with several functions that we can call and pass in callback functions into. For example: 

```javascript

let Promise = {
  then() {}, 
  catch() {}, 
  all() {}
  //.....
}
```
The `then` function is called anytime a promise is successful and the catch method of the promise is called when there is an error. We can also call another method on the promise called `all`, that we can use to call multiple promises as an array, we will see more on this later. Depending on browser implementation of ES6, as of ES2015 promises are native JavaScript, meaning that you will not necessarily need a third party library to use them in your code. 

let's say that you want to make an API call to retreive a list of tasks from the server and this is going to be promise based. When you make the call you will not be able to get the list of tasks immediately from the server, since this takes time to traverse the network, process the request and send back the response etc. Since we have a promised based AJAX call, we can imagine the promise to be like a receipt or a 'promise' so to speak that we will eventually receive the value that we requested or get a reason (error) as to why it could not be sent. 

###pending state 
promises transition from state to state and whenever we create a promise it will initially be in a 'pending' state. This means that the promises has not been resolved or rejected. Its status is pending, meaning that we are waiting on the outcome to either be resolved or rejected. 

###Resolved 
When we receive our list of tasks from the API, the state of our promise will change from 'pending' to fulfilled or resolved. We can then access our list of tasks inside the `then()` function. 

###Rejected 
If, for whatever reason, there is an error that causes the request to not be fulfilled, then the promise's state will be rejected and we can access this error via the `catch()` 

