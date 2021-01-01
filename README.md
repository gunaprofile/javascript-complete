# Javascript Complete

##  Working with Events

### Introduction to Events in JavaScript

* Refer : events-in-js.pdf

* Refer : https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Events

### Different Ways of Listening to Events


```js
const button = document.querySelector('button');

// button.onclick = function() {

// };

const buttonClickHandler = () => {
  alert('Button was clicked!');
};

const anotherButtonClickHandler = () => {
  console.log('This was clicked!');
};

// button.onclick = buttonClickHandler;
// button.onclick = anotherButtonClickHandler;

button.addEventListener();

button.removeEventListener();
```
### Removing Event Listeners

```js
const button = document.querySelector('button');

// button.onclick = function() {

// };

const buttonClickHandler = () => {
  alert('Button was clicked!');
};

const anotherButtonClickHandler = () => {
  console.log('This was clicked!');
};

// button.onclick = buttonClickHandler;
// button.onclick = anotherButtonClickHandler;

const boundFn = buttonClickHandler.bind(this);

button.addEventListener('click', boundFn);

setTimeout(() => {
  button.removeEventListener('click', boundFn);
}, 2000);

```
###  The "event" Object

```js
const buttons = document.querySelectorAll('button');

const buttonClickHandler = event => {
  event.target.disabled = true;
  console.log(event);
};

buttons.forEach(btn => {
  btn.addEventListener('click', buttonClickHandler);
});
```

### Supported Event Types

```js
const buttons = document.querySelectorAll('button');

const buttonClickHandler = event => {
  event.target.disabled = true;
  console.log(event);
};

buttons.forEach(btn => {
  btn.addEventListener('mouseenter', buttonClickHandler); // mouseenter event
});

window.addEventListener('scroll', event => { // scroll event
  console.log(event);
});
```

### Example: Basic Infinite Scrolling

* Let's have fun with the scroll event and create a list which you can scroll infinitely (explanations below)!

* You can run this code snippet on any page - just make sure that you can scroll vertically (either by adding enough dummy content, by adding some styles that add a lot of height to some elements or by shrinking the browser window vertically).

```js
let curElementNumber = 0;
 
function scrollHandler() {
    const distanceToBottom = document.body.getBoundingClientRect().bottom;
 
    if (distanceToBottom < document.documentElement.clientHeight + 150) {
        const newDataElement = document.createElement('div');
        curElementNumber++;
        newDataElement.innerHTML = `<p>Element ${curElementNumber}</p>`;
        document.body.append(newDataElement);
    }
}
 
window.addEventListener('scroll', scrollHandler);
```
* At the very bottom, we register the scrollHandler function as a handler for the 'scroll' event on our window object.

* Inside that function, we first of all measure the total distance between our viewport (top left corner of what we currently see) and the end of the page (not just the end of our currently visible area) => Stored in distanceToBottom.

* For example, if our browser window has a height of 500px, then distanceToBottom could be 684px, assuming that we got some content we can scroll to.

* Next, we compare the distance to the bottom of our overall content (distanceToBottom) to the window height + a certain threshold (in this example 150px). document.documentElement.clientHeight is preferable to window.innerHeight because it respects potential scroll bars.

* If we have less than 150px to the end of our page content, we make it into the if-block (where we append new data).

* Inside of the if-statement, we then create a new <div> element and populate it with a <p> element which in turn outputs an incrementing counter value.

### Working with "preventDefault()"

*  Now as a default, you will see that if I try to execute this and I reload this page, if I click submit here,

* This exists on any event object in Javascript, not just for the submit event and it does what the name implies, it prevents the default behavior the browser would apply for this event otherwise. Now the default behavior of course depends on the kind of event

* For the submit event on a form, the default behavior is to submit that form to a server. For a link for example, the default behavior would be to go to that link and you can always block the browser from doing that and from following its default behavior and then instead implement your own logic.

```js
const form = document.querySelector('form'); // default behaviour of submit button to reload form

form.addEventListener('submit', event => {
  event.preventDefault();  
  console.log(event);  
});
```
* So now with this added, you will see that if we now reload again and I click submit, now we don't lose this because now the page doesn't reload because now, this default of taking the form data and sending it to a server and hence reloading the page, that is now prevented.

### Understanding "Capturing" & "Bubbling" Phases

