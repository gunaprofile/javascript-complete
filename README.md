# Javascript Complete

## Async JavaScript: Promises & Callbacks

###  Understanding Synchronous Code Execution ("Sync Code")

* Refer : sync.pdf

* Let's start by understanding synchronous code and how Javascript works.

* I mentioned that Javascript is single-threaded. Until now, that didn't really matter to us that much but what single threading in the end means is that Javascript can only execute one task at a time,

* so let's say we have a bunch of code where we log something to the console, call a function, then maybe in that function or somewhere else after this function ran, we disable some button and then we maybe run another function. The important thing to realize here is that all these steps run in sequence after each other, they're not running simultaneously next to each other, instead Javascript will log something to the console, then call the function and do whatever needs to be done inside of that function, then go ahead and disable that button and then call that other function, so these things happen after each other, not at the same time.

* We could give them a label or some ID, let's simply use a, b, c and d and then we could say that for example, this second block, the B block here runs after A but it blocks C from running until it's finished. So the code in C won't be executed until some function execute it.

```js
const button = document.querySelector('button');
const output = document.querySelector('p');

function trackUserHandler() {
  console.log('Clicked!');
}

button.addEventListener('click', trackUserHandler);
```

* what happens here is that first of all, this code runs and Javascript selects this button and stores it in a constant, then only once this first line finished, the second line is executed and this task is performed and then this function declaration as you learned works a bit differently, that happens right at the start when Javascript reads the file at the beginning but then in the execution process so to say, this line executes after these two lines executed. So that's also important for us here because we need the button to be available in order to add an event listener, so we rely on this order which is enforced by Javascript.

* If it would be multi-threaded, it could maybe execute all tasks at the same time and here this would actually be bad because then we could not rely on the button to be available when we need it because if this line is executed at the same time as this line, well then the button might not have been selected yet and therefore adding an event listener to it would maybe fail.

* It's not something we have to worry about though because as I just explained, Javascript is single-threaded and therefore this order is guaranteed.

### Understanding Asynchronous Code Execution ("Async Code")

* Refer : async.pdf

* what if we have certain operations that take a bit longer and that expectedly take a bit longer?

* we have a console log statement but then we set a timeout and then we execute more code. Now the problem is setTimeout might take 2 seconds, 5 seconds, 10 seconds or maybe just 100 milliseconds but even 100 milliseconds would be a duration we probably would not want to wait because this operation could in general take longer and therefore block our code execution, so it prevents more code from being executed until setTimeout is done, at least if Javascript would treat this in the same way as it would treat all other code blocks.

* We simply have certain operations which can't be finished immediately, it's not just timers where we as a developer decide how long it should take, it's also other operations like HTTP requests or getting the user location which simply take a bit longer and typically you don't want to block your entire script until these operations finish because blocking your script would not just mean that the next line doesn't execute immediately, it would also mean that no other code can execute. So for example if you have an event listener to listen for a button click, then this handler function that should trigger when the button is clicked would also be blocked until the timer expired or until your HTTP request is done, so you would be blocking your entire page with these longer taking
operations and that's of course typically not what you want,

* this operation shouldn't need to wait for the previous one for this longer taking one to finish and thankfully Javascript and browsers have a solution for that, we have asynchronous code execution.

* So if we take this example with the timer but really just to emphasize this, the same would be true if we would be working with HTTP requests for example, so if we have certain operations which typically take longer, then we can actually offload them to the browser.

* So what we do there is we hand this operation of to the browser, by calling setTimeout we just tell the browser to set a timer but we then let the browser do that and therefore our code can execute right away because now the browser is actually able to use multiple threads, one for Javascript and one for another task for example and therefore this timer can be managed in the browser detached from our running Javascript code so that our Javascript code is not blocked and the browser is responsible for managing the timer and the same for HTTP requests where we wait for a response or for getting a user location where we wait for the location, the browser takes care of these tasks, manages them in multiple threads and therefore our Javascript code is not blocked.

* Now the browser however needs a way for it to communicate back to our Javascript code and for that we typically or we often use callback functions.

* The idea here is that for example on setTimeout, we pass a callback function as a first argument if you remember and that callback function is the function the browser should call once the operation is done

* so that the browser can kind of step back into our script and execute something there once it's done so that we can have our script continue to run but then we're also able to do something once this longer taking operation is done.

