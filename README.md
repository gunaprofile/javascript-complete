# Javascript Complete

## Deep Dive: Constructor Functions & Prototypes

###  Introducing Constructor Functions


```js
class Person extends AgedPerson {
  name = 'Max';

  constructor() {
    super();
    this.age = 30;
  }

  greet() {
    console.log(
      'Hi, I am ' + this.name + ' and I am ' + this.age + ' years old.'
    );
  }
}

const person = new Person();
person.greet();
```
* This is nothing new, this is how you use classes and this is what we learned about in the last course module. Now behind the scenes, classes utilize a concept that has been around in Javascript forever basically, for a very long time and that's the concept of constructor functions

* Refer : constructor-functions-classes.pdf

*  Now constructor functions are a special type of function that acts as a blueprint for objects, same as class does, that can hold and set up properties and methods just like a class and that can then be created with the new keyword.

* So of course in modern browsers and modern scripts, we typically work with a class but behind the scenes, this class here would essentially be written as a function with the function keyword, with any name of your choice in this case person where still the convention is to use a capital character to make it clear that this function should not be called as a normal function but together with the new keyword to be used as a constructor function because indeed with the new keyword, Javascript will call this function differently than it normally would and then other than that, you write this as a regular function and to replicate this class,

```js
function Person() {
  this.age = 30;
  this.name = 'Max';
  this.greet = function() {
    console.log(
      'Hi, I am ' + this.name + ' and I am ' + this.age + ' years old.'
    );
  };
}

const person = new Person();
person.greet();
```

### Constructor Functions vs Classes & Understanding "new"

* Now let's understand what happens here. Of course we can't expect that this function returns an object right because we don't have any return keyword in there.

* It only returns an object because of that new keyword. If we wouldn't have new here, if we just call a person like this (const person = Person();), you will see that now if I repeat this, I get an error,

* I get an error, cannot read property greet of undefined because we would be calling greet on person and person on the other hand indeed is undefined because we just execute the person function, this function returns nothing so it returns no value so no value is stored in this constant and on no value, on undefined we can't call greet. So calling person like this won't work because we effectively call it like a regular function,
the fact that it starts with a capital character is just a convention, not something that changes something magically in Javascript.

* So the naming here really is just a convention which you should follow but which has no technical impact, it's the new keyword that matters.

* What the new keyword does behind the scenes and that's effectively also what it does for classes is it executes such a function, in there it sets this equal to the object that's going to be created, so equal to an empty object initially if you will, then it adds all these properties to this empty object and then it returns this and this is this object. This is effectively what's happening here you could say,

```js
function Person() {
  this = {} // The new keyword does here behind the scenes when you call a function with it, with new
  this.age = 30;
  this.name = 'Max';
  this.greet = function() {
    console.log(
      'Hi, I am ' + this.name + ' and I am ' + this.age + ' years old.'
    );
  };
  return this; // The new keyword does here behind the scenes when you call a function with it, with new
}

const person = new Person();  
person.greet();
```

* So now that we understand a bit how constructor functions work and that the new keyword matters, 

* let's understand how this function relates to this class then. In the end, this class here is just something we call syntactical sugar for this constructor function because defining blueprints like this can be a bit cumbersome, it can be confusing and suddenly it doesn't behave like a normal function because you use new

* if you're coming from a different programming language, you might be used to having classes, this is why classes were introduced to give us an easier way of writing these blueprint definitions.

* Now behind the scenes, classes also do more than just set up such constructor functions, they also help us with a concept called prototypes.. that we will see soon

* Now, the constructor function we have inside of a class effectively allows us to define this class body ie (function Person), so all the instructions that should run when an object is created based on the blueprint inside of a class

* because in a constructor function like this, it's obvious that whenever we create a new object based on it, all the code in that function executes, so this is great for initializing it, for setting it up, for reaching out to some server or doing whatever you want.

* well almost, you have that constructor function, this special method in there to be precise where everything you place in it also executes when this object gets created, so whatever goes into that constructor method here in your class is the same that would go into this person constructor function.

### Introducing Prototypes

* Refer : prototypes.pdf

* There is this term, this concept called prototypes in Javascript and it's super important to Javascript, it's been around forever and Javascript indeed is a prototype-based language which uses prototypical inheritance.

* Well in the end the class syntax, that's important to understand, just is syntactic sugar for constructor functions and also for working with the prototypes because working with them can be confusing, classes make that easier but it's still important to understand what they do.

* So constructor functions and these prototypes power Javascript objects, but what are prototypes then?

* Consider this constructor function as I just explained it, a function which we can call with new, the equivalent roughly would be such a class where we then build a person, a concrete object by calling that constructor function with the new keyword.

* Now this object which we build based on that blueprint, based on that class or this constructor function has all the properties and all the methods defined in that constructor function.

