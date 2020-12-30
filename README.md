# Javascript Complete

##  Back to the DOM & More Browser APIs

###  Using "dataset" (data-* Attributes)

* data- attribute in general is a special attribute you can add to your own elements to attach any kind of data to them, so you could add data-id, data- whatever, data- whatever attribute or value you want to append or you want to store on one of your DOM nodes and the ID behind appending data or attaching data to DOM nodes simply is that you don't have to manage it in Javascript

```js
<ul>
    <li
      id="p1"
      data-extra-info="Got lifetime access, but would be nice to finish it soon!"
      class="card"
    >
      <h2>Finish the Course</h2>
      <p>Finish the course within the next two weeks.</p>
      <button class="alt">More Info</button>
      <button>Finish</button>
    </li>
    <li
      id="p2"
      data-extra-info="Not really a business topic but still important."
      class="card"
    >
      <h2>Buy Groceries</h2>
      <p>Don't forget to pick up groceries today.</p>
      <button class="alt">More Info</button>
      <button>Finish</button>
    </li>
</ul>
```
* you can add as many data attributes here as you wish. Now the only question is, how can we read from that attribute? And for that, you've got a special property you can read from. So here in app.js

```js
const projectElement = document.getElementById(this.id);
const tooltipText = projectElement.dataset.extraInfo; // get dataset ...
projectElement.dataset.someInfo = 'SOme info message' // set dataset ...
```
### Getting Element Box Dimensions

```js
// in developer tool select an element and then in console...

$0.getBoundingClientRect();

```
* you get back an object which gives you some useful information about this box, and you can run this for any element on the page.

* Now what this gives you is a couple of coordinates and sizes, to understand these values, you need to understand that the browser basically renders the page in a two-dimensional coordinate system with an x-axis from left to right and a y-axis from top to bottom

* and that's important. It's not like a traditional coordinate system where the y-axis would go from bottom to top and x from left to right at the bottom but instead it's all starting in the top left corner and that makes sense if you consider how a web page is rendered, it's rendered from top to bottom, not built up from bottom to top.

* This coordinate system also thinks in pixels and therefore in the top left corner, we would have the coordinate 0 0, x has a value of 0 and y has a value of 0. All the way on the right here, we would have y still equal to zero but x would be basically the width of your screen or the width of this document.All the way on the bottom left here, x would be 0, y would be just as high as your document and in the bottom right corner, x would be as big as your width and y would be as big as the height of this document.

* We also got the top and the left value and that's the same as x and y here as you can see and most of the time that will be the case, if you have a negative width or height, that can differ depending on the exact positioning and your CSS code but left and top simply gives you the coordinates of the leftmost and the topmost point of this box in the coordinate system and typically that's the same as x and y.

* More interesting is bottom and right, well that in the end is just the combination of left and top or x and y with width and height. So bottom and right basically tells you where the bottommost and the rightmost point is indeed coordinate system that starts on the top left and therefore width and height are also important of course, should be pretty self-explanatory, width gives you the total width of the box, height gives you the height of the box. So this is some rough general information you can get about this box.

### Working with Element Sizes & Positions

* Refer: browser/sizes.pdf

* now you can also get more specialized data by diving into your DOM element with special properties, for example there is offsetTop. offsetTop gives you the distance of the topmost point here to the top of this coordinate or to the start of this coordinate system and of course you don't just have offsetTop, you also have offsetLeft, makes sense I guess.

* offsetTop always is relative to your document start, NOT the viewpoint (ie it does not change upon scrolling). 

* You also however have clientTop and clientLeft and now that's a different thing as you can tell, where offset gives you the outer positioning, so the position of the box in the coordinate system, the client properties give you the inner positioning and specifically clientTop and clientLeft tell you how far it is from your left and topmost point to your left and topmost point of the content of this box and the content of the box is basically the entire box without any borders and potential scroll bars that might be rendered in there.

```js
$0.clientTop; // outer box top to the content box
$0.clientLeft; // outer box left to the content box
```
* You can also get some of the sizes for that, you can get the offsetWidth and you can get the offsetHeight and that's the entire width and height of this box, including all borders and scroll bars.

```js
$0.offsetWidth; //for eg 300
$0.offsetHeight;
```
* Now unsurprisingly, you also have clientWidth and clientHeight and that's again the inner width and height without borders and scroll bars, 

```js
$0.offsetWidth; //  ie 300-15(border)*2 = 270
$0.offsetHeight;
```
* therefore for the width, it's basically the width of 300 which we saw just a second ago minus the border times 2 because we got a border on the left and on the right and if we had a scroll bar, that would be deducted as well, the same for the height.

* Since we have scolling bars here can get some interesting data, we can for example get scrollHeight. Now what scrollHeight gives you is the entire height of the content including the part which is currently not visible in the box, so which is outside of the box at the bottom here for example or now at the top.

