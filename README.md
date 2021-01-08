# Javascript Complete

## JavaScript & Browser Support

### JavaScript & Browser Support

* Refer : what-is-browser-support.pdf

* Refer : js-syntax-vs-browser-apis.pdf

### Determining Browser Support For A JavaScript Feature

* Refer : determining-browser-support.pdf

* Refer : https://kangax.github.io/compat-table/es6/

### Determining Required Support

* Refer : determining-requirements.pdf

### Solution: Feature Detection + Fallback Code

* Refer : feature-detection-and-fallbacks.pdf

```js
const button = document.querySelector('button');
const textParagraph = document.querySelector('p');

button.addEventListener('click', () => {
  const text = textParagraph.textContent;
  if (navigator.clipboard) {
    navigator.clipboard
      .writeText(text)
      .then(result => {
        console.log(result);
      })
      .catch(error => {
        console.log(error);
      });
  } else {
    alert('Feature not available, please copy manually!');
  }
});
```
### Solution: Using Polyfills

* Refer : polyfills.pdf

### Solution: Transpiling Code

* transpilation.pdf

* For some features, you can't use feature detection, you can't use fallback codes you can't use polyfills and you can't ignore them, typically that's the case for core Javascript syntax features like let, const, async await, arrow functions, all of that. 

* These are features you as a developer might want to use because they allow you to write cleaner code, work around certain issues you have with older Javascript code and so on, so you want to use this modern code but you also need to support browsers that might not support these Javascript syntax features.

* If you search for arrow functions here, you see there it's even a bit worse, Internet Explorer for example doesn't support this at all, not even in version 11.

* Now what do you do if you still want to write modern code with all these features and you still want to support let's say Internet Explorer in this example or these older FIrefox or Chrome versions, what can you do in that case?

* You can't use a polyfill because it's a core language feature and if the let keyword is not recognized there is no way of making it work, unlike promises and fetch, these are not functions you call after all, instead const really is just a keyword that tells the engine what the code after it is all about, that it creates a variable that does not change and in the example of const for example and therefore this is a functionality that can't be rebuilt with other tools, it's just missing the const keyword, it's not recognized and therefore browsers that don't know it, that use a Javascript engine that doesn't know it I should say will just crash when they detect this keyword.

* Now to still make that code work or to still ship, so deploy code that does work, you can use transpilation.

* The idea behind transpilation is that you transpile, transform your code, your modern code into older code and this of course is not done manually because if it would be done manually, you could just write older code right from the start, instead you have tools for that, most prominently Babel is a tool that does that for you, it can even be integrated into your webpack workflow so that that all happens in one step.

* This allows you to write modern code, use cutting Edge Javascript language features and still ship, so deploy, cross browser code that for example still uses var instead of const and let or that uses normal functions instead of arrow functions.

* So how would that look like? Well for that, we need to quit our development server and now install that transpiler,

* Refer : https://babeljs.io/docs/en/usage

```js
npm install --save-dev @babel/core @babel/cli @babel/preset-env webpack
```
* this will now install these three packages into your project and we can then tweak the webpack configuration to use them.

* In the end what we got here is Babel loader, which is the integration into webpack which basically does the connection of webpack and the Babel tool,

* we got Babel core which is that Babel tool, so the tool that actually knows how to translate modern code to older code

* Babel preset env is a preset that controls which features are compiled in which way, so actually this is the package with the concrete translation rules you could say.

* So now we have these three packages,now how can we use them? Well just as explained here, you have to add this entry here to your webpack configuration,

```js
// Webpack.config
const path = require('path');
const CleanPlugin = require('clean-webpack-plugin');

module.exports = {
  mode: 'development',
  entry: './src/app.js',
  output: {
    filename: 'app.js',
    path: path.resolve(__dirname, 'assets', 'scripts'),
    publicPath: 'assets/scripts/'
  },
  devtool: 'cheap-module-eval-source-map',
  // devServer: {
  //   contentBase: './'
  // }
  module: { // Babel configuration
    rules: [
      {
        test: /\.m?js$/, // here we're basically saying any file ending with .js or mjs should be treated or should be handled by this rule,
        exclude: /(node_modules)/, // exclude node_modules - so that we don't start transpiling code which is part of third-party packages, these should be ready to use out of the box
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['@babel/preset-env']
          }
        }
      }
    ]
  },
  plugins: [new CleanPlugin.CleanWebpackPlugin()]
};
```
* This adds a module entry which basically gives instructions to webpack how to transform your different modules and a module in webpack world is just a file, so it basically tells webpage what to do with the files it's managing, 