* So coming back to this example, when we create a person based on this constructor function, this p object here has an age property, a name property and a greet property which holds a function and therefore effectively that's a method because all of that is defined in here.

* Now you learned about that idea of being able to inherit from other classes and therefore this has to be possible for other constructor functions as well.

* In order to share functionality, to provide some methods or some properties which are available in all objects based on this class or constructor function or some subclass or subconstructor function in quotation marks because that's not an actual term. In non-class based Javascript and also in class based Javascript but there it happens behind the scenes, this is implemented with the concept of prototypes.

* Every constructor function you build has a special prototype property which is not added to the objects you create based on it because it's not part of the function body but a property of that function object, keep in mind that functions are objects and that prototype is there by default you can also edit it, something we will do and it is then automatically assigned as a prototype to the object which is created when you instantiate that constructor function.

* Now a prototype is an object itself, there are objects and every object has such a prototype OK but what exactly is the idea behind a prototype, why does every object have a prototype?

* It's how Javascript shares code in the end and you can think of prototype objects as fallback objects, objects Javascript has a look at if it searches for a certain property or method it can't find on the object it started looking at.

* Let's say we have a person object which has a name property and a greet method and then you have some code which calls person.sayHello. Now clearly there is no sayHello method in this person object, right? So normally we would expect this to fail and it might fail if our prototype or the prototype of the prototype also does not have a sayHello method because that's what Javascript will do, it will not fail immediately, instead as I said, every object in Javascript has a prototype and a prototype is basically a connected object which is used as a fallback object.

* So here we have a prototype and as I said, a prototype is just another object and by the way in case you're wondering, yes this prototype object also will have its own prototype and I'll come back to this chain of prototypes a little bit later but back to this prototype here, so this is the connected prototype to the person object and this prototype object might have a sayHello method and if Javascript tries to access a certain method or property and doesn't find it on an object, it automatically looks at the prototype object and looks for the property there and if it doesn't find it there, it looks at the prototype of the prototype, all the way until it reaches the end of that chain and didn't find that property or method anywhere and in that case for a property it would return undefined, for a method it would throw an error.

* So prototypes are in the end just connected objects which serve as fallback objects. Now that might sound confusing,

* let me give you an example. If I console log p.toString, you will actually see that this does not yield an error but that this works, if I reload here, you see this strange output. The exact output doesn't matter, the important thing is that we don't get an error. Just to prove that we should expect one, if I call toStr which I know won't exist and I then reload, I actually get an error, toString is not a function or toStr is not a function.So why does toString work?

* Because actually, there is some kind of invisible base object you could say on which our object is based, of course it's based on this constructor function but this doesn't add a toString method. So our constructor function somehow seems to point at some other base class, some other base constructor function which also kind of is called or which kind of does something, which basically adds the toString method to our object, kind of. Well indeed toString is not added to the object though, if we log the entire object with everything that's in there, we see there are exactly three things in there but there is this thing, the underscore __proto__property.

* Now that's a special property, hence the strange name, it's not really a property you should use to assign a value to though it would work but not really recommended but it shows you what this so-called prototype of this object is and you can think of the prototype kind of as the base class, so a related, a connected object to which we can also reach out to ask it for properties or methods which you're trying to access which don't exist on this object itself,

* The idea is that when you build a blueprint with a constructor function or with a class which in the end just uses constructor functions and prototypes behind the scenes and you then build an object based on that constructor function, you can call a method on that object. Now let's say we're calling the brief method here on person and let's say our constructor function or class does not have a brief method. What Javascript then does is it first of all when we call brief, it checks is that part of the object itself, so was it defined in the blueprint because as you learned, when you then instantiate an object based on the blueprint, the object has everything defined in the blueprint. So is the brief method part of the person itself, of that object which we created based on the constructor function? If yes, then we can execute the method, if no, well then we look to the prototype and that's just something you can memorize, it's called prototype, it's basically a base class of this person object here in the end you could say. There is a connection which is automatically set up and every object you create by default has such a prototype even if you haven't set up any inheritance here. and we do that until we reached the end and the last prototype we check is the prototype of this special object thing which I also showed you earlier.ie global object

* Refer : deep1

```js
function Person() {
  this.age = 30;
  this.name = 'Max';
  this.greet = function() {
    console.log(
      'Hi, I am ' + this.name + ' and I am ' + this.age + ' years old.'
    );
  };
}

Person.prototype = { // every object we build based on this constructor function should have a prototype which is exactly this object.
  printAge() { // printAge added as fallback object of Person function
    console.log(this.age);
  }
};

console.dir(Person);

const p = new Person();

console.log(p.__proto__ === Person.prototype); // true;
p.greet();
p.printAge();
console.log(p.__proto__); // This will not have any constructor methodn

Person.prototype.printAge = function() { // printAge added as fallback object of Person function
    console.log(this.age);
};
```
### Prototypes - Summary

