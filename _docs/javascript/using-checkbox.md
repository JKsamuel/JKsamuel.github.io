---
title: Output the selected value in the checkbox
tags: 
 - Javascript
 - js
 - checkbox
description: Output the selected value in the checkbox(3different ways)
---

# Output the selected value in the checkbox
(3 different ways)

## 1. When select a checkbox, print out the selected value.
#### HTML
  ```html
  <input type = "checkbox" name="sports" value="tennis" onclick="getCheckboxValue(event)">Tennis
  <input type = "checkbox" name="sports" value="baseball" onclick="getCheckboxValue(event)">Baseball
  <div id="result"></div>
  ```
#### JavaScript
 ```js
 function getCheckboxValue(event){
     let result = "";
     if(event.target.checked){
         result= event.target.value;
     }else{
         result="";
     }
     document.getElementById("result").innerHTML = result;
 }
 ```
Create checkbox which has name attribute as sports,
When call Event on each checkbox the it calls the function 'getCheckboxValue(event)'

About function 'getCheckboxValue(event)'
when a value of checkbox.checked is true, then it calls checkbox's value and display it.


## 2. When select a checkbox, print out all values.
#### HTML
  ```html
  <input type = "checkbox" name="sports" value="tennis" onclick="getCheckboxValue(event)">Tennis
  <input type = "checkbox" name="sports" value="baseball" onclick="getCheckboxValue(event)">Baseball
  <div id="result"></div>
  ```
#### JavaScript
 ```js
 function getCheckboxValue(event){
     const query = 'input[name="animal"]:checked';
     const selectedEls = document.querySelectorAll(query);
     let result = "";
     selectedEls.forEach((el)=>{
         result += el.value + " ";
     });
     document.getElementById("result").innerHTML = result;
 }
 ```
To get the entire list selected from the entire check box, document.querySelectorAll() function was used.
This function takes the query statement as a parameter.
Returns the NodeList containing all elements that satisfy the query statement.

const query = 'input[name='animal']:checked';
The query statement used in the example above is search for elements with name='animal' and checked='true' among all input elements. Since the value returned by the querySelectorAll() function is an element list, use iterations to access each element and get a value value.


## 3. When button event occured, print out selected value
#### HTML
  ```html
  <input type = "checkbox" name="sports" value="tennis" onclick="getCheckboxValue(event)">Tennis
  <input type = "checkbox" name="sports" value="baseball" onclick="getCheckboxValue(event)">Baseball
  <button onclick="getCheckboxValue(event)">Submit</button>
  <div id="result"></div>
  ```
#### JavaScript
 ```js
 function getCheckboxValue(event){
     const query = 'input[name="animal"]:checked';
     const selectedEls = document.querySelectorAll(query);
     let result = "";
     selectedEls.forEach((el)=>{
         result += el.value + " ";
     });
     document.getElementById("result").innerHTML = result;
 }
 ```

 Nothing special