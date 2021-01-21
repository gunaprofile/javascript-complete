# Javascript Complete

## Security

### Intro

* In this module, I want to highlight what you should watch out for when writing Javascript code and some common pitfalls or some important things you need to keep in mind to ensure that your code is secure and you're not exposing security relevant data or anything like that in your scripts and so on. 

### Security Hole Overview & Exposing Data in your Code

* Refer : what-could-go-wrong.pdf

* So any data which could be abused by other people should not go in there, should not go into your client side Javascript code.

* Now in this project specifically, you might be wondering about things like our Google API key, isn't that for example a security relevant detail?

* Well it could be but in Google's case or in the Google API key case here, you can actually restrict the usage of that key in your Google developer console, there you could restrict that only certain IP addresses can use that key so that only the IP address where you are hosting your application can really send requests with that help of that key and if anyone steals that key and tries to use it, Google will block requests by that person's IP address.

* So this is some security mechanism which a lot of APIs offer if you use their API keys and therefore this is not a security relevant detail. 

* DB connection related details are sent from the backend server because backend server files won't exposed to the client

* So this code here is secure, it runs on a server and unless users can access our server, which of course requires them to effectively hack our server, they can't read that code, so this code is secure but if this code here were in our client side Javascript code, so in the code which is loaded by the browser and which the user can see, then users could see that data and abuse it.

### Cross-Site Scripting Attacks (XSS)

* Now let's move on to the next attack which is the most dangerous attack or the most dangerous attack pattern you can expose in your client side Javascript code and I'm specifically talking about the client side, so about the side which runs in the browser because the server side by default is way more secure and when we talk about server side attack patterns, they are not specific to Javascript, whereas cross site scripting and exposing security details is. So for the server side, there is a different range of potential attacks, one of them being CSRF attacks, others being SQL injection for example if you use a SQL database and I'm not talking about them here because they're not primarily related to Javascript.

* So back to the Javascript specific ones and there, to cross site scripting attacks, also shortened or abbreviated with XSS.

* This is an attack pattern where malicious Javascript code gets injected into your application and is executed there.

* this is really a dangerous attack pattern, you have to imagine if people are able to add code to your application which all users of your application therefore execute, they are effectively executing code on the behalf of these users and at the same time on the behalf of your application on the machines of those users 

* your code can do anything the code you write could do as well, so your code could send requests to certain domains which might allow access by your application with the right CORS headers, the code might read the local browser storage of the user and send that to some server of the malicious user who injected the code and much more. If you're able to inject Javascript code into another page so that it runs for every visitor of that page, you're able to gather a lot of data about these visitors, potential security relevant data included and you're also able to fake a lot of things and send a lot of requests, maybe to your own malicious pages on the behalf of the application and only the behalf of the users visiting that application where you injected your code into so that's really dangerous. 

* You've got full behind the scenes control if you inject code into another page. An example for that would be unchecked user generated content and I want to show you that example so that you understand what the problem is

```js
class LoadedPlace {
  constructor(coordinates, address) {
    new Map(coordinates);
    const headerTitleEl = document.querySelector('header h1');
    headerTitleEl.innerHTML = address; // here we are using innerHTML
  }
}
```

* Instead of address if i try to innerHTML a script tag with alert ... which is injected through URL 

* now what you see is now this is injected here, now we don't see a title here,instead we see the script was indeed added here because I'm rendering this as innerHTML.because of this exact scenario that you try or that someone tries to abuse a security hole in your application and inject Javascript code which executes on your behalf, also keep in mind that normal visitors off the page of course would probably not see that.

```html
<img src="" onError="alert('Mallicus injected alert')";>
```

* Now still, this doesn't really seem to hurt us, it's certainly unexpected but since the browser doesn't execute that script, we're fine, right? Well turns out there's more than one way of executing script code, besides these script tags which indeed are prevented by the browser to execute, what if we added an image here with a source that does not exist and then add an onError handler where we then instead execute our Javascript code, this is a valid HTML element.

