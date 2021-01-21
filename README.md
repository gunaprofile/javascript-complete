# Javascript Complete

## Meta-Programming: Symbols, Iterators, Generators, Reflect API & Proxy API

### Understanding Symbols

* what are symbols? Symbols and that's really interesting and good to know are a primitive value. I did mention that before in the course, we learned about strings, numbers, booleans there, now symbols are also just yet another primitive value,symbols are not objects, not reference values, they are primitive values.

* You can use symbols as object property identifiers, as keys in objects so to say, just like you can use numbers and strings. There are built-in symbols and you can also build your own symbols and that's the unique feature or the special feature about symbols, they guarantee uniqueness, every symbol you build is unique.

* So you can use symbols to add a key, to add a property to an object where you know that this name won't exist there yet because you just created the symbol, it will be unique and you can't override an existing symbol of the same name there because every symbol is standalone and unique and that's the biggest feature of symbols.

* Now let's see how we use them and what this uniqueness actually means.

* Let's create a new symbol, let's maybe name it uid for userId but you can name it however you want and then you create a symbol by calling symbol. Important, not new symbol, that would not work but just symbol, like a function basically.

* Now how can we use such a symbol? Well let's say we have a person object

* Now here we'll have to do some mental yoga, let's imagine you're building a library where you expose certain objects with the users of your library, just like the axios library which we used earlier exposes the axios object if you remember. Now in the objects you expose, you might have certain keys, certain properties which you want to ensure are not overridden by the users of your library.

* So let's say we had some library, maybe some authentication library which creates user objects and in the user object, you want to give the users of your library, the developers using your library the full power to change the objects your library spits out but you want to ensure that all the user objects your library generates always have some ID identifier which can't be overridden.

* If normal object you could override data easily that  is one scenario where symbols could come in handy.

* Now that's where we could use a symbol, instead of adding an ID like this if we want to ensure that only we, so only the code in our library knows how to interact with that ID,

```js
// Library land
const uid = Symbol();
console.log(uid);

const user = {
  // id: 'p1',
  [uid]: 'p1',
  name: 'Max',
  age: 30
};

user[uid] = 'p3'; //  I can change it but if I don't expose this symbol as part of my library,

// app land => Using the library

user.id = 'p2'; // this should not be possible!

console.log(user[Symbol('uid')]);
console.log(Symbol('uid') === Symbol('uid'));
```
* users of our library, users got no way of accessing this property here because they don't know this symbol and if you would try to access something like symbol uid here,so I create a new symbol on the fly and try to access this symbol on the user object
```js
console.log(user[Symbol('uid')]); // undefined This technically still is a new symbol, totally unrelated to this symbol,they are totally different objects.

console.log(Symbol('uid')===Symbol('uid')); // false
```
* check if that is equal to symbol uid and you'll see you get false as a response here as an output because these are not equal if I reload here. So that's the idea behind symbols,

* we do have unique identifiers which can't be accidentally overridden and only if we use that exact same symbol again, so inside of the library, if I then access this symbol again, I could change it.

### Well-known Symbols

* Refer : https://medium.com/@obaranovskyi/js-symbol-and-well-known-symbols-c3c9cc395b6d

* Now besides the symbols we can create and you can create as many as you want, there also are so-called well known symbols.
These are symbols that are built into modern Javascript, basically Javascript after ES5, so ES6 or higher and were built in before too but now you can access them I should say and these are symbols that can help you do certain things.

* Now we'll see the symbol iterator thing in action a little bit later for example, this allows you to actually make any object iterable with the for/of loop which by default is not possible for a regular object for example,it is possible for an array though

* Now we've got other well-known symbols there in the end, these are just symbols that are built into Javascript which you can access on this symbol object if you don't execute it like a function but instead as an object with a static property with the dot notation,then these are as symbols which were created by Javascript so to say and which you can therefore use in which Javascript then in turn also tries to find for certain actions, just as we as a library author can try to use the uid symbol here to for example reassign the value or extract the value, the Javascript engine also has certain built-in symbols which it also uses in certain circumstances,

* for example when the for/of loop gets executed, it tries to find that symbol iterator symbol here, the built-in iterator symbol on an object and see if the for/of loop can be used on that object because it requires that and some logic attached to it to use that but we'll see that when we dive into iterators and generators. 

