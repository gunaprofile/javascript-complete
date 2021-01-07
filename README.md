# Javascript Complete

## Utilizing Browser Storage

### Browser Storage Options

* Refer : what-is-browser-storage.pdf

* So what is browser storage? Now as I mentioned, we have the browser and we have the server, these are the two pieces that typically interact when we're serving a web page, the web page runs in the browser but a) it's served by a server and b) most web applications also talk to a server, either behind the scenes through Javascript or by using the default browser behavior where for example you can submit a form and the request is issued to the server.

* Now the server then has a server-side database where data can be stored, the browser also has a couple of storage mechanisms. So this is the big picture

* but now let's dive into two sides here. For the server side, so for the data which we send to the server with a HttpRequest, we typically store the important data, the data which also needs to persist, the data we also want to store locked away from the users because for example if we have a list of all users with all their e-mail addresses, of course that has to be stored on one central server. 

* When we stored something in the browser, then we effectively store it on the machine of our user, so on your computer for example.

* Now of course the implication of that is that this a) is data we as the application developer can not always tap into, only when you're visiting our page we can basically use the Javascript code to interact with the browser storage, so the data is not always available to us and in addition it also can't be shared with other users.

* So the browser storage is really limited in scenarios where it makes sense, for example if you're building an online shop, of course you wouldn't store the products or the orders or anything like that in the browser because you need to share the products with the other users, with all users of your web application, therefore such data needs to be stored on your server.

* The orders of course belong to a single user but you as the owner of the shop also need access to them so that you can fulfill them and therefore that would also be stored on a server side database.

* Now one thing however you could consider storing in the browser would be the shopping cart, the current shopping cart of the user, it only matters to that user and you might not need to store it on your servers. If you're not running any analytics on that or something like that, you can store it just where your user is, so in the browser of the user,

* same is true for authentication data, like a session ID, such data could be stored in a browser,so temporary data or convenience data which might improve the user experience but which is not essential to your entire offering.

* Now when we talk about browser storage, we got three major storage types - we got local storage and session storage which are very related and I'll explain the difference in this module, we get cookies and we got IndexedDB

* Now let's learn about the differences between these different storage engines, between local storage cookies and IndexedDB.

* Local storage and session storage is a simple key-value store, so it's like a Javascript object which we save in a file, so it's basically just a couple of key-value pairs.

* You could use that to store for example session ID of a user, some analytics key which you need to send to your analytics servers, something like that so often you use local storage to manage user preferences or basic user data.

* You can tap into local storage with the help of Javascript and only Javascript, so only the Javascript code that runs in the browser is able to communicate with local storage.

* It's easy to use, quite versatile but of course it's bad for complex data because it's just a key-value store, so if you would be building a rich web application, like Google Sheets for example where you might need to store a lot of complex data in the browser as well, then this is not really a great option

* Now also a relatively simple storage is cookies. Now cookies are also key-value pairs in the end, though we can configure them in various ways, for example we can set a cookie to expire at some time in the future so that it automatically gets deleted basically, for local storage we can also delete data but we have to do that manually through Javascript, for cookies we could set it as an option on one of our entries.

* We also since it's still relatively simple use it to manage basic preferences, session IDs, stuff like that and we can also access and clear it with Javascript, just like local storage by the way because I didn't mention it there, the user also can clear all that data through the developer tools, through the browser settings where you can clear all local data, that is possible.

* Now the special thing about the cookie is that it's a bit more clunky to use than local storage, it's not having such a nice API for working with it.

* It's equally versatile though, maybe a bit more even because you can set expiry dates and and that's really different cookies also typically are sent to the server with outgoing HttpRequests.

* So cookies, unlike local storage and session storage can also be read by the server because they're attached to outgoing HttpRequests in the headers of these requests, so that can be an extra plus.Now just like local storage because they're simple key-value stores, they're not suited for more complex data

* and then we get IndexedDB. IndexedDB is the most sophisticated storage device amongst these three,