* Now the source is not found, therefore onError should trigger and we can add this kind of event listener to our elements, we as a developer don't want to do that because it's not really a good way of writing code but as an attacker it's great, we can add Javascript code that executes right on a HTML element.

* and of course an alert is not that dangerous, it's just annoying and unexpected but here we could be doing other stuff too, we could be doing anything which might be security relevant.

* I could write a script in here and yes, we can write a longer script in here simply by separating our lines of semicolons, where I access the local storage, read data from it or read data from the local cookies and send that to my own malicious server for example. Visitors of the page will not see anything there, they don't get an alert, they don't get an error,

* and that is therefore really dangerous. Here, this is a security hole we have in this application, we have this hole because here, we're outputting innerHTML. Now how can we fix that?

* One solution to fix this XSS security hole we have here is to use text continents instead of innerHTML.

```js
class LoadedPlace {
  constructor(coordinates, address) {
    new Map(coordinates);
    const headerTitleEl = document.querySelector('header h1');
    headerTitleEl.textContent = address; // here we are using textContent
  }
}
```
* If we do that and we visit this, you'll see now the code which previously was executed and rendered as HTML is now output as text and therefore if we had any Javascript code in here, like for example our alert again where I say hi would not execute. So now we automatically are not vulnerable to cross site scripting attack and if you never use innerHTML anywhere in your code, you of course rule out a lot of potential attack patterns but with that alone you're not totally safe yet.

* For one, sometimes you might need innerHTML, so then you need a way around potential malicious code and in addition, it's not just innerHTML. If you're for some reason setting the onError handler of some HTML element dynamically to some user generated content in some way, Maybe you just want to have a user generated text which is output in here, well still then whenever you inject any user generated code anywhere into the DOM, this could be a potential issue so you always want to make sure you either inject it as only text with text content or if you set some other attribute of an element to some user generated content and potentially Javascript code could execute there, like with these onError and so on handlers, then you want to sanitize user generated content before you output it.

* So for example here, if if we really needed to use innerHTML, we want to sanitize the content before we output it here. Now sanitizing means that we strip away any Javascript syntax we might have in there, that starts with simple script tags but should also include common keywords like alert and so on.

* Now how can you work around the problem that you might need to inject user generated content somewhere where Javascript could execute? So you have no way of using a text content node here for example and you still want to make sure that the user generated data you're getting here is secure and doesn't contain any scripts that execute?

* You can use packages like sanitize-html. Refer : https://www.npmjs.com/package/sanitize-html

* This is a package which basically helps you sanitize text and remove any unwanted tags in there so that invalid code doesn't execute in there. Now one import note can be found here in the description right away, this sanitization actually should be done on the server if possible, that simply means that if we were in our Node.js application here, we might want to store any user input we get, so in our case the address, before we put it into the database. So we sanitize it with this package for example on the server-side before we store it so that the sanitized code is stored in a database because that's also something I mentioned, this attack pattern does not only exist if you can inject it into the URL, it also exists of course if you store something in the database

* So I want to make sure that whatever I send here is validated on this server before it's stored in a database and you can do this with sanitize-html as well.

* You can also use this in the browser but that is a bit too late, it's not it's not too late from a security perspective but you still might have hacked or adjusted code in your database which you also don't want, it doesn't do any harm in there but it's not ideal, if you ever forget to sanitize a browser, you then have an issue. So try to sanitize on the server not on the browser.

* Anyways if you wanted to sanitize then, well then you would just need to make sure you have this package installed and you then can run the sanitize-html function, pass your user generated content so that variable which holds the content the user has to sanitize-html and then it will strip away things like that, it will ensure that this is not part of the output, that instead you are able to render HTML but only secure HTML, this is what sanitize-html does.

```js
npm install sanitize-html
```

* Now we can import and use sanitize html

