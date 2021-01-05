# Javascript Complete

## JavaScript Tooling & Workflows

### Project Limitations & Why We Need Tools

* Refer : helpful-tools-and-why.pdf

* To understand which tools we might need, let's dig into these potential limitations we might face with basic Javascript projects as we use them thus far throughout the course.

* For example we might need to micromanage all our imports if we're not using Javascript modules or if we are using them as you saw and as I just explained in the last lecture, we might have a lot of unnecessary HttpRequests.

* So that's one problem we have, not ideal, the bigger our project grows, the more we have to manage imports or live with all these requests or both
at the same time.

* Another problem we might face at some point which I briefly touched on in the library's section of this course is that our code is not really optimized, it's not as small as it could be.

* Now when we write code, we of course use function names which are pretty clear, we use the variable names which give a clear hint about what's stored in there, we use a lot of extra whitespace to structure our code or put in other words, we're using a lot of features that make the code more readable to us humans but which are not important to the computer.

* If we would strip out all these features and ship as small of a code bundle as possible, we could load our page faster because the computer, the browser has to download less bytes. if we would use shorter function names, then there are less bytes to be downloaded

* if we use very short function names like A, B, C, D, well that would be hard to understand our code so that's something where a tool might be able to help us. 

* Generally, the problem is that we of course want to use all the latest and greatest Javascript features when we write code. Well the problem with that is that not all these latest and greatest features are supported by all browsers.

* So in an ideal world, we can write code that uses all these latest features and we can ship code, which means we can upload code to our web server that actually is a bit older, so that works on more browsers so that our modern code is maybe automatically translated to older code, would be amazing if we had a tool for that.

* We also, when we write code, when we work on our application, constantly need to reload the web page to see our changes in effect, wouldn't it be great if we did not only have a development server but actually a development server that automatically reloads the page whenever the code changes?
That would be amazing, right? It can really speed up your development time, your development flow if that happens, if you don't have to manually press that reload button all the time, so that would be another improvement.

* Having a development server which we already used but one which is a bit smarter and last but not least, something we haven't really done thus far is checking our code quality. Now code quality is a broad term, there are certain conventions which you should follow but there also are a lot of loose rules or a lot of ideas and how you could write your code, there isn't a single right or wrong way of writing code. Still, you might want to be consistent, so it might be interesting if you could configure your code for yourself and then automatically check it if your code follows the rules you set up for yourself to ensure it is properly written and has a good quality.

### Why to use them

* now it's time to solve them and for that, we can use various tools which thankfully were created and do exist. So let me walk you through the different problems we're trying to solve, which tools can help us with that and why exactly we use that tool.

* Now let's start with the development server for that. Now the idea here is that we can serve our application under more realistic circumstances but also as I just outlined, maybe that we can auto reload the page. Now for that thus far we used serve and that did not auto reload the page, now there are multiple alternatives to serve out there but one particularly interesting one is the webpack dev server 

* The idea behind a bundling tool is that it analyzes all our imports and exports which we have when we use Javascript modules and that it then combines these different files into a single bundled file or into a couple of file bundles, so that you don't have these dozens of HttpRequests which need to be sent but that you can write your code, split up across multiple files which makes it easier to use but that that code then before you upload it somewhere is actually merged back into a single file so that you have the better development experience but ship code which requires less HttpRequests and here, the by far, most popular tool is webpack.

* we also typically want to optimize our code when we ship it or when we build it for production, so when we're ready to deploy it onto our real server. During development you typically don't really care about such optimizations, they might even be bad because your code might be harder to debug in the development tools if all the functions were renamed and so on but when you're ready for deploying it, then you want to optimize your code.

* That means you want to shorten function names, remove access whitespace, remove everything which doesn't break your code if you remove it and for that, again webpack fortunately already has a couple of plugins built in which we can utilize to automatically optimize our code once we're ready to deploy it. Now that's the optimization,