* it's a client side database in the end, built into the browser which you can use with a query language, you can run more or less complex queries against IndexedDB, you can therefore manage complex data in there because you can have different tables with records that are connected and so on and you can access it with Javascript, you can also clear it with Javascript and just like all browser side storages, the user can always clear and erase all that data with a click of a button in the preferences and therefore none of these storages should be a storage you rely on.

* You can use it to improve the user experience but you have to live with the fact that your user can always clear these storages. Now IndexedDB is also a bit clunky to use, it has a Javascript API but that is a bit annoying and it's great for complex non-critical data, it offers quite good performance so it's really good if you have complex data in your application.

* I will say though, I'm really talking about rich client side applications there, something like Google Sheets and so on, so where you basically build a desktop level application in the browser and you might need to store a lot of temporary or see my temporary data in some storage on the browser and you don't want to store that on a server for whatever reason because you want to make sure that your application works offline or anything like that, then this could be used, in a lot of applications you'll not really need IndexedDB.

* Refer : browser1

### localStorage & sessionStorage

* So I want to start with local storage and session storage and I'll also show the difference there, let's start with local storage.

* Local storage is a great, easy to use key-value storage where you can store basic data. Let's say you have some userId for the user of this web application, so the user using your site on this machine which you want to store in local storage so that you can attach it to every request you're sending to your server to identify that user.

* Now you should be aware by the way that all that data can be manipulated by the user, so you should never treat this as the single source of truth but instead just as a starting point which you then could validate with some other mechanisms.

```js
const storeBtn = document.getElementById('store-btn');
const retrBtn = document.getElementById('retrieve-btn');

const userId = 'u123';
const user = { // you can't store methods in local storage it will be lost
  name: 'Max',
  age: 30,
  hobbies: ['Sports', 'Cooking']
};

storeBtn.addEventListener('click', () => {
  sessionStorage.setItem('uid', userId);
  localStorage.setItem('user', JSON.stringify(user)); // first parameter is a key,has to be a string, second argument is a value 
});

retrBtn.addEventListener('click', () => {
  const extractedId = sessionStorage.getItem('uid');
  const extractedUser = JSON.parse(localStorage.getItem('user')); // localStorage getItem by key
  //it's a synchronous action so it's actually not asynchronous as you could think, you don't need a promise or a callback here,this synchronously access the storage and gives you back a data immediately.
  console.log(extractedUser);
  if (extractedId) {
    console.log('Got the id - ' + extractedId);
  } else {
    console.log('Could not find id.');
  }
});
```

* Local storage - This here is the data we just stored here, our key and the value for that key and as you saw, I just deleted the test data, I can also delete this here, simply by hitting the delete key or the backspace on Mac and that is something every user can do, also every user can add data here, so you should, as I said, never take this as a single source of truth because this can be manipulated easily by the users of your application,

* it should just be a starting point or hold data you need in your Javascript code to enhance the interface because of course as you see here, you can interact with that from inside Javascript.

* Now you can also store more complex data, yes it has to be a string but keep in mind what you learned about JSON in the HTTP module. If we had a user object here, which has let's say a name key and an age and maybe also some hobbies, where we have sports and cooking and we want to store that in local storage, we'll have a problem

* If you try to store a object in local storage it won't store the actual value it will just store [object object] thing was stored because what actually happens when we store some data here is that Javascript will call toString on whatever we pass in and for an object, it doesn't give us human readable or machine readable version of the object, it gives us this square bracket object object output but what we can do is we can convert this to JSON because JSON actually is a string,remember it's called JSON.stringify.

* Now JSON is this format which also uses curly braces and so on but actually, the entire JSON data and that's just something you have to know is wrapped into quotes you could say.JSON data is string data, it's a machine readable string where the content of the string is such an object with the structure and the rules

* So therefore we can stringify that data to store our user data in here and if we then want to get it back, the extracted user, we can of course call JSON parse on the result of local storage get item for the user key. So with JSON.stringify and JSON parse, you can even store more complex data in local storage if you want. 