* Prototypes can be a confusing and tricky topic - that's why it's important to really understand them.

* A prototype is an object (let's call it "P") that is linked to another object (let's call it "O") - it (the prototype object) kind of acts as a "fallback object" to which the other object ("O") can reach out if you try to work with a property or method that's not defined on the object ("O") itself.

* EVERY object in JavaScript by default has such a fallback object (i.e. a prototype object) 

* It can be especially confusing when we look at how you configure the prototype objects for "to be created" objects based on constructor functions (that is done via the .prototype property of the constructor function object).

```js
function User() {
    ... // some logic, doesn't matter => configures which properties etc. user objects will have
}
User.prototype = { age: 30 }; // sets prototype object for "to be created" user objects, NOT for User function object
```

* The User function here also has a prototype object of course (i.e. a connected fallback object) - but that is NOT the object the prototype property points at. Instead, you access the connected fallback/ prototype object via the special __proto__ property which EVERY object (remember, functions are objects) has.

* The prototype property does something different: It sets the prototype object, which new objects created with this User constructor function will have.

```js
const userA = new User();
userA.__proto__ === User.prototype; // true
userA.__proto__ === User.__proto__ // false
```
### Working with Prototypes

```js
class AgedPerson {
  printAge() {
    console.log(this.age);
  }
}

class Person extends AgedPerson {
  name = 'Max'; // these are directly part of person object

  constructor() {
    super();
    this.age = 30; // these are directly part of person object
  }

  greet() { // this is part of the prototype object, because this is shared across all the instances of this class
    console.log(
      'Hi, I am ' + this.name + ' and I am ' + this.age + ' years old.'
    );
  }
}

const p = new Person();
p.greet();
p.printAge();
console.log(p.__proto__); // this will have constructor function
```
### The Prototype Chain and the Global "Object"

* Now it is important to understand that the prototype chain does not end after reaching out to that prototype of that person object. If we try to call a method like printAge which is not part of the person object itself because we don't set it up in the constructor function and we didn't add it to the person thereafter either, well then we go back to the fallback object which is the prototype, right? So it's this prototype here which indeed has the printAge method. If we called something else, for example if I tried to console log p.toString, the question is will this now still work. It worked earlier because we could fall back to an object, a base object which had the toString method, the question is, is that still the case now that we kind of edited the prototype here?

```js

function Person() {
  this.age = 30;
  this.name = 'Max';
  this.greet = function() {
    console.log(
      'Hi, I am ' + this.name + ' and I am ' + this.age + ' years old.'
    );
  };
}

Person.describe = function() {
  console.log('Creating persons...');
}

Person.prototype.printAge = function() {
  console.log(this.age);
};

console.dir(Person);

const p = new Person();
p.greet();
p.printAge();
console.log(p.length);
console.log(p.toString());
const p2 = new p.__proto__.constructor();
console.dir(Object.prototype.__proto__);

```
* yes it still works, we get no error. Now it still works because the prototype chain doesn't end at this default prototype. If we have a look at our prototype, it's this this object which has the printAge method and the constructor method but which itself also has a prototype. As I mentioned, every object has a prototype, not just the object we build based on a constructor function but also the fallback object of our object, so the prototype of our object

* That prototype, that fallback object has its own fallback object, its own prototype and we can reach out to it with the special __proto__ property here. If we expand it here, we see an object which has that toString method, the question just is where is this object coming from? 

* This in the end is the base object you always have access to. The default prototype every object gets, thanks to it being assigned as a default
to be assigned prototype on every constructor function can be found on the global object class or to be precise, object constructor function.

###  Classes & Prototypes

* it's really important to understand the difference between the prototype property on constructor functions where you configure what will be added to objects that are created based on that constructor function

* and the __proto property which is available on every object, not just on constructor functions or function objects which in the end points to the prototype that has been assigned to this object, so to the fallback object that has been assigned to that object, that is really important to understand and that's the idea behind prototypes.

* Now arguably, using classes and extending makes that a lot easier which is why we have that and why we use that but it is super important to understand this behind the scenes stuff because Javascript is based on it and not only will you find this in many interviews or will you be forced to use this in code because you can't use classes for whatever reason, it also helps you understand the code you write and it's such a core concept of Javascript has been around forever you got to know. 

* Refer : different-kinds-of-method-declarations.pdf

```js
class Person extends AgedPerson {
  name = 'Max'; // these are directly part of person object

  constructor() {
    super();
    this.age = 30; // these are directly part of person object
  }

  greet() { // this is part of the prototype object, because this is shared across all the instances of this class
    console.log(
      'Hi, I am ' + this.name + ' and I am ' + this.age + ' years old.'
    );
  }
}
```
* 

