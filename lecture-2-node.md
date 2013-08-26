CS558 Technology Lecture: Node.js
=================================
Last time we covered the basics of JavaScript.  Today, we will discuss how to write programs in JavaScript that interact with the operating system and other software components.

It is important to note that JavaScript, just like C or C++, is only a language.  It does not come with any built in notion of how to do things read a file, open a network socket or modify a web page.  Instead, these things must be defined by the run time environment.  There are two common environments where JavaScript is usually executed:

1.  The Document Object Model (or DOM), which is the environment that is used by most web browsers.
2.  The NodeJS environment, which is defined by node and exposes a set of primitives for working with operating systems.

While the DOM is the more traditional of these two, it is also vastly more complicated.  Many of the aspects of the DOM are related to things like reading and modifying the contents of a webpage, and for reacting to user events like mouse clicks or key presses.  By comparison, node.js is far simpler environment, primarily owing to the fact that it attempts to solve a much smaller set of problems.  The main goal of node.js is to provide a minimal set of primitives so that you can write system programs like web servers in JavaScript.

In this class, we will be primarily focused on algorithms and data structures and so it superficially won't matter much which environment we pick.  However, because node.js is much simpler than the DOM, we will prefer to write and test all of our programs within node.js, and then later if we want to use them within a browser we can use the same core components and execute them within the DOM.

# Introduction to node.js
As described earlier, node.js is a low level JavaScript environment for writing systems programs.  Broadly speaking the two main things that it provides are:

1.  A collection asynchronous IO primitives for working with files, sockets and other devices.
2.  A module system for organizing large projects and sharing code.

Both of these facilities are quite useful, but for the purposes of this course we will mostly be focusing on the latter.  However, we will touch on the former point a bit as it is worth knowing about it before moving onward.

## Setting up node.js
Installing node.js is relatively painless.  Just go to

* [http://nodejs.org](http://nodejs.org)

Download the installer:

<img src=lecture2/downloadnode.png>

Run it, and you are good to go!

Once it is done, you should be able to open up a shell and run node.  On windows machines this can be done using [powershell](http://en.wikipedia.org/wiki/Windows_PowerShell) or else you can download and set up [cygwin](http://www.cygwin.com/).  To launch node, open your shell and just type `node`:

<img src=lecture2/runnode.png>

It will open up an interactive JavaScript console, just like the one in Google Chrome that we used last time.  To close out of the repl press Control+C.

## "Hello World" in node.js
You can also use node.js to execute a script.  To get things started, first create a file in the same directory as your shell called "hello.js" which has the following contents:

```javascript
//Contents of hello.js
console.log("hello world")
```

Once that's done, we can execute the file by typing

```sh
> node hello.js
```

Which if you typed all that in correctly should print out,

```
hello world
```

## Basic file IO
Working with disk devices and other IO primitives in node.js is also pretty easy to do.  For example, here is a demo program that reads a text file called "data.txt" and writes an uppercase version of the file called "loud.txt"

```javascript
//Import file system library
var fs = require("fs")

//First read in the file as UTF8 string
fs.readFile("data.txt", function(err, data) {

  //If there was an error, throw an exception
  if(err) {
    throw err
  }
  
  //Otherwise, write the reversed contents
  var reversed = data.toString().split("").reverse().join("")
  fs.writeFile("reversed.txt", reversed, function(err, data) {
    console.log("done")
  })
})

//Print out a message
console.log("reading...")
```

If you run this program it will print out:

```
reading...
done
```

The important thing to note about this is how the result of reading the file is handled.  Instead of `fs.readFile` returning a file immediately, the code issues a read command to the disk and continues.  Once the file is fully loaded from disk, it then triggers an event which fires the callback that was passed as the second argument.  The callback can then take the data object which is a buffer containing the contents of the file and manipulate that.

Though this may seem a bit strange to those who are more familiar with sequential IO operations in languages like C, it has the advantage that the process is not blocked while waiting for the slow disk IO operation to complete.

To learn more about the `fs` object, you can read the [official node.js documentation](http://nodejs.org/api/fs.html).

## Simple HTTP server
The other thing that node.js is known for is that it is relatively easy to write efficient web servers in it.  Part of the reason that it is successful at this task is that most web servers are usually IO bound, and so using asynchronous primitives can greatly improve their scalability.  For example, here is how one might write a simple HTTP server in node.js that logs requests and prints out a default message as a response:

```javascript
//Contents of logserver.js
var http = require("http")

//First create an http server
var server = http.createServer(function(request, response) {

  //Whenever we get a request, log the URL
  console.log("Got request:", request.url)
  
  //Write a plain text response to tell the client we logged the result
  response.setHeader("Content-Type", "text/plain")
  response.end("Your request has been logged")
})

server.listen(8080)
console.log("Created HTTP server, listening on port 8080")
````

Save that as `logserver.js` and to start the server just type into the shell,

```sh
node logserver.js
```

Once that is done, open up your web browser and navigate to [http://localhost:8080/test.html](http://localhost:8080/test.html), and you will get a page that looks like:

<img src=lecture2/serverpage.png>

And in the terminal you should see a log of the request that looks like,

<img src=lecture2/serverlog.png>

Again, to learn more about how HTTP works in node you can [read the documentation](http://nodejs.org/api/http.html).

# CommonJS modules

## Using modules

## Creating modules


# npm

## Installing modules

## Publishing modules

Package.json

npm init

Semantic versioning




# Using CommonJS and npm in the browser



# References