* For now just be aware that there are bunch of built-in symbols, most of the time you will not really need them, though there is one for example which I want to show you and that's the toString tag symbol. We have an object here, we have the user object here.

```js
console.log(user.toString());  // [object object]
```
* Now technically if we would call to string here on the user and we console log the result of that, you however get this object object thing here.

* Now that simply happens because the to string method called on any object just prints this, the to string method exists because it exists on the default prototype every object has but of course the output here is not useful.

* The fact that we can look into the object with console log is a little convenience offered by the Chrome browser in the end, it's not something we can rely on, normally it should actually just print the result of to string because that's what console log typically does but for objects the browser gives us this extra utility of actually printing the object in an analyzable way.So to string is something we can call though to convert the object to a string which we can show to a user for example.Now as I mentioned, what we then get is this here [object object],

* now you can add a special symbol here to the user object, again with this dynamic property assignment syntax and here I want to use one of the built-in symbols.

```js
const user = {
  // id: 'p1',
  [uid]: 'p1',
  name: 'Max',
  age: 30,
  [Symbol.toStringTag]: 'Guna' //  toStringTag
};
```

* Now toString tag allows us to define a tag as a value, so a string tag here which will then be part of the output that is generated here,

```js
console.log(user.toString()) // [object Guna] 
```

* So this allows us to for example tweak what the to string method outputs here, of course we could also simply override the entire to string method if we want to customize it completely but if we just want to change that tag which is part of the default output, we can do that with this built-in symbol.

* Now whilst this is not something you will use everyday because we could argue how useful and important that is, it shows you what the idea behind these well-known built-in symbols is, they are used by some Javascript code, some default Javascript code in this case here by the to string method which exists on every object and Javascript looks up these symbols and if it then finds them, which it does here when we define it in our own object, it uses them.

* this would not work if we would try to create our own symbol to string tag

```js
const user = {
  // id: 'p1',
  [uid]: 'p1',
  name: 'Max',
  age: 30,
  [Symbol('toStringTag')]: 'Guna' // toStringTag - just an identifier for you as a human, this is totally ignored by Javascript, this does not create a symbol of the same kind we just used. would not work if we would try to create our own symbol to string tag
};
```
* This creates a brand new symbol which Javascript simply doesn't look for,

* so we have to use this symbol which is built-in which Javascript is aware of and which Javascript looks for because it uses this all the time internally. So that's the idea behind symbols, you can create unique identifiers for your objects which you have to know in order to use them.

### Understanding Iterators

* An iterator is simply an object in the end which has a next method, which then in turn returns a result of a certain structure.

* For that let's say we have another constant, company and that holds an object where we have employees and there I got Max and I got Manu and I got Anna in there. Now this is a regular object, we make it iterable or we turn it into an iterator or you could say by adding a next method here which we can add like this,

```js
const company = {
  curEmployee: 0,
  employees: ['Max', 'Manu', 'Anna'],
  next() {
    if (this.curEmployee >= this.employees.length) {
      return { value: this.curEmployee, done: true };
    }
    const returnValue = {
      value: this.employees[this.curEmployee],
      done: false
    };
    this.curEmployee++;
    return returnValue;
  }
};

console.log(company.next()); // {value :'Max', done : false};
console.log(company.next()); // {value :'Manu', done : false};
console.log(company.next()); // {value :'Anna', done : false};
console.log(company.next()); // {value : 3, done : true};
console.log(company.next()); // {value : 3, done : true};
```
* Now what's the idea behind this next method though? Well now we can console log company next and execute this a couple of times down there. If we save that and we reload, you see we print all different employees until we're done.

* So what we do here in the end is we create our own method that allows us to loop through a specific field of this object and we could have any looping logic we want in here. So this in the end kind of makes this a loopable object,

* So we have a method which we can call multiple times and every time it will return something different and not just that, it will actually go through some list of data, it will exhaust some list of data and when we exhausted that list, we'll not get an error, we'll simply get a response, we'll get back a value where we see that we're done because of this uniform return value where we always have a done property which is true or false,

* we also know if we can call next again and have more values left or if that's not the case. Now this alone as I said might not be totally helpful yet but this already is a logic you could use in some applications where you have an object which you use as a data storage and you want to go through some custom logic there to output some data which is stored in there, you don't want to use a for/in loop because you're maybe not interested in all the properties of this object but just in some of the values in one specific property of this object and then such a custom looping logic could be quite interesting here

