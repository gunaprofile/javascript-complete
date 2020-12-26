# Javascript Complete

##  More Object

### Objects

* Refer : what-are-objects.pdf

### Objects & Primitive Values

* Objects are reference values - you learned that.

* It might not have been obvious yet but it's also important to recognize that, in the end, objects are of course made up of primitive values.

```js
const complexPerson = {
    name: 'Max',
    hobbies: ['Sports', 'Cooking'],
    address: {
        street: 'Some Street 5',
        stateId: 5,
        country: 'Germany',
        phone: {
            number: 12 345 678 9,
            isMobile: true
        }
    },
};
```
* Event though complexPerson has multiple nested reference values (nested arrays and objects), you end up with primitive values if you drill into the object.

* name holds a string ('Max') => Primitive value

* hobbies holds an array (i.e. a reference value) which is full of strings ('Sports', 'Cooking') => Primitive values

* address holds an object which in turn holds a mixture of primitive values like 'Some Street 5' and nested objects (phone), but if you dive into phone, you find only numbers and booleans in there => Primitive values

* So you could say: Primitive values are the core building blocks that hold your data, objects (and arrays) are helpful for organizing and working with that data.

### Adding, Modifying & Deleting Properties

```js
let person = {
  name: 'Max',
  age: 30,
  hobbies: ['Sports', 'Cooking'],
  greet: function() {
    alert('Hi there!');
  }
};

// ...

// person.age = 31;
delete person.age; // deleting a property basically set to undefined
// person.age = undefined;
// person.age = null;
person.isAdmin = true;

console.log(person);
```

* deleting a property basically set to undefined, but it is a good rule to keep in mind that you should actually never assign undefined to any value.

* NULL which really just means the person has this property. We just don't have a value in there right now.

### Special Key Names & Square Bracket Property Access

```js
const movieList = document.getElementById('movie-list');

movieList.style['background-color'] = 'red';
movieList.style.display = 'block';
```
###  Property Types & Property Order

```js
let person = {
  'first name': 'Max',
  age: 30,
  hobbies: ['Sports', 'Cooking'],
  greet: function() {
    alert('Hi there!');
  },
  1.5: 'hello'
};

console.log(person[1.5]);
```
### Dynamic Property Access & Setting Properties Dynamically

```js
const userChosenKeyName = 'level';

let person = {
  'first name': 'Max',
  age: 30,
  hobbies: ['Sports', 'Cooking'],
  [userChosenKeyName]: 'anything', // [userChosenKeyName] ==> 'level'
  greet: function() {
    alert('Hi there!');
  },
  1.5: 'hello'
};

const keyName = 'first name';

console.log(person[keyName]);

```

#### Rendering Elements based on Objects

```js
const addMovieBtn = document.getElementById('add-movie-btn');
const searchBtn = document.getElementById('search-btn');

const movies = [];

const renderMovies = () => {
  const movieList = document.getElementById('movie-list');

  if (movies.length === 0) {
    movieList.classList.remove('visible');
    return;
  } else {
    movieList.classList.add('visible');
  }
  movieList.innerHTML = '';

  movies.forEach((movie) => {
    const movieEl = document.createElement('li');
    movieEl.textContent = movie.info.title;
    movieList.append(movieEl);
  });
};

const addMovieHandler = () => {
  const title = document.getElementById('title').value;
  const extraName = document.getElementById('extra-name').value;
  const extraValue = document.getElementById('extra-value').value;

  if (
    title.trim() === '' ||
    extraName.trim() === '' ||
    extraValue.trim() === ''
  ) {
    return;
  }

  const newMovie = {
    info: {
      title,
      [extraName]: extraValue
    },
    id: Math.random()
  };

  movies.push(newMovie);
  renderMovies();
};

addMovieBtn.addEventListener('click', addMovieHandler);

```
### for-in Loops & Outputting Dynamic Properties

```js
movies.forEach((movie) => {
  const movieEl = document.createElement('li');
  let text = movie.info.title + ' - ';
  for (const key in movie.info) {
    if (key !== 'title') {
      text = text + `${key}: ${movie.info[key]}`;
    }
  }
  movieEl.textContent = text;
  movieList.append(movieEl);
});
```
### Adding the Filter Functionality