```js
const button = document.querySelector('button');
const output = document.querySelector('p');

function trackUserHandler() {
  console.log('Clicked!');
}

button.addEventListener('click', trackUserHandler);
```

* If you want to have a look at that, in this example here, I have an event listener and this is basically a similar thing. If we would have an ongoing listener, this would block the entire script execution because we would have to wait for does the user now click, does the user now click, that's not what we want to do, instead when we add an event listener here, we also hand this task of to the browser which manages these listeners behind the scenes so that our script execution can continue but then we have this track user handler function here which we pass as a second argument which effectively is this callback function, the function browser should call once it's done, so in this case not once it's done but once such a click occurs, so that the browser can always step back into our script execution and execute this function once a click occurs.

### Blocking Code & The "Event Loop"

* Refer : event-loop.pdf

* So we have a basic idea of what the browser is doing with longer taking operations, now let's see something interesting.

* Let's add a for loop here, a normal for loop where we start with i set equal to zero but now we have a very high number as an exit condition, let's say 10 million and we increment i by one here every time this runs.

```js
const button = document.querySelector('button');
const output = document.querySelector('p');

function trackUserHandler() {
  console.log('Clicked!');
}

button.addEventListener('click', trackUserHandler);

let result = 0;

for (let i = 0; i < 100000000; i++) {
  result += i;
} 

console.log(result);
```

* and you see, this takes a timer until it prints the result and you learned in the number section that it will have problems with such big numbers, keep in mind that i gets bigger and bigger and we add the bigger and bigger i to the already bigger and bigger number here. Now the interesting thing is here that after reloading, it took a time to print this. Now let's try to click this button before this result is there, what you'll see is that clicked only appears after the result is there as well. So if I reload, I can hammer this button and nothing happens and all the click events are only processed once the loop basically is done.

* Now here, we see the single threading in action. We have this event listener, we handed this off to the browser and therefore this event listener is not blocking Javascript but this loop here, this actually is not handed off to the browser and there is no way of handing this off, this loop executes and Javascript execution is blocked until this operation is done because you can only execute one operation at a time. Now that actually proves the point I showed you on the slide earlier already,

* also interesting however is that this handed off task also is only able to execute so to say or to execute this function once the loop is done. However it makes sense if you think about it, I mentioned there is only one thing you can execute in Javascript at a given time and whilst the loop
is running that is the loop. If we click whilst the loop is running, the browser recognizes that click, sure and it knows it should call this function so it does that but this function is a Javascript function and in Javascript, we're occupied with this for loop. So this function is kind of memorized by Javascript, that it needs to execute that but it only does that once this operation finished

* and that's really important to understand, that's how Javascript works and how it works with async code and with synchronous code and what it actually does is it uses a concept called the event loop.

* So what is the event loop? In the end, the event loop helps us deal with async code you could say, it helps us deal with callback functions which typically are used in such async code scenarios.

* Refer : event-loop.pdf - Now in this code here on the left, you see we're defining two functions,
then here I set a timer, once this timer is done, we call the show alert function which is the second function we define here and after the timer, we also call greet here.

* Now when this code executes, the stack which is part of the Javascript engine as you learned will do certain things. Now also be aware that certain other things will be offloaded to the browser, that the browser kind of gives us a bridge where we can talk to certain browser APIs from inside our Javascript code to for example offload certain work.

* So let's say this code executes now, now what happens is the two functions are created, that's the first thing which happens, the greet and the show alert function

* and then the first function that is called is actually the built-in setTimeout function.

* Now setTimeout does one important thing, it reaches out to the browser because it actually is a browser API made available in Javascript and sets up the ongoing timer there, in the browser so to say, so the browser manages this timer as I mentioned earlier and then in Javascript, this function is done and as you learned, it is really done, it does not block any other code execution. So the timer is still there but that is managed by the browser, Javascript is done for now.

* So the next thing which actually happens is not that show alert executes, keep in mind that this takes 2 seconds, the timer takes 2 seconds, instead Javascript doesn't wait for this as I explained and it instead moves on to the next line, it moves on to the greet method, so now greet executes, before the timer is completed right away after setTimeout is done and the timer is offloaded to the browser, then greet executes in the greet function if you have a look at the left here, we call console log so that's of course also executed and with that we're basically done with the code on the left.