* So prevent default is a very important method on the event object which you can use to control what the browser does with that event.

* We also got another important method for which we first of all need to understand how exactly the event behaves. Our events in Javascript, in browser side Javascript have two phases through which they run essentially and where they then trigger your event listeners, a bubbling and a capturing phase.

* Refer : event1

* we have section -> div -> button 

* Now we click that button here, now what actually happens is that the browser runs through two phases where it checks for listeners to that event.

* First, it runs through a phase which is called the capturing phase,

* second it runs through a phase which is called the bubbling phase.

* Now the capturing phase goes from outside to inside, now what does this mean?

* It's important to understand that a click event on such a nested button here cannot just be listened to with event listeners on the button but for example also with an event listener on that section and the browser during the capturing phase checks if you got a capturing event listener on let's say the section registered which would then actually run its function before any event listeners registered on the button because it's from outside to inside in the capturing phase and the section is outside of the button.

* The bubbling phase on the other hand does the opposite, it goes from inside to outside. Now all event listeners you add with add event listener are by default registered in that bubbling phase which means if we have an event listener on the button and on the section, the button event listener will run first, the section event listener will run second. We can change this and we can also do interesting things with that bubbling and capturing behavior.

### Event Propagation & "stopPropagation()"

```js
const button = document.querySelector('button');

const div = document.querySelector('div');

div.addEventListener('click', event => {
  console.log('CLICKED DIV');
  console.log(event);
});

button.addEventListener('click', event => {
  console.log('CLICKED BUTTON');
  console.log(event);
});
```

* So it executes from inside to outside because as I mentioned, by default all event listeners are registered in the bubbling phase which means that capturing phase which runs first is totally ignored

* when the browser checks from inside to outside for that element on which the event occurred, it first finds that on the element itself, we had a listener, therefore this runs first but then it checks if there was a listener on the surrounding element, which in this case is the div and there we also had a listener so it executes that as well, then the browser by the way also goes onto the body and checks if there we had a click listener but we don't so therefore it doesn't execute any code for that because there is no code to execute but it goes from the element from which the element occurs to all its ancestors and checks for event listeners on them and if they are there, it executes them, this is what the browser does.

* Now we could switch to the capturing phase by adding an extra third argument on the event listeners.

```js
div.addEventListener('click', event => {
  console.log('CLICKED DIV');
  console.log(event);
}, true);
```
* but if we set this to true, we're telling the browser this event listener should be part of the capturing phase.

* Now we don't have to switch all event listeners to the capturing phase, we can just add this one here and now you will see that it will actually run before this button event Listener here runs.

* So if I reload now and click this button, you'll see the div was clicked first or the event listener ran first and then the button, because this listener here triggered in the capturing phase which comes before the bubbling phase and since we registered this listener for the capturing phase, it executed its code. 

* So this can be useful if you want to switch the order and you have event listeners on the element itself but also on some ancestor and for whatever reason, you want to execute the ancestor listener first, that can be done by passing that extra third argument.

* Now this entire process of having multiple listeners for the same event because the event does not just trigger on the element itself but also on ancestors, that's called propagation, it means that the event propagates up, it bubbles up 

* in the capture phase, it kind of goes from outside to inside but it basically means the event does not just occur on the element itself but also on all ancestors or at least we can listen on all ancestors, it occurred on this button but it's listenable on all ancestors because it propagates up, it bubbles up, it can be used anywhere above.

* Now you can prevent this however. Let's say we do have an event listener on the div here but we really only want to react to clicks on the div here, not to clicks on a button in the div, that could be a requirement you have in an app where you really want to make sure that button clicks don't trigger the div click listener. To make sure that the click event for the button for example doesn't propagate anymore, you can call event.stopPropagation.

```js
button.addEventListener('click', event => {
  event.stopPropagation();
  console.log('CLICKED BUTTON');
  console.log(event);
});
```
* That's not the same as prevent default, prevent default stops the default behavior the browser might perform upon such an event, for a click event on a button, we have no default behavior for a click event on a link for example, we would have one, there the browser would leave the page and go to that links destination. Stop propagation does nothing with the default behavior, the default behavior still executes, so for a form for example, the form would still be submitted but it stops the propagation which means any other listeners for the same type of event on some ancestor elements will not trigger their event listeners for this event.

