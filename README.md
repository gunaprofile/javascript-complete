# Javascript Complete

## What is JavaScript?

* Let's have a look at a couple of definitions.

* Javascript is a dynamic, weakly typed programming language which is compiled at runtime. It can be executed as part of a web page in a browser or directly on any machine in a so-called host environment.

* Okay, not entirely clear yet, let's have a look at an additional definition.

* Javascript was created to make web pages more dynamic, that's kind of the idea why Javascript was invented in the past you could say, more about the history of Javascript in a second by the way.

* So it was invented to change the content on a page directly from inside the browser and originally, it was called LiveScript but due to the popularity of Java back at the time, it was renamed to Javascript to resemble Java by its name.

* Javascript however is totally independent from Java, that's important to understand and has nothing in common, it really was just named Javascript because Java was popular and people who invented Javascript wanted Javascript to be a thing I guess but I'm not sure, are these definitions really clear?

* Do you now know what Javascript is? Does dynamic, weakly typed compiled at runtime make web pages more dynamic? Does this tell you something?

* Well let's take a step back maybe, how do web pages work?

* You are the user visiting a web page and when you do visit a web page, you use browser for that. You use your client, your machine, your laptop, your computer, where you have a browser installed on it, you enter a URL or click on a search result on Google and the web page gets loaded,

* To be precise when you initially visit a web page, a request is sent to the server,

* so to a computer in the Internet where that web page, where the HTML file is hosted and that server then loads that web page and sends it back to your browser in a so-called response 

* and the most basic form of response we know or we typically see and use when we work with the Internet, when we visit a page is simply an HTML page which is sent back by the server to the client.

* Now let's say on that loaded web page, let's say it's an online shop doesn't really matter, the user clicks a button to submit a form, for example to order some products.

* Now this will then trigger a new request which is sent by the browser to the server.

* to send this form submission to the server and the server will handle the incoming request, maybe store some order data in a database and once it's done, it will reply back with a new response, with a new web page, a new HTML document which is sent back to the client, maybe the order confirmation page. 

*  So this is how we interact with the web, this is how web pages traditionally work.

* Now Javascript helps us make this more reactive, it helps us make a web page more reactive and skip that second request response flow in some circumstances to instead change the already loaded page and do something there. (Refer : Intro/intro1)

## JavaScript in Action!

* Refer index.html

```html
<a href="info/dynamic.html">dynamic, interpreted</a>
and
<a href="info/weakly-typed.html">weakly typed</a>
```

* We have these 2 links dynamic interpreted and weekly type that if you click on any of these links you're taken to a new page.

* Now, what we have here is a traditional web page and what we also have here is a new request being sent to a server. If this were served by a server instead of our local file system and the new page being loaded right we're always loading a new page here.

* Of course, there's nothing wrong with that, but a more modern way a more user friendly way of doing that. Might be to not load a new page here, but instead to show this piece of information, maybe in an? Overlay to the existing page.

* so that we don't need to fetch a new HTML page and break the user experience of it, therefore, but instead that we stay on this existing page and only tweak it a little bit so we kind of need to tweak the existing HTML code, which was loaded by the browser whilst. We are on the page so without loading a new HTML file.

* That's the idea and that is where JavaScript can help us and why JavaScript was invented and why it. Is so important these days be cause any modern website you visit all these exciting user interactions you have there? With drop downs with overlays and so on all of that is driven by JavaScript 

* Now inside the assets/scripts we have app.js we need to include this dynamic javascript file.

```html
<script src="assets/scripts/app.js"></script>
```

* And go to these 2 links we have here to dynamic interpreted and the weekly type link. Now, Dear. Let's remove that link to info. Dynamic HTML and add a hash instead. And instead add a new attribute here.

```html
<a href="#" data-text="That means that code is not pre-compiled but instead evaluated, compiled and executed at runtime (e.g. when the browser executes the script)." class="info-modal">dynamic, interpreted</a>
and
<a href="#" data-text="Weakly typed languages assign types (like 'number') to variables (data
containers) at runtime - i.e. you (the developer) can't set the types
you want to use in certain places in advance. Only indirectly by making
sure you're always working with the correct values." class="info-modal">weakly typed</a>
```
* here we added data-text and class attribute

* Now make sure you get rid of any line breaks. You might have in their becaus that will not work, so that this is all one long line of code in the end.