```js
const searchMovieHandler = () => {
  const filterTerm = document.getElementById('filter-title').value;
  renderMovies(filterTerm);
};

const renderMovies = (filter = '') => {
  const movieList = document.getElementById('movie-list');

  if (movies.length === 0) {
    movieList.classList.remove('visible');
    return;
  } else {
    movieList.classList.add('visible');
  }
  movieList.innerHTML = '';

  const filteredMovies = !filter
    ? movies
    : movies.filter(movie => movie.info.title.includes(filter)); // movie -> includes -> filter title

  filteredMovies.forEach(movie => {
    const movieEl = document.createElement('li');
    let text = movie.info.title + ' - ';
    for (const key in movie.info) {
      if (key !== 'title') {
        text = text + `${key}: ${movie.info[key]}`;
      }
    }
    movieEl.textContent = text;
    movieList.append(movieEl);
  });
};
```

### The Object Spread Operator (...)

```js
var person = {name: "Max", hobbies: ["h1","h2","h3","h4","h5","h6"]};
var anotherPerson = person;
person.age = 31; // this will affect both person and anotherPerson

var person2 = {...person} // this will create a entire new person2 object
person.age = 41; // this will change only person not person2

person.hobbies.push("h7"); // this will affect person and person2 also why ??

```

* that's just something which I explained in the arrays section already. This is normal, this is how reference values behave and the spread operator does not do a deep copy where it goes through all level of nested reference values you might have in this object or array and then copies it from scratch, instead it just copies the top level key-value pairs into a brand new object and any nested reference values are kept, the addresses there are kept, there are no hobbies created. If you would want to copy those as well, you would have to do it manually by assigning a new hobbies array where you copy over the old array and that's actually quite interesting

```js
var person = {name: "Max", hobbies: ["h1","h2","h3","h4","h5","h6"], age : 30, lastName : "Karthick"};
var person3 = {...person, hobbies: [...person.hobbies], age : 50 } // this will copy/ create / overwrite person3 hobbies and age(if need to update age not must)
```
* Now it's not a must do that you always do that if you have nested reference values, you don't always need to copy everything, it's just an option that you can do that if you then plan on changing hobbies for example on person and you don't want that reflected on your copy persons, then you would have to use that approach, if you don't plan on changing hobbies ever, then you of course don't need to create that deep copy because ultimately, every operation costs a little bit of performance.

### Understanding Object.assign()

```js
var person = {name: "Max"};
// var person2 = {...person}  we can do like this but there is alternative approach is object.assign()
 var person2 = object.assign({}, person); // this will create a new reference object
```

### Object Destructuring

* Just like array destructuring 

```js
 const { info, ...otherProps } = movie;
```

### Checking for Property Existance

```js
if('info' in movie) { // info is a property of movie object
  // your logic
}
```
* Oppisite to check if not property exists

```js
if(!('info' in movie)){ 
  // your logic
}
```
### Introducing "this"

*  what's the this keyword?

* sometimes you want to bake certain logic into your objects,

```js
const newMovie = {
    info: {
      title,
      [extraName]: extraValue
    },
    id: Math.random().toString(),
    getFormattedTitle: function() {
      return this.info.title.toUpperCase(); // this refers to newMovie object 
    }
  };
```
* the this keyword will refer to whatever called that function, whatever was responsible for executing that function.

* So this is the keyword to tell Javascript look into the object where this function is part of, though to be precise as you learned, look at the thing which is responsible for executing the function which typically is this newMovie object since this function is part of that object and then dive into some info property, into some title property and try to call toUppercase on this.

* Then wherever need formatted title we can call like below

```js
const { getFormattedTitle } = movie;

or

let text = movie.getFormattedTitle() + ' - ';
```

### Method shorthand syntax


```js
const newMovie = {
    info: {
      title,
      [extraName]: extraValue
    },
    id: Math.random().toString(),
    getFormattedTitle() { // . So you no longer have a traditional key-value pair here with a colon but instead you have this syntax.
      return this.info.title.toUpperCase(); 
    }
  };
```
* So this is now a method shorthand syntax and it behaves just as before,