* Now at some point, the timer is completed though and let's say that actually happens whilst our greet console log code executes. Of course this code executes very fast, it does not take it long, it's not such a long if loop as we wrote it but still, it takes a couple of milliseconds even if it's just a few and now let's say whilst we're executing console log, the timer finishes.

* Now we need some way of telling our Javascript code, the Javascript engine so to say, that the show alert function which was register this a callback for the timer should be executed and for this, a message queue is used.

* This is provided by the browser and it's also linked to Javascript so to say. Now in this message queue, the browser registers any code that should execute once we have time for it, in this case the timer. So here, the show alert function, this callback so to say is register this a to-do task once the Javascript engine has time for it,

* so now the timer is done and this task is registered. Please note, show alert does not execute at this point, it's just register this a to-do. The only thing which is really executed in Javascript
at this point is greet and console log because that's what we're currently doing there.

* So now with that, let's say in Javascript we're done with that console log executed and therefore greet also is done and call stack is empty again. Now we need to get that message or this show alert to-do in our call stack, we know it's a function in Javascript, it should now be executed and for this, we use the event loop.

* The event loop, just like the message queue, is built into the browser and most Javascript environments, for example also Node.js have that concept of having such an event loop.

* It's just important to understand that it's not part of the Javascript engine, it's really part of the host environment of Javascript, so of the thing which uses that Javascript engine.

* So the event loop is part of the browser and the job of the event loop in the end is to synchronize the call stack in the engine with our waiting messages. So in the end, what the event loop does is it runs basically all the time and it always sees, is the stack empty and do we have pending to-dos, and if the stack is empty, then the event loop executes so to say and it pushes any waiting messages or any to-do functions therefore into the call stack.

* So the event loop waits until the call stack is empty and once that is the case, it moves our message or the callback function we registered earlier as a function that is now actively executed into our call stack.

* So the message queue is now empty and now this function runs in our Javascript code. So here show alert runs, this calls the built-in alert function here as you can see on the left, once this is done, the call stack is empty again.

* This is what the browser does behind the scenes with the event loop and with our code and also with these callback functions we hand off to the browser APIs and that is a pattern that typically is used for async operations.

### Sync + Async Code - The Execution Order

```js
const button = document.querySelector('button');
const output = document.querySelector('p');

function trackUserHandler() {
  navigator.geolocation.getCurrentPosition( // this function will take some time to locate the position
    posData => {
      console.log(posData); // this code executes second
    },
    error => {
      console.log(error); // this code executes second
    }
  );
  console.log('Getting position...'); // this goes to event loop first this code executes first
}

button.addEventListener('click', trackUserHandler);
```
### Multiple Callbacks & setTimeout(0)

```js
const button = document.querySelector('button');
const output = document.querySelector('p');

function trackUserHandler() {
  navigator.geolocation.getCurrentPosition( // this executes third
    posData => {
    // Now the idea here really just is to show you that you can nest these operations, you can 
    // have an async operation in an async operation but of course this timer is only kicked off 
    // once the location is there, so once this outer callback function executed.
      setTimeout(() => {
        console.log(posData);   // this executes fourth
      }, 2000);

    },
    error => {
      console.log(error);
    }
  );

  // so if you set a timer of zero here, you will still see something interesting
  setTimeout(() => {
    console.log('Timer done!'); // this executes second
  }, 0);
  console.log('Getting position...'); // this executes first
}

button.addEventListener('click', trackUserHandler);
```
* you see actually getting position ran first and then we saw timer done even though I set a timer
of zero and the reason for that is related to what I explained a second ago

* We hand something off to the browser, no matter if it's an event listener, the position fetching or this timer and then for the browser to execute the callback function, it always has to take the route over the message queue and the event loop

* and therefore this code "console.log('Getting position...')" always executes first, this executes right away after set timeout executes and even though the timer immediately finishes, this setTimeout function still only executes after this "console.log('Getting position...')"  line is done

* and this by the way has one important implication, this (setTimeout Zero) is the minimum time after which the callback function will be executed, it's not the guaranteed time, if it were guaranteed, it would need to run immediately. If we had the long running for loop here, then this timer would only execute once the long running for loop is done which might take three seconds.

* So this is not a guaranteed time, no matter if it's zero or 3000 or whatever it is, it's not guaranteed, it's the minimum and the same for set interval. It's the minimum where Javascript and the browser try to run this but they are only able to run this function if the call stack is empty and if something is blocking the call stack, that will go first.