* So now you will see that clicked div will not be printed to the console when we click on a button. So if I reload and I click on a button, we only see the clicked button event listener, I have to click somewhere else on the div to trigger that div click listener.So that's what stop propagation does,

* you also have stop immediate propagation, that is useful if you have multiple event listeners on the same element, so if we had more event listeners on the button, then after the first event listener, the other button listeners, so the other listeners on the same element also wouldn't run anymore. With just stop propagation, all button event listeners would execute and only ancestor element clicked listeners would not execute.

```js
button.addEventListener('click', event => {
  event.stopPropagation();
  event.stopImmediatePropagation();
  console.log('CLICKED BUTTON');
  console.log(event);
});
```
* That's the difference here and just as prevent default, this can be a very useful tool to make sure that you can handle the events in exactly the way you want. So you learned about stop propagation, it's important to understand that not all events propagate.

* Again, common sense is a good starting point to find out whether an event propagates or not, for a click, it makes more sense, when we click something we could also want to listen to a click on a parent element

```js
const button = document.querySelector('button');

const div = document.querySelector('div');

div.addEventListener('mouseenter', event => {
  console.log('CLICKED DIV');
  console.log(event);
});

button.addEventListener('mouseenter', event => {
  event.stopPropagation();
  console.log('CLICKED BUTTON');
  console.log(event);
});
```
* For a hovering over something, it makes less sense. If I add mouse enter here to my button and also to my div and I reload, you see now I entered the div, so yes I'm still printing clicked div but it happens because of me hovering over it, now I hover over the button and therefore you see indeed, I only trigger clicked button and not the div as well,

* So mouse, mouse enter, mouse move events typically don't propagate, if you're not sure whether they propagate or not, whether they bubble up, you can always just print the event object and have a look at it because you will find a bubbles property in there and as you see, it says false here which means this does not bubble up and hence you don't have to call stop propagation because it won't propagate anyways and there are a couple of events, drag events for example for drag and drop which don't propagate up because that typically would lead to an undesired behavior.

### Using Event Delegation

* With event propagation, you can do quite interesting things, specifically you can implement a pattern which is also called event delegation,

* we've got two possible approaches. One is that we select all list items and we add multiple listeners


```js
const listItems = document.querySelectorAll('li');

listItems.forEach(listItem => {
  listItem.addEventListener('click', event => {
    event.target.classList.toggle('highlight');
  });
});
```
* So let's have a look at the alternative which uses this delegate pattern, this delegate approach.

* There instead of adding multiple listeners on each list item, we take advantage of event propagation, of the event bubbling up and therefore we get access to the entire list with document query selector ul, so with the list that holds the list items.

* Now we can register an event listener on that list, so I listen to a click event on the entire list because since events bubble up, we can also listen to a click on the list when we actually clicked on a list item because that's how Javascript events behave, most events at least.

```js
const list = document.querySelector('ul');

list.addEventListener('click', event => {
  event.target.classList.toggle('highlight');
});

```
* So now if I save that and I reload, I got the same behavior as before but now with only one event listener instead of multiple event listeners

* because we're taking advantage of event propagation and then we're adding a listener on the next higher element which in this case is the list instead of every child element itself, this is the event delegation pattern.

* Now this event delegation pattern can become problematic if we have a more complex structure.

* In case if we have some complex structure like below

```js
<ul>
  <li>
    <h2>Item 1</h2>
    <p>Some text</p>
  </li>
  <li>
    <h2>Item 2</h2>
    <p>Some text</p>
  </li>
  <li>
    <h2>Item 3</h2>
    <p>Some text</p>
  </li>
  <li>
    <h2>Item 4</h2>
    <p>Some text</p>
  </li>
  <li>
    <h2>Item 5</h2>
    <p>Some text</p>
  </li>
</ul>
```
* and now if we reload, we have a problem, if I click on the title, yes now you see this gets the red background, not the entire element.

* So now because we were referring to event target, we have a problem, we have a problem because event target is the actual DOM element on which we clicked and that's the lowest possible element, so that's either our h2 tag or if we clicked outside of h2 and outside of paragraph, it's the entire list item or it's just a paragraph or whatever else we might have in there, so just coloring or using event target is not helping us here.

* Now we also do have another property available and that is event.currentTarget. This is different from event target, let's see if that helps us.