* we also might want to write modern code and ship code that also works on older browsers and for that, we need a code compilation tool, also transpilation called. That simply is a tool which takes our code and transpiles it to code that also works on older browsers.Now there, the most popular solution is Babel but we'll not dive into this right now, instead we'll dive into this later in the browser support module because there's a bit more you need to know about browser support and how to manage that before you can set up such a tool.

* Now last but not least, you might want to check your code quality, so make sure that you follow certain patterns and conventions, that you consistently write your code in the same way and for that the most popular tool for Javascript is ESLint which you can also use.

* Refer : helpful-tools-and-why.pdf

###  Workflow Overview

* Refer : setup.pdf

* Before we get started, let's see how we would combine these tools though. Well the idea would be that when we write our code, we want to have two different workflows which we can start.We either have our development workflow which should start whenever we save changes to our code or our production workflow which you typically want to start with a certain command because that should not run whenever you change your code but only once you're ready for it and you explicitly want to start it.

* Now no matter which workflow we're in, we'll always need npm and Node.js.

* So this should already be installed globally, if not make sure you do it, we do it together in this module because Node.js will be used by many of these tools behind the scenes to execute code on your machine and do these various optimizations and npm is the Node Package Manager which typically is used to install these different tools and also to orchestrate them, to start the different workflows and so on.

* Now with that let's see what we have inside of these workflows. Well when we build for development, we want to have linting set up so that we basically can check our code and we always see if we have code quality issues in our code. We then of course also want to bundle our code so that we combine our different files into a couple of bigger files, so that we don't send all these unnecessary HttpRequests and we then want to have a development server which automatically reloads whenever our code did change and that in a nutshell would be what we want to do when we develop and when we changed our code.

* Now during production, we still might want to lint but we're doing this all the time during development so this kind of already should have been done by this point but of course we also want to bundle our code with the help of webpack, we still want to ship a couple of files instead of dozens of files. Now however, we also want to compile our code, to translate it from modern Javascript to Javascript that runs in older browsers as well but again we'll cover this in a separate module and once this is done, we want to optimize the code, something we don't do during development because it can make debugging harder but we definitely want to do it in production. Well and then we have our production ready code which we get as an output and which we can now upload to the server where we need it.

* Refer : setup.pdf

### Setting Up a npm Project

* First step is to installl ESLint in visual studio

* In visual studio , CMD + Shift + P =>>>> enable ESLint

* Install Node js -> in your project terminal type npm init

* This will package.json  If you open that, you should see something like this

```js
{
  "name": "javascript-complete-guide",
  "version": "1.0.0",
  "description": "JavaScript - The Complete Guide",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Maximilian Schwarzmueller",
  "license": "ISC"
}
```
*  With that, this project here can be managed with npm which means we can now install project specific packages with npm

### Working with npm Packages

* Now install eslint package
```
npm install --save-dev eslint
```
* Refer : https://www.npmjs.com/package/eslint

* you now have a dev dependencies entry where you see ESLint and the version number under which it was installed.

```js
"devDependencies": {
  "eslint":"^6.4.0";
}
```
* This symbol here simply means that we're not necessarily focusing only on this specific version but then we would be fine with this version or any version higher than that.

* We also have a package-lock.json file which holds more detailed information about this dependency and all the dependencies of this dependency and you can simply ignore that file, that package-lock.json file.

* Now important, you also now have a node modules folder. This folder holds the dependencies you installed and that's why this folder is quite big,all the dependencies of this dependency and the dependencies of the dependencies of the dependency and so on. So we have a lot of packages in there and in the end here, we also see ESLint, here you can see the code that the ESLint tool uses. It's all Javascript code but important to you, you should never change code in node modules, this is third-party package code, none of your business, we take it as granted and this node modules folder is managed by npm.

* Indeed you can delete it and if you want to share your code, you always should delete it and recreate it with npm install in your project folder. When you run npm install, npm will go into your package.json file, look at your dependencies and development dependencies and install all the dependencies it finds there,

