# Javascript Complete

## Modular JavaScript (Working with Modules)

### Splitting Code in a Sub-optimal Way

* so the core application logic is really completely in that app.js file. Now we might want to split that, why? 

* To keep our code maintainable, to make it easier to work on it, also if you're working on a team if you have multiple files, it's easier than if you're all working on the same file because that is certain to break at some point of time, even if you can add it on the same file at the same time, you will certainly run into situations where you add something which a colleague then deletes accidentally and so on.

* So having multiple files helps you a lot when you are working in the team but even if you're working alone, you just find stuff quicker if you have multiple files. 

* So an idea would be to split this file such that all classes have their own files. That might sound like file overkill but actually it is indeed the standard, the default for most projects to have one thing per file, one class per file, one big function per file.

* Maybe you have a file with multiple smaller functions, shorter functions but typically it's one piece, one thing per file and therefore here we could add a new subfolder to keep things organized and add a utility folder and maybe an app folder.

* Now of course we have to refactor our code here,Now we can also split the other code here into seperate file, and then we can import wherever we need

```html
<script src="assets/scripts/Utility/DOMHelper.js" defer></script>
<script src="assets/scripts/App/Component.js" defer></script>
<script src="assets/scripts/App/Tooltip.js" defer></script>
<script src="assets/scripts/App/ProjectItem.js" defer></script>
<script src="assets/scripts/App/ProjectList.js" defer></script>
<script src="assets/scripts/app.js" defer></script>
```
### A First Step Towards JavaScript Modules

* If you have programming experience with some other programming languages, then you might notice approach of specifying import statements or references to other files directly in a file. 

* So you could say my tooltip here actually extends the component, so this tooltip.js file definitely needs access to the component.js file, this is a dependency we have. Now we fix or we implement this dependency currently by ensuring the order,for example if I would switch tooltip and component, this would break because tooltip would try to use component before component is defined.

* So we have a dependency here, project item for example uses the tooltip class, though only at runtime when this code executes, so we might be able to mess up the order here and still get away with it, nonetheless here, we also have a dependency. So it would be nice if we could say, at the top of the tooltip.js file for example, this file depends on component and then we could remove this component import here and tooltip would automatically import component when it needs it,

* Before we add component as dependency in tooltip, let us first export compenet.js

* The keyword is the export keyword. This is a keyword built into Javascript which signals to Javascript that inside of a module, I want to make this class here available to other files as well.

```js
// Component
export class Component {
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
```
* Now we need to switch to modules and for this, we go to our script here, to our root script which we always will need to import here because there is at least one script which we have to import into HTML file because whilst scripts will be able to point at each other, we need one starting point and to that starting point script, to the script tag we add type and set this to module

```html
<script src="assets/scripts/Utility/DOMHelper.js" defer></script>
<!-- <script src="assets/scripts/App/Component.js" defer></script> -->
<script src="assets/scripts/App/Tooltip.js" defer></script>
<script src="assets/scripts/App/ProjectItem.js" defer></script>
<script src="assets/scripts/App/ProjectList.js" defer></script>
<script src="assets/scripts/app.js" defer type="module"></script>
```
* This is understood by the browser and tells the browser that now this script and all scripts referenced by this script,currently there are none, soon there will be more, will use modules.

### We Need a Development Server!

* After this change we will get lot of error messages

* Where is this error coming from? What does this error even mean? 

* Modules are a relatively new feature and as you might imagine, since scripts can point at other scripts that should be imported, we have to be strict regarding security, we need to make sure that a script can't import a script from another page which might be malicious.

* You might think why would I add such code to my own scripts. Well if you're using third-party libraries, then such a library might be compromised and might try to download files from other malicious files,

* this is why we have a cross origin request policy which basically means cross origin, so cross domain requests are not allowed, you're only allowed to download scripts from the same domain your page is running on. 

* Now the problem we're facing here is that we're serving this app with the file protocol. Remember, what we always did thus far is we double clicked index.html in the Windows Explorer or in the MAC finder and this opens it up in the browser but actually does not open it up as it would beopened if it is served by a web server,