### The "this" Keyword And Its Strange Behavior

* So now what's the problem with this?

* Let's go back to the place where we use get formatted title and let's go back to destructuring. So I pull out get formatted title from movie, we can still do that even with the shorter function or method to be precise syntax, it still can be pulled out with object destructuring but of course now we just use it like that.

```js
const { getFormattedTitle } = movie;
let text = getFormattedTitle() + ' - '; // since we destructured we can directly use like this
```
* Now with this change this will throw an error "cannot read the property of title" because info is undefined in below code

```js
getFormattedTitle: function() {
      return this.info.title.toUpperCase(); // this.info is undefined why ???
    }
```
* we do have this info property which holds an object in movie so what's wrong? Remember, this does not automatically refer to the object that kind of surrounds it, it instead refers to who or what was responsible for calling this function

* and I mentioned that the best way to memorize this is to look in front of that function. Previously, we had movie. and therefore movie, that object, was in the end responsible for triggering this function.

* Now we have nothing there and then the thing responsible for triggering the function is our global execution context. In non-strict mode which I'm using here, this will then actually refer to the window object

* that's just the default in non-strict mode and then this, if it refers to nothing else, refers to the global object.

* If we were in strict mode which we can quickly enter of course by just adding use strict at the top, so if we were in strict mode, if we tried it again, you will see that this will actually be undefined, either way it will never refer to my movie.

* So how can we work around that then, how can we make sure that this refers to the right thing? We can do that with something we already learned about earlier in this course, the good old bind method.

* In the past we used bind() to preconfigure argument. Now we can also use bind to not only preconfigure arguments a function will get but also to preconfigure what this will refer to.

* So in this case here, of course one fix would be to simply go back to movie.right, this worked and there is nothing wrong with it

```js
let text = movie.getFormattedTitle() + ' - ';
```
* but if that isn't an option or for whatever reason we don't want to do it, we can use bind here as well,

```js
let { getFormattedTitle } = movie;

getFormattedTitle = getFormattedTitle.bind(movie); 

```
* now we're saying when this function executes which it does here, then this inside of the function, so here this keyword should not refer to what it normally would refer to, which is the thing that is responsible for executing the function but instead in this example here, it should refer to this movie

```js
"use strict";
const addMovieBtn = document.getElementById('add-movie-btn');
const searchBtn = document.getElementById('search-btn');

const movies = [];

const renderMovies = (filter = '') => {
  const movieList = document.getElementById('movie-list');

  if (movies.length === 0) {
    movieList.classList.remove('visible');
    return;
  } else {
    movieList.classList.add('visible');
  }
  movieList.innerHTML = '';

  const filteredMovies = !filter
    ? movies
    : movies.filter(movie => movie.info.title.includes(filter));

  filteredMovies.forEach(movie => {
    const movieEl = document.createElement('li');
    const { info, ...otherProps } = movie;
    console.log(otherProps);
    // const { title: movieTitle } = info;
    let { getFormattedTitle } = movie;
    getFormattedTitle = getFormattedTitle.bind(movie);
    let text = getFormattedTitle() + ' - ';
    for (const key in info) {
      if (key !== 'title') {
        text = text + `${key}: ${info[key]}`;
      }
    }
    movieEl.textContent = text;
    movieList.append(movieEl);
  });
};

const addMovieHandler = () => {
  const title = document.getElementById('title').value;
  const extraName = document.getElementById('extra-name').value;
  const extraValue = document.getElementById('extra-value').value;

  if (
    title.trim() === '' ||
    extraName.trim() === '' ||
    extraValue.trim() === ''
  ) {
    return;
  }

  const newMovie = {
    info: {
      title,
      [extraName]: extraValue
    },
    id: Math.random().toString(),
    getFormattedTitle() {
      console.log(this);
      return this.info.title.toUpperCase();
    }
  };

  movies.push(newMovie);
  renderMovies();
};

const searchMovieHandler = () => {
  const filterTerm = document.getElementById('filter-title').value;
  renderMovies(filterTerm);
};

addMovieBtn.addEventListener('click', addMovieHandler);
searchBtn.addEventListener('click', searchMovieHandler);

```
### call() and apply()