```js
let employee = company.next();

while(!employee.done) {
  console.log(employee.value);
  employee = company.next();
}
```
* now we have a dynamic loop, not a for/of loop but still a loop which will in the end go through all employees with our own iterator logic.

### Generators & Iterable Objects

* So that's an iterator. Now iterators are nice and with a while loop, you can create your own looping logic

```js
for (const employee of company) {
  console.log(employee);
}
// we will get error as company is not iterable.
```
* Now we get this next method but Javascript is actually not looking for a next method when you use a for/of loop, instead what it is looking for is a special symbol.

* It's looking for a symbol which I can add here in company of type symbol.iterator, another well-known symbol built into Javascript obviously otherwise for/of couldn't be looking for it and now what is the value for that symbol, that's the interesting question.

* The value has to be an iterator. Now remember, company overall is an iterator because any object with a next method is an iterator.

* So what we store for this symbol iterator property now should be such an object with the next method, so not company should have the next method, well it can but that doesn't help us but what we store in the symbol iterator properties should be an object with the next method so that we can use for/of and now instead of creating such an object manually again, we can use something else which builds us such an iterator and that would be a generator.

* A generator is something in Javascript which builds us an iterator object, so which builds as an object that does have a next method built-in.

* A generator in the end is a special type of Javascript function which automatically generates iterators for you, so which generates objects that have a next method you could say, that's a generator. 

* You create such a generator by using the function keyword and then and that's special, you add a star after this keyword, this turns this function into a generator.

```js
const company = {
  employees: ['Max', 'Manu', 'Anna'],
  [Symbol.iterator]: function* employeeGenerator() { // any name as user decide - you can use any name you want here or of course also write an anonymous function, * is really important

  // Now inside of this generator function, you can now write your looping logic.
    let currentEmployee = 0;
    while(currentEmployee < this.employees.length) {
      yield this.employees[currentEmployee];
      currentEmployee++;
    }
  }
};
```
* now here, I'm looping through all my employees using my own next methods, I'm still using this, we still have that

* Now what does this all do? Well this here in the end generates a new object, this function when it's called generates a new object which has a next method on its own.

* this generator function builds you such an object. It gives you basically what we wrote before manually out of the box and the yield keyword will then basically define the return value of every call to the next method in that generated object.

* Now it sounds complex, we'll see it in action in a second of course and it executes until it encounters yield and then it returns the value after yield and pauses at the point where you had called yield and when you then execute the same function again, it does not start from scratch again but continuous running at the point where it paused before

* Lets use a functname instead of Symbol.iterator

```js
const company = {
  employees: ['Max', 'Manu', 'Anna'],
  getGenerator : function* employeeGenerator() { 
    let currentEmployee = 0;
    while(currentEmployee < this.employees.length) {
      yield this.employees[currentEmployee];
      currentEmployee++;
    }
  }
};

console.log(company.getGenerator().next()); // {value :'Max', done : false};
console.log(company.getGenerator().next()); // {value :'Max', done : false};
console.log(company.getGenerator().next()); // {value :'Max', done : false};
console.log(company.getGenerator().next()); // {value :'Max', done : false};
console.log(company.getGenerator().next()); // {value :'Max', done : false};
```
* Now as you see, it always however yields me Max here, the reason for that is that whenever I call get employee, I generate a new iterator.

```js
const company = {
  employees: ['Max', 'Manu', 'Anna'],
  getGenerator : function* employeeGenerator() { 
    let currentEmployee = 0;
    while(currentEmployee < this.employees.length) {
      yield this.employees[currentEmployee];
      currentEmployee++;
    }
  }
};

const it = company.getGenerator();

console.log(it.next()); //{value :'Max', done : false};
console.log(it.next()); //{value :'Manu', done : false};
console.log(it.next()); //{value :'Anna', done : false};
console.log(it.next()); //{value :undefined, done : true};
console.log(it.next()); //{value :undefined, done : true};
```

* now why is that useful now? Well for one we have a very simple way of creating an iterator, we don't have to write our own next method as we did before, we just have this short logic here and yield is the special thing here together with the star. This allows Javascript to build such an iterator behind the scenes and whenever yield is encountered, this basically is the point where Javascript saves the current state of execution and the next time you call the next method on the created iterator, it will continue from that point on and therefore then give you the next value and the next value and the next value, wrapped into such an object where the actual value you yielded can be found on the value key and you get this additional done property that tells you whether there are more values left or not.