```js
$0.scrollHeight
```

* So scrollHeight is the entire height including the non-visible parts because they're currently scrolled out of view.

* You also have another interesting value, scrollTop, scrollTop gives you information by how much you scroll that content in the box.So if I scroll the content all the way to the bottom for example, you see now I have a value of 240 there

```js
$0.scrollTop// if scrolled to top then value is 0, if scrolled to bottom then value is total scroll height,eg 240
```
* Refer: browser/sizes.pdf

* One note about the entire document width, if you want to get that, you got two options, you can use window.innerWidth and window.innerHeight and that should give you the width and height you have here inside of your window, so without the dev tools, without that URL bar at the top and so on but the problem with that is that if you had a visible scroll bar on Internet Explorer for example or on Windows in general, you might have that, then this will actually include the scroll bar and not subtract it from the width and height and therefore actually give you more width and height than you actually have available for your content.

* Hence a better way of getting the real available width and height is

```js
document.documentElement.clientWidth;
document.documentElement.clientHeight;
```
### Positioning the Tooltip

```js
create() {
    const tooltipElement = document.createElement('div');
    tooltipElement.className = 'card';
    tooltipElement.textContent = this.text;
    
    const hostElPosLeft = this.hostElement.offsetLeft;
    const hostElPosTop = this.hostElement.offsetTop;
    const hostElHeight = this.hostElement.clientHeight;
    const parentElementScrolling = this.hostElement.parentElement.scrollTop;

    const x = hostElPosLeft + 20;
    const y = hostElPosTop + hostElHeight - parentElementScrolling - 10;

    tooltipElement.style.position = 'absolute';
    tooltipElement.style.left = x + 'px'; // 500px
    tooltipElement.style.top = y + 'px';

    tooltipElement.addEventListener('click', this.closeTooltip);
    this.element = tooltipElement;
  }
```
### Handling Scrolling

* scrollTo takes two coordinates - x and y, where you can define how much you want to scroll to the left or right and how much you want to scroll to the top or bottom.

```js
$0.scrollTo(0, 50); // now I tell Javascript that I want to scroll down to 50 pixels
```
* Now you can also not just scroll to absolute values, you can also scroll relatively with scrollBy.With scrollBy I tell Javascript by how many pixels I want to scroll down.


```js
$0.scrollBy(0, 50); // 50px down 
$0.scrollBy(0, 50); // 50px down more..
```
* I'll have the same effect as before actually but you'll see a difference now if I am at 50 and I repeat this command, now I scroll down 50 pixels more.

* If you want to make a specific element visible with scrolling though, there is an even easier way.

```js
static moveElement(elementId, newDestinationSelector) {
  const element = document.getElementById(elementId);
  const destinationElement = document.querySelector(newDestinationSelector);
  destinationElement.append(element);
  element.scrollIntoView({behavior: 'smooth'}); // scrollIntoView
}
```
* Now of course what you might notice is that scrolling here always means jumping, it immediately is there, we have no animation. 

* We could apply the same smooth animation to scrollTop too

```js
$0.scrollTo({top: 50, behavior: 'smooth'});
```
### Working with <template> Tags

* We can use a special HTML tag in our HTML code to kind of setup such to be used HTML code which we don't want to render right from the start but which we want to eventually use from inside our Javascript code and for this we can use a special tag, the template tag

```js
<template id="tooltip">
  <h2>More Info</h2>
  <p></p>
</template>
```
* the special thing about the template tag is that its content by default is not rendered but it's part of the DOM, so you can query it and use it and then use it in your Javascript code but it's not getting rendered by default at the beginning.

```js
const tooltipTemplate = document.getElementById('tooltip');
const tooltipBody = document.importNode(tooltipTemplate.content, true); // true here 
tooltipElement.append(tooltipBody);
```
### Loading Scripts Dynamically

* You can also create and run a script with Javascript, now let me show you how that would work. 

* It's more interesting if you have some other script file which you only want to download at a certain point of time, so where you want to control when the browser loads this script from inside your Javascript code.

```js
class App {
  static init() {
    const activeProjectsList = new ProjectList('active');
    const finishedProjectsList = new ProjectList('finished');
    activeProjectsList.setSwitchHandlerFunction(
      finishedProjectsList.addProject.bind(finishedProjectsList)
    );
    finishedProjectsList.setSwitchHandlerFunction(
      activeProjectsList.addProject.bind(activeProjectsList)
    );

    document
      .getElementById('start-analytics-btn')
      .addEventListener('click', this.startAnalytics); // executes that script only button is added
  }

  static startAnalytics() {
    const analyticsScript = document.createElement('script'); // <script></script>
    analyticsScript.src = 'assets/scripts/analytics.js';
    analyticsScript.defer = true; // once parsing is complete this script executes
    document.head.append(analyticsScript); // add this to  head
  }
}
```

