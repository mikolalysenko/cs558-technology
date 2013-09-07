CS558 Technology Lecture 4: Browsers
=================================================
In this lecture, we are going to discuss the basics of using JavaScript in the browser.  This is not a course in web development, and so this material is in some sense not required, but being able to understand and use the browser can make it much easier to visualize and debug your projects.  It also allows you to make demos that you can publish on the web:

# The DOM
The document object model (or DOM) is the basic API of the web.  When you execute JavaScript within a browser, the DOM is the interface that the browser gives so that you can interact with items on the page.  The specification of the DOM is largely an ongoing process, and many of the details exist for a variety of technical, historical and social reasons.  The overall behavior though is exhaustively documented by the W3C and you can get a complete and precise description of the whole system from the W3C (which is the standards organization that oversees the DOM):

* [The Document Object Model: Overview](http://www.w3.org/DOM/)
* [DOM Technical Reports](http://www.w3.org/DOM/DOMTR)

Obviously, this is a ton of material and I would not expect any of you to read this in full detail, but rather to refer to it as needed when issues come up.  Instead, my goal here is to just give you a quick description of the basics of how to navigate around the contents of a page using JavaScript.  Underneath it all, the DOM is a set of tools for manipulating HTML documents.  HTML is a hierarchical document format, where pages are made of out tags that recursively have child tags and so on.  The nodes of the trees in an HTML document are called `elements`, and each element can have an arbitrary number of child elements and attributes.  For example, here is a typical HTML document:

```html
<html>
  <head>
    <title>Example document</title>
  </head>
  <body>
    <p id="foo">
      This is a paragraph and <a href="http://w3c.org">this is a link.</a>
    </p>
  </body>
</html>
```

## Searching the DOM
The most basic task in dealing with the DOM is searching for specific elements on the page.  You can find individual elements using the [`querySelector` method](https://developer.mozilla.org/en-US/docs/Web/API/document.querySelector) and sets of elements with the variant [`querySelectorAll`](https://developer.mozilla.org/en-US/docs/Web/API/document.querySelectorAll) that returns an array of all matches:

```javascript
element.querySelector("#someID")
element.querySelectorAll(".foo .bar")
```

`querySelector` must be invoked on an element, and searches the children of the element by an inorder traversal looking for the first child element that matches the given pattern or "selector".  A selector is a way to identify a type of element on a page, and the behavior of the selectors are fully enumerated by the W3C in the [official CSS selector reference](http://www.w3.org/TR/CSS2/selector.html).  There are quite a few variations of selectors out there, but the 3 most important variants are as follows:

* Universal selectors (eg. `"h1", "a", "body"`): Which select all elements on a page that match the given selector.
* ID selectors (eg. `"#foo", "#someID"`):  These start with a hashtag (#), and select elements whose id attribute matches.
* Class selectors (eg. `".someclass"`): Which select all the elements having a specific CSS class.

If you don't want to query a specific element, but rather want to search the whole document you can call `querySelector` on the root of the page which is referenced by the global `document` variable.  For example, the following snippet

```javascript
var fooParagraph = document.querySelector("#foo")
```

would return a reference to the `"#foo"` paragraph element in the above document.

## Modifying the DOM
It is also possible to rewrite the contents of the DOM at run time using JavaScript.  The most brute force way to do this is using the `innerHTML` property which literally replaces the HTML describing an element in the document in place.  For example,

```javascript
document.querySelector("#foo").innerHTML = "A new paragraph"
```

Doing this is a bit slow and also destroys the children of whatever element we are updating (breaking any references or listeners that we might have hooked onto the page).  To avoid this, the DOM also exposes several more structured interfaces which give more precise control over the structure of the document.

For example, to create a new element we can use the [`document.createElement` method](https://developer.mozilla.org/en-US/docs/Web/API/document.createElement):

```javascript
var testParagraph = document.createElement("p")
```

createElement takes a string which represents the type of the element to create.  Initially created elements are not part of the document, and have to be inserted using either the [appendChild](https://developer.mozilla.org/en-US/docs/Web/API/Node.appendChild) or the [insertBefore](https://developer.mozilla.org/en-US/docs/Web/API/Node.insertBefore) functions:

```javascript
element.appendChild(testParagraph)            //Appends testParagraph to the end of element's children list
element.insertBefore(testParagraph, child)    //Inserts testParagraph into the list of elements children before `child`.
                                              //If child is not specified, testParagraph is inserted at the beginning
```

Finally, we can also remove a child from an element by calling [removeChild](https://developer.mozilla.org/en-US/docs/Web/API/Node.removeChild):

```javascript
element.removeChild(child)
```

## Events
In addition to manipulating the state of the document, it is also important to be able to respond to inputs and events from the user.  In the DOM, this is handled using asynchronous events/callbacks, much the same way node.js deals with IO operations.  Every element in the DOM has an interface that exposes some number of events that can handled in JavaScript.  The precise nature of these events and what triggers them differs from element to element, but the overall interface is the same.  To handle and event, you use the [addEventListener function](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget.addEventListener):

```javascript
var element = document.querySelector("p")

function clickHandler(event) {
  element.appendChild(document.createTextNode("click!"))
}

element.addEventListener("click", clickHandler)
```

You can also remove an evenet listener from an element if you don't want it anymore using the [removeEventListener function](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget.removeEventListener):

```javascript
element.removeEventListener(clickHandler)
```

Each event gets passed an event object as the first argument which has a number of properties that vary depending on the type of event that is being handled.  For more information, you should look up the official documentation or check out some of the articles on the MDN.

## More details
That pretty much covers the basics of writing JavaScript in the browser, and hopefully you didn't find it terribly complicated.  Where things get tricky is when we start looking at the specific properties and behaviors of the various elements, but fortunately it is pretty easy to find references that explain how all these details work in each specific circumstance.  The Web API is incredibly rich and has an utterly staggering number of features.  Using these things it is possible to create rich and responsive applications, as well as interactive demos.

# Using modules in the browser
By default, JavaScript is executed by the DOM using script tags.  For example, if you want to run some script file in a page, you just link it in using the `<script>` tag:

```html
<html>
  <body>
    <p>This page has a script on it</p>
    <script type="text/javascript" src="myscript.js"></script>
  </body>
</html>
```

Scripts executed in this way do not come with any built in notion of modules, and so writing JavaScript using this style can quickly get a bit awkward.  Fortunately, there are plenty of simple tools out there that make it easy to run scripts using CommonJS modules/npm within a browser.  For the most part, this process is fairly seamless however, modules that use operating system specific functions like file system access obviously will not work within the DOM environment.

Probably the most popular and best supported tool for running modules within a browser is [browserify](https://github.com/substack/node-browserify).  If you already have npm set up, you can easily install browserify using the following command:

```sh
npm install -g browserify
```

Once that's done, you can then compile your JavaScript to run in a browser using the following shell command:

```sh
browserify -d myscript.js > compiled.js
```

Browserify will parse all your code and build a single bundled script that you can include in your page.  For additional performance and size savings, this resulting script can also be minified using tools like uglify.js or jsmin.

The above command needs to be run each time you update your project, which can get a bit tedious if you are developing something.  To speed things up, you should use a live reloading development server, like [beefy](https://github.com/chrisdickinson/beefy) for example.  Again, installing beefy is pretty easy to do using npm:

```sh
npm install -g beefy
```

And then to run your script in a browser, you can just do:

```sh
beefy --open myscript.js
```

Which should pop open a new browser window that runs your script.  For added efficiency, beefy supports live reloading, which means that as soon as you modify your script it will reload the page so you don't even have to refresh anything.

Finally, the last major tool for browser based module development is [requirebin](http://requirebin.com/).  Requirebin lets you use modules from npm directly in your browser without even downloading or installing anything locally.  It can be very useful for quickly sharing and prototyping ideas, though for larger projects you would obviously want to use a separate text editor and build process.

# Introduction to the canvas API



# Further extensions

## GitHub pages

# References
If you ever have any questions about how the DOM works in all the gory details, the definitive reference is the official W3C specification, which you can find here:


Needless to say, it is an