* If we reload and I click somewhere on the h2 tag, well event current target is the entire unordered list here because current target, unlike target is not the actual element on which you clicked but the element on which you added the listener.

* So current target here always refers to the list element because that's where we register our listener. it's still not the element on which we clicked, it's still not the list item, it's just a list which also is not what you need.

* Well we can use a combination of event target and DOM traversal which I covered earlier in the course, in the DOM module to get access to the list item.

* What do we know about event target? Well we know it's inside of our list and if we have a look at our list, we see it's definitely somewhere inside of the list item, because we have just the list and the list items.

* So we know it's inside of our list item, hence it's either the h2 tag, the paragraph or the list item itself. Now you learned about a specific method that can help us here and that would be on event target which is some DOM object inside of our list, the closest method. Closest exists on all DOM objects and it traverses up in the ancestor tree and there you can select the closest element with a certain CSS selector, you could select by ID, by class or just by tag and in this case I'm looking for the closest li.

```js
const list = document.querySelector('ul');

list.addEventListener('click', event => {
  event.target.closest('li').classList.toggle('highlight');
});
```

###  Triggering DOM Elements Programmatically

* Now sometimes, you don't want to listen to an event or at least not just listen to an event, you also might want to trigger an event programmatically.

* Let's say when I click any list item for whatever reason, I also want to trigger a click event on my button.

```js
const form = document.querySelector('form');

form.addEventListener('submit', event => {
  event.preventDefault();
  console.log(event);
});
```
* Now of course, we could store this function in a named function and then just call this function from inside here but let's say for whatever reason, that's not really an option, we really want to click that,

```js
list.addEventListener('click', event => {
  event.target.closest('li').classList.toggle('highlight');
  form.submit();
});
```
* when the list is clicked or any item in the list, I want to call form and now you can call click there for example to simulate a mouse click on it or also submit, as a method just like that and this does not just exist for the form but basically for any DOM element, a lot of the events you can listen to can also be triggered programmatically, especially something like clicks or form submissions.

* So now with that if you save this and you reload, you will see that if I click any list item here, the page reloaded because the form was submitted.

* Now you might be wondering why our event listener here for the form submission is not doing its work and preventing the default.Now if you trigger a submit event programmatically, then indeed the submit event listener is skipped,typically event listeners execute.

```js
button.addEventListener('click', event => {
  event.stopPropagation();
  console.log('CLICKED BUTTON');
  console.log(event);
});

list.addEventListener('click', event => {
  event.target.closest('li').classList.toggle('highlight');
  // form.submit();
  button.click();
});
```
* If I for example instead of submitting the form simulate a click on my button and please note that I do have a click listener on the button, you will see that if I do that and I reload, this click listener does execute, though with a dummy mouse event where all coordinates are zero by the way. For form submission, the event listener is not triggered. So triggering such an event programmatically is not exactly the same as when a user clicks on it, it can bypass certain listeners like a form submission listener. Still from time to time it can be useful and it is good to be aware of it.

* Now if you still wanted to work around that by the way, you could of course select the submit button here in the form and instead of calling submit on the entire form, you call click on the submit button, then it will basically do the same as if a user click the submit button which will trigger the submit the event listener and allow you to prevent the default, so that would be possible.

### Event Handler Functions & "this"

* We already saw earlier in the course that inside of the event listener functions, this is actually bound to something else, to the event's source as I mentioned and therefore in the past, we had to use bind or arrow functions to make sure that this actually points at the right thing.

* Now let's quickly see this in action again, on this button here where I added an event listener, let's not just console log event but also console log this here and see what we get. Now please note that here I'm using an arrow function though.

```js
button.addEventListener('click', event => {
  event.stopPropagation();
  console.log('CLICKED BUTTON');
  console.log(event);
  console.log(this); // window object
});
```
* So now if I reload and I click this click me button, you see I get the window object there because as I said, I'm using an arrow function and that ignores any bindings you might have assigned to this when this function gets called and here, the thing calling the function is the browser.

* The browser does bind this to something else though and to see this, let's convert this here into a regular function so to say, without the arrow but with the function keyword and still with the event object of course and let's see what this points at right now with a normal function being used here.

```js
list.addEventListener('click', function(event) {
  event.target.closest('li').classList.toggle('highlight');
  button.click();
  console.log(this); // <ul> .... </ul> 
});


```
* so to be precise as you see if you click down there, it points at the current target,so not at the concrete target you clicked on but at the current target,

