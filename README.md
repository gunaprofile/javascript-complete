# Javascript Complete

## Introduction to Testing

### Testing Setup

* Refer : test1

* we need some tools to help us with creating these tests. What do we need? We typically need three kinds of tools -

* we need a test runner which is simply a tool that executes our tests and summarizes the results and gives us some output on the results, we can use something like mocha there, that is a popular test runner.

* We also have assertion libraries and there we define our tested logics, our expected outcomes, so these assertion libraries give us tools to define expectations, to define comparisons, conditions we
want to check as part of our tests. There Chai is common or a popular assertion library, though what
we will use is a very popular library both for running tests and asserting and that is jest

* jest is relatively new or at least it was rewritten not that long ago and it's a very popular library for writing tests because it is both a test runner and an assertion library and it's really powerful and a lot of fun to work with.

* For end-to-end, we also sometimes need a headless browser, so basically a browser where we don't have to click manually but where we can use the browser API, the DOM and so on without a user interface necessarily because we'll analyze it in code anyways and there we can use puppeteer which is a very popular solution for that. We need that headless browser mostly for end-to-end testing whereas the test runner and the assertion library are also needed for that but also then are the only things we need for unit and integration tests.

### Writing & Running Unit Tests

* This is our js file..."util.js"

```js
const generateText = (name, age) => {
  // Returns output text
  return `${name} (${age} years old)`;
};

exports.createElement = (type, text, className) => {
  // Creates a new HTML element and returns it
  const newElement = document.createElement(type);
  newElement.classList.add(className);
  newElement.textContent = text;
  return newElement;
};

const validateInput = (text, notEmpty, isNumber) => {
  // Validate user input with two pre-defined rules
  if (!text) {
    return false;
  }
  if (notEmpty && text.trim().length === 0) {
    return false;
  }
  if (isNumber && +text === NaN) {
    return false;
  }
  return true;
};

exports.checkAndGenerate = (name, age) => {
  if (!validateInput(name, true, false) || !validateInput(age, false, true)) {
    return false;
  }
  return generateText(name, age);
};

exports.generateText = generateText;
exports.validateInput = validateInput;
```
* This is the test file "util.test.js"
```js
const { generateText, checkAndGenerate } = require('./util'); // Import generateText and checkAndGenerate functions

test('should output name and age', () => {
  const text = generateText('Max', 29);
  expect(text).toBe('Max (29 years old)');

  // const text = generateText('Max', 29);   we can write multiple expectations like this..
  // expect(text).toBe('Max (29 years old)');
});

```
* To run this we need to add test script to our package.json

```json
{
  "name": "js-testing-introduction",
  "version": "1.0.0",
  "description": "An introduction to JS testing",
  "main": "app.js",
  "scripts": {
    "start": "webpack app.js --mode development --watch",
    "test": "jest --watch" // here
  },
  "keywords": [
    "js",
    "javascript",
    "testing",
    "jest",
    "unit",
    "tests",
    "integration",
    "tests",
    "e2e",
    "tests"
  ],
  "author": "Maximilian Schwarzmüller/ Academind GmbH",
  "license": "ISC",
  "devDependencies": {
    "jest": "^23.6.0",
    "webpack": "^4.20.2",
    "webpack-cli": "^3.1.2"
  }
}
```

* Now in our terminal just run 'npm test'

### Writing & Running Integration Tests

* So let's now write an integration test 

* which uses a lot of other functions which has a lot of dependencies. So testing this function would be an integration test but this is a function that does neither take an input nor return an output, instead it's a function that really added something, that DOM, so this is something which you might want to test in an end-to-end testing scenario or in a user interface test but an integration test would be kind of hard here, we would have to do a lot of manual DOM interaction inside of our test which you can do but which you might not want to do.

* Instead there is something else I can test and that is basically a part of that function you could say. In the end what I'm doing in add user is I do select a couple of elements, then I do validate the input and then I generate a text based on the input. Now we could outsource this into a separate function and that is a part of writing modular code that I mentioned earlier, you are forced to write modular code which also makes your code easier to manage and reuse though.

```js
const { generateText, checkAndGenerate } = require('./util'); // Import generateText and checkAndGenerate functions

test('should output name and age', () => {
  const text = generateText('Max', 29);
  expect(text).toBe('Max (29 years old)');

  // const text = generateText('Max', 29);   we can write multiple expectations like this..
  // expect(text).toBe('Max (29 years old)');
});

test('should generate a valid text output', () => {
  const text = checkAndGenerate('Max', 29);
  expect(text).toBe('Max (29 years old)')
});
```
### Writing & Running e2e Tests

* Now that we had a look at unit tests, what they are, why we use them and integration tests, let's have a look at end-to-end tests or user interface tests.

* For this, I'll need to install another tool.

```js
npm install --save-dev puppeteer
```
* puppeteer, which is a headless version of Chrome, of the Chrome browser. So it basically is a browser we can use to interact with the DOM and so on,

* we can even run in a version with the head, so where we see the browser and we can basically define steps that should be executed in that browser so that we can automate certain processes on our web page and then of course test the result of these processes as well.

