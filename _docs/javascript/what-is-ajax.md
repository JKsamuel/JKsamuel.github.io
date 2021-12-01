---
title: What is Ajax?
tags: 
 - Javascript
 - Ajax
 - Library
description: I'm currently learning JavaScript at school, but I'm still don't understand the definition exactly, so I tried googling.
---

# Ajax
### Ajax is not a specific technology in itself.

Asynchronous JavaScript and XML, while not a technology in itself, is a term coined in 2005 by Jesse James Garrett, that describes a "new" approach to using a number of existing technologies together, including HTML or XHTML, CSS, JavaScript, DOM, XML, XSLT, and most importantly the XMLHttpRequest object. When these technologies are combined in the Ajax model, web applications are able to make quick, incremental updates to the user interface without reloading the entire browser page. This makes the application faster and more responsive to user actions.

Although X in Ajax stands for XML, JSON is preferred over XML nowadays because of its many advantages such as being a part of JavaScript, thus being lighter in size. Both JSON and XML are used for packaging information in the Ajax model.

**Reference**: [https://developer.mozilla.org/en-US/docs/Web/Guide/AJAX#getting_started](https://developer.mozilla.org/en-US/docs/Web/Guide/AJAX#getting_started)

According to my prof. Andrzej and Sam, there are two APIs for Ajax.<br>
The older XMLHttpRequest API is supported on all browsers, but it’s clunky and ugly.<br>
The new fetch API is much better, but it is not supported on Internet Explorer.

So, I learned useing the fetch API in this course.

Anyway, What is the Ajax?

According to Sam who is professsor Mohawk College, he gave us one of examples, when you start typing in to the Google search box, you immediately get **suggestions** from the Google database about what to search for. When you view a social media feed, it starts you with a small list of items, and as you scroll down, it loads more and more items for an endless news feed. Both of these are examples of Ajax in action. 

  [example]
   ```javascript
   fetch("hello.txt", {credentials: 'include'})
        .then(response => response.text())
        .then(success);
   ```
 <img src="/assets/img/fetchAPI.png" width="400px" height="300px">   

* The second parameter to the fetch call an object literal.<br>
* In JavaScript, an object is just an associative array.<br>
* {credentials: include} -> It forces the fetch method to send authorization headers and cookies when it sends the HTTP Request.
 
  [example the fetch chain for JSON]
  ```javascript
   fetch(url, {credentials: 'include'})
        .then(response => response.json())
        .then(success);
   ```

**Reference:**
+ MDN Web Docs: [MDN - Ajax](https://developer.mozilla.org/en-US/docs/Web/Guide/AJAX)
+ From my school resources
