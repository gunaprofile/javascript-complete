# Javascript Complete

## Working with Http Requests

### Why and What ?

* Refer : why-what.pdf

### How The Web Works

* Important: For this module, you should have a basic understanding of how the web works.

* This article can be helpful to refresh your knowledge: https://academind.com/learn/web-dev/how-the-web-works/ https://www.youtube.com/watch?v=hJHvdBlSxug&feature=youtu.be

###  More Background about Http

* Refer : http-overview.pdf

* let's have a closer look at this HttpRequest thing. How does this actually work?

* Well we have our script, our client side, so the code that runs in the browser and we have our server, the backend, the server side.

* The backend, so our server, could run on a totally different domain, a totally different server than our frontend. So we might be serving the HTML page and the Javascript which is responsible for what the user sees on mypage.com

* whereas the server side logic might be hosted on a totally different server, let's say on mybackend.com.

* Still, these two pieces or these two ends can communicate and the job on the client side is to get the user input, validate it and then to send it off to the server because the job on the server side is to store and retrieve it in a database typically and the database would run on yet another server, it's important to understand this point that you don't directly connect your client side Javascript code to a database for security reasons because you would expose your database credentials in the client side code which you don't want to do but instead you talk to a web server and that web server might then talk to a database server.

* Of course using a database is totally optional but of course often the server side will use one to persistently store data and then get it from the database.

* Now the communication between the client side and the server happens with the help of HttpRequests and responses and these requests and responses have to be configured and set up in a certain way, following a certain structure.

* When you're sending a request to the server, it needs to know the address, so it needs to know the URL made up of the domain and the path, the path is the part after the domain, so mypage.com/posts, mypage.com would be the domain, /posts would be the path.

* In addition, each HttpRequest has a HTTP method that is assigned to it.

* Now you have a couple of available HTTP methods, you have for example get, post, patch, put and delete.

* These methods describe what you want to do, though I will say it's totally up to the server to decide for which method URL combination the server wants to do what, so with the method you don't tell the server what to do, the server just exposes different endpoints and for example might support a post request to /posts whereas it maybe might not support a patch request to /posts but again, it's up to the people writing the server side code to decide which HTTP method URL combinations are supported.

* Typically of course you go for combinations that make sense logically because there the logic is that get requests typically are there to get data, post requests are there to create data on the server or create data on the server which might be stored in a database there, patch and put are there to update data, patch in the sense of updating the existing data, put in the sense of overriding it and delete as you can guess is there to delete data, that's what you commonly use but again, it's the server side that decides which HTTP method URL combinations are supported, on the client side, you then can send requests to these combinations. 

* If you use a combination that is not supported by the server side, you'll simply get a HTTP error as a response.

* Now other parts of a HttpRequest are potential headers, that is extra metadata which can be attached to HttpRequests and some requests, for example a post request, also hold a request body or extra data which is attached to a request. As you can imagine, if we're getting a list of posts, there is no data we could attach, we want a list of posts and we're done.

* If you want to add a post, if you want to create a brand new post, that of course is different, then you want to attach the data that is required for the post creation, like the title and the content to the request you are sending, so that's the idea here.

* Now that data then can be sent in different formats and again, it's the server that tells you which formats it expects or supports.

* The most common format is JSON data, which I'll dive into in more details in a second but we also have FormData which is supported in Javascript and which is fairly popular and you could also send binary files or other formats but again, it's up to the server to decide which data format is supported for which HTTP method URL combination.

* Now that is all the theory, if that is all brand new to you, attached you find a couple of resources that allow you to dig deeper into how the web works, into HttpRequests and HTTP methods.

### Getting Started with Http

* now the tricky thing is we need a server, the browser side Javascript is not enough.

* Now we could write our own server with the help of Javascript and Node.js, Node.js is a Javascript runtime outside of the browser which basically enables you to execute Javascript anywhere and it's a popular solution for writing server side code.

*  For the moment, in order to focus just on the browser side, we'll use a dummy API, a dummy web service we can talk to and that's this page, Refer : https://jsonplaceholder.typicode.com/

* This basically is a page that gives us a dummy web service, a dummy API we can send different requests to, any data we send to it of course is not permanently stored there, it's just a dummy API which fakes to store the data but it's great for a getting started with all of that.

### Sending a GET Request

```js
const xhr = new XMLHttpRequest(); // this object will allow you to send HttpRequests, it is built into the browser and all browsers support this object,

xhr.open('GET', 'https://jsonplaceholder.typicode.com/posts');

xhr.send();
```