### Setting Timers & Intervals

*  there are some other nice features the browser exposes to you in Javascript that also allow you to influence the user experience and one of these cool features is a timer which you can set or actually two different kinds of timer as you can set in your Javascript code.

* Let's say instead of starting our analytics here, when the user clicks a button, we want to do this three seconds after the page was loaded and for that indeed you can set a timer.

```js
const timerId = setTimeout(this.startAnalytics, 3000);

document.getElementById('stop-analytics-btn').addEventListener('click', () => {
  clearTimeout(timerId);
});
```
### The "location" and "history" Objects

```js
location.href = 'google.com' // this will move to new location
```
* You can also navigate with replace, this is a method which you execute to which you pass a URL, the difference to setting ref is simply that you can't go back because it will replace this page in that browser history which is in the end stored, so the back button won't be able to go back to this page after you used replace. 

```js
location.replace = 'google.com' 
```
* Another alternative would be the assign method, that's pretty much the same as setting the ref property and you can use both interchangeably depending on whether you rather navigate by setting a property or by calling a method

* on whether you rather navigate by setting a property or by calling a method and of course you got a couple of other properties and methods in there as well, for example you got host which tells you on which host this page is running, since we serve this locally from the files, we got no host there but if you are on some other page, like google.com for example, then you will see that host actually

```js
location.host()
location.origin // "https://www.google.com/gmail"
location.pathname // "/gmail/"
```
* You also got origin which is the full domain including the protocol which was used, also the pathname which is the part after the domain

* also the pathname which is the part after the domain and here I'm just on slash nothing.

* you also have window.history or just history therefore.

* Now location and history kind of work together, location allows you to edit the browser history by navigating around or by replacing the page, history then allows you to interact with that history, for example you can call the back method to go back.

```js
history.back(); // back to last page
history.length;
history.go(5);
```
### The "navigator" Object

* Now besides location and history, there also is the navigator object, also quite interesting. This allows you to interact with the browser and the operating system of your user so to say, of course only in a limited way, not unlimited access.

```js
navigator.userAgent // "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.88 Safari/537.36"
```
* Now what's up with this? This actually looks the way it does look for historic reasons, browser vendors in the past used to fake this to make sure that their browsers could have access to all features website might be using in their scripts because in the past, browser support was pretty different for different web features and hence, some programmers working on websites used user agent to find out if the user is using let's say Internet Explorer and therefore they wouldn't run a certain script if that browser was used. Now as browsers evolved and maybe Internet Explorer 7 included a feature which version 6 didn't include, that script that prevented Internet Explorer from getting access to certain features of the website would still block access even though support might now have been added to the browser.

* Therefore browser vendors started to put basically all browser names into this user agent string because this is set by the browser, so the people building the browsers of course can influence what shows up in there and hence, browsers basically faked to be another browser so that they could get full access to whatever the script wanted to do,so that the users of that browser had no limited or restricted user experience.Hence this is not really that useful.

* Refer : https://developer.mozilla.org/en-US/docs/Web/HTTP/Browser_detection_using_the_user_agent

* Refer : https://developer.mozilla.org/en-US/docs/Web/API/Window/navigator#Example_1_Browser_detect_and_return_a_string

* So I just wanted to show this because I find it kind of amusing to understand how this evolved over time,

* Now navigator also exposes many other features, for example it also exposes access to the clipboard API which allows you to add something to the clipboard or paste something into some input field for example.

```js
navigator.geolocation.getCurrentPosition((data)=>{
console.log(data)
};)
```
* so this function will be called for you by the browser once the location was fetched and then here if you enter this, you have to allow that the browser fetches your location and eventually, you'll get that position object with your coords object in there where you can find out which coordinates the user has and there's more in navigator.

### Working with Dates

* one last built-in or global constructor function you could say that helps you deal with dates and that's the date object and constructor function.

* Refer : https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date

```js
new Date(); // Wed Dec 30 2020 12:29:49 GMT+0530 (India Standard Time)

const date = new Date();
date.getDate();
date.getDay();
date.getTime();
```
### The "Error" Object & Constructor Function

* Now besides the date object, there's also one other built-in object which I want to show you because it can be important and that's the error object built into Javascript.

```js
throw new Error('Something Went Wrong!');
```
* you also get a stack trace which basically tells you where this error was thrown, in this case in the console which is why we see anonymous here but if we would do this in a file, we would get the file name and the file numbers and so on.

* So besides just passing some error message, this can give you more information, also since it's an object, you can add stuff to it.

```js
const customError = new Error('Something Went Wrong!');
customError.code = 404;
console.dir(customError);
```
* That's all possible and you can always console dir