```js
import sanitizeHtml from 'sanitize-html';

class LoadedPlace {
  constructor(coordinates, address) {
    new Map(coordinates);
    const headerTitleEl = document.querySelector('header h1');
    headerTitleEl.innerHTML = sanitizeHtml(address);
  }
}
```

* So now we protect ourselves against cross site scripting attacks. Again this is best done on the server before it's stored in a database

### Third-Party Libraries & XSS

* There also is another way of launching cross site scripting attacks which is also important to be aware of and to keep in mind. Let's say you fixed all your holes in your code by either using text content or in places where you need to render HTML code by using sanitize-html or something comparable. Now you still might be vulnerable to cross site scripting attacks and do you have any idea where they might be coming from? How could someone still get Javascript code into your page which then executes when users load your page?

* Let's say you fixed all these holes where you output user generated content which is the number one hole, well how about third-party packages?

* Any third-party Javascript package you add to your project executes code when your app runs because you use the code of that package, in the case of Google Maps, we use it in the map.js file There we use that new Google Maps map thing, where do you think is this coming from? It's coming from the Google Maps Javascript SDK, so it is based on the Javascript code we import from Google's servers. So we trust Google that the code is secure because if it weren't, since this runs on the machines of our users, this of course would also be able to execute malicious code there.

* So when adding third-party libraries, you want to make sure that it's from parties whom you trust,
for example Google is quite trustworthy of course but you also of course need to be sure or very sure at least that these companies or library authors protect themselves against being compromised.

* You might trust Google but if someone gets access to this code on their servers which sounds unlikely but if this is done by a frustrated Google engineer let's say, then this person can of course rewrite this code which you import such that it still works but that in addition to working, it does something malicious.So third-party libraries of course also can be hacked and if they are, the malicious code that's injected into the library runs in your app and on the machines of your users.

* So you need to be sure that the libraries you're using are secure, that they're trustworthy and that they are also defending against being hacked.So you want to make sure you're working with third-party libraries which are actively maintained so that security holes in the libraries are fixed but at the same time, where contributions which in the case of open source libraries which most libraries are can be made by anyone, that contributions are  checked.

* Of course you can check the source code of the third-party library as well and if it's a relatively straightforward simple library, it might be worth scanning over that code and looking for anything strange. Does it try to access browser storage? Does it try to send HttpRequests even though it's a library that shouldn't do that? Have a look at that and analyze that code. It might sound dumb but this is a potential security issue, so you want to make sure you protect yourself against it.

* If I run npm install here to install all the third-party packages we're executing here, you'll see that once this is done, we get a little output here, all these packages and found zero vulnerabilities. Not every vulnerability it finds here is an issue though and for some projects, 

### CSRF Attacks (Cross Site Request Forgery)

* Refer : csrf.pdf

* let's move on to cross site request forgery. What is this attack? It can be related to cross site scripting attacks but it doesn't have to be. The idea behind cross site request forgery is that people trick you into clicking on a link that leads to a prepared page where they abuse your local cookies to then send a request to the page you would normally talk to as well, let's say your online banking backend but since you are coming from a prepared page, you then do something there automatically without you recognizing that you don't want to do, for example send money to someone you don't want to send, maybe that even as possible to happen behind the scenes without you noticing.

* So this attack pattern also depends on injected content, so injected Javascript code but it can
also be just HTML content which submits a form to the trusted server, the problem is that the form is part of an untrusted page which you might not recognize if the URL look similar and then requests on your behalf with your cookies can be made to the trustworthy server, so the only danger and the only problem is that this is all happening from a malicious site.

* So actions might be executed on the trustworthy page with your data without you knowing that it happens and therefore you might want to protect against that.

* Let's have a closer look at this pattern, though I do talk about it in more detail and also how to prevent it in my Node.js course because this is more a server side issue. You need to protect against CSRF attacks on the server side by using a mechanism which is a CSRF token and again I do discuss this in my Node.js course where we work on the server side on the backend all the time.