### Drag & Drop - Theory

* let's have a look at drag and drop and how we can implement such a behavior in a web page.

* So to make elements draggable, you have to mark them as such and you do that by adding the draggable attribute or setting the draggable property on the DOM elements to true, so both needs to be set to true, either the attribute or directly through the property.

* Once you did that, this element is draggable. The next step then is to listen to a drag start event on the element which is draggable,this will be triggered whenever a user starts dragging the event. 

* In the event listener there, you can also interact with the event object to describe the drag operation if you are copying or if you're moving which will for example also affect how this is displayed to the user, how the cursor changes and so on and you can also append data to the event

* because a drag and drop operation is actually a combination of different events, on different elements on your page and in order to make sure that they can work together and that you know in the place where you dropped something what you started to drag in another place, you can add data to that drag event which is then invisibly kind of passed around to the other events related to dragging and dropping.

* So once we started our drag operation and we configured the drag event, we have to tell Javascript where the item can be dropped.Of course, the user can drop the item anywhere but we typically don't support it everywhere on our page, instead we kind of mark the areas where an item can be dropped by adding an event listener to the element on which the other element can be dropped.

* We add a listener to the drag enter and the drag over events. You can omit the drag enter event, you definitely need the drag over event. Both will be triggered whenever an item is dragged onto that element, the difference between the two elements is that drag over basically also triggers for child elements of the element where you added it, drag enter won't trigger there and then here, the trick is that in the event listeners you set to these events here, you have to call prevent default because the default is always that a drop operation is cancelled, so that drag and drop isn't allowed onto an element because as I said for most parts of your page, you don't want allow it and therefore that's the browser default, you have to prevent that default to allow a drop operation, 

* so it's an important step to listen to these events and prevent the default to allow a drop.

* Now before we react to a drop, you can also optionally listen to a drag leave event if you want to update the UI for example, updates some styles when the item is dragged away from the element on which it was dragged over. That is optional, not optional at least if you want to do something upon a drop is that you listen to the drop event on the same element where you listened to drag enter and drag over

* The drop event will only be triggered if you prevented the default in drag enter and drag over and the item is then dropped onto that same element.

* Now in that drop event, you can then do whatever you want to do upon a drop, update some data, update the UI, move the element on the UI, anything like that because that's also important.

* When you make an element draggable, you'll get some visual feedback that looks like the user is really dragging the element but it's actually not getting moved in the DOM technically, you have to do that programmatically through Javascript if you want to update the DOM after performing such a drag and drop operation.

* You also optionally can then listen to a drag end event, not on the place where it was dropped but on the dragged element itself and there you could also then update the UI or some data, whatever you want to do.

* The drag end event is always fired even if the drop is cancelled, so if the element was dropped in an invalid area but as I will show you, you will get some property on the event object which tells you whether the drop was successful or not.

* Refer: event2

### Configuring Draggable Elements

```js
<ul>
        <li
          id="p1"
          data-extra-info="Got lifetime access, but would be nice to finish it soon!"
          class="card"
          draggable="true" //draggable
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
          draggable="true" //draggable
        >
          <h2>Buy Groceries</h2>
          <p>Don't forget to pick up groceries today.</p>
          <button class="alt">More Info</button>
          <button>Finish</button>
        </li>
      </ul>
```
* So now this is draggable, how do we proceed? Well the next step was to listen to the drag start event,

* I will also set up a listener to the drag start event and for this, I'll add a new method here and I'll name it connect drag, you can name it however you want, of course I'll just name it like this to be in line with connect more info button and connect switch button and then here in the constructor, I will call this connect drag like that.

