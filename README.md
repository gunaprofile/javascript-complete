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

* Now Javascript helps us make this more reactive, it helps us make a web page more reactive and skip that second request response flow in some circumstances to instead change the already loaded page and do something there.