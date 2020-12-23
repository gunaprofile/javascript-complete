# Javascript Complete

## Working with the DOM (Browser HTML Code) in JavaScript

### What's the "DOM"?

* Now what is that DOM thing? For that, let's understand what the browser does with the HTML code which is part of any web page we create and how Javascript works with that.

* So we get Javascript and we get the browser, these two pieces which interact all the time

* Refer : dom1

* So what actually happens is that when this HTML file, the HTML document is downloaded, the browser goes over it and parses it and renders it

* because the browser simply reads that HTML file from top to bottom and when it encounters a script, it executes it, when it encounters other HTML elements, it simply parses and renders those and in the end, it then renders the pixels on the screen which you need to see to see a button, to see a title and so on, so that's what's happening here. Now Javascript might be part of that web page, Iitcertainly is in all the web pages we work with in this course and it's loaded by the browser.

* Now don't forget that Javascript is a hosted language,that means the browser provides the environment for Javascript to run, it provides the Javascript engine which in the end parses and understands all the Javascript code and executes it in the end.

* So the browser provides that, it also provides a bunch of APIs, a bunch of functionalities into which Javascript can tap so that Javascript can interact with the browser.

* We also saw that already and it will become super important here now when we work with the loaded and rendered HTML code because the browser actually exposes functionality to let Javascript interact with that rendered HTML code and in the end that's called the Document Object Model,the abbreviation of course is DOM and that's where this term comes from.

* So the DOM in the end is this loaded and rendered HTML code or to be precise, the object representation of this code which the browser creates behind the scenes into which we can tap with Javascript.

* So Javascript can work with a bunch of objects which will be exposed to us as Javascript objects which in the end represent what the browser rendered or what the browser made of that HTML code which was provided.

* I also briefly want to mention that the DOM is not strictly tied to browsers, there are other tools which can also parse HTML that's not even restricted to Javascript, so in other languages like Python you also can find certain tools or certain plugins, extensions which allow you to read and work with HTML code or also with Javascript if you're running it through Node.js. let's say, that does not have this DOM functionality built in because you typically use it on the server side or it's simply detached from a browser but you can still add certain packages to kind of bring that back in and still be able to parse and read a HTML file.

* The browser however has it all built in for you and in the end, it has to import and built-in global objects you could say which kind of grant you this access and we can see one on this screen already, this document object here in the end which is globally available which is not created by you but which the browser exposes to you to give you access to all the different ways of interacting with that HTML page.

* Now I mentioned the document, this is one important piece in working with the loaded HTML code, there also is another important global object and actually, document is a property of that other global object and that's the window object.

* Refer : dom2

* Now the difference is that document in the end is the root DOM node which the browser exposes to you. That means that this is really the topmost entry point to get access to all that rendered HTML code.

* So this provides you various methods and functionalities to get access to the elements, to query for elements, to query for HTML elements, to interact with its DOM contents, so to interact with a loaded HTML code

* window on the other hand is a global object which as I just said actually has document as property, so window is the real topmost global object made available to you in Javascript in the browser and that in the end reflects the active browser window or tab if you want to call it like this.

* So it's basically your global entry point, your global storage if you want to call it like this for your script, so it gives you access to all the features that the browser wants to expose to you, the root entry point is always to the window object but it also gives you some window specific properties for example for measuring the window width or anything like that.

### Document and Window Object

* console.dir(document) -  document also part of window object
* window - you won't be able to use window to interact with a totally different web page loaded in a different tab because that would be a huge security issue if you could start reading information from another tab on your web page here, that would not be something you would want to do because you could fetch important information from other tabs.

* So whilst it's called window, it really just means the currently loaded tab but also the dimensions of the general window and so on, that's how you can think of it. 

### Understanding the DOM and how it's created

* Refer : the-document-object-model-dom.pdf

* In developer-tool select dom element and then in console do $0 - then you can check selecte dom element

* try console.dir($0); you print that selected element object

### Nodes & Elements - Querying the DOM Overview

* Refer : nodes-vs-elements.pdf, querying-elements.pdf

* Refer : dom3

* So you can get access to single elements with query selector and get element by ID