### Getting Started with Promises

* So we learned a lot about async code and that is crucial knowledge you need as a Javascript web developer, no way around that, you need to understand this.

* Now with this out of the way, let's have a look at readability, this code here can be cumbersome to read, right?

* If we have such code where we have callbacks nested into each other,it can be hard to track what's nested in where, which variables can access where, this is really not nice code to look at.Now thankfully Javascript has a solution for that and that is the concept of promises.

* Promises allow us to write cleaner code

* More modern web APIs offered by the browser typically embrace promises and often only support promise syntax but these older functionalities, like setTimeout which have been around for forever almost, well these still use callbacks. Now we can actually wrap them into promise support code if we wanted to and that's what we'll do here to also understand how a promise works internally.

* a promise is in the end an object with the functionality or with the idea of having such a then method which you can call on it.

* promise is the constructor function or the class built into Javascript

* Now the promise here itself takes a function as an argument, it takes a function, the constructor takes a function and this is actually a function the promise API so to say will execute right away when this promise is constructed.

* So when we build the promise here, this function which we pass to the promise constructor is executed right away, it's called from inside the constructor you could say and it's a way for us to configure what this promise should actually do, what it should wrap itself around.

* This function takes two arguments, so this function we pass to the constructor, this function here, takes two arguments and each argument itself actually is a function.

* It's a resolve function, therefore typically called resolve and a reject function, typically called reject but you can name these parameters however you want,it's up to you,

```js
const setTimer = duration => { // duration -> argument
  const promise = new Promise((resolve, reject) => { // promise is the constructor function or the class built into Javascript

  // Now in the function body, as I mentioned this executes right away when the promise is created.

  // In the function body here, you can define what should happen
  
    setTimeout(() => {
      resolve('Done!'); // resolve message Done! // it can be anything string, obj, Array, number
    }, duration);

  });

  // only after creating the promise where the timer is then kicked off, I will return the promise here
  return promise;
};

```
* Only after creating the promise where the timer is then kicked off, I will return the promise here

* Now here inside of setTimeout, I will call resolve,so I'll use this first argument which I said is a function by simply executing it. 

* Now we'll not pass the concrete function here into promise, instead that is built into the browser. 

* The browser executes this function for us when it creates a promise and it passes the resolve and the reject functions into this function here for us.

* So the resolve function is coming from the browser, from the Javascript engine to be precise, that is passing in that resolve function and it's a function which internally will mark this promise object which is created here in the end as resolved, so as done

* Now we can call this setTimer function
```js
function trackUserHandler() {
  navigator.geolocation.getCurrentPosition(
    posData => {
      setTimer(2000).then(data => { // then executes whenever this function resolve
        console.log(data, posData); // here in data we will receive resolve data
      });
    },
    error => {
      console.log(error);
    }
  );
  setTimer(1000).then(() => {
    console.log('Timer done!');
  });
  console.log('Getting position...');
}

button.addEventListener('click', trackUserHandler);
```
* What we did here is sometimes also called promisifying a built-in API, I'm wrapping the set timeout API with my promise logic so that I yes have some extra effort up there, it's a bit more complex code but then I can always use the timer here instead of that callback way here with just my promise way. 

### Chaining Multiple Promises

```js
const button = document.querySelector('button');
const output = document.querySelector('p');

const getPosition = opts => {
  const promise = new Promise((resolve, reject) => {
    navigator.geolocation.getCurrentPosition(
      success => {
        resolve(success);
      },
      error => {},
      opts
    );
  });
  return promise;
};

const setTimer = duration => {
  const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve('Done!');
    }, duration);
  });
  return promise;
};

function trackUserHandler() {
  let positionData;
  getPosition()
    .then(posData => {
      positionData = posData;
      return setTimer(2000);
    })
    .then(data => {
      console.log(data, positionData);
    });
  setTimer(1000).then(() => {
    console.log('Timer done!');
  });
  console.log('Getting position...');
}

button.addEventListener('click', trackUserHandler);
```
### Promise Error Handling