* Now you might argue though that this syntax is a bit redundant or this code here is a bit redundant, if we pull out get formatted title just to then reassign it to itself with the right this configuration and you'd be right.

* Bind is useful whenever you want to preconfigure a function for the future execution but here we actually plan on executing the function right away, so instead of doing that we can also use a different method which we can call on a function.

* Besides bind, you also have call. Call also takes multiple arguments, the first argument just as for bind is what this, the this keyword should refer to inside of the function that is about to be executed, second and more arguments, you can add as many as you want, are then the arguments that are passed into the function if it needs any.

* Now here we don't need to pass in any arguments so I'm only interested in the first parameter which we pass to call and that again would be movie here.

```js
 let { getFormattedTitle } = movie;
    getFormattedTitle = getFormattedTitle.call(movie);
```
* So how is call different from bind then? Well bind prepares a function for future execution, bind returns a new function object in the end which we then store here in get formatted title, call does not do that, call instead goes ahead and executes the function right away.

* so it executes a function for you when you want to change what this refers to, that's where call is important.

* we also got apply, apply is pretty similar to call, it also will execute the function right away, the difference is that there, first argument still is what this should refer to but then you don't have an infinite amount of additional arguments but instead it only takes one additional argument and that however has to be an array and that can now be your values for the different arguments this function might be taking

```js
 let { getFormattedTitle } = movie;
    getFormattedTitle = getFormattedTitle.apply(movie,[]);
```

* So in the end, the difference is call allows you to pass additional arguments as a comma separated list, apply allows you to pass additional arguments as an array,

* always keep in mind, this inside of a function always refers to what called that function or to be precise, always refers to the thing in front of your function execution here you could say.

### What the Browser (Sometimes) Does to "this"

* Now one use case where this thing in front of your function execution will not really work is for example when you bind or set your function on an event listener. So the key thing really is that this refers to what called a function, the thing with what's in front of the function only works if you're executing the function on your own in your code.

* If you're setting it to an event listener like we're doing here with add movie handler, then we'll see something interesting,

```js
const searchMovieHandler = function (){
  console.log(this)
  const filterTerm = document.getElementById('filter-title').value;
  renderMovies(filterTerm);
};

searchBtn.addEventListener('click', searchMovieHandler);
```
* would you expect that this refers to here? Well since there is nothing in front of it, it would be the global context right but that's wrong,

* what's in front of it thing only makes sense if you're executing a function on your own, so if you added parentheses or if you're using apply or call, here we're not doing that.

* We're indirectly executing this if you will by binding it to an event listener. So therefore we have to focus on the first definition I named, which is that this refers to who's responsible for calling this and here, that will actually be the event if you will, so the browser will trigger that event where we click on a button the browser then triggers the function,

* so the browser is kind of responsible for executing this but in the end it's this event which is responsible, right? It's this click event which in the end is responsible for triggering this function and if we break it down even more it's the button that is responsible for executing this,and that's actually how the browser configures this. 

* When a function executes based on an event, then this inside of the function will actually refer to the object, to the element that's triggered that event which in the end triggered that function.

* To Make this really clear: The browser binds "this" for you (on event listener) to the DOM element that trigger that.

* However that's now also important, only if you're not using an arrow function because there is something special about arrow functions to which I'll come back in a second.

* click on search, we indeed see the button is output there, so this inside of a function that's triggered based on an event listener refers to the element or to the thing that is responsible for triggering this event.

* Now I said for arrow functions that would be different though, so let's have a look at that now.

### "this" and Arrow Functions

```js
const searchMovieHandler = () => {
  console.log(this); // this refers to window object
  const filterTerm = document.getElementById('filter-title').value;
  renderMovies(filterTerm);
};

addMovieBtn.addEventListener('click', addMovieHandler);
searchBtn.addEventListener('click', searchMovieHandler)
```
*  arrow functions have a lot of advantages, for example shorter syntax and so on and one other advantage which they have which really only makes sense now that we know about this is that they don't know "this". 

* Every function has its own this, every function created with the function keyword or with this method shortcut here has its own this binding,

* so in the end it ensures that this inside of that function is bound to something, it's bound to whatever is responsible for executing the function.

