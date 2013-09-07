CS558 Technology Lecture 4: Working with browsers
=================================================
In this lecture, we are going to discuss the basics of using JavaScript in the browser.  This is not a course in web development, and so this material is in some sense not required.  However, being able to use and understand the browser can make it much easier to visualize and debug your code.  It also allows you to access 

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
The most basic task in dealing with the DOM is searching for specific elements on the page.  You can find individual elements using the `querySelector` method and sets of elements with the variant `querySelectorAll` that returns an array of all matches:

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

For example, to create a new element we can use the `document.createElement` method:

```javascript
var testParagraph = document.createElement("p")
```

createElement takes a string which represents the type of the element to create.  Initially created elements are not part of the document, and have to be inserted using any of the following methods:

```javascript
element.appendChild(testParagraph)            //Appends testParagraph to the end of element's children list
element.insertBefore(testParagraph, child)    //Inserts testParagraph into the list of elements children before `child`.
                                              //If child is not specified, testParagraph is inserted at the beginning
```

Finally, we can also remove a child from an element by calling:

```javascript
element.removeChild(child)
```

## Events
The above tells

## More details
That pretty much covers the basics of manipulating the DOM, and hopefully you didn't find it terribly complicated.  Where things get tricky is when we start looking at the specific properties and behaviors of the various elements.  The basic Web API is incredibly rich and has an utterly staggering number of features.  Using these things it is possible to create rich and responsive applications that 

# Using modules in the browser



# Introduction to the canvas API

# Further extensions

# References
If you ever have any questions about how the DOM works in all the gory details, the definitive reference is the official W3C specification, which you can find here:


Needless to say, it is an