### JSON Data & Parsing Data

```js
const listElement = document.querySelector('.posts');
const postTemplate = document.getElementById('single-post');

const xhr = new XMLHttpRequest();

xhr.open('GET', 'https://jsonplaceholder.typicode.com/posts');

xhr.responseType = 'json';

xhr.onload = function() {
  // const listOfPosts = JSON.parse(xhr.response);
  const listOfPosts = xhr.response;
  for (const post of listOfPosts) {
    const postEl = document.importNode(postTemplate.content, true);
    postEl.querySelector('h2').textContent = post.title.toUpperCase();
    postEl.querySelector('p').textContent = post.body;
    listElement.append(postEl);
  }
};

xhr.send();
```

### JSON Data Deep Dive

* Typically, data is transferred as "JSON" data between your client-side code and your backend ("the server"). JSON stands for JavaScript Object Notation and it looks like this:

```js
{
    "name": "Max",
    "age": 30,
    "hobbies": [
        { "id": "h1", "title": "Sports" },
        { "id": "h2", "title": "Cooking" }
    ],
    "isInstructor": true
}
```

* JSON data supports objects ({}), arrays ([]), strings (MUST use double-quotes!), numbers (NO quotes) and booleans (also NO quotes).

* All object keys (e.g. "name") HAVE to be wrapped by double quotes. No quotes or single quotes are NOT ALLOWED!

* Actually, the whole JSON "object" is wrapped in quotes itself because JSON data in the end is just a string that contains data in the format shown above.

* You can test it yourself - take the following non-JSON JavaScript object and apply JSON.stringify() on it. This will convert it to JSON data. If you do that in the dev tools console, you'll see that you in the end get a string which contains data formatted as shown above.

```js
const person = { // this is NOT JSON - it's a normal ("raw") JavaScript object!
    name: 'Max',
    age: 30,
    hobbies: [
        { id: 'h1', title: 'Sports' },
        { id: 'h2', title: 'Cooking' }
    ],
    isInstructor: true
};
 
const jsonData = JSON.stringify(person); // convert raw JS data to JSON data string
console.log(jsonData); // a string with machine-readable JSON data in it
console.log(typeof jsonData); // string
```
* We use JSON data because it's easy to parse for machines - and as an extra benefit it's also quite readable to us humans.

* If you receive some JSON data and you want to convert it back into normal JS data, you can use JSON.parse():

```js
const parsedData = JSON.parse(jsonData); // yields a "raw" JS object/ array etc.
```
* Also nice to know: You're NOT LIMITED to objects when converting data to JSON. You can also convert numbers, arrays, booleans or just strings - all data types JSON supports:

```js
const jsonNumber = JSON.stringify(2); // "2"
const jsonText = JSON.stringify('Hi there! I use single quotes in raw JS'); // ""Hi there! ...""
const jsonArray = JSON.stringify([1, 2, 3]); // "[1,2,3]"
const jsonBoolean = JSON.stringify(true); // "true"
```
### Promisifying Http Requests (with XMLHttpRequest)

```js
function sendHttpRequest(method, url, data) {
  const promise = new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest();

    xhr.open(method, url);

    xhr.responseType = 'json';

    xhr.onload = function() {
      resolve(xhr.response);
      // const listOfPosts = JSON.parse(xhr.response);
    };

    xhr.send(JSON.stringify(data));
  });

  return promise;
}

async function fetchPosts() {
  const responseData = await sendHttpRequest(
    'GET',
    'https://jsonplaceholder.typicode.com/posts'
  );
  const listOfPosts = responseData;
  for (const post of listOfPosts) {
    const postEl = document.importNode(postTemplate.content, true);
    postEl.querySelector('h2').textContent = post.title.toUpperCase();
    postEl.querySelector('p').textContent = post.body;
    listElement.append(postEl);
  }
}

fetchPosts()
```

###  Sending Data with a POST Request

```js
async function createPost(title, content) {
  const userId = Math.random();
  const post = {
    title: title,
    body: content,
    userId: userId
  };

  sendHttpRequest('POST', 'https://jsonplaceholder.typicode.com/posts', post);
}
createPost('DUMMY', 'A dummy post!');
```

###  Triggering Requests via the UI