* then you can add a rules entry here and that takes multiple rules because you can control different kinds of files with different so-called loaders.

* A rule is a Javascript object where you have a test property, there you define how the file you want to translate should be identified.

* then we have the concrete action that should be taken with the use key here.

* It's again an object where we configure the loader that should be applied, so that Babel loader should take care about such Javascript files and that as an option, it should use that preset-env ruleset.

* This is now what we're doing here and we also should do that by copying that in the webpack config prod file

* Arguably there it's even more important because this is the code we ultimately want to ship to our users.

* So here under testing, we might test this in modern browsers anyways, though you should still compile it there so that you have the same experience as the users who use your actual code but it's the users who get your production ready code that actually needs that code that works in older browsers as well.

* So now that's a first step, now we're not entirely done though, we need to configure which browsers we want to support because that's the cool thing about @babel/preset-env,

* I said it's a set of rules that controls which features are translated to which older code, for example that let and const are translated to var. Now of course the exact translation you want depends on which browsers you're targeting, this is where we come back to our market analysis

* that the different features modern Javascript offers have different browser support, for example const and let have quite decent browser support so we might not even need to compile them if all we want to target is Internet Explorer 11 and higher and these partial features which are missing are not a problem to us. 

* On the other hand, there might be other features which are also important to us which are not supported in the browser we want to target and therefore they should be compiled.

* So we want to tell Babel and this preset-env package for which browser is the compilation should be fine tuned and thankfully, Babel works exactly like that, it wants this information and then it really gives you code that is fine tuned to the browsers you want to support.

* Now how do we pass that information to Babel however? We can do that with the package.json,

```js
{
  "name": "javascript-complete-guide",
  "version": "1.0.0",
  "description": "JavaScript - The Complete Guide",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack",
    "build:dev": "webpack-dev-server",
    "build:prod": "webpack --config webpack.config.prod.js"
  },
  "author": "Maximilian Schwarzmueller",
  "license": "ISC",
  "browserslist": "> 0.2%, not dead", // browserslist
  "devDependencies": {
    "@babel/core": "^7.6.2",
    "@babel/preset-env": "^7.6.2",
    "babel-loader": "^8.0.6",
    "clean-webpack-plugin": "^3.0.0",
    "eslint": "^6.4.0",
    "webpack": "^4.40.2",
    "webpack-cli": "^3.3.9",
    "webpack-dev-server": "^3.8.1"
  }
}
```
* browser list is a separate package and tool that is used under the hood by @babel/preset-env and attached, you find a detailed description of which options you can set there.

* You can do stuff like set > 2% which means you want to output code that works in browsers that have a market share of greater than 2% and of course every time you build, this is checked and this under the hood taps into a source which is always kept up-to-date so that you build code that works in browsers with greater than 2% market share.

* now with that we can run npm run build prod or just build to get the unoptimized output

* and now in scripts here we see an app.js file with our code and yet now the interesting thing is of course this is the webpack world so it has all this webpack extra code but what we can see if we scroll down a bit 

* Now if I change this to let's say we want to support browsers with a market share greater than 0.2 which of course includes way older and smaller browsers and I now rebuild this, let's check the output again. If we now go down there, we see we get a var in there. So now our code really was translated to code that would work in older browsers as well and this all happens magically with the help of webpack Babel loader and the browsers list

* Refer : https://babeljs.io/docs/en/

* Refer : https://github.com/babel/babel-loader

* Refer : https://babeljs.io/docs/en/babel-preset-env

* Refer : https://github.com/browserslist/browserslist#full-list

* attached you find the link to the documentation of which options you can set there because you can not just target market shares, you can also be specific about different browser versions, so you could say Chrome version 58, Internet Explorer version 11 or higher and so on, so you can also set stuff like that.

* You can also combine multiple queries or multiple instructions, for example you can add not dead here, this means only browsers that still have official support, for example Internet Explorer 10 at the point of time recording this would be considered dead because it's not receiving any official support anymore, Internet Explorer 11 is at least receiving some support still.