* it just opens it with this file protocol, which simply just means it displays the content of the file and is able to execute Javascript conveniently. Thus far, this wasn't a problem because a lot of features worked just fine with the file protocol but web server dependent features, like using this cross origin policy don't work there, this policy requires the page to be served from a real web server in order to be validated and therefore we now need to serve our app here, our page through a web server and not through the file protocol.

* Now that sounds difficult, it sounds like we need to setup our own server, host it somewhere, maybe pay for it, the good thing is you don't.

* Refer : https://www.npmjs.com/package/serve

* if you google for servejs, you should find this serve package here on npmjs.com, we had a look at npmjs.com in the third-party library module and here, you get a basic web server which you can install on your system and run on your system, you don't need to deploy your page anywhere, don't need to rent any servers, it spins up a little server on your machine, works on Windows and macOS and this server will serve your application.Now to install it, we need to install Node.js

* We just need to install the Node.js runtime on our machine because this serve package requires Node.js because that little server which it spins up is written in Node.js.

* you can run a special command to install this serve tool,

```js
npm install -g serve
```
* and now what this will do is it will install this serve tool globally, Node.js. So with that installed, back in our project you can open up a terminal which is navigated into this project,

```js
serve // in your project run this "serve"
```

* now here with serve installed, you can simply type serve and it will automatically search for the index.html file and serve that through this dummy web server. So now this gives you the address you have to enter in the browser to view your project, in my case it's localhost:5000 and if I do enter this here, I load the same page as before

### First import / export Work

* Exporting it is one part but that still does not make it globally available, it just means that we can now import the exported thing and you can not just export classes by the way, you can also export functions, variables, constants and so on so that you can use the exported thing in another file,

* Now we can add this as dependency in tooltip that will automatically download component.js file. 

```js
import { Component } from './Component.js'; // here we need to import Component 

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
```
* Now this is good but what's not so good is that our application will still not work if I reload. The reason for that is that we still load the tooltip file with such a traditional import.

* We need to add type module there, to make sure that it can utilize module features which is the export keyword but also the import keyword. If we do so and we reload,

```html
<script src="assets/scripts/Utility/DOMHelper.js" defer></script>
<!-- <script src="assets/scripts/App/Component.js" defer></script> -->
<script src="assets/scripts/App/Tooltip.js" defer type="module"></script>
<script src="assets/scripts/App/ProjectItem.js" defer></script>
<script src="assets/scripts/App/ProjectList.js" defer></script>
<script src="assets/scripts/app.js" defer type="module"></script>
```
### Switching All Files To Use Modules

* So of course this was just a start, we got more dependencies, we got more files. Tooltip does have no other dependency as far as I can tell but project item for example has tooltip as a dependency. Now we can still work with multiple imports as we currently do but actually, I really want to switch to a module-only setup.

```js
    // <script src="assets/scripts/Utility/DOMHelper.js" defer></script>
    // <script src="assets/scripts/App/Component.js" defer></script>   
    // <script src="assets/scripts/App/Tooltip.js" defer type="module"></script>
    // <script src="assets/scripts/App/ProjectItem.js" defer></script>
    // <script src="assets/scripts/App/ProjectList.js" defer></script>
    <script src="assets/scripts/app.js" defer type="module"></script>
```
* So I'll comment out all these imports here, also the DOM helper and I'll only leave the app.js import here as our root entry point.

* So let's start our way from the bottom up, let's start in app.js because that's our entry file, this will be the file that is loaded first.

* We're using project list here, so project list is a dependency of this app class in the app.js file. So we should go to the project list Javascript file and export project list here.

```js
import { ProjectList } from './App/ProjectList.js';

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
    analyticsScript.src = 'assets/scripts/Utility/Analytics.js';
    analyticsScript.defer = true;
    document.head.append(analyticsScript);
  }
}

App.init();
```
* So if we now save that, we'll see that we'll get a different error, now project item is not defined. This error is stemming from project list and that makes sense, we're able to get the project list into the app file so now the Javascript code execution moves on to project list and tries to run all that code, set up this class and it of course will see that project item is simply not known to this file because this is a dependency of project list which we're not stating yet.