```js
class AgedPerson {
  printAge() {
    console.log(this.age);
  }
}

class Person {
  name = 'Max';

  constructor() {
    // super();
    this.age = 30;
    // this.greet = function() { ... }
  }

  // greet = () => {
  //   console.log(
  //     'Hi, I am ' + this.name + ' and I am ' + this.age + ' years old.'
  //   );
  // };

  greet() {
    console.log(
      'Hi, I am ' + this.name + ' and I am ' + this.age + ' years old.'
    );
  }
}

// function Person() {
//   this.age = 30;
//   this.name = 'Max';
//   // this.greet = function() { ... };
// }

// Person.prototype.greet = function() {
//   console.log(
//     'Hi, I am ' + this.name + ' and I am ' + this.age + ' years old.'
//   );
// };

// Person.describe = function() {
//   console.log('Creating persons...');
// }

// Person.prototype = {
//   printAge() {
//     console.log(this.age);
//   }
// };

// Person.prototype.printAge = function() {
//   console.log(this.age);
// };

// console.dir(Person);

// const p = new Person();
// p.greet();
// p.printAge();
// console.log(p.length);
// console.log(p.toString());
// const p2 = new p.__proto__.constructor();
// console.dir(Object.prototype.__proto__);

const p = new Person();
const p2 = new Person();
p.greet();
console.log(p);

const button = document.getElementById('btn');
button.addEventListener('click', p.greet.bind(p));
```
### Built-in Prototypes in JavaScript

* Now one important additional word about prototypes in Javascript. They're essentially everywhere and we have also taken advantage of them in the course already without knowing that we did so, for example with all the array methods I explained to you. If you dive into the MDN docs, you see that the array methods - concat or filter are actually part of array.prototype, so they're defined on the fallback object, on the prototype every array is connected to.

* And it's not just arrays, it's also the case for strings for example.Now strings are kind of a special type because they're primitive values but as I explained, they still kind of behave like objects when you do call a method on them, some Javascript internal and therefore here, we also got a string prototype which is then utilized in the end which holds methods that help us with strings, something like slice for example which we also know from arrays but also string exclusive methods like replace or split which we saw in the array course section and that's in general a pattern you'll see throughout Javascript. There are a bunch of built-in objects which are used as prototypes, so as fallback objects for other built-in objects.

### Setting & Getting Prototypes

* One thing we hadn't had a look at yet is how you can actually change the prototype of an object which already was created or which you are not creating with your own constructor function.

* but what if you already have an object and you somehow want to change the prototype after it has been created or you want to create a new object without your own constructor function for whichever reason and you still want to set a different prototype. Now let me make it clear that both cases are definitely niche use cases, very advanced use cases, you might not encounter them every day but you will certainly find some scenarios where it can be useful if you are able to change the prototype of an existing object or of an object which is not created with one of your own constructors.

* This __proto thing in the end is just an unofficial feature which was implemented by all browsers actually but it's not really the way you're meant to work with prototypes in Javascript, it's just due to the evolution of Javascript and that certain methods of interacting with prototypes weren't always available, that we have this special __proto__property which can be useful for looking into a prototype.

* If you just want to look into the prototype during development, definitely use __proto. So that's get prototype of, more interesting for us here is clearly set prototype of. This takes two parameters and the first one is the object where you want to set the prototype, so my course object here, the second then is a prototype you want to use and that's the interesting thing now.

```js
const course = {
  // new Object()
  title: 'JavaScript - The Complete Guide',
  rating: 5
};

// console.log(Object.getPrototypeOf(course));
Object.setPrototypeOf(course, {
  // Here you can override the prototype that has been assigned to an object after the object was created
  // ...Object.getPrototypeOf(course), // here we will get current prototype - So this would be a way of making this work whilst keeping the old prototype object functionality,
  printRating: function() {
    console.log(`${this.rating}/5`);
  }
});

const student = Object.create({
  printProgress: function() {
    console.log(this.progress);
  }
}, {
  name: {
    configurable: true,
    enumerable: true,
    value: 'Max',
    writable: true
  }
});

// student.name = 'Max';

Object.defineProperty(student, 'progress', {
  configurable: true,
  enumerable: true,
  value: 0.8,
  writable: false
});

student.printProgress();

console.log(student);

course.printRating();
```

* it's also important to keep in mind that since we set the prototype to a new object, that new object which we set as a prototype will have its own prototype which is also object.prototype. So actually through that entire prototype chain, we would have access to toString and so on even without that line here because this object which we set as a new prototype will have that default object prototype itself and we would be able to access anything on that prototype from our course object as well thanks to that prototype chain where we always go to the next prototype in line if we can't find a certain property or method on the given object.