* So CSRF stands for cross site request forgery and the idea is that if you're the user of a web app and you have some backend, let's say the online banking site which uses Node.js or any other programming language, this is not exclusive to Javascript, uses any programming language and it talks to database, the server. Now you are visiting the frontend of this page, let's say that the user interface of your online banking site and now when you have such a setup, typically the server-side application identifies you with a so-called session,

* so you logged in and then the server side generates this session object, stores session data on the server and sends you the session ID which is stored in a cookie, that's a typical authentication setup you have in web pages where frontend and backend work together.

* Now then you might be sending an intended request, for example to send money to be and all is good.

* The problem is that if you now visit a fake site because someone sends you a link to it, which looks like the trustworthy site but which isn't, then you visit this page and there, people send a request to the trustworthy backend, therefore the browser attaches cookies that belong to this trustworthy backend which is your session ID for example and everything works.

* The problem just is since you're on a fake site, the request might be one that is intended by the fake site but not by you, for example to send money to C and this request will succeed because you sent the right cookie, the right session ID and you sent it through the right backend. So again more details can be found in a Node.js course but this is also a potential tech pattern but not directly linked to Javascript. It can be implemented with cross site scripting attacks but it can also be implemented by using such a fake site which does not require cross site scripting attacks.

### CORS (Cross Origin Resource Sharing)

* The last security relevant concept you should be aware of to be precise is cross origin resource sharing and I did talk about this earlier already. It's not really an attack pattern but a security concept.

* The idea here is that requests from your browser side application can only be made to a backend, so to a server side that runs on the same server, so that the HTML page and the script that executes there was served by the same server you're trying to send the request to, that's how browsers by default work with your pages, this is what they allow by default.

* Now in modern web applications, you often need to send requests to other servers though, so a server which did not serve the running HTML and Javascript code but which only provides some API endpoints, some URLs to which you can send data, to store data or to fetch data, this is something we had to look at in the Node.js module.

* Then you can control via server side headers and only via server side response headers whether this request, this incoming request is allowed and you send back a valid response or not. So this is a concept where by default browsers would not allow you to reach out to some other server but with the right server side headers, you can override this. So it's a concept which in the end should secure servers so that not everyone can access their resources, not every page can access their resources at least but with the right headers on the server side, you can allow this and an example for that actually would be Javascript modules.

* This built-in Javascript concept also required a development server if you remember and it required a development server because Javascript modules, so Javascript files you import into another file are only allowed if they're from the same server.

* Another example though would be the Node.js application we worked on. There, we have our different routes which we handle on the backend, like add location and the request which is sent there is not sent from some page that is served by the same server, it's sent by a page that runs on a different domain, on a different server and still it works because here, I set these right CORS headers which tell the browser that's okay, this page which sends the request is allowed to get the data and therefore the browser won't block it.

### Different Types of Websites

* Static Websites (just HTML + CSS + JS)

* Single-Page-Applications (SPAs, HTML + CSS + JS with only one HTML page being served, client-side JS is used to re-render the page dynamically)

* Dynamic/ Server-side rendered Web Applications: Websites where the HTML pages are created dynamically on the server (e.g. via templating engines like EJS).

* Refer : different-kinds-of-apps.pdf
* Refer : deployment-steps.pdf

### Static Host Deployment (no Server-side Code)

* Development webpack configurations

```js
const path = require('path');
const CleanPlugin = require('clean-webpack-plugin');

module.exports = {
  mode: 'development',
  entry: {
    'SharePlace': './src/SharePlace.js',
    'MyPlace': './src/MyPlace.js',
  },
  output: {
    filename: '[name].js',
    path: path.resolve(__dirname, 'dist', 'assets', 'scripts'),
    publicPath: 'assets/scripts/'
  },
  devtool: 'cheap-module-eval-source-map',
  devServer: {
    contentBase: './dist'
  },
  module: {
    rules: [
      {
        test: /\.m?js$/,
        exclude: /(node_modules)/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: [
              [
                '@babel/preset-env',
                { useBuiltIns: 'usage', corejs: { version: 3 } }
              ]
            ]
          }
        }
      }
    ]
  },
  plugins: [new CleanPlugin.CleanWebpackPlugin()]
};
```