* Add one important word here after the SRC attribute and that's the defer attribute.

```html
<script src="assets/scripts/app.js" defer></script>
```
* By simply clicking the reload I can hear and then click on one of these links.And now you will get this nice overlay

* Now, what we have here. Is knew content which is dynamically added to the existing web page? By JavaScript were not loading a new web page here Instead, this overlay is shown by JavaScript. And this of course, is a way better user experience.

* Becaus now we don't send the user off to a new page. We don't wait for new HTML code to be downloaded which change the existing page.

* This is something you see on a lot of modern web pages. Because it's faster it's more like in a mobile app where you also don't have to wait for things to complete. And effort this is one of the core reasons why JavaScript is that important, it allows us to make web pages more dynamic

* JavaScript is a dynamic weakly typed programming language. It's interpreted language, which means it's compiled on the fly. It's compiled before it runs and not compiled during development.As some other programming languages are it's a hosted language that runs in different environments, though, thus far in the last example. We just saw it. Run in the browser, but I'll come back to our environments in this course as well.

* And the most prominent use cases to case. We just thought we run code in a browser on a web page to make it more dynamic.

* So now we understand why we use JavaScript and what it allows us to do but what do these turns? Dynamic weakly typed, and interpreted as well as host language mean in detail.(Refer : Intro2)

## How JavaScript Is Executed

* How is the Javascript code in our browser executed and not just in our browser but typically in any environment where you run Javascript? 

* Let's say you write your Javascript code and you want it to have some effect on the web page if we talk about the browser as the environment where we run our script. 

* Then you have one important thing built into any environment where you want to run Javascript code and that's a Javascript engine.

* It's built into the browser as I said, there in Chrome for example, in the Chrome browser, it's v8, that's the name of the engine

*  in Firefox the name would be spider monkey and of course other browsers also either reuse these engines or have their own engines.

* Now the job of the engine is to parse code, so parse, read and understand your Javascript code, then on the fly compile it to machine code because machine code executes faster, so it reads your code but it does not necessarily execute it like that but instead, it now takes that code and compiles it to code which is faster to execute by the machine and then it executes that machine code.

* This all happens in the browser with the help of the Javascript engine and then when that code is executed, we have that effect on our web page.

* Now important, modern engines have a lot of optimizations there, they might start executing your uncompiled code and then compile the code whilst they're also already executing it to get started executing faster and then switch to the compiled code dynamically and so on, so we have a lot of optimizations going on here and we will dig a bit deeper into what the Javascript engine is exactly in a separate module

* for now this is all we need to know, the browser has a built-in tool that takes our code, compiles it, optimizes it and executes it and also a bit more technical side note, all of that happens on a single thread.

* Now this is very technical but you might know that in a computer, you have certain tasks that are executed, for example the browser you opened is a task or actually might consist of multiple different tasks that are executed together. The Javascript code is also yet another task which your operating system in the end has to take care of and this runs on a single thread.

* Of course we have multi-threading on modern machines so that they can do multiple tasks at the same time but the Javascript code execution runs on one single thread there, so the Javascript code execution always happens in one single thread on your operating system.

* I will come back to this concept of single threading and which implications this has for Javascript.

* But with that we know what interpreted means, what compilation means and how the Javascript code is executed.(Refer : Intro3)

## Dynamic vs Weakly typed

* Javascript is a dynamic, interpreted programming language but it's also a weakly typed programming language. Now what does this mean?

* For one, the dynamic interpreted part as you learned means that it's not pre-compiled, other languages like C++ are compiled during or after development, so before you share them with the end users, Javascript is on-the-fly compiled and that means that the code is evaluated and executed at runtime, it also means that the code can change at runtime.

* Now of course not the code you wrote and it won't magically change, of course the code you wrote gets executed but in that code, you can do some things which you are not allowed to do in other programming languages,

* In Javascript you are allowed to dynamically switch the type of data there. In a variable, you might start by storing some text and suddenly at a later point of time, you store a number in there instead, in the same variable.

* one thing you can simply keep in mind is that the dynamic thing in Javascript means that it is parsed and interpreted and compiled at runtime and that therefore it is able to do certain things, for example switching the data of a variable other programming languages are not allowed to do. Now it's not necessarily good to switch the type of data dynamically or unexpectedly but you can do that, the main takeaway however is the on-the-fly compilation and interpretation.