* Let's start using puppeteer now and for that, I'll go to my utiltest.js file and I will import puppeteer.

```js
const puppeteer = require('puppeteer');

test('should create an element with text and correct class', async () => {
  
  const browser = await puppeteer.launch({
    headless: true, // actually run a browser with a user interface as well
    // slowMo: 80, // basically slow down the automated operations so that we have a chance of seeing what's happening
    // args: ['--window-size=1920,1080'] // this will help us to test responsive features as well
  });

  // puppeteer returns us a callback

  const page = await browser.newPage();

  await page.goto(
    'file:///Users/mschwarzmueller/development/teaching/youtube/js-testing/index.html'
  );
  await page.click('input#name');
  await page.type('input#name', 'Anna');
  await page.click('input#age');
  await page.type('input#age', '28');
  await page.click('#btnAddUser');
  const finalText = await page.$eval('.user-item', el => el.textContent);
  expect(finalText).toBe('Anna (28 years old)');
}, 10000);

```

### Dealing with Async Code

* your tests will also trigger that code which might send a HttpRequest or which might do something which makes the test take longer, might hit a backend API or might towards results. So now we'll have a look at what we can do to make our test simpler and focus on the core things and really only test what matters to us.

```js
// test case loadTitle calls API
const { loadTitle } = require('./util');

test('should print an uppercase text', () => {
  loadTitle().then(title => {
    expect(title).toBe('DELECTUS AUT AUTEM');
  });
});

```
* we still have a problem here, the problem is we're still hitting our API. So here in util.js, I'm using fetch data and if you remember, we're defining that in the HTTP file, so here and there we're making a real API request. We typically don't want to do that in testing, we might exceed certain API quotas if we do that, if we're testing post requests, we might even edit something on the API which we certainly don't want to do as part of testing, certainly not with our production API at least.

* So hitting the API is typically not something we want to do, there also are other reasons against it, what would we achieve by really making that HttpRequest? Do you want to test if your API works correctly? If you're building that API, you should do these tests during the API development, not when working on your frontend, these two should be separated,  so that is certainly not something you want to test.

* If the API works or not is not something you test in your frontend code, you might test error fallbacks or anything like that but not if the API works correctly. You also don't want to test if the get method exposed by axios works correctly That is a method provided by a third-party package and we rely on that package doing its job, so we also don't test third-party package functionalities.

* We want to test our own code and our own code here for example is that we extract data and that here, that we transform this to uppercase and that we pull out the title.

### Working with Mocks

* Now to focus on that, we do something which is called mocking, we mock features which means we replace features we don't want a test with some dummy implementations and how could this look here?

* Now jest gives us a great way of preventing us from using that fetch data method. We can create a new folder called __mocks, all lowercase,Now here, we can add a http.js file, so the same file name as the file that has the function we want to replace and now in http.js, we can now do some magic to set up some functions that will replace our real functions when running the tests. 

* Inside the __mocks folder create the same filename which we are calling loadTitle
```js
//https.js
const axios = require('axios');

const fetchData = () => {
  console.log('Fetching data...');
  return axios
    .get('https://jsonplaceholder.typicode.com/todos/1')
    .then(response => {
      return response.data;
    });
};

exports.fetchData = fetchData;

```
* Now lets write our mock http file
```js
//__mocks/https.js
const fetchData = () => {
  return Promise.resolve({ title: 'delectus aut autem' });
};

exports.fetchData = fetchData;

```
* Now we can go to utiltest.js and in there, we want to add jest mock at the beginning and mock the http.js file like this,

```js
jest.mock('./http'); // here we added mock http

const { loadTitle } = require('./util');

test('should print an uppercase text', () => {
  loadTitle().then(title => {
    expect(title).toBe('DELECTUS AUT AUTEM');
  });
});

```
* Now if I run npm test, you see it also took close to a second because it had to do some magic behind the scenes but you don't see that console log statement because what now happens is when this test gets executed, when this testing file gets executed, jest goes ahead and replaces the HTTP file with our mocked file, so all the functions that are export that here are then replacing the functions we are normally exporting in the real HTTP file for our tests only.

* So this is how we can test async code or how we can replace code, how we can replace functionalities if we don't want to in this case hit the API, the same would be the case for let's say code that accesses the filesystem, you don't want to do that, instead you want to mock such functionalities.

* You can by the way also mock global packages, like axios itself. 

```js
const get = url => {
  return Promise.resolve({ data: { title: 'delectus aut autem' } });
};

exports.get = get;
```
* with that just comment http mock file reference. so axios.js this module should be used automatically, we don't have to instruct jest to do that 

```js
// jest.mock('./http');  no need to add for axios

const { loadTitle } = require('./util');

test('should print an uppercase text', () => {
  loadTitle().then(title => {
    expect(title).toBe('DELECTUS AUT AUTEM');
  });
});

```

* Refer : https://jestjs.io/docs/en/getting-started

* Refer : https://www.harrymt.com/blog/2018/04/11/stubs-spies-and-mocks-in-js.html