* This outside of the arrow function and this inside of arrow function refers to the same thing.

* Now if you paid close attention, this might be strange, shouldn't use strict lead to undefined being logged here? Why do we log the window? Use strict indeed leads to undefined being logged instead of the global window object

* if you're using this in a normal function with the function keyword and this is not bound to anything else, that's the use case we had earlier,

* Now arrow functions don't know this, they don't know the this keyword, therefore they don't trigger this strict mode fix or however you want to call it, where this inside of functions doesn't lead to the global object being referenced because this inside of an arrow function behaves exactly like this outside of that arrow function because again, arrow functions simply don't know this, they don't assign any special meaning to it, this inside just behaves as outside of the arrow function and there, strict mode does not lead to this, not referring to the global object, that's just a fix that is applied to this inside of normal functions that's not bound to anything else.

* but then this might be undesired but actually in a lot of use cases, arrow functions can fix strange this behavior.

* this refers to the same thing it would refer to outside of the function.

* Just keep it in mind that arrow functions don't bind this to anything, instead they keep the context or the binding this has to the binding it would have outside of the function

* For now, just be aware of that special behavior of arrow functions, they work differently with this than normal functions, with the function keyword and that can or cannot be desired.

```js
const memebers = { 
  teamName : "Bonjour",
  people : ["Guna", "maneesh"],
  getTeamMembers(){
    this.people.forEach(p => {
      console.log(p+ " - " + this.teamName);
    });
  }
}
// Output -> memebers.getTeamMembers();
// Guna - Bonjour
// Maneesh - Bonjour
```
* This should refer to our members object inside of get team members because I'm calling get team members on members like this with a dot, so this should refer to members

* let me show you how this would work without an arrow function there.

```js
const memebers = { 
  teamName : "Bonjour",
  people : ["Guna", "maneesh"],
  getTeamMembers(){
    this.people.forEach(function(p){
      console.log(p+ " - " + this.teamName); 
    });
  }
}
// Output -> memebers.getTeamMembers();
// Guna - undefined
// Maneesh - undefined
```
* why undefined ?? Previously it worked, now it doesn't anymore, now why is that?

* The reason is the way we create this function which we pass to ForEach. Now when I create this function like this with the function keyword.

* This function gets executed on our behalf by ForEach and that in the end happens when we call get team members here.

* So we don't know how exactly the browser executes function here for us. For event listeners, we saw that there, it would actually bind this to the object that triggered the event, that was something the browser did, now for ForEach, it doesn't seem to do any binding and therefore it just lets this be bound to the global object.

* It certainly does not bind it to the surrounding object because this function gets executed because of ForEach, which is inside of that object but that's the only connection, it's not our object itself that would trigger this function somehow, instead it's ForEach and therefore the browser which triggers this function.

* So this has the wrong value when we use it like that and the correct value if we use an arrow function instead here inside of ForEach because the arrow function doesn't change the binding of this and therefore this has the binding it would have if we write it outside of this function and what is outside of this function? Right, it's the get team members function and what is the binding of this in get team members? It's our object. That's why it works if we have an arrow function here.

* and why doesn't work when we have this function. This function tries to bind this and it binds it to what this refers to when the function executes which is the global object,

### "this" - Summary

* The this keyword can lead to some headaches in JavaScript - this summary hopefully acts as a remedy.

* this refers to different things, depending on where it's used and how (if used in a function) a function is called.

* Generally, this refers to the "thing" which called a function (if used inside of a function). That can be the global context, an object or some bound data/ object (e.g. when the browser binds this to the button that triggered a click event).

* 1. this in Global Context (i.e. outside of any function)

```js
function something() { ... }
 
console.log(this); // logs global object (window in browser) - ALWAYS (also in strict mode)!

```
* 2. this in a Function (non-Arrow) - Called in the global context

```js
function something() { 
    console.log(this);
}
 
something(); // logs global object (window in browser) in non-strict mode, undefined in strict mode
```
* 3. this in an Arrow-Function - Called in the global context

```js
const something = () => { 
    console.log(this);
}
 
something(); // logs global object (window in browser) - ALWAYS (also in strict mode)!
```
* 4. this in a Method (non-Arrow) - Called on an object