```js
const button = document.querySelector('button');
const output = document.querySelector('p');

const getPosition = (opts) => {
  const promise = new Promise((resolve, reject) => {
    navigator.geolocation.getCurrentPosition(
      (success) => {
        resolve(success);
      },
      (error) => {
        reject(error);
      },
      opts
    );
  });
  return promise;
};

const setTimer = (duration) => {
  const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve('Done!');
    }, duration);
  });
  return promise;
};

function trackUserHandler() {
  let positionData;

  getPosition()
    .then((posData) => {
      positionData = posData;
      return setTimer(2000);
    })
    .catch((err) => {
        // catch rejected   error
      console.log(err);
      return 'on we go...';
    })
    .then((data) => {
      console.log(data, positionData);
    });
  setTimer(1000).then(() => {
    console.log('Timer done!');
  });
  console.log('Getting position...');
}

button.addEventListener('click', trackUserHandler);
```
### Promise States & "finally"

* You learned about the different promise states:

* PENDING => Promise is doing work, neither then() nor catch() executes at this moment

* RESOLVED => Promise is resolved => then() executes

* REJECTED  => Promise was rejected => catch() executes

* When you have another then() block after a catch() or then() block, the promise re-enters PENDING mode (keep in mind: then() and catch() always return a new promise - either not resolving to anything or resolving to what you return inside of then()). Only if there are no more then() blocks left, it enters a new, final mode: SETTLED.

* Once SETTLED, you can use a special block - finally() - to do final cleanup work. finally() is reached no matter if you resolved or rejected before.

* Here's an example:

```js
somePromiseCreatingCode()
    .then(firstResult => {
        return 'done with first promise';
    })
    .catch(err => {
        // would handle any errors thrown before
        // implicitly returns a new promise - just like then()
    })
    .finally(() => {
        // the promise is settled now - finally() will NOT return a new promise!
        // you can do final cleanup work here
    });
```
* You don't have to add a finally() block (indeed we haven't in the lectures).

### Async/ await

* So really great to have promises, it's really a great syntax that makes working with async code so much easier.

* Now nothing wrong with doing it like this, modern Javascript also has an alternative syntax to that which and that's really important to memorize, which is why I'm saying it right away, which still utilizes promises but which actually allows you to omit the then and catch method here and therefore make your code look a bit more like synchronous code,so the normal code you write everywhere without promises than it actually is and that's async await.

* Now what is async await about? You can use async await in functions and only in functions, that's important, you enable it so to say by adding the async keyword in front of your function keyword, for functions created like this, you would use async here in front of your function name, so on the right side of the equal sign, so that's how you do it on expressions, on declarations you add it here in the front of the function.

* and then with async added here, the function internally changes so to say or what happens in there invisibly changes. With async in front of it, this function automatically returns a promise,

```js
const button = document.querySelector('button');
const output = document.querySelector('p');

const getPosition = (opts) => {
  const promise = new Promise((resolve, reject) => {
    navigator.geolocation.getCurrentPosition(
      (success) => {
        resolve(success);
      },
      (error) => {
        reject(error);
      },
      opts
    );
  });
  return promise;
};

const setTimer = (duration) => {
  const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve('Done!');
    }, duration);
  });
  return promise;
};

// By adding async here which is a keyword supported by Javascript, this entire function wraps all its content into one big promise,

async function trackUserHandler() { // async returns a promise
    let posData;
    let timerData;

// The more interesting thing now is that inside of this invisible promise, 
// you have access to another keyword because we added async here, we can use the await keyword.

  // We add that in front of any promise, so for example here get position returns a promise,
    posData = await getPosition();
    timerData = await setTimer(2000);
    console.log(posData, timerData);
}

button.addEventListener('click', trackUserHandler);
```
* Now what does await then do if we add it in front of a promise?

* Well it kind of waits for the promise to resolve or to fail and the next line thereafter will only execute once that is the case.

* So except for the error handling which we temporarily lost,this seems way better but what does it actually do?

* Doesn't it break all the things I taught you about Javascript, that Javascript is non-blocking and so on because here, we clearly are blocking the execution, right?

* Well it looks like we are and that can be the dangerous thing about async await. It looks like we're changing the way Javascript works, that suddenly, we wait for async code to finish and that we block script execution until that is the case but that's actually thankfully not what's happening.

* Instead as I mentioned, async wraps everything inside of trackUserHandler into one big promise and then it actually goes ahead and whenever we await for some other promise which is wrapped in that big promise to resolve, it in the end just returns this as part of that big promise it created for us and adds an invisible then block, in the then block it will then get the result of this promise and store it in this variable which is available in that big overall promise so to say.