```js
class ProjectItem {
  hasActiveTooltip = false;

  constructor(id, updateProjectListsFunction, type) {
    this.id = id;
    this.updateProjectListsHandler = updateProjectListsFunction;
    this.connectMoreInfoButton();
    this.connectSwitchButton(type);
    this.connectDrag(); //  connectDrag
  }

  showMoreInfoHandler() {
    if (this.hasActiveTooltip) {
      return;
    }
    const projectElement = document.getElementById(this.id);
    const tooltipText = projectElement.dataset.extraInfo;
    const tooltip = new Tooltip(
      () => {
        this.hasActiveTooltip = false;
      },
      tooltipText,
      this.id
    );
    tooltip.attach();
    this.hasActiveTooltip = true;
  }
 
  connectDrag() { //  connectDrag
    document.getElementById(this.id).addEventListener('dragstart', event => { // dragstart event
    // in here, we can configure that drag event as I mentioned

    // you can configure which kind of operation it is so that the browser can display the correct cursor and you can also, if you wanted to, change this preview image which is generated here, so the thing attached to our mouse. By default it's a preview of the DOM element, now you could also point at any other image here

    // I don't want to change that preview image,I want to append some data though, I want to append the ID of the element we're dragging, so that later

    // when we drop it somewhere, we can extract that ID from the event object and do something with that because otherwise, we'll not know which kind of element was dragged in the place where it is dropped.

      event.dataTransfer.setData('text/plain', this.id);
      event.dataTransfer.effectAllowed = 'move';
    });
  }

  connectMoreInfoButton() {
    const projectItemElement = document.getElementById(this.id);
    const moreInfoBtn = projectItemElement.querySelector(
      'button:first-of-type'
    );
    moreInfoBtn.addEventListener('click', this.showMoreInfoHandler.bind(this));
  }

  connectSwitchButton(type) {
    const projectItemElement = document.getElementById(this.id);
    let switchBtn = projectItemElement.querySelector('button:last-of-type');
    switchBtn = DOMHelper.clearEventListeners(switchBtn);
    switchBtn.textContent = type === 'active' ? 'Finish' : 'Activate';
    switchBtn.addEventListener(
      'click',
      this.updateProjectListsHandler.bind(null, this.id)
    );
  }

  update(updateProjectListsFn, type) {
    this.updateProjectListsHandler = updateProjectListsFn;
    this.connectSwitchButton(type);
  }
}

class ProjectList {
  projects = [];

  constructor(type) {
    this.type = type;
    const prjItems = document.querySelectorAll(`#${type}-projects li`);
    for (const prjItem of prjItems) {
      this.projects.push(
        new ProjectItem(prjItem.id, this.switchProject.bind(this), this.type)
      );
    }
    console.log(this.projects);
  }

  setSwitchHandlerFunction(switchHandlerFunction) {
    this.switchHandler = switchHandlerFunction;
  }

  addProject(project) {
    this.projects.push(project);
    DOMHelper.moveElement(project.id, `#${this.type}-projects ul`);
    project.update(this.switchProject.bind(this), this.type);
  }

  switchProject(projectId) {
    // const projectIndex = this.projects.findIndex(p => p.id === projectId);
    // this.projects.splice(projectIndex, 1);
    this.switchHandler(this.projects.find(p => p.id === projectId));
    this.projects = this.projects.filter(p => p.id !== projectId);
  }
}
```
* Refer for setData : https://developer.mozilla.org/en-US/docs/Web/API/HTML_Drag_and_Drop_API/Recommended_drag_types

* so you can attach a broad variety of things, of data and the type which we use here will be important for when we then plan on making or adding a droppable place in our application because there, we could check what is getting dragged over it so that we don't accept any droppable data but only of a specific type.

* Refer for effectAllowed : https://developer.mozilla.org/en-US/docs/Web/API/DataTransfer/effectAllowed

*  find this link attached where you can have a move operation, a copy operation and so on. Now the browser cursor should be updated accordingly

### Marking the "Drop Area"

* To mark something as a drop zone or as a place where you can finish a drag event so to say, you need to add an event listener and prevent the default

* So we need to go to that list and listen to these events and prevent the default. For that here in the project list class which is in the end responsible for managing our lists, I'll add a connect droppable method and trigger that from inside the constructor, so that when we create a new project list, we in the end set up this droppable event listening here.

```js
class ProjectList {
  projects = [];

  constructor(type) {
    this.type = type;
    const prjItems = document.querySelectorAll(`#${type}-projects li`);
    for (const prjItem of prjItems) {
      this.projects.push(
        new ProjectItem(prjItem.id, this.switchProject.bind(this), this.type)
      );
    }
    console.log(this.projects);
    this.connectDroppable();
  }