```js
const company = {
  employees: ['Max', 'Manu', 'Anna'],
  [Symbol.iterator]: function* employeeGenerator() {
    let currentEmployee = 0;
    while(currentEmployee < this.employees.length) {
      yield this.employees[currentEmployee];
      currentEmployee++;
    }
  }
};

for (const employee of company) {
  console.log(employee);
}
// Max
// Manu
// Anna
```

* because what a for/of loop does is it goes to this object you're looping through and it searches for this symbol iterator thing there, then it executes the function which it finds there which should be a generator so that in the end this returns an iterator, right?

* If you execute that generator function, you get an iterator as you learned and then Javascript executes the next method on this iterator it got for you as long as done is not true and extracts the thing in the value property into this constant here which you can then use and consume inside of your for/of the loop body and this is how you can write your own for/of loopable objects with the help of this special well-known iterator symbol and this generator function.

* Now of course iterators and generators, these are very advanced features but this really can be handy if you want to write your own looping logic. You can either do it manually by adding your own next method and making sure it gets called correctly or by simply writing such a generator function which together with the yield keyword allows you to define when the function should pause or when a new value should be emitted the next time the function is called again and again until there are no more values left, so until there are no more yield calls found in the function execution which in this case happens once you make it out of this while loop

* and then together with this special symbol, you can use for/of, you can also use the spread operator now on this object for example

```js
console.log([...company]); // ["Max", "Manu", "Anna"]
```
* If I console log company here, this normally would not work, not like this at least but here you see it works and it gives me these three employees because the spread operator also behind the scenes looks for this iterator symbol, it then goes through all the values we get there and adds them as elements here in this new array.

### Generators Summary & Built-in Iterables Examples

* So iterators and generators can be a confusing concept.

* In the end, just keep in mind that a generator function, the function with that star here and with the yield keyword is a function that returns a new object which has a next method

* using a generator function is simply easier, it's less code. What you yield here in the end will be the returned value of the next method later 

* the generator function will create an object with such a next method where the next method will automatically return an object with a value property, with whatever you yielded here or undefined if you're not yielding anything new anymore and a done property which is either true if it is done or false if there are more values that can be extracted and it's this exact concept, this iterator symbol with such an iterator object that is used under the hood by arrays.

* Now if we check the prototype of the array object, you will actually find this iterator symbol if you watch closely, this is something an array has on its prototype. It has all these array methods you learned about - filter, find and so on but it also has this special iterator symbol and the function here is then just a generator which yields us such an iterator object, an object with a next method.

* So an array, this default array we worked with all the time internally just uses the same logic we set up here and at least for that, even if you don't need to write this code on your own that often, this should be helpful for understanding how arrays work internally and how they are managed and how the data they return is managed

### The Reflect API

* what is the reflect API all about?

* It is totally not related to iterators and generators and symbols, it is related to objects though.

* It is an API that allows us to control objects, to do things with Javascript objects and in the end it's just an object itself, reflect is an object in Javascript which groups, with a bunch of static methods on this class so to say, which groups a lot of functionalities that help us work with objects.

* It has standardized and grouped together methods and the reflect API, just like all these features, can be used to control how our code is used, how our code behaves and so on.

* Now let's see an example and let's see what the reflect API offers us.

* Now we got this reflect API which we can access on this global reflect object by adding a dot thereafter because this has a bunch of static methods we can use, because this has a bunch of static methods we can use, for example it has a define property method, it has a delete property method, it has a getPrototypeOf method and it has a setPrototypeOf method and for example we can use setPrototypeOf to set the prototype of our course object to an object where I have a to string method, my own to string method where I return this.title

```js
const course = {
  title: 'JavaScript - The Complete Guide'
};

// Now we got this reflect API which we can access on this global reflect object by adding a dot thereafter because this has a bunch of static methods we can use,

Reflect.setPrototypeOf(course, {
  toString() {
    return this.title;
  }
});

Reflect.deleteProperty(course, 'title');

// Object.deleteProperty(course, 'title');

// delete course.title;

console.log(course);
```
* The obvious question that just could arise at this point is why would we use reflect for this? If we used a global object, object with a capital O, we also have a setPrototypeOf method, we also have a define property method and it generally works the same way as the reflect API methods. So why do we have the reflect API if we can already do these things with the existing object API?