* in this case ESLint and all the dependencies the ESLint package needs and that's also where this version number then is important, npm install will install at least this version of this package because of this version number we specified here.

### Linting with ESLint

* Well for example with our extension. For that, again hit command shift P or a control shift P to open this command panel, type ESLint and choose create ESLint configuration.

* Now you get a couple of options down there in the terminal and you can choose how you want to use ESLint, if you want to check the syntax or if you want to check the syntax and also find problems or if you want to do that and also enforce a specific code style, so you can basically choose how strict you want to be there. Now I'll go for the last option there and now you have to choose if you're using modules and how you're using them and we're using the default Javascript modules import export syntax, we can ignore the other option for now.

* We're also asked if we're using any specific framework and we are not, so we can choose none of these here, if we're using Typescript the answer is no so we can just enter an n here and hit enter. Where our code runs, it runs in the browser so we select this option, hit space and hit enter and then we can also choose if we want to use a popular style guide as a preset, so basically a code style guide defined and developed by other people. If you want to answer a couple of questions about our style to create our own guide or if our file should be inspected so that a style guide is generated. Now I'll try this last option here and I want to have a look at assets/scripts/app.js, scripts app.js. Hit enter,we can then choose which kind of config file should be generated, I want to get a JSON config file and after all of that, you should get such a "eslintrc.json" file.

* Again of course, you can read all about how you can configure it and use that in the official documentation which you find on npmjs and in the resources linked to from that page.

* If we look into this file, we see this is the configuration that was assumed, it basically has a couple of rules set up now and here, the rules part is indeed the most interesting thing. You see there are a lot of rules here, basically a lot of things you can check for.

* Now for example, in there, if we search for quotes, we see that for quotes it accepts single quotes. Now this means that if I go into my app.js file and I would use double quotes here which is not a technical error, it would not like that. Now how can we see that it doesn't like that? Well, simply reload your project, so close Visual Studio Code and reopen it and ESLint should kick in and here it's telling me that strings must use single quote.

* It's also complaining about global this which is not defined, no undefined variables must be used. Now here we of course know that global this is actually a reserveed word which is available. We can click on quick fix for this and actually disable this specific rule for this line, this comment here is then added automatically and it tells ESLint please ignore this line here or ignore this specific rule for this line here to be precise.

* Like thid ESLint share you differenet error you can quick fix option or rectify that error

* You can off sepecific rule also in "eslintrc.json" 

### Configuring ESLint

* ESLint offers a lot of different options so that you can really fine-tune it exactly to your requirements.

* You can set up your own rules from the ground up (basically what we started doing in the lectures) but you can also use presets and pre-configured rulesets.

* To fully understand all options you can configure in .eslintrc.json, this part of the official docs should be helpful: https://eslint.org/docs/user-guide/configuring

* To explore all available rules and what they mean, explore this part of the official docs: https://eslint.org/docs/rules/

* Want to use a preset? Here you go: https://www.npmjs.com/search?q=eslint-config (just click on one of the results and follow the instructions provided there)

* Also check out the docs in general: https://eslint.org/docs/user-guide/getting-started

###  Bundling with Webpack

* So we added linting, let's now have a look at bundling our code, so combine all these individual code files, these individual scripts into one large script so that we can write our code in a distributed way, so that we can write our code like this with multiple code files but that in the end they are merged together so that we don't send all these unnecessary HttpRequests and for that, we'll use a tool called webpack.

* Webpack is a bundling tool, it's also more than that, it helps us orchestrate our entire build workflow as you will see but to get started we'll just use it, with its core task in mind, as a bundler.Now webpack has a really good documentation at webpack.js.org,

* webpack is a complex tool. To get started with it, you don't need to configure much and we'll actually be able to do a lot with it

* Lets install webpack for our project
```
npm install --save-dev webpack webpack-cli
```
* So now we've got a webpack installed, how do we now use it?