```js
const person = { 
    name: 'Max',
    greet: function() { // or use method shorthand: greet() { ... }
        console.log(this.name);
    }
};
 
person.greet(); // logs 'Max', "this" refers to the person object
```
* 5. this in a Method (Arrow Function) - Called on an object

```js
const person = { 
    name: 'Max',
    greet: () => {
        console.log(this.name);
    }
};
 
person.greet(); // logs nothing (or some global name on window object), "this" refers to global (window) object, even in strict mode
```

* this can refer to unexpected things if you call it on some other object, e.g.:

```js
const person = { 
    name: 'Max',
    greet() {
        console.log(this.name);
    }
};
 
const anotherPerson = { name: 'Manuel' }; // does NOT have a built-in greet method!
 
anotherPerson.sayHi = person.greet; // greet is NOT called here, it's just assigned to a new property/ method on the "anotherPerson" object
 
anotherPerson.sayHi(); // logs 'Manuel' because method is called on "anotherPerson" object => "this" refers to the "thing" which called it
```
* If in doubt, a console.log(this); can always help you find out what this is referring to at the moment!

```js
const person = {
    name: 'Max',
    greet() {
        console.log(this); // ???
        console.log(this.name);
    }
};
 
const { greet } = person;
greet();

// The Global object (or undefined) That's correct! Due to the way greet is called (not "on" any object), "this" will refer to the global object or undefined (in strict mode).
```

### Getters & Setters

* there's one last nice feature which I want to introduce here and that are getters and setters, now what's that?

* Now we have normal properties here, info for example and sometimes we want to control how a property can be set, so how a value can be assigned or how you can get it, so how you can retrieve it. Let's say here on our title, here I'm checking the validity of the inputs and then I'm assigning these properties all in one go when I create the new movie object. Well let's say for the title, so for properties where we know in advance that they will be part, I want to do that differently.

```js
  const title = document.getElementById('title').value;
  const extraName = document.getElementById('extra-name').value;
  const extraValue = document.getElementById('extra-value').value;

  if (
    extraName.trim() === '' ||
    extraValue.trim() === ''
  ) {
    return;
  }

const newMovie = {
  info: {
    set title(val) { // and for the outside world, you have title with your getter and setter
      if (val.trim() === '') {
        this._title = 'DEFAULT'; // _title  to make it clear that this is like the internal value
        return; 
      }
      this._title = val;
    },
    get title() { // title- name can be anything not necessarily title, and for the outside world, you have title with your getter and setter
      return this._title;
    },
    [extraName]: extraValue
  },
  id: Math.random().toString(),
  getFormattedTitle() {
    console.log(this);
    return this.info.title.toUpperCase();
  }
};
```

* Now So how do we now use that?

* You use it like a property, so you don't add parentheses here, instead you can assign a value like you assign it to a property because setters and getters are a special kind of property, a special syntax

understood by Javascript 
```js
// setter which will be triggered whenever we assign a value to this title property 
newMovie.info.title = title; // title -> we already called at top as document.getElementById('title').value;
console.log(newMovie.info.title); // the getter is triggered whenever we access the property, for example like this.
```
*  When I access it and want to read from it, then the getter runs, so then Javascript looks for this title property and again even though this looks like a method, we don't execute it as one because combined with the get keyword, Javascript allows us to access it like a normal property and it will execute this function for us and here we could also do additional transformations, additional checks

* So getters and setters can be nice if you want to add some extra validation, maybe a fallback or add some extra transformation when getting a value and as I mentioned, what also is nice is of course that it allows you to also work with kind of read only values.

* If we wouldn't have a setter here, if we tried to assign it, we'll actually get an error if I reload here and we try to enter Javascript here and then any values here, you'll see I get an error because I can't set a property which only has a getter.

* So this allows us to add read-only properties and make sure you can only assign it here initially maybe but never thereafter by adding a getter but no setter.

*  You can also add both or none of that, you'll not need getters and setters for every single property you use in your projects, actually you will only use them sometimes but it's nice to know about them and to be able to create read-only properties, to add extra validation with fallback values and to also set up some default transformations for when you're reaching out to some data.

