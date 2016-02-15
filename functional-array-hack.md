# How to Hack a New Array of Arbitrary Length
### And how to use that hack to create 500 ```Bubble```s

Creating an array populated with 500 ```Bubble```s is best accomplished with a loop: it's logically and syntactically straightforward, and very readable indeed. (As you may recall from lab on Thursday.) That said, JavaScript is a functional language, and therefore, it should be possible to get this done using functions rather than loops. And it is! However, it's silly and hacky, and is only of interest as a sort of curio.

That said, it does introduce some useful concepts which we will cover in Coding Lesson 4: return values and the ternary operator.

Here's the code, with comments:

```javascript
/*
 This sketch.js file is for Bubbles, and does not use any loops at all to execute the 500 starting bubbles daily.
 Primarily, this is an object lesson in how to hack the functional capacities of JavaScript.
 The magic is in two functions, newEmptyArrayOfLength() and makeArrayWithRandomBubbles().

 The first uses Array.apply() to call the constructor function of the array and forcing the length property of the array to the value we specify.

 The second uses Array.map() to create a new array from the empty array of arbitrary length returned by newEmptyArrayOfLength() populated by Bubbles with random x and y positions.
*/

var bubbles = [];

var updateAndDisplay = function (bubble) {
  bubble.update();
  bubble.display();
};

var bubbles = []; // bubbles is an empty array of zero length

// newEmptyArrayOfLength returns an array of the specified length, with each element undefined
var newEmptyArrayOfLength = function(arrayLength) {
  return Array.apply(null, {length: arrayLength}); // this is the weird hacky line of code
  // what it does is call the Array constructor, not by using new Array(), but rather using the function.apply() function
  // (we will see function.apply() in our KeyListener object)
  // the really short version is that it creates an Array object with the length property set to the passed argument arrayLength
  // what this looks like in practice is an array of length arrayLength, with each element undefined.
  // the reason we have to do this is that new Array() creates a truly empty array; its elements aren't even undefined, and that means functions like map() or forEach() won't work.
};

// newRandomBubble returns a Bubble with a random x and y, constrained by the width and height of the current canvas
var newRandomBubble = function() {
  return new Bubble(random(0, width), random(0, height));
};

// makeArrayWithRandomBubbles returns an array populated by Bubbles with random positions.
// it does this using both of the functions above, as well as Array.map(), which creates a new array of the same length as array it's called on, populated by putting into the corresponding slot the return value of the callback function
// it also sets a default value to numberOfBubbles: 500. It does this using what's called the ternary operator.
var makeArrayWithRandomBubbles = function(numberOfBubbles) {
  // this is the ternary operator, and sets numberOfBubbles to a default of 500
  numberOfBubbles = (numberOfBubbles) ? numberOfBubbles : 500;
  // here's the Array.map() magic
  return newEmptyArrayOfLength(numberOfBubbles).map(newRandomBubble); // this line calls map() on an empty array, passing newRandomBubble as the callback function
};



setup = function () {
  createCanvas(600, 600);
  // your code goes here
  bubbles = makeArrayWithRandomBubbles(); // bubbles now contains 500 random Bubbles
};

draw = function () {
  background(0);

  if (mouseIsPressed) bubbles.push(new Bubble(mouseX, mouseY));

  bubbles.forEach(updateAndDisplay);
};
```