* So in the end, it replicates this then block behind the scenes you could say and the same of course for this promise. So it doesn't really block code execution, you could say this code simply gets transformed behind the scenes, it gets transformed to a version which still uses then blocks,
it's the normal promise API, the normal promise object with the normal then method, just transformed behind the scenes, so that we as a developer can write shorter code and the overhead of adding the then blocks and so on is done by Javascript behind the scenes you could say and that's really important to take away. Async await does not change the way Javascript works or executes, it just transforms this code behind the scenes and therefore we just have a look that's different and it looks like we're actually waiting here and we are of course in this function but only in this function because we have a couple of then methods chained after each other here in the end, that's what's happening here

* and therefore we can reap the benefits of having more readable code without any downsides, you just have to be aware of the fact that there is no magic going on and Javascript is not suddenly changing but instead, that we're just having the then stuff going on behind the scenes 

### Async/ await & Error Handling

* So async await can be great to write shorter, more concise code but of course here, we lost error handling, we have no replacement for the catch block at the moment, how do we do that with async await?

* Well, await always moves on to the next line as long as the previous line resolved. Now what if we have an error?

* Well since this looks like normal synchronous code, where we execute line after line and in the end, we are doing that because of the invisible transformation, we can use the normal synchronous error handling we learned about way earlier in the course already, we can use try catch. So here we can try something,

```js
async function trackUserHandler() { 
    let posData;
    let timerData;
    try{
        posData = await getPosition(); // if any of this await fails it will move to catch block
        timerData = await setTimer(2000);// if any of this await fails it will move to catch block
    }catch(error){
        console.log(error);
    }
    console.log(posData, timerData); // this will always run after try catch
}
```
* So this is how you can handle errors correctly in an async await world, instead of catch with try catch and you can of course put whatever you want into try, you just should be aware of the fact that if one of the things failed, the other steps are skipped just as it was the case with catch in the end and on the other hand if both succeed, you of course don't make it into catch but we always execute the line thereafter. 

### Async/ await vs "Raw Promises"

* So async await is a nice alternative to the then and catch blocks and ultimately, it's up to you what you prefer.

* I actually like to use then and catch instead of async await because I really see a danger here with async and await that you could think that these steps really run after each other because Javascript always executes all the code after each other when this actually is not the case, async code and all these async operations are always provided by the browser such that they can be offloaded to the browser as you learned it earlier in the module and the browser comes back to you once it's done. That has not changed and async await does not change this, it just has some invisible transformation behind the scenes, if you understand that, well this obviously is a bit shorter than this down there so you might prefer that, ultimately it's up to you. In modern browsers, this might be a bit better from a performance perspective, async await might be a bit better simply because browsers can also parse and execute this in a more optimized way but it will
not be a huge difference, might not always be a difference for all possible examples or use cases and therefore ultimately, I'd go with whatever you prefer,

* the most important thing here just is that you understand what's happening behind the scenes. One real downside you can have with async await though is that you can't run tasks simultaneously inside
of the same function.

```js
const button = document.querySelector('button');
const output = document.querySelector('p');

const getPosition = opts => {
  const promise = new Promise((resolve, reject) => {
    navigator.geolocation.getCurrentPosition(
      success => {
        resolve(success);
      },
      error => {
        reject(error);
      },
      opts
    );
  });
  return promise;
};

const setTimer = duration => {
  const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve('Done!');
    }, duration);
  });
  return promise;
};

async function trackUserHandler() {
  let positionData;
  let posData;
  let timerData;
  try { // this executes first
    posData = await getPosition();
    timerData = await setTimer(2000);
  } catch (error) {
    console.log(error);
  }
  console.log(timerData, posData);
  // getPosition()
  //   .then(posData => {
  //     positionData = posData;
  //     return setTimer(2000);
  //   })
  //   .catch(err => {
  //     console.log(err);
  //     return 'on we go...';
  //   })
  //   .then(data => {
  //     console.log(data, positionData);
  //   });
  setTimer(1000).then(() => {
    console.log('Timer done!'); // this executes third
  });
  console.log('Getting position...'); // this executes second
}

button.addEventListener('click', trackUserHandler);
```
* So now if I reload and I click block for example, you see getting position and timer done executes thereafter.

* Now why is that happening? Because of await, this does not pause code execution but behind the scenes, all these steps in the end get wrapped into their own then blocks and therefore, this has its own then block as well and therefore this only executes after this is finished.