  connectDroppable() {
    const list = document.querySelector(`#${this.type}-projects ul`);

    list.addEventListener('dragenter', event => {
      if (event.dataTransfer.types[0] === 'text/plain') {
        list.parentElement.classList.add('droppable');
        event.preventDefault();
      }
    });

    list.addEventListener('dragover', event => {
      if (event.dataTransfer.types[0] === 'text/plain') {
        event.preventDefault();
      }
    });

    list.addEventListener('dragleave', event => {
      if (event.relatedTarget.closest(`#${this.type}-projects ul`) !== list) { // event.relatedTarget
        list.parentElement.classList.remove('droppable');
      }
    });

    list.addEventListener('drop', event => {
      // Now to react to a drop, we can add another event listener, still on the list because that's the area where we drop and that's the drop event
      const prjId = event.dataTransfer.getData('text/plain'); // event.dataTransfer
      if (this.projects.find(p => p.id === prjId)) {
        return;
      }
      document
        .getElementById(prjId)
        .querySelector('button:last-of-type')
        .click();
      list.parentElement.classList.remove('droppable');
      // event.preventDefault(); // not required
    });
  }

  setSwitchHandlerFunction(switchHandlerFunction) {
    this.switchHandler = switchHandlerFunction;
  }

  addProject(project) {
    this.projects.push(project);
    DOMHelper.moveElement(project.id, `#${this.type}-projects ul`);
    project.update(this.switchProject.bind(this), this.type);
  }