* The reflect API is newer, it was added recently with ES6 to Javascript, the object API is way older. Now that alone of course is no reason for using it, instead the reflect API has two advantages over the object API -

* for one there are subtle differences regarding the behavior of some methods, for example if a method fails to do its job, on the object API it might return undefined or fail silently, on the reflect API you might get a better error or it just returns True or False for a given method to tell you if something works or not

* Refer the differences : https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Reflect/Comparing_Reflect_and_Object_methods

* Additionally, the reflect API bundles all features that you need to work with objects, for example it also has a delete property method which we don't find on the object API, there this is not a thing, if I try to call delete property here on course and pass in the title with the object API, then you will see that if I print my course down there and I reload, this does not work because object delete property is not a function. The object API has no delete property, instead previously in older Javascript, if you wanted to delete a property in an object, you would have to use the delete keyword and then target the property you want to get rid of. Now you can do that but it's a bit of a strange code, having this special operator here, I think it's a bit cleaner to have a method you can call on an object if you want to get rid of a property because on the other hand we also do have a method for defining a property, so why not have one for deleting it?

* Well that's for example one thing the reflect API added, there you now have a proper delete property method and that simply is cleaner than using this delete operator.

* it's there to group all the methods you need to configure your objects, to work with your objects and you don't have to fall back to different approaches where some things are defined on object and other things are not defined there and you suddenly need a special operator. Now the reflect API gives you everything you need to work with objects all bundled in this global reflect object here and that's basically the idea behind the reflect API.

* Whenever you need to do such meta work on your objects - delete a property, define a new one, change the prototype after the object has been created, then you should consider using the reflect API over the other scattered methods or operators you might be able to use for that, that's the primary reason for the existence of the reflect API.

### The Proxy API and a First "Trap"

* And that leaves us with the proxy API, just like the other features here it's basically a meta feature which allows us to tweak our objects or add some extra functionality to our code that kicks in when our people work with our code, which is why it's called meta programming.

* We configure our code to behave in a certain way when other people use it, which is this extra level of thinking we added here.

* Now the proxy API is all about creating so-called traps for certain object operations and you can almost take the words trap here literally. With the proxy API, you can step it up on certain operations and execute your own code, for example if someone wants to retrieve a property, so the value for a property of an object, with the proxy API you can set up some logic that runs in that case which allows you to either change what is returned or do some additional task, again for this meta level where you might want to track property access or make sure the objects your library exposes are used properly.

```js
const course = {
  title: 'JavaScript - The Complete Guide'
};

Reflect.setPrototypeOf(course, {
  toString() {
    return this.title;
  }
});

// Reflect.deleteProperty(course, 'title');

// Object.deleteProperty(course, 'title');

// delete course.title;

console.log(course);

const courseHandler = {
  get(obj, propertyName) { // this is not userdefined name check link for different trap
    console.log(propertyName);
    if (propertyName === 'length') {
      return 0;
    }
    return obj[propertyName] || 'NOT FOUND';
  }
};

// Now this proxy constructor function takes the object on which this proxy should be applied, so here I'll pass in the course object,


const pCourse = new Proxy(course, courseHandler); // this creates a new proxy object 
console.log(pCourse.title, pCourse.length, pCourse.rating); // JavaScript - The Complete Guide 0 NOT FOUND
```

* Refer Traps here : https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy#a_complete_traps_list_example

*  I override what I fetched here with the help of the proxy API. Please note that neither course nor pcourse were changed by that proxy.

* symbols, iterators, generators, the reflect API and the proxy API all have their use cases but the use cases are pretty advanced and often found in scenarios where you are writing code which is not directly impacting end users of an application, so people visiting the web site but instead other developers that might depend on your code because you are authoring a third-party library, because you are working in a team or you're writing your own reusable code which you maybe want to reuse in different projects you're working on.

* Then these features can help you control how the code can be used, how it behaves, how certain things are locked down or controlled behind the scenes. You can make your objects loopable with the iterator, symbol and generators and iterators and with symbols in general, you also have a feature that allows you to have properties and objects which are not that easy to access and which most importantly are not that easy to change because you need the exact same symbol to access it.