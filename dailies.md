# Dailies

### Week 1 Dailies
#### Five dailies
* Draw a house
* Draw a car
* Draw a rocket
* Draw an object that belongs to the category of "things that move" (you do not need to animate it)
* Draw an object that belongs to the category of "things that do not move"

### Week 2 Dailies
#### Three dailies
* Refactor one of your shapes from week 1 using variables and functions and, possibly, objects to make it more reusable and extensible. (The ideal here, although it is not *quite* a requirement, is that you have an object that has an x and a y property that moves the object as a single entity.) NB: You may wish to radically simplify your shape before attempting refactoring. *The spec here is this:* at least four of your own variables and one of your own functions. This is the minimum; you will probably want to take this substantially further.
* ```ball``` #1: make the ball bounce horizontally
* ```ball``` #2: make the ball bounce vertically

### Week 3 Dailies
#### Three dailies
* ```ball``` #3: Make the ball bounce in both directions. By this I mean: the ball moves in both directions, horizontal and vertical, and the ball continues its horizontal motion after bouncing off the top or bottom—and likewise, it continues its vertical movement after bouncing off the left or right side.

And then, two of the following three uses/modifications of [movable-shape](https://github.com/eng7006/movable-shape):
* ```movable-shape``` #1: Animate the car so that it "drives" horizontally using ```movable-shape```.
* ```movable-shape``` #2: Animate the rocket so that it "takes off" vertically using ```movable-shape```.
* ```movable-shape``` #3: Animate one of your "things-that-move" from week 1 using ```movable-shape```.

NB: The best way to work with ```movable-shape``` is to fork it, and immediately create your two different branches. Then edit each branch separately, and submit separate pull requests for each branch.

### Weeks 4 & 5 Dailies
There are five dailies related to ```bubbles```:

* Start with a blank canvas, adding a ```Bubble``` wherever the user clicks.
* Start with 500 ```Bubble```s on the canvas. (You may wish to either include, or not, the interactive functionality from the first daily.) **Hint:** This requires a loop, and cannot be done (easily) using a callback (although if you're curious, [here's the hack to do it functionally](https://github.com/eng7006/blob/master/functional-array-hack.md)).
* Instead of collecting at the top, the ```Bubble```s should move upwards continuously. By this I mean, once a ```Bubble``` reaches the top, instead of floating and fizzing there, it should move upwards off the canvas, *and reappear by emerging from the bottom of the canvas*. A variation: instead of a particular ```Bubble``` disappearing at the top and reappearing at the bottom, when a ```Bubble``` disappears at the top, a new, different ```Bubble``` emerges at the bottom at a random x position. Either way, the effect should be a constantly upward-moving field of ```Bubbles```.
* Whatever method you use in ```draw()``` to iterate across the ```Bubble```s in the first daily (or wherever you perform the iteration), write the same code using the alternative method. If you used ```forEach()``` and a callback, use a loop (either ```for``` or ```while```), whichever you prefer. Or, if you used a loop, write the iteration using ```forEach()```.
* Finally, create a sketch that animates a lot of somethings that are neither ```Bubbles``` nor ```Balls```. Keep these simple!

On this last daily project, a few suggestions:
* You'll want to create a constructor function and a prototype object. Feel free to adapt the code from either ```bubbles``` or ```oo-balls```.
* This prototype object should have the same basic structure as a ```Bubble``` or a ```Ball```: a relatively simple object with a position (x and y) and speed (x and y).
* It should also likely contain, according to convention, both ```update()``` and ```display()``` functions, as all our objects in p5 tend to.
* Beyond that, these can be whatever you'd like.