* Well to use webpack, we typically create a configuration file where we tell webpack, that webpack tool, what to do. For that I'll add a new file on my top level next to the eslintrc.json file and the package.json file and this is a webpack.config.js

```js
const path = require('path'); // Node.js path package

module.exports = {
  mode: 'development', // by default production
  entry: './src/app.js', // starting point of the application
  output: { // output directory
  // Now keep in mind, this will be our bundled code, this will be one code file in the end which contains all the other merged code files.
    filename: 'app.js',
    path: path.resolve(__dirname, 'assets', 'scripts'),
    publicPath: 'assets/scripts/'
  }
};
```
* Our web server then imports app.js from the assets scripts folder, the problem just is that webpack writes our app.js file such that it looks for this 0.app.js file, in the end not in the same folder as the app.js file is, so not in the scripts folder but on the root level of our server and that's what we can change with public path, we in the end can tell webpack here that our files are stored in a different path.

* This is a file which webpack will use to do its thing, to do its job Now this file under the hood uses or is executed by Node.js.

* The one thing which is new though is how we export something in Node.js world? Instead of using the export keyword to make something available outside of this file, we use module.exports and set this equal to a Javascript object here. Now this is new, we haven't seen that before, this simply is the syntax Node.js uses for exposing this object outside of this file and webpack, the webpack tool we installed, will go ahead and import this object, so this will be our configuration object where we configure webpack.

* Now to use it, let's go to package.json and there you have this script section. This allows you to set up some scripts which you can run through the command line, so here you can simply add a new script

```js
// package.json
{
  "name": "javascript-complete-guide",
  "version": "1.0.0",
  "description": "JavaScript - The Complete Guide",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack" // webpack
  },
  "author": "Maximilian Schwarzmueller",
  "license": "ISC",
  "devDependencies": {
    "eslint": "^6.4.0",
    "webpack": "^4.40.2",
    "webpack-cli": "^3.3.9"
  }
}

```
* webpack will automatically search for such a webpack.config.js file which is why it's important that you name it exactly like this and take this configuration into account.

* Now to run webpack we need to create as

```js
npm run build
```

* So indeed and that's just something you have to know, webpack as an extra utility feature doesn't require you to add the extension to your imports,we needed that when we ran the code like this in the browser but webpack actually just wants to import without any extension and it then looks for files with .js at the end automatically

* so in all our dependency import remove .js

* so it looks like we already got optimization going on and a 1.app.js file.

* Now we get two files as output because I do have that lazy loading as it's also sometimes called in project item, where we only load the tooltip code when we need it and therefore this tooltip code and all the dependencies that only belong to the tooltip are packed into its own bundle, so that this still is only loaded when we need it, instead of upfront in one big bundle, so that we get the best of both worlds. Load some code only when we need it but load the code we need right from the start in one go instead of dozens of individual HttpRequests, that's why we have two bundles.

### Bonus: Multiple Entry Points

* In the example project, we only have one main entry point: app.js.

* In bigger projects - with multiple HTML pages - you might have multiple scripts for the different pages (HTML files) you might be building. Hence you might need more than one entry point because you want to build more than one bundle (i.e. not every HTML page uses the same script).

* This can easily be configured with Webpack:

* Instead of

```js
entry: './src/app.js'
```
* use

```js
entry: {
    welcome: './src/welcome-page/welcome.js',
    about: './src/about-page/about.js',
    // etc.
}
```
* Now Webpack will look up all these entry points and create one bundle per entry point - you can then link to these bundles in your respective HTML files.

* A simple rule that makes sense for most projects is:

* One entry point per HTML file because you typically have one script per HTML file.If you share a script across multiple HTML files or you have a file that does not need any script, you of course can deviate from that rule.

* You can learn more about multiple entry points with these two resources:

* Code Splitting (i.e. generating more than one bundle): https://webpack.js.org/guides/code-splitting/