  switchProject(projectId) {
    // const projectIndex = this.projects.findIndex(p => p.id === projectId);
    // this.projects.splice(projectIndex, 1);
    this.switchHandler(this.projects.find(p => p.id === projectId));
    this.projects = this.projects.filter(p => p.id !== projectId);
  }
}
```
### Firefox Adjustments

* I do recommend to follow along in Chrome but in case you're using Firefox, you might be seeing some strange behaviors / errors. Here are some code adjustments you can make to make it work in Firefox as well:

* In

```js
list.addEventListener('drop', event => {
```
* add the following line at the beginning:

```js
event.preventDefault
```
* I.e. it should look like this:
```js
list.addEventListener('drop', event => {
    event.preventDefault();
    // other code...

```
* In

```js
list.addEventListener('dragleave', event => {
```

* adjust the if statement to look like this:

```js
if (event.relatedTarget.closest && event.relatedTarget.closest(...) {...}
```
* I.e. it should look like this:

```js
list.addEventListener('dragleave', event => {
    if (event.relatedTarget.closest && event.relatedTarget.closest(...) {...}
```
* Here's the complete adjusted app.js file:

```js
class DOMHelper {
  static clearEventListeners(element) {
    const clonedElement = element.cloneNode(true);
    element.replaceWith(clonedElement);
    return clonedElement;
  }
 
  static moveElement(elementId, newDestinationSelector) {
    const element = document.getElementById(elementId);
    const destinationElement = document.querySelector(newDestinationSelector);
    destinationElement.append(element);
    element.scrollIntoView({ behavior: 'smooth' });
  }
}
 
class Component {
  constructor(hostElementId, insertBefore = false) {
    if (hostElementId) {
      this.hostElement = document.getElementById(hostElementId);
    } else {
      this.hostElement = document.body;
    }
    this.insertBefore = insertBefore;
  }
 
  detach() {
    if (this.element) {
      this.element.remove();
      // this.element.parentElement.removeChild(this.element);
    }
  }
 
  attach() {
    this.hostElement.insertAdjacentElement(
      this.insertBefore ? 'afterbegin' : 'beforeend',
      this.element
    );
  }
}
 
class Tooltip extends Component {
  constructor(closeNotifierFunction, text, hostElementId) {
    super(hostElementId);
    this.closeNotifier = closeNotifierFunction;
    this.text = text;
    this.create();
  }
 
  closeTooltip = () => {
    this.detach();
    this.closeNotifier();
  };
 
  create() {
    const tooltipElement = document.createElement('div');
    tooltipElement.className = 'card';
    const tooltipTemplate = document.getElementById('tooltip');
    const tooltipBody = document.importNode(tooltipTemplate.content, true);
    tooltipBody.querySelector('p').textContent = this.text;
    tooltipElement.append(tooltipBody);
 
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
}
 
class ProjectItem {
  hasActiveTooltip = false;
 
  constructor(id, updateProjectListsFunction, type) {
    this.id = id;
    this.updateProjectListsHandler = updateProjectListsFunction;
    this.connectMoreInfoButton();
    this.connectSwitchButton(type);
    this.connectDrag();
  }
 
  showMoreInfoHandler() {
    if (this.hasActiveTooltip) {
      return;
    }
    const projectElement = document.getElementById(this.id);
    const tooltipText = projectElement.dataset.extraInfo;
    const tooltip = new Tooltip(
      () => {
        this.hasActiveTooltip = false;
      },
      tooltipText,
      this.id
    );
    tooltip.attach();
    this.hasActiveTooltip = true;
  }
 
  connectDrag() {
    const item = document.getElementById(this.id);
    item.addEventListener('dragstart', event => {
      event.dataTransfer.setData('text/plain', this.id);
      event.dataTransfer.effectAllowed = 'move';
    });
 
    item.addEventListener('dragend', event => {
      console.log(event);
    });
  }
 
  connectMoreInfoButton() {
    const projectItemElement = document.getElementById(this.id);
    const moreInfoBtn = projectItemElement.querySelector(
      'button:first-of-type'
    );
    moreInfoBtn.addEventListener('click', this.showMoreInfoHandler.bind(this));
  }
 
  connectSwitchButton(type) {
    const projectItemElement = document.getElementById(this.id);
    let switchBtn = projectItemElement.querySelector('button:last-of-type');
    switchBtn = DOMHelper.clearEventListeners(switchBtn);
    switchBtn.textContent = type === 'active' ? 'Finish' : 'Activate';
    switchBtn.addEventListener(
      'click',
      this.updateProjectListsHandler.bind(null, this.id)
    );
  }
 
  update(updateProjectListsFn, type) {
    this.updateProjectListsHandler = updateProjectListsFn;
    this.connectSwitchButton(type);
  }
}
 
class ProjectList {
  projects = [];
 
  constructor(type) {
    this.type = type;
    const prjItems = document.querySelectorAll(`#${type}-projects li`);
    for (const prjItem of prjItems) {
      this.projects.push(
        new ProjectItem(prjItem.id, this.switchProject.bind(this), this.type)
      );
    }
    console.log(this.projects);
    this.connectDroppable();
  }
 
  connectDroppable() {
    const list = document.querySelector(`#${this.type}-projects ul`);
 
    list.addEventListener('dragenter', event => {
      if (event.dataTransfer.types[0] === 'text/plain') {
        list.parentElement.classList.add('droppable');
        event.preventDefault();
      }
    });
 
    list.addEventListener('dragover', event => {
      if (event.dataTransfer.types[0] === 'text/plain') {
        event.preventDefault();
      }
    });
 
    list.addEventListener('dragleave', event => {
      if (event.relatedTarget.closest && event.relatedTarget.closest(`#${this.type}-projects ul`) !== list) {
        list.parentElement.classList.remove('droppable');
      }
    });
 
    list.addEventListener('drop', event => {
      event.preventDefault();
      const prjId = event.dataTransfer.getData('text/plain');
      if (this.projects.find(p => p.id === prjId)) {
        return;
      }
      document
        .getElementById(prjId)
        .querySelector('button:last-of-type')
        .click();
      list.parentElement.classList.remove('droppable');
      // event.preventDefault(); // not required
    });
  }
 
  setSwitchHandlerFunction(switchHandlerFunction) {
    this.switchHandler = switchHandlerFunction;
  }
 
  addProject(project) {
    this.projects.push(project);
    DOMHelper.moveElement(project.id, `#${this.type}-projects ul`);
    project.update(this.switchProject.bind(this), this.type);
  }
 
  switchProject(projectId) {
    // const projectIndex = this.projects.findIndex(p => p.id === projectId);
    // this.projects.splice(projectIndex, 1);
    this.switchHandler(this.projects.find(p => p.id === projectId));
    this.projects = this.projects.filter(p => p.id !== projectId);
  }
}
 
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
 
    // const timerId = setTimeout(this.startAnalytics, 3000);
 
    // document.getElementById('stop-analytics-btn').addEventListener('click', () => {
    //   clearTimeout(timerId);
    // });
  }
 
  static startAnalytics() {
    const analyticsScript = document.createElement('script');
    analyticsScript.src = 'assets/scripts/analytics.js';
    analyticsScript.defer = true;
    document.head.append(analyticsScript);
  }
}
 
App.init();
```