* Production webpack configurations

```JS
const path = require('path');
const CleanPlugin = require('clean-webpack-plugin');

module.exports = {
  mode: 'production',
  entry: {
    'share-place': './src/SharePlace.js',
    'my-place': './src/MyPlace.js',
  },
  output: {
    filename: '[name].[contenthash].js',
    path: path.resolve(__dirname, 'dist', 'assets', 'scripts'),
    publicPath: 'dist/assets/scripts/'
  },
  devtool: 'cheap-source-map',
  module: {
    rules: [
      {
        test: /\.m?js$/,
        exclude: /(node_modules)/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: [
              [
                '@babel/preset-env',
                { useBuiltIns: 'usage', corejs: { version: 3 } }
              ]
            ]
          }
        }
      }
    ]
  },
  plugins: [new CleanPlugin.CleanWebpackPlugin()]
};
```

* In package.json

```js
{
  "name": "my-place",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build:dev": "webpack-dev-server",
    "build": "webpack",
    "build:prod": "webpack --config webpack.config.prod.js"
  },
  "author": "Maximilian Schwarzmüller",
  "license": "ISC",
  "browserslist": "> 0.5%",
  "devDependencies": {
    "@babel/core": "^7.6.2",
    "@babel/preset-env": "^7.6.2",
    "babel-loader": "^8.0.6",
    "clean-webpack-plugin": "^3.0.0",
    "core-js": "^3.2.1",
    "regenerator-runtime": "^0.13.3",
    "webpack": "^4.40.2",
    "webpack-cli": "^3.3.9",
    "webpack-dev-server": "^3.8.1"
  },
  "dependencies": {
    "sanitize-html": "^1.20.1"
  }
}
```
* we can run npm run build: prod

* and what this will give us is an output which we can ship to a web server to then host it there, to a static web server to be precise.

* So here, we got our dist folder and in the end we just need to take that entire dist folder content and deploy that. Now the only adjustment we specifically need to do here is our output, our Javascript files of course have this random hash in there and we should respect this in our imports.So on these Javascript files in the end, you can copy that file name and take the my-place file and replace this import here in assets index.html with this name here and do the same for share place.Now you can also set up more elaborate build workflows, where the HTML file is automatically adjusted but this requires advanced webpack knowledge and so on and therefore we'll take this manual approach here. ("my-place.bc41ad65d190b1b6795d.js")

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>My Place</title>
    <link rel="stylesheet" href="../assets/styles/app.css" />
    <link rel="stylesheet" href="../assets/styles/my-place.css" />
    <script
      src="https://maps.googleapis.com/maps/api/js?key=AIzaSyCueuEMBmaAK6XUl6pfsL0J5NTF6HwpjtY"
      defer
    ></script>
    <script src="../assets/scripts/my-place.bc41ad65d190b1b6795d.js" defer></script>
  </head>
  <body>
    <header>
      <h1></h1>
    </header>

    <section id="selected-place">
      <div id="map"></div>
    </section>

    <section id="share-controls">
      <a href="/">Share a New Place!</a>
    </section>
  </body>
</html>

```

* Now once you did that, once you prepared these script imports in the two HTML files and you have your optimized script outputs here, you're ready to go and you're ready to upload it to a web server. 

### Injecting Script Imports Into HTML Automatically

* In the previous lecture, we manually adjusted the HTML files to import the generated JavaScript files. For most projects, this is fine - you're probably not going to push out a new version of your scripts every few minutes. But you could also automate this process if you wanted to - with the help of a special plugin for Webpack: The HtmlWebpackPlugin.

* Refer : https://github.com/jantimon/html-webpack-plugin