* So these are some things you can set up there and this will influence how your code is transpiled and how you can make your modern code which does use constant arrow functions also work in older browsers because const and let is translated to var and an arrow function for example is translated into a normal function.

* Now one problem we still might have in this example here for example is that we still use then, so we still use promises and now we have code that works in older browsers from a Javascript syntax perspective but it still might not really work there because whilst we transpiled core syntax features like let and const, other features like promises might still not be working in these older browsers. So we might want to combine this approach with a polyfill and as it turns out, this is also rather simple.

### Improvement: Automatically Detect + Add Polyfills

* We have this great tool, Babel loader combined with Babel preset-env which takes our browser instructions here and then gives us code that just the works in these old browsers or almost,

* together with feature detection it might work there because we're not even trying to get the clipboard API and therefore execute this promise if the browser doesn't support it but if we had other code that also relies on promises that can't be compiled to older code by Babel, we would have code that works in older browsers from the core syntax perspective but doesn't really work because we still use other features like promises that aren't supported in the older browsers, so we might need to add a polyfill here into our code so that we have the best of both worlds, feature detection, transpiled code and polyfills for features that we need.

* Now of course if we know we worked with promises, we can search for promise polyfill, follow the installation instructions here and add it to our project and that would be absolutely fine but if we have a project that's getting bigger and bigger, it can be cumbersome to manually manage all these polyfills, to find out which polyfills we need, you have to check everything, that's annoying 

* that's annoying because after all the advantage with Babel is that we don't have to think about modern Javascript syntax, we can just write it and rely on Babel to compile it, would be nice if the same would happen for polyfills.

* If Babel sees that we use a promise, it would be nice if it would just include a polyfill for that so that we don't have to do it and the good thing is Babel is capable of doing that. 

* Under the hood, it relies on a package named core-js, if you google for that, you will find that github library and in the end core-js is like a collection of polyfills you could say, it's a huge package with a bunch of built-in polyfills.

```js
https://www.npmjs.com/package/core-js

https://github.com/zloirock/core-js/blob/master/README.md
```
* If you dive into the repository there, you see you've got a bunch of subpackages there, bunch of code in there.

* Now that's nice, you can import core-js and suddenly you have access to all the Javascript features of the world in all browsers so to say. 

* The problem is you could do that, you could import or install core-js with npm for example and just import it at the top of your file but then you would really add everything, even features you don't need.

* So let's do that first and see how we can then improve that,

```js
npm install --save core-js
// not save-dev but save because it will be a third party library that's part of our final code and not just some development tool,
```

* Let's install that and now let's add it at top of our app.js file simply by writing import core-js like this.

```js
import 'core-js'; //core-js

const button = document.querySelector('button');
const textParagraph = document.querySelector('p');

button.addEventListener('click', () => {
  const text = textParagraph.textContent;
  const promise = new Promise();
  console.log(promise);
  if (navigator.clipboard) {
    navigator.clipboard
      .writeText(text)
      .then(result => {
        console.log(result);
      })
      .catch(error => {
        console.log(error);
      });
  } else {
    alert('Feature not available, please copy manually!');
  }
});

```

* This will work, webpack will be able to handle this but we'll have a problem there.

* Let's run npm run build: dev to start our development server again. 

* If we do that and we reload our page, it works but if we go to the network tab, we will see that we have a huge app.js file,it has 2.4mb.

* Now it's also that big because it's not optimized at all and it has a lot of debugging instructions but still that is huge.

* so let's come in and out, save that, it automatically rebuilds and now you see it's just 900kb. It is still pretty big for this small of an app but again that is because of all the development mode overhead code, in production it will be way smaller.

* So already here we see it's only a third of the size as it is when we add core-js, so simply throwing in core-js like this might not be that nice because we bloat our application just to make promises work because it adds so many other features which we aren't even using here, so we want to be a bit more careful.

* Well we can use core-js also in a different way and import just what we need, for example core-js features promise.

```js
import 'core-js/features/promise';
```
* If we tweak our import to just use the features part and there the promise part of the package and we save that, we see if we go back to our application, now it's still bigger than before, bigger than the 900kb without the import but only a bit. So really not that much and far away from the 2.4mb we saw before, so this is better but we still have to manually manage what we use and that's ultimately not what I want to do, Babel should do that for me