* Entry Point Configuration: https://webpack.js.org/concepts/#entry

* And in general, check out the official Webpack docs to dive into it in detail: https://webpack.js.org/guides/

### Using webpack-dev-server

* So now we got the webpack basics set up, we're able to create our output here. Now what we don't have is that development server that would automatically reload whenever we change something and that would be nice to have. For that, we have to install an extra package

```
npm install --save-dev webpack-dev-server
```

* we can then use this dev server to serve our application. For that let's go to webpack.config.js file and there, maybe after output, you can add a dev server entry

```js
const path = require('path'); 

module.exports = {
  mode: 'development', 
  entry: './src/app.js', 
  output: { 
    filename: 'app.js',
    path: path.resolve(__dirname, 'assets', 'scripts'),
    publicPath: 'assets/scripts/'
  },
  // devServer: {
  //   contentBase: './'
  // }
};
```
* This tells dev server where your root HTML file can be found and here, that's just ./, it's the same folder as the config file, therefore you actually also can omit this setting here altogether, it will just work because that will be the default anyways.

* So now to use that dev server, we can go to package.json and tweak our script, we got the build script here, now let's also add a build: dev script maybe, you can again name this however you want

```js
{
  "name": "javascript-complete-guide",
  "version": "1.0.0",
  "description": "JavaScript - The Complete Guide",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack",
    "build:dev": "webpack-dev-server" // build:dev - you can name however you want
  },
  "author": "Maximilian Schwarzmueller",
  "license": "ISC",
  "devDependencies": {
    "eslint": "^6.4.0",
    "webpack": "^4.40.2",
    "webpack-cli": "^3.3.9",
    "webpack-dev-server": "^3.8.1"
  }
}
```

* this again will take our webpack config into account and will do all the webpack things but it also launches that development server that's tied to the output.

* So if we now run

```js
npm run build: dev 
```
* it should spin up that development server and still do our build but now this is a process which keeps on running and you should keep this running until you are done for the day,

* As long as it is running, it watches for changes in your entry file or in any file associated with that and it will rebuild whenever you do change something there. You can visit your page on this address which it outputs up there, in my case localhost:8080 

### Generating Sourcemaps

* Now what about debugging? What if I want to debug my code?

* Well if we go to the sources tab and you check out assets scripts, you will find a file which is pretty hard to debug, all the webpack code is in there and is pretty impossible to debug your code there but you should also have a webpack folder here so to say. There you should find a .folder and there in source, you find your original code files, at least mostly original but these are also tweaked by webpack and the app.js file is empty.

* So this is better for debugging but still not ideal. To get a good debugging experience, we need to go back to the webpack.config file and there, add a so-called dev tool.

* Add the dev tool entry here and now you need to parse in a string here or provide a string where you use one of the provided development tools which in the end just means how webpack links your file to the original code. so how well this is from a debugging experience.

* There you got different levels of detail and the better the linking, the slower the build process and so on, the bigger the output. So for development, you want to have good linking for good debugging, for production you of course want to minimize this to have as small of an output as possible and to also speed up the build process.

* So which options do we have here? We can find that in the official docs, Refer : https://webpack.js.org/configuration/devtool/

```js
const path = require('path');

module.exports = {
  mode: 'development',
  entry: './src/app.js',
  output: {
    filename: 'app.js',
    path: path.resolve(__dirname, 'assets', 'scripts'),
    publicPath: 'assets/scripts/'
  },
  devtool: 'cheap-module-eval-source-map' // devtool
  // devServer: {
  //   contentBase: './'
  // }
};
```
* now if we save that, we have to restart our build dev process to take the new configuration file into account and with that done, if you go back to our project and reload there, you should find that in the webpack folder in the .folder now in the source folder, you really find your original code here if you inspect these files and you can also place breakpoints there,we can do everything you learned in the debugging section.