* So async await is not that great if you have a function in which you need to execute or kick off multiple things simultaneously and don't want to execute everything after each other, then this is not ideal because right now we really got a different behavior than before.

* Of course you can fix this by offloading this into a different function which you kick off simultaneously with this function and so on, so there are ways around that, it's just important to be aware of the fact that everything in this function executes after each other if you start using async await.

* Another downside or thing to be aware of is that async await is only available on functions. So if you had some code outside of a function, let's say here you had some promise based code, you call
set timer here which of course returns a promise as you learned because we promisify this, then you
can't use await here because await can only be used in functions which are marked with async and this is not part of a function, it's part of this global anonymous function which you get automatically but that's not an async function so to say.

* So here you would have to wrap this into for example an IIFE in order to be able to use that,

```js
(async function(){
    await setTimer(1000);
})()
```
* Yes, this would work but of course that's also not that beautiful if we really just want to replace this line here where we otherwise would have to use then. So async await might not always be preferable if you need to introduce a function just to use it, if you have a function anyways where you use a promise in, then using async await of course is not too difficult.

### Promise.all(), Promise.race() etc.

* Now to come to an end, there are a couple of nice methods you have related to promises which sometimes 

* Let's say you have multiple promises which you want to orchestrate in a certain way, which means you want to make sure that they are executed together in a certain way.

* and for example if getPosition fails, set timer will not be executed, it will be skipped.

* Now let me go to the end of this file here and in there, let's say I have the same promises, get position and set timer, one second, and I want to make sure that only one of them executes and that's the one which is faster. Of course for this dummy example, you could argue if it makes sense to either get the user position or to wait for some timer but actually you could say yes it makes sense, I want to wait for one second and if I haven't gotten the position by then, then I want to do something else. Sure you could achieve the same by setting a timeout in the configuration object of get current position but if we didn't have that option or we had some other reasons to do it differently, well then here what we can do,

```js
// race only cared about the fastest promise,
Promise.race([getPosition(), setTimer(1000)]).then(data => {
  console.log(data);
});
```
* Race itself also returns a promise, a promise with the result of the fastest promise you pass to it and you could build a normal then catch chain here based on race, it returns a normal promise, the only special thing is that the data returns will be the result of the fastest promise here.

* They will both be kicked off at the same time and the one which first finishes will then be handled by the subsequent then catch promise chain.

* Now it's worth noting that the other promise which didn't win is not cancelled, its results just ignored but for HTTP requests, it might be important to know that the request is still being sent, its result is just ignored.

* So race can be useful if you're only interested in one of the two conditions and you then want to use the result of that promise and ignore the other one.

* Sometimes you also have code that only should execute after a couple of promises have finished and
of course you can achieve that by chaining multiple then blocks or by using async await,for example here this code only executes after these two promises have finished but an alternative also is that you use promise all.

```js
Promise.all([getPosition(), setTimer(1000)]).then(promiseData => {
  console.log(promiseData);
});
```
* here you also pass in an array of promises and I'll go with the same promises as before and now here, you'll then get a normal promise as a result but the data of that promise will be the combined data of your other promises.

* as you see promise data is an array where you have the data returned by each promise in the position of that promise when you passed it to promise all. So here, we had get position first and set timer second, therefore in the data array we get back here, we have the result of get position as a first argument and the result of set timer as a second argument.

* So this can be useful if you want to wait for all promises to finish before you then want to use the combine data and we could have rewritten this code for example with promise all. Of course you can also do some operation which is not depending on the promise data at all in case you just wanted to wait for a couple of things to finish before you then execute some other code, that's where promise all can shine.

* So if one of the promises fails, is rejected, then the other promise is not executed and we're not waiting for all to resolve or all to reject

* If you want to wait for all resolved or all rejected and you might have such scenarios where you just worry about have all succeeded or have all failed, then you also have promise.all settled.

```js
Promise.allSettled([getPosition(), setTimer(1000)]).then(promiseData => {
  console.log(promiseData);
});
```
* So we get a little bit of a different data, still an array but an object that describes what the promise did and the interesting thing here is that now since you got this detailed description, you got more flexibility.

* It does not cancel the execution of other promises if one of them is rejected, instead it still completes or looks at all of them and then gives you a detailed summary of which promise failed and which promise succeeded so that you can use this knowledge here