* with core-js installed and you need to install it manually, that's important but with it installed, we can now go back to the webpack config here and tell Babel to actually use that polyfill or do that auto polyfilling for us.


* we can now go back to the webpack config here and tell Babel to actually use that polyfill or do that auto polyfilling for us.

* For that here in presets, we need to tweak this and actually wrap this preset in a nested array here,so an array in an array because that allows us to add a second element to that inner array which is a configuration object for this preset.

* Now in this object here, we can configure this preset-env there, there is a use built-ins option.

* Now this option is the option that allows us to control polyfilling behavior, the default is false (useBuiltIns: false) here which means no polyfills are added automatically.

* Now we can set that to usage or to entry (useBuiltIns: 'entry') , with entry you manually need to add polyfill imports, very generic imports like this general core-js import we had there earlier and Babel will then replace it with the actual polyfills you need or you just use usage here and then Babel will add polyfill entries as it detects it,

* so it basically checks which features your code is using and then it will add imports for these feature polyfills automatically.

* So let's try usage here. Now one note, you need one other package which you can install with npm install --save and that'sthe regenerator runtime package here.

* Refer : https://www.npmjs.com/package/regenerator-runtime

```js
npm install --save regenerator-runtime
```
* This is simply another polyfill package handling some other features which core-js does not handle which Babel also will try to utilize if it sees that it needs it. 

* Now with that, we're not entirely there though, at the moment you also need to add a core-js option here, might not be required in the future but right now it is and that in turn takes an object as well where you set the version to three. This basically tells Babel loader which version or Babel preset-env I should say, which version of core-js you're using because there was a bigger change between version two and three and we're using version three here, the latest version which is better and we need to tell Babel preset-env that we're doing that so that it uses this package correctly.

```js
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
                { useBuiltIns: 'usage', corejs: { version: 3 } } // presets corejs config
              ]
            ]
          }
        }
      }
    ]
  },
```
* Now with that, let's run npm run build: dev 

* Now as you see, the size hasn't changed, the reason for that is that Babel checks my app.js file and it doesn't really see that I use a promise here or to be precise, it sees no need to add a polyfill for this promise here and correctly so because the clipboard API won't be available in browsers that don't support promises anyways, so we don't really need to polyfill for promises if the only place where we need a promise is an API that won't work in these old browsers anyways.


```js
const button = document.querySelector('button');
const textParagraph = document.querySelector('p');

button.addEventListener('click', () => {
  const text = textParagraph.textContent;
  const promise = new Promise();
  console.log(promise);
  if (navigator.clipboard) {
    navigator.clipboard
      .writeText(text)
      .then(result => {
        console.log(result);
      })
      .catch(error => {
        console.log(error);
      });
  } else {
    alert('Feature not available, please copy manually!');
  }
});
```
* So to still show you that it works however, let's create our new own promise here, simply a dummy promise with which I do nothing else but output it

* but that it's enough to show Babel and Babel preset-env that I do have a promise it should polyfill.If I now save, indeed you see app.js grows to 1.1mb.

* So it's bigger because this now includes some core-js polyfill, we can also check this response here and search for core, core-js and you will see there is some core-js content in there. So now this is getting included here and we therefore take advantage of Babel and its auto polyfilling feature. 

* Now an alternative configuration you can set here is that you change this to entry (useBuiltIns: 'entry'), if you change this to entry here, then you need to add imports and app.js at the top of the file, you need to import core-js/stable and import regenerator runtime/runtime.

```js
presets: [
  [
    '@babel/preset-env',
    { useBuiltIns: 'entry', corejs: { version: 3 } } 
  ]
]
```

```js
import 'core-js/stable';
import 'regenerator-runtime/runtime';

const button = document.querySelector('button');
const textParagraph = document.querySelector('p');

button.addEventListener('click', () => {
  const text = textParagraph.textContent;
  const promise = new Promise();
  console.log(promise);
  if (navigator.clipboard) {
    navigator.clipboard
      .writeText(text)
      .then(result => {
        console.log(result);
      })
      .catch(error => {
        console.log(error);
      });
  } else {
    alert('Feature not available, please copy manually!');
  }
});

```
* if I reload, we now have a far bigger file because now we include almost all polyfills. The reason for that is that in the end, Babel of course analyzes your code but it does not load polyfills based on the features you're using but actually based on the browsers list you specified. So it loads all polyfills you would need to support browsers that meet this specification.