* So now we have a great setup for development and we can now also debug our code and with that, we bundle our code, we have the development server and we can also debug our code and we have a good development experience.

* We also added linting which you can of course also fine tune to your requirements regarding the rules you used there, now what's missing is that we also have a finished production workflow, where we optimize our code and spit out as small of a codebase as possible.

###  Building For Production

* So with the development part out of the way, let's now build for production.

* Now for production simply means that we want to spit out optimized code, that we don't want to have such a detailed source mapping so that finding our original code is harder but that's not the main reason because people will always be able to read our code if they really want to but instead we just want to speed up that process and we want to make sure that the output files are as small as possible.

* Now for that, to make that happen, we need a different config file for production, so I'll add another webpack.config file, however I will name it .config.prod.js and this will be our config file for production.

```js
const path = require('path');

module.exports = {
  mode: 'production', // production mode
  entry: './src/app.js',
  output: {
    filename: 'app.js',
    path: path.resolve(__dirname, 'assets', 'scripts'),
    publicPath: 'assets/scripts/'
  },
  devtool: 'cheap-source-map' // change the dev tool here to a production ready one 
  // devServer: {
  //   contentBase: './'
  // }
};
```
* Now we can add a new script here in package.json

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
    "build:prod": "webpack --config webpack.config.prod.js" // build:prod
  },
  "author": "Maximilian Schwarzmueller",
  "license": "ISC",
  "devDependencies": {
    "eslint": "^6.4.0",
    "webpack": "^4.40.2",
    "webpack-cli": "^3.3.9",
    "webpack-dev-server": "^3.8.1"
  }
}
```

* If you run 

```js
npm run build:prod
```
* it will build our code or our project for production and output that here in the assets scripts folder.

* So now this is what we get there, our optimized code as you can tell. Now let's still test if that works however by running serve now, our other development server

* Now let's still test if that works however by running serve now, our other development server and therefore let's visit localhost:5000 and this is still looking good, looks like the entire application works as it should, now with our production ready code as you can see if you go to the network tab and you inspect one of these files. 

* This is actually reformatted by Chrome to be more readable, the original code looked like this and this certainly is our production code.

* So now we also build for production and therefore we have two workflows now; one with this build dev script which spins up to development server which we use all the time whilst we're writing code and one which we use right before we're ready to deploy our scripts, to use our scripts on our page and we typically run this build: prod command whenever we have a new change, finished a new version, finished and we now want to build that, output it and push it to our servers.

### Final Optimizations

* Now with our two workflows setup, we're generally done but there are two improvements I want to implement here,

* one is our assets scripts folder, it grows and grows and whenever we build something new and files our names slightly different, the old files are kept around and the new files are added. Now this is not ideal, instead I would want to clear my scripts folder with every build and then just add the newly generated files there to get rid of the old files which I really don't need anymore and that's easy to do with webpack,

* we just have to tweak the configuration a little bit, we have to install a new package first of all with

```js
npm install --save-dev clean-webpack-plugin
```
* This installs a new package which we can implement in our webpack config, in our webpack workflow that cleans up the webpack output folder, so the path you're specifying here (webpack.config.js), in our case assets scripts. 

* To use it in a webpack config file, I just have to import it, again with this const syntax, clean plugin could be a name you want to use there and require clean-webpack-plugin.

```js
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
  plugins: [
    new CleanPlugin.CleanWebpackPlugin()
  ]
};
```
* In case you're wondering how I know this, from the official docs, you learned all of that in the official docs and the docs really are the place to learn that.

* Now of course I don't just want to add this here in the normal webpack config file but also in the prod file.

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
  plugins: [
    new CleanPlugin.CleanWebpackPlugin()
  ]
};
```
* If we do that and we repeat npm run build prod, we'll see that our scripts folder now is a bit emptier because all the unused files were deleted which is indeed what I want.

* So that's one nice thing that's good to have, that's one useful improvement, another improvement is the naming of these files.

