###Inheritance in ES6 Classes 

In the previous example when we wanted to inherit from the generic car object, we had to write a lot of code to achieve this and this code can be confusing to understand at times. The process of inheritance in ES6 is simplified and we don't have to actually write a lot of code to achieve what we want.

To understand how inheritance is simplified in ES6, we will rewrite the previous example where we created an Audi car model and inherited from the generic car object's properties and methods. To do this in ES5 we had to write a lot of code. Let's rewrite the code now in ES6: 

```javascript 
//our generic car object in ES6 
class Car {
  constructor(carSpec) {
    this.name = carSpec.name; 
    this.model = carSpec.model; 
    this.description = carSpec.description; 
  }

  //drive method
  drive() {
    console.log("Driving.....");
  }
}



//new car model (Audi)
class Audi extends Car {
  constructor(carSpec) {
    super(carSpec);// Car.constructor(); 
    this.engine = carSpec.engine; 
  }

}

let audi = new Audi({name: "Audi", model: "A3", engine: "A313", description: "Best Audi Model"}); 

console.log(audi);

//prints 
// {
//   description: "Best Audi Model",
//   engine: "A313",
//   model: "A3",
//   name: "Audi"
// }


audi.drive(); //calls the inherited instance method from Car - "Driving....."

```
The ES6 code is actually a lot simpler! We have our initial car object, that takes in a carSpec options object as an argument. In the Car object's constructor method we initialize the variables `name`, `model` and `description`. Nothing new here. Then we created a new car object model Audi, and as you will have noticed we added a few things to the original Car object. After the class name we added the keyword `extends` followed by the name of the class that we want to inherit from. In our case here it is the Car object. This means that we want to inherit all of properties and instance methods of the Car object. 

We also want to have the name, model and description properties to be assigned as instance properties of the new Audi model, to achieve this we need to call the constructor method of the Car object. We can achieve this by using the keyword `super` and invoking it with parenthesis `super()`. This means that we won't have to do the same assignment that we did in when we wrote the constructor method of the Car object, this will be done for us the moment that we call super. However, we also need to pass along the `carSpec` options object to the super method so that when it runs, our class can assigned with a name, model and description. Then below the call to the super method, we can assign our own unique class instance, in our cases the Audi model has an engine property, we can pull this off the `carSpec` object that is passed in and assign to the engine property of the class. 

As mentioned we also have access to all of the methods of the Car object, so we can simply call the drive method on the new instance of the Audi model and it will run the inherited drive method. 