* Now why would you do that? Well this is a good option if you don't know which exact features you want to use and if your code is not all the code that should be taken into account.

* If you're using a bunch of third-party packages which might be using features older browsers don't support, Babel does not check these third-party packages, so then your browser list might be the best indicator to show which browsers you want to support so that you are better safe than sorry and load all polyfills these third-party packages might need and that's why you might want to go for this entry configuration instead of usage.

* Here however I want to use usage because for our project where we use no third-party packages, this makes more sense. With that of course we can comment out these imports, save that and restart our build process so we're back to the smaller package here.

* So now to conclude this, let's now also copy this setup for the presets here that utilizes our core-js polyfilling into our production code and let's replace this preset here in the array, not the entire array, just the preset with that array so that we have an array in an array to make sure that we now also apply this automatic polyfilling to our production code.
So now if you run npm run build: prod,

```js
const path = require('path');
const CleanPlugin = require('clean-webpack-plugin');

module.exports = {
  mode: 'production',
  entry: './src/app.js',
  output: {
    filename: '[contenthash].js',
    path: path.resolve(__dirname, 'assets', 'scripts'),
    publicPath: 'assets/scripts/'
  },
  devtool: 'cheap-source-map',
  // devServer: {
  //   contentBase: './'
  // }
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
* we basically do the same as in development mode but now we get our production ready code which also uses polyfills our code might require.

### What about Support Outside of Browsers?

* So that was a lot of talking about browser support. Now Javascript is a hosted language, it does not just work in browsers but also in other runtimes like with Node.js.

* How is the situation there? Now with Node.js and all the other environments, the important thing is that you control which version of Javascript you use or which version of Node.js you use which then in turn supports certain features. So you have direct control over the environment where your code will run and that's the huge difference to browser side Javascript, to web application, there you don't have control over that.

* If a feature is available, you can use it in a controlled environment, in an uncontrolled environment, this is simply beyond your control. Therefore we have to build applications with the tools you learned about in this module to make sure 
they work in all the environments for all the users we want to target with our web application.

### Browser Support Outside of JavaScript Files

* Now of course it doesn't stop at our Javascript file though, if we go to index.html, we also have our script import and there here I'm just importing app.js like this because webpack bundles it all together into one big file. If you would be using Javascript modules without webpack, then you might remember that you had to add type module, you can also leave it here when you use webpack but you technically don't need it because you don't really use modules anymore,

```html
<!-- <script src="assets/scripts/app.js" defer type="module"></script> here module not needed -->
```
* webpack bundles it all into one file after all.

* Now if you do add it however because you're maybe not using webpack, you're working with multiple files and modules, then older browsers will not support this type module script and then you can add a script where you add the no module tag just like this, which will be used by older browsers as a fallback. To be precise, older browsers will not understand this script tag and ignore it and they will not understand no module but they'll just ignore this attribute and instead execute the script.

```html
<script src="assets/scripts/app.js" defer type="module"></script>
<script nomodule></script>
```
* Modern browsers on the other hand do understand type module and execute this script and they also do understand the no module attribute and therefore they ignore this script.

* So this script can hold any fallback code that you want to use in older browsers which don't support modules, again if you're bundling all your code together, you don't need that however.

* Another thing that might be interesting is users who disabled Javascript. Now most users these days have it enabled but some might have it disabled.

* Now depending on your web site you're building, on the application you're building, that might mean that your entire app doesn't work properly, imagine rich web applications like Google Docs and so on, they really rely on Javascript and you can't make them work with fallbacks without Javascript.

* In that situations, you might want to show some message to your users though and that can be done with noscript HTML tag 

```js
<body>
   <button>COPY</button>
   <p>Some text you could copy...</p>
   <noscript>
   Please enable javascript to use this page
   </noscript>
</body>
```
* now your users at least know why it's not working. So this is not really a fallback that magically makes your application work but at least it tells your browsers who have Javascript disabled why the page is not working properly.