* I'm generally fine with these names but if you would deploy these files to a server, so if you take your web page or the HTML files and the scripts that belong to them and you move that onto a server and users start visiting your page, then browsers will typically cache Javascript and CSS files which means they store copies of these files and if the file hasn't changed, the next time the user visits a page, they will keep that stored, that cached file.

* Now that is an improvement which often makes sense and you can control it from a server side by setting the right headers on your resources which is a server admin task and not really directly related to Javascript but you can also force browsers to download a new version of a file by simply changing the file name because then it's a new file to the browser and it will therefore for download it and not use any cached version.

* Now for that, we want to generate these file names automatically and generate a new file name whenever we produce a new build   and that's something where webpack also helps us.

* It makes it easy for us to add, not a random but a deterministic but unique element to every file name which changes with every build.

* For that all we have to do is go to our config and there, let's say to our production config because that really only matters for our production output and there in the output config, in the file name, we can add a dynamic element, we can add square brackets here like this

```js
const path = require('path');
const CleanPlugin = require('clean-webpack-plugin');

module.exports = {
  mode: 'production',
  entry: './src/app.js',
  output: {
    filename: '[contenthash].js', // here like this !!!!  it has to be written like this because this is a keyword to 
     //webpack which tells webpack that a hash should be generated here,
    path: path.resolve(__dirname, 'assets', 'scripts'),
    publicPath: 'assets/scripts/'
  },
  devtool: 'cheap-source-map',
  // devServer: {
  //   contentBase: './'
  // }
  plugins: [
    new CleanPlugin.CleanWebpackPlugin()
  ]
};
```
* it has to be written like this because this is a keyword to webpack which tells webpack that a hash should be generated here, a hash that is different whenever a file changed, if a file didn't change, webpack will keep the existing hash.

* If we do that and we save this, you'll see what I mean when we rerun npm run build prod.

* So this is now an output which will change every time we change the underlying files and we rebuild and therefore since since the file name now changes, browsers will redownload these files.

* Of course we now just need to make sure that in our index.html file, we update this import to meet our latest hash here, only for production however because if I go back to dev, then this will not be the case,

```html
<!-- i mean here -->
<script src="assets/scripts/app.js" defer type="module"></script>
```

### Using Third Party Packages with npm & Webpack

* With our enhanced project setup now where we use npm to manage our development dependencies and webpack to bundle our code and the webpack dev server, we now also got a new way of adding third-party packages.

* Let's take lodash again as an example, you saw that in the libraries module, here I will add it again. Now in the libraries module, I showed you that you can download the script or fetch such a CDN link and add all these imports in your index.html file.

* Well now that we actually use npm and bundling, we got a third way of adding lodash or any other third party library basically. 

```
npm install --save lodash
```
* not save dev because it's not a development dependency but actually a dependency which we need as part of our final code which we ship and then the name of the package which is lodash.

* Now if you hit enter, this is getting installed and once this process finished, it will for one be added to the node modules folder and you also see you now have a new dependencies entry with lodash as a dependency in your package.json file.

* You can import everything as let's say underscore to keep that's special variable name lodash uses from lodash.

```js
import * as _ from 'lodash/array'; // Like this !!!!
import { ProjectList } from './App/ProjectList';

// eslint-disable-next-line no-undef
globalThis.DEFAULT_VALUE = 'MAX';

console.log(_.difference([0, 1], [1, 2])); // _.difference !!!!

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
*  With this if you build again, this should work and should include lodash into our bundle.

* Now some libraries and lodash is one of them also offer some improved imports to not import everything because if you just import from lodash, you import the entire lodash package. If you're only interested in some features, you could import from lodash/array for example to only add array functionalities and difference is an array functionality.

* So now this would still work as you can tell but we would actually import less and therefore have a smaller bundle. It always depends on the exact library you're working with, each library has its own import syntax you can use and the official documentation is typically the best way of finding out how you can add this library to your project and how you can import from that library.