* Be aware that any methods you added here will get lost because they're not encoded into JSON and also keep in mind that you don't want to store overly complex data in local storage because that's just not what it's built for.

* In addition you can't rely on the data to persist because users can delete it and browsers also can delete it if they're running out of disk space for example, these are things you have to keep in mind. 

*  And with that, to finish up the local storage part, let's see what session storage is about and for that, I'll actually store the userId with session storage and hence of course also extract it with the help of session storage.

* Now if I go back to the application and I reload, you'll see after reloads, the data also still is there in session storage but now copy the URL, open a new tab and close the existing one and reopen the page, so we basically closed the browser and reopened it. Now you see session storage is empty,If you check local storage though, the data still is there and that's the difference. 

* Session storage data lives as long as your page is open in the browser, so as long as you have it in an active tab even if you reload the page. Thereafter if you ever close that tab or close all tabs where your application is running or a close the entire browser, session storage is cleared.

* Local storage survives this, local storage is never cleared unless the user clears it manually or the browser clears it because it's running out of space or anything like that

### Getting Started with Cookies

* Now cookies are these special storage in a sense because they are attached to outgoing HttpRequests and that can be helpful depending on which kind of application you're building, of course your server needs to be prepared to do something with your cookies, otherwise them being added to outgoing requests adds no extra value,

* it won't be a problem but it also won't give you any special benefits, you need some server side code which also reads the cookies so that this actually is an advantage

* but let's have a look at what we can do with cookies here in the browser.

```js
const storeBtn = document.getElementById('store-btn');
const retrBtn = document.getElementById('retrieve-btn');

storeBtn.addEventListener('click', () => {
  const userId = 'u123';
  const user = {name: 'Max', age: 30};
  document.cookie = `uid=${userId}; max-age=360`; // set with max-age
  document.cookie = `user=${JSON.stringify(user)}`;
});

retrBtn.addEventListener('click', () => {
  console.log(document.cookie); // this will give access to all the cookies
  const cookieData = document.cookie.split(';');
  const data = cookieData.map(i => {
    return i.trim();
  });
  console.log(data[1].split('=')[1]); // user value
});
```

* So if you save this and you go back and click on store, let's check out the application tab and what you'll notice there under cookies is that you don't see anything there, now let me press store again multiple times, nothing shows up there. Now reason for that is unlike local and session storage, cookies really only are available if your web page is getting served with a real server.

* We already installed a server, let run "serve" in terminal

* what this will do is it will serve the index.html file in that folder automatically when you visit the address that it's being shown here, localhost:5000 in my case here.

* So now if we load localhost:5000 instead of the page through the file protocol, if we revisit the cookies here, I see one dummy cookie which is probably set by some other tool, some extension but if I now click store here and I reload, you see the userId show up there.

* So cookies can accessible Only in the localhost or deployed server you can't access with the index file in opened with browser

* If the cookie contains HttpOnly access (a special flag) the HTTP only flag and that means this cookie is only accessible by the server, not accessible from browser side Javascript, an extra security mechanism.

* Now one thing you probably see there is not just that this works though but that we don't have such a nice retrieval API as we had it with local storage. 

* Also keep in mind that just as with local storage, the user can always delete all cookies, either through the dev tools and the application tab here or simply with browser preferences where you can also clear cookies and you can clear cookies no matter if they have an expiration date or not

* If we would want to say that our userId should not expire with the end of the session, then we can add one of two flags, max age or expires.

* To add the max age flag, you add a semicolon after your key-value pair and then whitespace max-age and set this equal to a maximum age. Now the maximum age should be expressed in seconds,

* the alternative to that is that you set expires to a value and expires now takes a date. Now that date needs to have a certain format and attached you find a link to MDN where you see examples, where you also see that format for example, so that would be an alternative. If you know it's not one minute or ten days or whatever it is, it is a fixed date, then you could use expires.

* Refer : https://developer.mozilla.org/en-US/docs/Web/API/Document/cookie