* Query selector takes a CSS selector as you could use it in a CSS file, even pseudo selectors are supported and gives you therefore a lot of flexibility and power when it comes to selecting elements with complex query in your DOM. Get element by ID on the other hand takes an ID which you might have assigned to an HTML element and selects an element by that and since ID should be unique on your web page, this is a method which returns one element.

* The query selector method takes any CSS selector and therefore this might be a selector that matches multiple elements but in this case, query selector, this method will always give you access to the first matched element on the page, so in the DOM.

* DOM Nodes are just Javascript Object in the end - ie reference values. These methods returns the object reference(address)

* query selector all, get elements by tag name and there are other methods as well which I'll show you give you access to collections of elements, array-like objects, typically a node list, though that also is not always the case but typically it's such a node list which is not a real array but array-like, so which supports certain behaviors of an array but not all of them.

* You have different ways of querying elements, you can for example use the above methods - query selector all takes a CSS selector as query selector but unlike query selector, it does not return the first element which it matched but all elements that match the selector. Get elements by tag name give you all the elements that have a certain HTML tag and then there are other selectors to select by class name or by the name attribute which you might have assigned, though the query selector all method is the most flexible one where you have the power to select anything you want by CSS selectors.

* let's dive into nodes and elements. I kind of use these terms interchangeably but actually should be careful and you should at least understand what the difference is.

* So you have nodes and elements and nodes are the objects that make up the DOM, everything in the DOM is a node. HTML tags are just element nodes as I mentioned, I will also refer to them as just elements but in the end, these are element nodes to be super precise.

* We also have for example text nodes and there are other nodes as well which aren't really that important though and which with you'll not really work. Attached, you will find a thorough document about all supported nodes but text nodes and element nodes are the important ones.

* Refer : https://developer.mozilla.org/en-US/docs/Web/API/Node/nodeType

* Now elements therefore are in the end just element nodes as I just explained, so elements are really just the nodes which are created off of HTML tags which were rendered, not the text in there.

* Now why does this matter? Because on element nodes, on elements therefore, you have special properties and methods to interact with the elements, to change their style, to change their content and so on.

* You also have special properties on methods and text nodes but you simply don't work with text nodes as often as you will work with elements because typically you want to add a new HTML element, you want to remove one, you want to change the style of one, you want to edit it in any other way. 

* For text, you typically just want to change the text and then you typically just go to the elements that holds the text and change the child content which is the text of that element, which is why you don't work with text nodes as often.

*  So which exact properties and methods are available on every element, so on every rendered HTML tag depends on the kind of element though, for example on the input where the user can enter content, you have ways of reading that input, on other elements where users just can't edit the content, you don't have access to that of course and you have   different ways of selecting elements,

### Selecting Elements in the DOM

```html
<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>DOM Interaction</title>
    <script src="app.js" defer></script>
  </head>
  <body>
    <header><h1 id="main-title">Dive into the DOM!</h1></header>
    <ul>
      <li>Item 1</li>
      <li class="list-item">Item 2</li>
      <li class="list-item">Item 3</li>
    </ul>
    <input type="text" value="default text">
  </body>
</html>
```

```js
// app.js
const h1 = document.getElementById('main-title');

h1.textContent = 'Some new title!';
h1.style.color = 'white';
h1.style.backgroundColor = 'black';

const li = document.querySelector('li:last-of-type');
li.textContent = li.textContent + ' (Changed!)';

const body = document.body;

// const listItemElements = document.querySelectorAll('li');
const listItemElements = document.getElementsByTagName('li');

for (const listItemEl of listItemElements) {
  console.dir(listItemEl);
}
```

###  Node Query Methods

* Here's a summary of the various methods you got to reach out to DOM elements (note: you can only query for element nodes).
Besides the below query methods, you also got these special properties on the document object to select parts of the document:

* document.body => Selects the <body> element node.

* document.head => Selects the <head> element node.

* document.documentElement => Selects the <html> element node

#### QUERY METHODS

```js
document.querySelector(<CSS selector>);
```
* Takes any CSS selector (e.g. '#some-id', '.some-class' or 'div p.some-class') and returns the first (!) matching element in the DOM. Returns null if no matching element could be found. More information: https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelector

```js
document.getElementById(<ID>);
```
* Takes an ID (without #, just the id name) and returns the element that has this id. Since the same ID shouldn't occur more than once on your page, it'll always return exactly that one element. Returns null if no element with the specified ID could be found. More information: https://developer.mozilla.org/en-US/docs/Web/API/Document/getElementById

```js
document.querySelectorAll(<CSS selector>);
```
* Takes any CSS selector (e.g. '#some-id', '.some-class' or 'div p.some-class') and returns all matching elements in the DOM as a static (non-live) NodeList. Returns and empty NodeList if no matching element could be found. More information: https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelectorAll

```js
document.getElementsByClassName(<CSS CLASS>);
```
* Takes a CSS class g (e.g. 'some-class') and returns a live HTMLCollection of matched elements in your DOM. Returns an empty HTMLCollection if not matching elements were found. More information: https://developer.mozilla.org/en-US/docs/Web/API/Document/getElementsByClassName

```js
document.getElementsByTagName(<HTML TAG>);
```
* Takes an HTML tag (e.g. 'p') and returns a live HTMLCollection of matched elements in your DOM. Returns an empty HTMLCollection if not matching elements were found. More information: https://developer.mozilla.org/en-US/docs/Web/API/Element/getElementsByTagName There also is the getElementsByName() method which really isn't used commonly (https://developer.mozilla.org/en-US/docs/Web/API/Document/getElementsByName).

### Exploring and Changing DOM Properties

* Refer : evaluating-and-manipulating-elements.pdf

### Attributes vs Properties

* Refer : attributes-vs-properties.pdf

### Selecting Multiple Elements & Summary

```js
// const listItemElement = document.querySelectorAll('li');
const listItemElement = document.getElementByTagName('li');

for(const listItemEl of listItemElement){
    console.log(listItemEl);
}

```

* Minor difference which I also already mentioned earlier, getElementByTagName gives you a live list which reflects changes to the selected elements, querySelectorAll does not gives you a live list.


```js
// app.js
const h1 = document.getElementById('main-title');

h1.textContent = 'Some new title!';
h1.style.color = 'white';
h1.style.backgroundColor = 'black';

const li = document.querySelector('li:last-of-type');
li.textContent = li.textContent + ' (Changed!)';

const body = document.body;

// const listItemElements = document.querySelectorAll('li');
const listItemElements = document.getElementsByTagName('li');

for (const listItemEl of listItemElements) {
  console.dir(listItemEl);
}
```
* Where can you "find" the DOM?

* Don't forget that JavaScript is a "hosted language". The browser as host environment exposes this DOM API to your JS code automatically.

* What's a difference between document.querySelector('#someId') and document.getElementById('someId')?

* querySelector uses a CSS selector and can match ANY elements (depending on provided selector), getElementById looks only for ID

* What's the difference between querySelector() and querySelectorAll()?

* querySelector finds the first matching element querySelectorAll find all matching elements (and hence returns an array-like object)

### Traversing the DOM - Overview

* what does traversing the DOM mean? It means that once you selected one element, one node therefore, you might be interested in diving into all of its child nodes, for example to add it all list items in a list or anything like that, so rather than manually selecting every element you might be interested in with query selector or so on, you could take an element which you already did select and then move to its children or its siblings and so on based on that element, that's what's traversing the DOM means. 

* Now for that, I first of all want to clarify a couple of terms which I'll use - children, descendants, parent and ancestors because it's important to understand what these things mean.

* So let's have a look at what child or children means, what descendant or descendants mean, what parent or parents mean and what ancestor or ancestors mean.

* Refer : dom-traversal.pdf

### Traversing Child Nodes

```js
const ul = document.querySelector('ul');
ul.children[1] // this will return 2nd element of li
```
* ul children, this gives you a so-called HTML collection which is an array-like object in the end, so it's not a real array but it supports looping and so on,

* This also is a great place to understand the difference between child nodes and child element nodes

* Here I'm using children and this gives me access to all child element nodes, so only child nodes that are based on HTML tags, HTML elements, text nodes are excluded here.

* DOM tree, of that tree that is created here by the browser to reflect your HTML document and that there, even the whitespace which you add in your HTML file for readability reasons is treated as a text node because it is text in your HTML file, (Refer : dom4)

* So therefore, actually in our HTML code, here of course I also have a bunch of whitespace in front of the body and in front of the header and of course also in front of the list item here, I also have a line break between the unordered list and the list item.

* So here the difference we can actually see is that children gives me this collection of element nodes that are direct children of this unordered list, so no other descendants and also and no other types of nodes.

* If we use child nodes instead, now you see instead of this HTML collection array-like object, we now have a node list array-like object and if we output this, indeed we see there's way more content in there. There are a bunch of text nodes as you can now clearly tell and of course also our element nodes but also the text nodes because child nodes selects all child nodes

```js
const ul = document.querySelector('ul');
ul.childNodes[1] 
```
* whereas children only select child element nodes and child nodes therefore also includes text nodes and here this text for example, in front of this first list item, that is exactly this whitespace which you in the end have here.

* So that's children and child nodes which allows us to select child nodes, often you want to work with children because in most cases you're interested in getting access to the child element nodes but if you need access to all nodes, you can use child nodes

* let's say you select that the unordered list with query selector ul and there, you want to get access to the first or the last list item.

```js
document.querySelector('li: last-of-type');
```
* that will work but for one, this will cost a little bit of performance because query selector has to scan the entire page and then scan this quite complex CSS query and find the fitting elements and it also just might be redundant if you already got access to the unordered list any ways.

```js
const ul = document.querySelector('ul');
ul.firstchild
ul.firstElementchild
ul.lastchild
ul.lastElementchild
```
* If you then know you want to work with the first list item, you can use first child to get the first child node, which in this case is a text node or first element child to get the first element child node which would be the first list item here
```js
const ul = document.querySelector('ul');
ul.querySelector('li: last-of-type')
```
* So this might simply be quicker and cheaper to get access to than with a brand new query on the entire document.

* Of course as a compromise, you could also go for query selector li last of type, that's certainly better than scanning the entire document but it might still be faster to simply use last element child if you know that you want the last element child in the end.

### Using parentNode & parentElement

* now, let's actually go in the opposite direction. Let's say we already selected a list item, let's say the first one so that we can use a query method without any issues and now we want to reach out to the unordered list, so to the parent.

* Now needless to say, you can of course get access to the unordered list for example with query selector ul, right? But it's about understanding how you can traverse the document so that in a scenario where you have a more complex document, you are able to easily reach out to the parent for example. 

```js
const li = document.querySelector('li');
li.parentElement
li.parentNode
// both parentElement and parentNode are same..
```
* you can't have more than one parent element or parent node. So here, parent node gives us access to the nearest parents node, parent element to the nearest parent element node and in this case, this is actually the same and to be honest, in almost all cases it's the same, why?

* Because only element nodes can have child nodes and therefore of course if you're on a child node so to say and you want to know all about the parent, it's an element node, text nodes can't have child nodes, you can't nest other content into text nodes for example, they can only hold text, no other nested content.!!!!!!!!!

*  So you might wonder, why do we have parent node and parent element then?

* Well there is only one exception to that rule and that is 

```js
document.documentElement.parentElement // null
document.documentElement.parentNode // we get the entire document object as a parent node 
```
* this might be nice to know it is something which will probably never really matter in the code you write, in your projects because if you want to get access to the document, you can just select it with the document object. Traversing to it from some child node might just be a lot of work for something which is already available on this document object, right? So I don't really see a great use case for this, I just wanted to let you know why both exists, in reality as I said for all other elements, parent element and parent node always refers to the nearest parent element and that always is guaranteed to be an element node and therefore you can use either of the two depending on what you prefer.

* now let's say from that first list item, I want to get access to the body
we can directly get document.body but this is just for our understanding

```js
const li = document.querySelector('li');
li.parentElement
li.parentNode
// both parentElement and parentNode are same..
```
*  I want to show you that of course selecting the unordered list is easy with parent node or parent element but if we want to reach out to another ancestor and header for example wouldn't be an ancestor because it's not wrapped around the unordered list,

* so if you want to get access to another ancestor, parent node and parent element don't help us because with those, we only get access to the nearest direct parent, not to any other ancestor. To get access to another ancestor like body which is an ancestor, we can use the closest method 

* the closest method takes a CSS selector just like query selector and here for example, I can enter body and if I hit enter, you see now the body element is selected and just to prove that this wouldn't work with header which isn't an ancestor, if we try that, we get null.

```js
const li = document.querySelector('li');
li.closest('body'); // with this we will get body element
li.closest('header'); // null - because header is not ancestor of li
```
* So closest is a nice method for selecting any ancestor anywhere up in the element tree !!!

### Selecting Sibling Elements

* what if I actually want to select the header based on a selection of the unordered list or even of the list item?

```js
const ul = li.parentElement; // both refers same-element
const ul = document.querySelector('ul');  // both refers same-element
```

* So we have the unordered list selected and now I want to get access to the header here. Now the header is a sibling of the unordered list, it's on the same level as unordered list. It's not a parent or an ancestor and it's also not a child or a descendant, it's on the same level

* and for that we can use the previous sibling or element sibling properties which are made available by the browser.

```js
const ul = li.parentElement; 
ul.previousSibling // gives us the previous sibling node ie text node
ul.previousElementSibling // gives us the previousElementSibling ie Header
```

* Of course if I want to reach out to the header, I'm not interested in the nearest node, I'm not interested in this text node which is basically between the header and the unordered list, I want to get the nearest element node and I can do that with previousElementSibling

* this gives us the nearest previous sibling element node, not any node but the nearest element node which of course in this example here is the header

* If I want to get the sibling after the unordered list, so the input here well then of course I can use my unordered list here and then we have next sibling to get the next sibling node.

```js
const ul = li.parentElement; 
ul.nextSibling // gives us the next sibling node ie text node
ul.nextElementSibling // gives us the nextElementSibling ie input node in this example
```
* So this is how that works and how you can work with these different DOM traversal techniques, this is always useful if you already selected some element in your document and you know the next operation I want to do should be done to the next sibling or to the first child or something like this.

* Using these traversal properties can simply be quicker, that always starting a new query with query selector and so on and it's also not just faster for you as a developer, where you don't have to find a selection criteria, it's also faster for the browser because with query selector, if you use document query selector, it of course has to go over all elements in your document and find matching ones, that is still not super bad but it's certainly worse for performance than if you say ul next elements sibling. So if you have a possibility of reaching out to the element you want to select with this command, definitely do it because this is then a quick and cheap way of getting access to that sibling element in this case.

### DOM Traversal vs Query Methods

* One important node about all these DOM traversal techniques here by the way - it's super important to be aware of them and as I mentioned, for example if you want the first list item for unordered list and you have the unordered list selected anyways, it might be best to just use first element child to get access to the list item than to run a brand new query with query selector, both for performance reasons but also save some code.

* Here, it also makes sense because a list item is pretty guaranteed to always be a child of an unordered list and therefore even if you later go to your HTML code and you change things around, it's to be expected that this relation between unordered list and list item will be the same. So if you have code where you select the first element child on an unordered list, it will probably always be a list item. That might not be true for your entire page,

```js
const ul = document.body.firstElementChild.nextElementSibling  // document.body.firstElementChild -> header

const li = ul.firstElementChild;
```
* The problem with that is, for one this is a bit hard to read. If I'm reading your code, it's not immediately obvious for me that you are selecting an unordered list here. Sure, I can tell by this constant name but if you're picking a different name here, it gets hard and this code might be simply difficult to understand because I have to look at the HTML code to see what you're doing there, so that's one downside.

* The bigger problem is that if you wrap your ul with section element then it won't work

* of course, don't be shy to use the query methods, they're not bad or anything like that. Just because this could yield a better performance does not mean that the other approach yields horrible performance, you'll not even feel the difference.
Just try to be smart regarding what you use and that is also something that will come with experience, there also is no strict wrong or right, it's important to know both and in cases where the relation is always the same, DOM traversal properties like this one can be nice for general selection things, like generally selecting an unordered list

### Styling DOM Elements

* Refer : styling-dom-elements.pdf

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>DOM Interaction</title>
    <style>
      .red-bg {
        background-color: red;
        color: white;
      }

      .visible {
        display: block;
      }

      .invisible {
        display: none;
      }
    </style>
    <script src="app.js" defer></script>
  </head>
  <body>
    <header><h1 id="main-title">Dive into the DOM!</h1></header>
    <section class="red-bg visible">
      <ul>
        <li class="list-item">Item 1</li>
        <li class="list-item">Item 2</li>
        <li class="list-item">Item 3</li>
      </ul>
    </section>
    <button>Toggle Visibility</button>
    <input type="text" value="default text" />
  </body>
</html>

```
* app.js

```js
const ul = document.body.firstElementChild.nextElementSibling;
const firstLi = ul.firstElementChild;

console.log(firstLi);

const section = document.querySelector('section');
const button = document.querySelector('button');

// section.style.backgroundColor = 'blue';
section.className = 'red-bg';

button.addEventListener('click', () => {
  // if (section.className === 'red-bg visible') {
  //   section.className = 'red-bg invisible';
  // } else {
  //   section.className = 'red-bg visible';
  // }

  // section.classList.toggle('visible');
  section.classList.toggle('invisible');
});
```

### Creating Elements with JS - Overview

* Refer : dom5

### Adding Elements via HTML in Code

* So innerHTML is useful whenever you want to change everything, all the HTML content of an element, it's not so good if you only want to add something to the existing content, that's one thing to keep in mind. Therefore a better way of updating this, to keep the user input here

```js
const div = document. querySelector('div');

div.insertAdjacentHTML('afterend', '<div id="two">two</div>');
```
* Refer : https://developer.mozilla.org/en-US/docs/Web/API/Element/insertAdjacentHTML

* 'beforebegin': Before the element itself.
* 'afterbegin': Just inside the element, before its first child.
* 'beforeend': Just inside the element, after its last child.
* 'afterend': After the element itself.

```js
<!-- beforebegin -->
<p>
  <!-- afterbegin -->
  foo
  <!-- beforeend -->
</p>
<!-- afterend -->
```

* So this is one way of adding new content to an existing element with some HTML code which you write in Javascript and this is therefore a great way of manipulating this. Still whilst this might seem perfect, there are scenarios where you don't want to do it like this.

### Adding Elements via createElement()

* Refer : creating-and-inserting-elements.pdf

* Performance is not an issue here with this adjacent HTML method and also not that we would lose user input, so that's not the reason.

* Well a downside of this approach is that you tell the browser which element to render or which content to render and that can be any HTML content, doesn't have to be a single element, can be any as complex as you want HTML code

* The downside just is you have no direct access to the newly rendered content.
```js
const div = document. querySelector('div');

div.insertAdjacentHTML('afterend', '<div id="two">two</div>');

div.querySelector('p');
```

* Now the more complex the HTML code is you inserted here, the more complex is to query for the right things so that you get the objects with which you can work, where you can add event listeners, where you can change the properties and so on. So the missing direct access to the inserted elements can be a problem.

*  if you find yourself inserting content like this and then querying for it thereafter, you're basically running two steps to get access to a newly added element when there actually is only one step required and that's the case with create element, with this other approach of adding new elements.

```js
const list = document. querySelector('ul');

const newLi = document.createElement('li');

list.appendChild(newLi);

newLi.textContent ="Item 4";

```

* Typically of course, you would first finish up configuring this and set the text content and anything else and then append it instead of first appending it and then completing it because you only want to append it once you are done configuring it and you can set the text content and the style prop and so on even if you haven't added the item to the DOM yet because it's already there in Javascript memory, you can work with it.

### inserting DOM Elements

* We did insert it with append child, nothing wrong with that but there also is append. The difference is not only that append is shorter but that append also does not only take our new li which we created but that here you can also add a string, some text for example which is then inserted as a text node next to the other element nodes,so if you want to insert a text node you can conveniently do it like this.

```js
const list = document. querySelector('ul');
list.append("some string ");
```
* so if you want to insert a text node you can conveniently do it like this.

* Another difference by the way is that you can add multiple nodes here, simply separated by commas as multiple arguments to append to add multiple nodes at once in that place, that's also a difference to append child.

* Refer : https://developer.mozilla.org/en-US/docs/Web/API/ParentNode/append

```js
const list = document. querySelector('ul');

const newLi = document.createElement('li');

list.appendChild(newLi);

newLi.textContent ="Item 4";

list.prepend(newLi);// insert it as the first element,

// IE doesn't support append, prepend
```

* we want to add it before the currently last list item. Now we can get access to that with list and then last element child,

```js
const list = document. querySelector('ul');
const newLi = document.createElement('li');
ist.prepend(newLi); // added at the top
newLi.textContent ="Item 4";
list.lastElementChild.before(newLi); // Now Item 4 added before Item 3
```
* but the interesting thing we see here is that item 4 is now actually removed from the beginning of the list and basically moved into this place. If you have an element selected, either because you selected it in the DOM with query selector or because you created it with create element and that element is already part of the DOM, so it is already rendered, if you then insert it somewhere else, this is not copied or anything like that,

* instead the existing element is detached from the place where it was and moved to the new place and I guess this actually makes a lot of sense since objects are reference values as you learned and the DOM objects you're working here are nothing but normal objects in the end and therefore if we do something with it and we add it somewhere else, we always work with the same object, so of course it's detached from the existing place and moved to the new one, it's just something to be aware of. If you want a brand new one, you have to create a brand new one with document create element.

```js
const list = document. querySelector('ul');
const newLi = document.createElement('li');
list.prepend(newLi); // added at the top
newLi.textContent ="Item 4";
list.lastElementChild.after(newLi); // Now Item 3 added before Item 4

list.firstElementChild.replaceWith(newLi); // Replace one with new Li

```

```js
const list = document. querySelector('ul');
const secondLi = list.children[1];
const newLi = document.createElement('li');
newLi.textContent ="Item 4";
secondLi.insertAdjacentElement('afterend',newLi); // Now item4 inserted after item2 element

```
###  Cloning DOM Nodes

* So you learned that inserting an element more than once will move it and not copy it, if you would want to copy an element, well then you can do this with another method which is available on every DOM node object,

* It takes one optional argument and that's a boolean which can be true or false which by default is false, which simply determines whether a deep clone, so with all child and descendant elements should be done or not.

* If you pass false here, which is the default, then only the list item itself is cloned but no nested elements you might have in there.

* If you pass true here, then not only the direct child element but also all child elements of that element and all descendants in general will be part of the clone.

```js
const newLi = document.createElement('li');
newLi.textContent ="Item 4";
const newLi2 = newLi.cloneNode(true);
list.append(newLi, newLi2); // Item 4 will be added twice
```
### Live Node Lists vs Static Node Lists

* let me come back to that live node list versus non-live node list thing.

```js
const list1 = document.querySelector('ul');
const ListItems = document.querySelectorAll('li'); // So of course not just in this list1 but on the entire page,
```

```js
const ListItems2 = document.getElementsByTagName('li'); 
const newLi = document.createElement('li');
newLi.textContent ="Item 4";
list.append(newLi);
```

* We got our two list items constants, list items and list items 2 and on list items, you can already see this still is a node list with three items. So our most recent addition is not reflected here, that's what I mean with non-live array or non-live list.

* Now, that's not necessarily a disadvantage from a performance perspective, from a memory consumption perspective, this might even be better and I also want to highlight that of course the individual objects in there are still reference values, so an object in there is still of course a live reference to the DOM objects that are responsible for what we see on the page.

```js
ListItems[0].textContent = "Item 11"; // this will get reflected on the screen but still this is not live list ie this is non-live list
```
* Now if we have a look at list items 2 however which is this HTML collection I got with get elements by tag name, we can see this indeed is a live list which also includes our most recent addition. getElementsByTagName, getElementsByClassName always gives live list

* It could lead to a higher memory consumption if you're managing a lot of such collections which change all the time but again, that will also probably only matter in rare niche cases but for the most part, query selector all simply should be used because it is more flexible, supports richer queries and therefore often is a common choice if you want to query for multiple items.

### Removing Elements

```js
const list = document.querySelector('ul');
list.remove(); // otherthan IE
list.parentElement.removeChild(list);  // for IE
```
*  this is the safest way of removing an element, reaching out to its parent and then remove child and then passing the item you want to remove but again, remove also has pretty good support. If Internet Explorer support doesn't matter for you, then just remove on the item you want to remove

### Insertion & Removal Method Summary

* Refer : insertion-removal-summary.pdf

### Summary: Insert, Replace, Remove

* There are many ways of creating, inserting, replacing and removing DOM elements - here's a summary of the options you have. For browser support, check the provided links and also the "Browser Support" module you find later in the course.

#### Create & Insert
* You got two main options: Provide an HTML snippet (e.g. via innerHTML) to a valid HTML snippet and let the browser render it OR create a DOM object in JS code and append/ insert it manually. The latter approach has the advantage of giving you direct access to the DOM object (useful for setting its properties or adding event listeners). The downside is that you have to write more code.

#### Adding HTML Code:

```js
const root = document.getElementById('root-el'); // selects something like <div id="root-el">
root.innerHTML = `
    <div>
        <h2>Welcome!</h2>
        <p>This is all create & rendered automatically!</p>
    </div>
`;
```
* Important: Any existing content in root is  completely replaced when using innerHTML. If you want to append/ insert HTML code, you can use insertAdjacentHTML instead: https://developer.mozilla.org/en-US/docs/Web/API/Element/insertAdjacentHTML

```
const root = document.getElementById('root-el'); // selects something like <div id="root-el">
root.insertAdjacentHTML('afterbegin', `
    <div>
        <h2>Welcome!</h2>
        <p>This is all create & rendered automatically!</p>
    </div>
`);
```
#### Creating & Inserting DOM Objects Manually:
```js
const someParagraph = document.createElement('p'); // creates a "p" element (i.e. a <p> element)
const root = document.getElementById('root-el'); // selects something like <div id="root-el">
root.append(someParagraph);
```

* In this example, we create a paragraph and append it to root - append means that it's inserted at the end of root (i.e. inside of it but AFTER all other child nodes it holds).

#### Insertion Methods:

* append() => https://developer.mozilla.org/en-US/docs/Web/API/ParentNode/append

* Browser support is decent but for IE, appendChild() could be preferred => https://developer.mozilla.org/en-US/docs/Web/API/Node/appendChild

* prepend() => https://developer.mozilla.org/en-US/docs/Web/API/ParentNode/prepend

* Browser support is decent but for IE, insertBefore() could be preferred => https://developer.mozilla.org/en-US/docs/Web/API/Node/insertBefore

* before(), after() => https://developer.mozilla.org/en-US/docs/Web/API/ChildNode/before & https://developer.mozilla.org/en-US/docs/Web/API/ChildNode/after

* Browser support is okay but IE and Safari don't support it. Consider insertBefore() (https://developer.mozilla.org/en-US/docs/Web/API/Node/insertBefore) or insertAdjacentElement() (https://developer.mozilla.org/en-US/docs/Web/API/Element/insertAdjacentElement) as substitutes.

* Important (no matter how you insert elements): Whenever you insert elements, you MOVE the element to that new place if you already inserted it before. It's NOT copied (you can copy an element via someElement.cloneNode(true) though).

#### Replace
* You can replace elements in the DOM with two main methods:

* replaceWith() => https://developer.mozilla.org/en-US/docs/Web/API/ChildNode/replaceWith

* replaceChild() => https://developer.mozilla.org/en-US/docs/Web/API/Node/replaceChild

* replaceWith() is a bit easier to use and has decent browser support - with IE being the exception. To support that as well, consider using replaceChild().

#### Remove
* You can remove elements with three main methods:

* someElement.innerHTML = '' => Clears all HTML content of someElement and hence removes any objects rendered in there.

* someElement.remove() => Removes a single element (someElement) from the DOM (https://developer.mozilla.org/en-US/docs/Web/API/ChildNode/remove). Browser support is good, IE again doesn't like it though. Use removeChild (see below) instead.

* someElement.parentNode.removeChild(someElement) =>  Removes the provided child element (NOT the element on which you call it). Provides broad browser support but of course requires a bit more code (https://developer.mozilla.org/en-US/docs/Web/API/Node/removeChild).

* What about Text Nodes?
* You can easily create & insert text nodes in one go:
```js
someElement.textContent = 'Hi there!';
```
* This creates and inserts the text node with a content of 'Hi there!'. Want to append to existing text?

* Just use:
```js
someElement.textContent = someElement.textContent + 'More text!';
```