* Now what about the weakly typed part here?

* This means that when we work with data in Javascript, for example text data or numbers, you don't have to tell Javascript that you're going to work with a text now or you're going to work with a number now, instead data types are assumed, are inferred automatically so to say, that's also related to this dynamic nature where data types can also change from one line to another.

* Now this might sound strange but in other programming languages, you have to define the type of data a variable will hold in advance. In Javascript, that's not the case, you don't have to tell Javascript that this variable, this data container will hold a number, instead you store a data in there and if that happens to be a number, so it is. In other programming languages, you have to tell the language that you're going to store a number in there and if you store something else, you would get an error.

* That's not the case in Javascript, it's forgiving there because it's dynamic and it also doesn't care about strong type definitions in advance, instead you just store data in there and go with every type of data that holds. So data types are not set in stone but can change.

* Refer : dynamic-vs-weakly-typed.pdf

## JavaScript Runs In A Host Environment

* Now the last thing we saw on the previous definition slide is that Javascript runs on a host environment, so that means a Javascript engine can be part or can be executed in different environments.

* The most well known environment is the browser, modern browsers have Javascript engines built in and they're therefore capable of executing Javascript code but you also can run Javascript in other environments,

* for example on the server side, so right on a computer without having a browser in between, so not inside of a browser but simply execute code like this on your machine.

* Now Javascript was invented to run in the browser, to make web sites more dynamic, to be able to change things on a web site without loading a new page you could say because Javascript is able to closely work together with the loaded HTML code, with CSS, you can also use Javascript to send background HTTP requests, so to send some behind the scenes requests and fetch data without reloading the page and much more and it will do all of that throughout the course.

* Now there also are certain things Javascript can't do when it runs in the browser environment though, for example it can't access your local file system for security reasons because otherwise every web page you visit would be able to read your file system, maybe delete files on your computer and so on which would be horrible of course and in general it is running in a sandbox you can say, it's not able to interact with your operating system and so on, the browser gives you certain things you can do in this environment and doesn't allow other things.

* Now as I said, the browser is only one environment though, the one for which Javascript was invented but not the only one we're restricted to right now, instead the Javascript engine Google developed, v8 is the name of the engine, was extracted by some people to run Javascript anywhere because the idea was if we have the engine in the browser, why don't we take it out of the browser and then make it available as a standalone tool which you can use to execute Javascript anywhere else directly on your machine and this tool is in the end called Node.js.

* Node.js can be executed on any machine and therefore it's also often used to build web back-ends, to build web servers, server side Javascript is also something you often hear as a description for that.

* Now you might know about server side languages like PHP or also Java and other languages as well, well Javascript with Node.js can also be used to run it on the back-end of a web page, so on the server and not on the client in the browser of your users.

* Node.js also has certain things it can do and can't do, since it runs directly on a machine, in Node.js you're able to access the local file system, so you are able to write files and so on because unlike browser side Javascript, Node.js code has to be executed actively by you, it's not like you visit a web page and then this starts happening, instead it can only access the file system on the machine where it executes which for example is the server where it runs, not your machine and it's also able to interact with the operating system and so on.

* However on the other hand since it doesn't have direct access to the loaded web page, it can't manipulate kHTML or CSS like browser side.

* Javascript is able to do, so we basically have even inverse access capabilities here and these are the different environments where Javascript is able to run.

* Refer : js-host-environment.pdf

## JavaScript vs Java

* Now Java and JavaScript are 2 totally independent programming languages. That's really important, they have a totally different syntax different principles. They have almost nothing in common besides the name I would say, although JavaScript runs in the browser and. As learned all other environments. Java does not run in the browser.You can use it on the server side. To render HTML dynamically there and send it back to users you can also use it in a non web development context. But it is not supported directly in the browser.

* You can't use it for the same things as you can use JavaScript. You also as I mentioned have different principles, Java for example, is strictly object oriented and strongly typed there.

* You need to define which kind of data you're about to store in a data container. Where is JavaScript is really flexibel you don't have to work with objects only there and will dive into object? Orientation and what that means for out the course no worries. And it's also weakly typed as you learn.

* Refer : java-vs-js.pdf