### Getting Started with IndexedDB

* Refer : https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API/Using_IndexedDB

* Now IndexedDB is the most complex storage and therefore just as before, you find an attached resource that allows you to dive way deeper, here I just want to explore the basic functionality.

* Now the basic functionality is that you work with an in-browser database and for that you can leave that development server running, you can also use the file protocol for IndexedDB though, here I'll do it with the server.

* So the first step with IndexedDB is that you create a database or open a connection to an existing one. For that, we can call IndexedDB 


```js
indexedDB.open('StorageDummy', 1); // Now to open, you pass the name of your database, the name is totally up to you
// Second argument can be passed, can be the version of the database, you can use that to keep track of different versions, update a database, change it and so on,
```
* Now when this runs for the first time and the database doesn't exist yet, it will create it otherwise it will just open a connection,

* Now this is not promise based or anything like that unfortunately, so we can't call then and get the database connection object as an argument, that's unfortunately not how it works.

* Instead this open method here returns a so-called request, a db request as I'll call it.

```js
const dbRequest = indexedDB.open('StorageDummy', 1);
```

* Now on db request, you can add two event handlers, two event listeners, either with add event listener or for best cross browser support, with onSuccess which should point at a function and then also additionally onError which should point at a function.

```js
const storeBtn = document.getElementById('store-btn');
const retrBtn = document.getElementById('retrieve-btn');

let db;

const dbRequest = indexedDB.open('StorageDummy', 1);

dbRequest.onsuccess = function(event) {
  db = event.target.result;
};

dbRequest.onerror = function(event) {
  console.log('ERROR!');
};
```
* Now in both functions, you get access to an event object and now you can interact with that.

* onSuccess however, I want to continue. There, we get access to the database to which we connected or which was just created through that event object, event.target.result is a field you can access here, a property you can access and that will hold access to the database that was created.

* Now we can configure that database, remember this fires when this open request succeeded so now we can configure that once this happened. Now I mentioned that IndexedDB would work with tables and records like databases you know from servers,now actually the terminology is a bit different, it's object stores which you work with, a bit like tables, you can have multiple object stores and each object store can store multiple objects I guess but it's a bit like tables and records in the end.

* Now with the database you get access to, you can use it to create an object store by calling create object store

```js
const storeBtn = document.getElementById('store-btn');
const retrBtn = document.getElementById('retrieve-btn');

let db;

const dbRequest = indexedDB.open('StorageDummy', 1);

dbRequest.onsuccess = function(event) {

  db = event.target.result;
  const objStore = db.createObjectStore('products', { keyPath: 'id' }); // Now this is a function that takes a couple of parameters,
  // first one is the name of the object store,
  //  a key path property, where you set up that single key which every item has by which it can be identified
  // let's say for a product, that's an ID field, it's up to you, it just has to be a field which later exists once you start adding data.
};

dbRequest.onerror = function(event) {
  console.log('ERROR!');
};
```
* Let's say we're building an app where we want to store some products here on the client side, though keep in mind what I said earlier, business critical or security relevant information as well as data that has to be shared with other clients should not be stored here, so it's really just data which can get lost and which we just need to enhance the user experience,

* so let's say we're storing a couple of products. Now. every object store needs one key, one property that exists on every stored object by which this object can be identified.

* We can use the object store object for that and there, we'll have a transaction property on which we have an onComplete listener. 

* Now onComplete will trigger once the object store creation finished in the end, so it's now in here where we can interact with the database and this object store

* Now to store new data, we reach out to the database because you could store it in any object store and there, you also have a transaction method though now which you execute like such, so you execute a transaction method, which takes two arguments.

* The first argument is the name of your object store, so the name we set up here, in my case products

* So I'll point at products here, second argument is the mode under which you want to access this store and that could be read or like in my case, read write, I want to be able to write as well.