```js
import { ProjectItem } from './ProjectItem.js';
import { DOMHelper } from '../Utility/DOMHelper.js';

export class ProjectList {
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
      if (event.relatedTarget.closest(`#${this.type}-projects ul`) !== list) {
        list.parentElement.classList.remove('droppable');
      }
    });

    list.addEventListener('drop', event => {
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
```
### More Named Export Syntax Variations

* Now regarding modules, there are a couple of alternative syntaxes and I want to show you which syntaxes exist.

* now let's say we would not have that class as a wrapper but we would export two individual functions.

* Now these functions can also be exported by adding an export keyword, you can export everything; functions, classes, constants, variables, everything which you use in your code like this.

```js
export function clearEventListeners(element) {
  const clonedElement = element.cloneNode(true);
  element.replaceWith(clonedElement);
  return clonedElement;
}

export function moveElement(elementId, newDestinationSelector) {
  const element = document.getElementById(elementId);
  const destinationElement = document.querySelector(newDestinationSelector);
  destinationElement.append(element);
  element.scrollIntoView({ behavior: 'smooth' });
}
```

* So just importing from a file is not enough, you need to be specific what you import, you don't import the entire file content. So here the solution of course would be to switch DOM helper, which I'm not using in this file anymore, to move element and now I'm importing that function which I'm referencing further down below and with that import added, if we now reload, this works again.

* Now we can use this in our projectList component as named import

```js
import { ProjectItem } from './ProjectItem.js';
import { moveElement, DOMHelper } from '../Utility/DOMHelper.js'; // Named Import moveElement

export class ProjectList {
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
      if (event.relatedTarget.closest(`#${this.type}-projects ul`) !== list) {
        list.parentElement.classList.remove('droppable');
      }
    });

    list.addEventListener('drop', event => {
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
    moveElement(project.id, `#${this.type}-projects ul`);
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

* Sometimes you do want to import everything that's in a file, let's say here in project list,but maybe you want to bundle all these things which are getting exported by this file in one object which you then have available in the projectlist.js file on which you can access these different exported features through the dot notation and that is possible with a special syntax.

*  If you have named exports like we do have it here, so where you have multiple export statements in front of something, then you can go to the file where you import that and remove this curly brace list here and instead add a star

```js
import * as DomObj from '../Utility/DOMHelper.js'; // Export everything
```
* Now we can access in your file with dot notation

```js
DomObj.moveElement // something like this
```
* what this tells Javascript is that you want to bundle together all the exports of that file into this object and now in this file, in the projectlist.js file in this case here, you can and you have to access this bundled object with the dot notation to then access the DOM helper class for example or the two functions.

* So this is an alternative which you can use if you prefer that for whatever reason, this is how you can bundle multiple exports together and then access them through this helper object in the importing file.

### Alias names

```js
import { moveElement as DOMmove, DOMHelper } from '../Utility/DOMHelper.js'; // Alias name with as keyword
```

### Working With Default Exports

```js
export default class { // here we used default keyword export
```

* In place of default export we can import with any name like below

```js
import cmp from './Component.js'; // this will be default export imported here

class Tooltip extends cmp {
  ...
  .....
}
```
### Combined both default and named export


```js
export class ProjectList {

}
export default class { // here we used default keyword export

}
```

* In place of default export we can import with any name like below

```js
import cmp, { ProjectList } from './Component.js'; // 

class Tooltip extends cmp {
  ...
  .....
}
```
### Dynamic Imports & Code Splitting

* so this is how all these things are getting imported. Now obviously we're sending a lot of HttpRequests here and for a small project like this, this is no problem, for a large project, this is not that great because even though the files are all small, sending a request, getting the response and parsing the response always has a bit of that's time which is always there, which you can't get rid of, network latency, the browser getting started, it's time which you can't get rid of.

* So the more requests you send, the more of that time you accumulate and therefore having hundreds of modules would not be a great idea because you would import hundreds of files and you would send hundreds of HttpRequests.

* One other improvement which we could add here is also that some features are not always needed, for example the tooltip.

* The tooltip here, this Javascript file is loaded because we might eventually show a tooltip but unless we click this button, we don't really need the file and of course it's a small file but in bigger applications where you have more logic in your files, that can add up.

* In addition, the tooltip file is the only file that needs the component, so actually we have to add the two together.

* So if you would only load these files when we need them, we would speed up the initial page load because we would request less files to be downloaded and parsed and that's also something we can do and something we can do in this module already.

* So to load modules conditionally, we can use an alternative import syntax.

* The import syntax you solve thus far at the beginning of the page is the static import syntax, it statically defines the dependency of a file. As an alternative to that, you have the dynamic import syntax and for some scenarios

* for example project list which is imported in app.js and which we need right at the start to construct our lists, there it makes no sense to load that dynamically because you'll always need it but for the tooltip, it can make sense. In project item, I need the tooltip here if we click on this show more info button

* So therefore instead of importing tooltip up there statically, which means it will be downloaded when the project item file executes, I can also just download it here and request it here. How? With a special syntax. 

```js
// ProjectItem module
import { DOMHelper } from '../Utility/DOMHelper.js';
// import { Tooltip } from './Tooltip.js'; // remove static tooltip

export class ProjectItem {
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
    import('./Tooltip.js').then(module => { // Dynamic import module
      const tooltip = new module.Tooltip(
        () => {
          this.hasActiveTooltip = false;
        },
        tooltipText,
        this.id
      );
      tooltip.attach();
      this.hasActiveTooltip = true;
    });
   
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
```
* Now this import function is built into the browser and exposed in Javascript and it gives you a promise, so you can add a then block here or use async await of course.

* now this will only reach out to this file, we should add the extension though, it will only reach out to this file here when this code here runs and not at an earlier point of time.

### When Does Module Code Execute?

* Now one important question might remain though, when you're working with modules and you have some code that runs inside of the module file, so which is not a class which you find on export but let's say here in console.log("project item") before export function, if we have a console console.log("project item") created, is this code executed or is only exported code executed?

* if I now reload, we see that output. So code in your modules runs when the module is imported and loaded for the first time and that's important,
for the first time.

* that's important, for the first time. If you have a module which is imported multiple times, like DOM helper which I imported in project item but also in project list, then it still only runs once. So if I go here into my DOM helper class and I add console log here, DOM helper executing,
we'll see that this only gets printed once even though we import from this file twice but it only runs on the first import so to say.

* So if I reload here, we see DOM helper executing but it never runs thereafter. What if we have some code in a dynamically loaded file, like tooltip which we just dynamically loaded Well let's output something here, tooltip loading or running, whatever you want. Let's output this here,
let's reload, of course we see no output initially because the file hasn't been loaded.

* So code in modules does execute and of course sometimes you want that, you need that behavior but it only executes once when a module is imported and used for the first time.

### Module Scope & globalThis

* Now before we quit, one additional word about scope. Now you learned that you can only use in other files what you export in the files you want to use the features from. So if you want to use that tooltip class, because we're using modules now, we can only use it in files that import tooltip, which is why we export it here, if you wouldn't export it, you also couldn't import it.

```js
window.DEFAULT_VALUE_OF_MAX = "Default value"; // we can set global like this
```
*  So the window object can be used to still share some data globally on your app, you need to explicitly add something to window but then this is an option, a hack around the otherwise scoped data so to say

* and hence, you should really just use it as a last resort if you really need to share some global data and for some reason, exporting and importing is not an option, otherwise don't do this.

* Now besides window however, you also now have another identifier and that's globalThis.

```js
globalThis.DEFAULT_VALUE_OF_MAX = "Default value";
```
* We haven't seen that before, as the name suggests, it's basically an alternative to this which points at some globally available object, the idea is also that this is available both in browser side Javascript and Node.js side Javascript, the window object is not available in both.

```js
console.log(globalThis.DEFAULT_VALUE_OF_MAX);
```
* the more interesting part therefore is what's inside global this besides our custom value? So if I just output global this in project list and I reload, you see it's the window object again.

* So in the end, "globalThis" in modules replaces this as your pointer at the window object because "this" inside of modules is not defined, it's just what it is, they run in strict mode and there "this" does not point at window but we got global this as an alternative and an extra benefit is that globalThis will also work in Node.js.