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
```