```js
async function createPost(title, content) {
  const userId = Math.random();
  const post = {
    title: title,
    body: content,
    userId: userId
  };

  sendHttpRequest('POST', 'https://jsonplaceholder.typicode.com/posts', post);
}

fetchButton.addEventListener('click', fetchPosts);
form.addEventListener('submit', event => {
  event.preventDefault();
  const enteredTitle = event.currentTarget.querySelector('#title').value;
  const enteredContent = event.currentTarget.querySelector('#content').value;

  createPost(enteredTitle, enteredContent);
});
```

###  Sending a DELETE Request

```js
postList.addEventListener('click', event => {
  if (event.target.tagName === 'BUTTON') {
    const postId = event.target.closest('li').id;
    sendHttpRequest('DELETE', `https://jsonplaceholder.typicode.com/posts/${postId}`);
  }
});
```
###  Handling Errors

```js
async function fetchPosts() {
  try {
    const responseData = await sendHttpRequest(
      'GET',
      'https://jsonplaceholder.typicode.com/pos'
    );
    const listOfPosts = responseData;
    for (const post of listOfPosts) {
      const postEl = document.importNode(postTemplate.content, true);
      postEl.querySelector('h2').textContent = post.title.toUpperCase();
      postEl.querySelector('p').textContent = post.body;
      postEl.querySelector('li').id = post.id;
      listElement.append(postEl);
    }
  } catch (error) {
    alert(error.message);
  }
}
```
### Using the fetch() API

* So we got started with HttpRequests, with this XMLHttpRequest object.

* By the way you might be wondering where the name is coming from, in the parsed, actually quite some time ago, this was basically introduced so we could fetch XML data behind the scenes. Nowadays of course it's more common to use JSON data, the name however still is the same.

* Now there is more you can do with it, more you can configure and you can dive deeper into XMLHttpRequest as you can actually dive deeper into almost all things you can do work with in Javascript.

* Refer : https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/Using_XMLHttpRequest

*  now however I want to have a look at a more modern alternative which was added to browsers a few years ago and which actually is not supported in all browsers, especially older browsers, Internet Explorer don't support that new API which you can use and that new API I'm talking about is the fetch API.

```js
function sendHttpRequest(method, url, data) {
  return fetch(url).then(response => { // default method is GET 
    return response.json(); // we have convert response to json in fetch API
  });
}
```
* It was invented to be more modern, to be less clunky with these strange event handlers there which is really not modern Javascript at all, that's why this new API was introduced and that new API is commonly called the fetch API.

* Now it is called such because it all works around one key function, the fetch function and this is built into browsers, it's exposed in Javascript just like that, you don't have to call it as a method on any object or anything like that, it's a globally available function in modern Javascript,

* Now fetch by default is promise based, so that's the first native promise API we see in this course. We don't have to promisify ourselves, it uses promises on its own.

* one difference is unlike our XMLHttpRequest approach here, fetch does not give us the parsed response as we had it available here with xhr.response but instead, it gives us a streamed response, which means basically that the response object we get here does not hold the body, the response body in a way that would be ready to be used, instead what you would have to do and we can do that here inside of the sendHttpRequest function, instead here, you get your response object and there you have access to a response JSON method and you can return the result of this because that actually yields a new promise as you see.

* .json() will parse the body of the response and transform it from JSON to Javascript objects and arrays but it actually does not just parse that, so it's not just a replacement for JSON.parse, it also turns the streamed response body which you have in the response object into a snapshot so to say with which you can work.

* So long story short, you need to call response.JSON to convert your response body, the streamed unparsed response body into a snapshot parsed body.

* Besides response JSON, you also have response text for example to return plain text, so that will then just do the conversion from streamed to snapshot or response blob if you would be downloading a file, that would give you access to the file.

* Here however, we got JSON data so we need to return that.

### POSTing Data with the fetch() API

* In order to be able to create a post, we need to make sure that this function can succeed and for that, we need to make sure that we use the HTTP method in the fetch API and also the data, the body that should be appended to the outgoing request.

* Right now, we always just send a fetch request to a URL and therefore we always send a get request. Now just as with XMLHttpRequest, we can configure the outgoing request, however unlike XMLHttpRequest, you don't pass the request method here directly as an argument to the fetch function here,

* instead you can pass a second parameter here, a second argument which should be an object where you can configure the request and there you've got a couple of keys you can set as you see, a lot of them are pretty niche and rarely need it but method is one of the more important ones.

```js
function sendHttpRequest(method, url, data) {
  return fetch(url, {
    method: method, // default method is GET
    body: JSON.stringify(data) // convert data to JSON
  }).then(response => {
    return response.json();
  });
}
```

###  Adding Request Headers

* Headers can be important. Now thus far we haven't really needed them but for some APIs, you need to describe which type of data you sending for example or other APIs might need some extra authentication data and headers are basically metadata you can attach to outgoing requests.

*  If you inspect requests, you see there are some default headers added by the browser and the response also has headers set by the server.

* Now we can add more headers to the outgoing request and we do that here on fetch by adding a new key to that configuration object which is the headers key.

```js
function sendHttpRequest(method, url, data) {
  return fetch(url, {
    method: method,
    body: JSON.stringify(data),
    headers: {
      'Content-Type': 'application/json' // this tells the server hey my request has JSON data.
      // You can of course add more than one header by adding more than one key-value pair here.
    }
  }).then(response => {
    return response.json();
  });
}
```
* Similar like XMLHttp request we could add headers like below

```js
const xhr = new XMLHttpRequest();
xhr.setRequestHeader('Content-Type', 'application/json'); 
// xhr.setRequestHeader('Content-Type', 'application/json'); // you can add multiple headers by calling this method multiple times
```

### fetch() & Error Handling

```js
function sendHttpRequest(method, url, data) {
  return fetch(url, {
    method: method,
    body: JSON.stringify(data),
    headers: {
      'Content-Type': 'application/json'
    }
  })
    .then(response => {
      if (response.status >= 200 && response.status < 300) {
        return response.json();
      } else {
        return response.json().then(errData => {
          console.log(errData);
          throw new Error('Something went wrong - server-side.');
        });
      }
    })
    .catch(error => {
      console.log(error);
      throw new Error('Something went wrong!');
    });
}
```
### Working with FormData

* Now besides different ways of sending HttpRequests, we also can send different data, in theory at least, it always depends on the server and the URL you're sending this to, which kind of data the server accepts, for example our dummy server here really only works with JSON data. When we for example send the data to fake create a post, then we have to add JSON data there, it does not accept any other data but of course you might be working with different APIs in your project and there, different data might be accepted as well.

* Now you can send files, you can send binary data and add this as a body here, so you could add a pointer at a file which a user picked in a file picker for example or another very popular data format, you could add FormData, Now let's see what FormData is

```js
const form = document.querySelector('#new-post form');
async function createPost(title, content) {
  const userId = Math.random();

// That's a constructor function built into Javascript or built into the browser, provided in Javascript.
  const fd = new FormData(form); // form here from form dom
//   fd.append('title', title);
//   fd.append('body', content);

// Now FormData builds a new object to which you can add key-value pairs, so a bit like a Javascript object like this but internally 

// or when it's sent over the wire, it will be sent in a different way than JSON data, it's not JSON format, it has a different format as you will see.

  fd.append('userId', userId);

  sendHttpRequest('POST', 'https://jsonplaceholder.typicode.com/posts', fd);
}