```js
const storeBtn = document.getElementById('store-btn');
const retrBtn = document.getElementById('retrieve-btn');

let db;

const dbRequest = indexedDB.open('StorageDummy', 1);

dbRequest.onsuccess = function(event) {

  db = event.target.result;
  const objStore = db.createObjectStore('products', { keyPath: 'id' }); 

  objStore.transaction.oncomplete = function(event) {
    const productsStore = db
      .transaction('products', 'readwrite') // Name of your store, mode
      .objectStore('products'); // Now this should open up a connection to that object store, on the resulting object,
      // you can now call object store as a method and pass in that name again.
    productsStore.add({
      id: 'p1',
      title: 'A First Product',
      price: 12.99,
      tags: ['Expensive', 'Luxury']
    });
  };

};

dbRequest.onerror = function(event) {
  console.log('ERROR!');
};
```
* You need to call that object store method because you could pass in multiple object store names to the transaction and then this would allow you to select a single object store and this will now give you direct access to that object store we're trying to establish a connection to.

* You can store that object store access here in a constant, products store for example

* Now let's save that and let's reload this page and we get an error. Now this error tells us that the database is not running a version change transaction.

* Now the thing is we defined this onSuccess callback here, this onSuccess function which would fire when the database is successfully created.

* It turns out that we actually interact with the database, you need a different handler which is onUpgradeNeeded,

```js
const storeBtn = document.getElementById('store-btn');
const retrBtn = document.getElementById('retrieve-btn');

let db;

const dbRequest = indexedDB.open('StorageDummy', 1);

dbRequest.onsuccess = function(event) {
  db = event.target.result;
};

dbRequest.onupgradeneeded = function(event) {
  db = event.target.result;

  const objStore = db.createObjectStore('products', { keyPath: 'id' });

  objStore.transaction.oncomplete = function(event) {
    const productsStore = db
      .transaction('products', 'readwrite')
      .objectStore('products');
    productsStore.add({
      id: 'p1',
      title: 'A First Product',
      price: 12.99,
      tags: ['Expensive', 'Luxury']
    });
  };
};

dbRequest.onerror = function(event) {
  console.log('ERROR!');
};

storeBtn.addEventListener('click', () => {
  
});

retrBtn.addEventListener('click', () => {

});

```
* so if we save this now with this changed and we reload, this error goes away. Let's now visit the application tab and there under IndexedDB, you should find that storage dummy database.

* if we expand this, we can see our key but we also see the value which was stored for that key which is this Javascript object and therefore this is more structured data stored in there without the need to encode it with JSON or anything like that

* and of course you can store multiple such objects with ease here, all identified through their key.

* So that was a lot of work and it already shows that this is more complex to work with but also more powerful

### Working with IndexedDB

```js
const storeBtn = document.getElementById('store-btn');
const retrBtn = document.getElementById('retrieve-btn');

let db;

const dbRequest = indexedDB.open('StorageDummy', 1);

dbRequest.onsuccess = function(event) {
  db = event.target.result;
};

dbRequest.onupgradeneeded = function(event) {
  db = event.target.result;

  const objStore = db.createObjectStore('products', { keyPath: 'id' });

  objStore.transaction.oncomplete = function(event) {
    const productsStore = db
      .transaction('products', 'readwrite')
      .objectStore('products');
    productsStore.add({
      id: 'p1',
      title: 'A First Product',
      price: 12.99,
      tags: ['Expensive', 'Luxury']
    });
  };
};

dbRequest.onerror = function(event) {
  console.log('ERROR!');
};

storeBtn.addEventListener('click', () => {
  if (!db) {
    return;
  }
  const productsStore = db
    .transaction('products', 'readwrite')
    .objectStore('products');
  productsStore.add({
    id: 'p2',
    title: 'A Second Product',
    price: 122.99,
    tags: ['Expensive', 'Luxury']
  });
});

retrBtn.addEventListener('click', () => {
  const productsStore = db
    .transaction('products', 'readwrite')
    .objectStore('products');
  const request = productsStore.get('p2');

  request.onsuccess = function() {
    console.log(request.result);
  }
});

```