*  And speaking about that differentiation between client side and server side JavaScript clientside means in the browser server side with the help of node JS means. On a machine that is connected to the Internet, which might be serving webpages, but which doesn't run code.Directly on the machines of the end users

* client side part is really the origin of JavaScript.

* And as you learn different browser vendors so different companies developing browsers provide their own JavaScript execution engines for example,V8 Engin that's the name of the JavaScript engine Chrome users. And the important thing there is that in the browser.

* You are able to interact with the web page. From inside JavaScript you can manipulate the loaded web page. You can also use certain browser features so called. APIs for example, to get the user location

* on the server side. The idea was to well, extract the engine out of the browser. And allow you to run it outside of the browser.

* So that you can reuse your JavaScript knowledge, but now use JavaScript for different tasks.

* And you can now run JavaScript anywhere with node js for example, in a server side and there, you have special our features. Our APIs you can tap in for example, to work with the file system or handle incoming HTTP requests. 

* Now the syntax concepts, and core features are exactly the same, though(Refer : Intro4)

* It's JavaScript origin. There are no alternatives to JavaScript in browser so you might not be interested in using JavaScript on a server if you are interested in front end.Browser side web development, though you have to learn JavaScript cause that is the only programming language, you can use there in the end.

## A Brief History Of JavaScript

* In 1995, Netscape introduces LiveScript which thereafter was renamed to Javascript.

* Now in 1996, Microsoft also released its own version of Javascript in Internet Explorer. So same idea, generally same syntax but also differences, so we already had an issue there. Back then Javascript also wasn't able to do a lot of things it was mainly used for a spammy things, for annoying overlays and pop-ups but another problem was that you had to write very different scripts for different browsers.

* In late 1996, people saw that this fragmentation could be a problem and therefore, Javascript the language was submitted to the ECMA (European Computers Manufacturers Association) committee to start standardization.

* This simply is an organization which will stand responsible for standardizing Javascript so that you have one standard which then could be implemented by multiple browsers.

* So then we had ongoing standardization efforts until 2005 roughly and Microsoft didn't really join the party, well we know how Microsoft was in the late 90s, early 2000s, ultimately they supported the standardized Javascript version.Still there were differences but we had a more common way of using Javascript.

* Now the efforts of standardization continue and we had huge progress until 2011 I'd say and Microsoft eventually joined forces and became part of the standardization process and of contributing to building that standard and adding new features

* I would say especially since 2010, 11, it really picked up and it is under active development. It's evolving, new features are getting added, it's getting a better and better programming language and whilst it was quite clunky around 2006 and 7 and despite standardization, every browser still did its own thing in some regards. We nowadays really have a uniform language with some small differences but mainly one core language we can use in different browsers.

* Still different browsers have their own features you can then use but the core language is the same I'll explain how you find out which differences we still have and how you work around them.

* Refer : js-history.pdf

* But in the end, we now have a standardized language which is really great to use and we have that thanks to this ECMA international organization. Now this organization in the end manages a language called ECMAScript but ECMAScript and Javascript actually have a strong relation.

* ECMAScript is the actual language which is evolved by the ECMA international organization but ECMAScript is then implemented as Javascript by browser vendors, so by the companies working on the browsers. So it is the most famous ECMAScript implementation, Javascript is that most famous ECMAScript implementation, others would be ActionScript or Jscript and you don't really need to know those.

* So Javascript in the end is ECMAScript we could say and we have that organization that evolves ECMAScript and with every new standard version you could say which is the end then created and finalized, browser vendors take that and implement it into browsers.

* So ECMAScript itself isn't directly used but browser vendors implement it into their Javascript engines.

* Each browser comes with its own Javascript engine though and therefore it's each browser and the engine used by the browser which defines which exact features are supported and actually browsers also sometimes implement certain features at an earlier point of time than they're really finalized in ECMAScript.

* So whilst discussions might still be going on over final implementation details, browser vendors could already go ahead and implement a certain feature in their engine already so that you can already use next generation Javascript code earlier in one browser than in the other 

* you will learn how to use next-gen Javascript when writing your code and still ensuring that it runs in all browsers. 

* So ECMAScript is under active development and browser vendors are contributing there as well, are shaping how Javascript looks like and which new features we want there and therefore Javascript is under active development and therefore it's an always evolving language.

* This is important because in 2015, 2016, Javascript saw a major overhaul, a brand new version you could almost say with a lot of important changes 

