#### What is Webpack?

Webpack is a module bundler. It takes the code you write and bundles them. But that's not all that it does. Webpack can transpile, combine and minify your code. It can also do code splitting, where blocks of code are loaded on demand instead of sending one huge file to the client. Webpack is also known to work well with React.js.

Webpack requires that the code be divided into modules. You can use any module system (AMD, CommonJS or ES6 Modules). The examples in this article will use ES6 modules.

#### Why Webpack?

Using a module bundler like Webpack solves a lot of problems. Loading individual javascript files in an application will trigger multiple calls to the server. Processing each of those requests and sending the resources to the client takes time. The bigger the application the longer the time and that's bad UX. By combining the files you drastically reduce the number of requests to the server. Also when the application is built across multiple files, the size of those files also comes into play. Minifying the combined file through a production grade minifier reduces the size of the files you request.

Another problem of loading multiple files is that of dependencies and load order. Resolving them is a huge cognitive load. This is where Webpack comes in. Since Webpack needs the code to be split into modules, its easier to resolve dependencies. It also becomes easier to make sense of which files are dependent on what.

Webpack is not the only solution out there. Task runners like Grunt or Gulp can also get the job done. Since both of these tools are built around a plugin ecosystem, you can pick and choose plugins based on your requirements.

#### Installing Webpack

Webpack is available as a Node package. To install Webpack:

```
$ npm install -g webpack
```

This command will install Webpack globally and is available from the command line through the ```webpack``` command. It's important to note that Webpack works only with Node and not Bower. 

#### CLI

Now that we have access to the ```webpack``` command, create two files: app.js and index.html.

In app.js:

```
console.log("Loading...");

document.write("Well, hello there Webpack!!!!");
```

In index.html:

```
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Getting started with Webpack: Part 1</title>
</head>
<body>
    <script src="bundle.js"></script>
</body>
</html>

```

Now in the command line, type the command:

```
webpack ./app.js bundle.js
```

The first argument in the command is the input file, the file you are giving to Webpack to operate on. The second argument is the output file. When this command is executed you should see a new file called ```bundle.js``` in the directory.

Now, it would be cumbersome if you have to do this everytime you change something in your code. To fix that, Webpack can watch for changes in your file and automatically create the output file. To do this, type the following command:

```
webpack ./app.js bundle.js --watch
```

#### webpack.config.js
##### entry
##### output
##### module loaders
##### plugins