function sendHttpRequest(method, url, data) {
  return fetch(url, {
    method: method,
    // body: JSON.stringify(data),
    body: data,
    // headers: {
    //   'Content-Type': 'application/json'  // we will not send json in formData,
    // }
  })
    .then(response => {
      if (response.status >= 200 && response.status < 300) {
        return response.json();
      } else {
        return response.json().then(errData => {
          console.log(errData);
          throw new Error('Something went wrong - server-side.');
        });
      }
    })
    .catch(error => {
      console.log(error);
      throw new Error('Something went wrong!');
    });
}
```
* FormData and that's a special type of data which is added to the outgoing request.

* If we have a look at the other headers, we see that there content type was automatically set to this value which was automatically derived by the browser in the end.

* Now the advantage of FormData is that for one, you might find it more structured to build your data like this, b) a second great thing about it is that you can easily add files, if you append some file, you can have any identifier you want here, you could point at a file here, for example picked in a file input and then as a third argument, even provide a file name like photo.png which the server can use. So you can easily send a mixture of text, content text, key-value pairs and files to the server which can be really convenient depending on what you build 

* and a third advantage is you can also use this to automatically parse a form and that's actually what I also want to show you here.

* Now for it to succeed though, you need to make sure that your inputs have a name attribute

```html
<form>
    <div class="form-control">
        <label for="title">Title</label>
        <input type="text" id="title" name="title" />
    </div>
    <div class="form-control">
        <label for="content">Content</label>
        <textarea rows="3" id="content" name="body"></textarea>
    </div>
    <button type="submit">ADD</button>
</form>
```
* The name attribute is important otherwise FormData is not able to identify these inputs and get the data out of there and store it correctly in the FormData.

* Now you can then still append extra data which might not be included in the form, like the userId here and with all of that