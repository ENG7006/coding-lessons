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

#### Weeks 6 and/or 7 Dailies
We will start with three substantially separate dailies, but I will ask you to put them (or something like them) together in the following week(s). I am introducing only three dailies now, but will demonstrate the end product you'll be building at the beginning. We'll build this over two or three weeks, depending on folks' progress.

At every step, look deeply into the support code; it's highly commented.

You should create each of these objects in separate files, editing your ```index.html``` to include them. This is because you will want to draw all these files together at some point.

Only ```KeyedUpBall``` will be available right away; other repos will be posted over the course of the days following the coding lesson.

##### ```KeyedUpBall```
Create a sketch in which you can move a ball around a screen using keys on the keyboard. Your ball can be any size or color you want. Beyond that, it must meet the following specs:
1. The ```KeyedUpBall``` can have any (descriptive) name you like, but it *must* be a prototype object.
2. The ball must move left, right, up, and down, as well as diagonal combinations thereof, in response to key presses. These can be any keys you want, but typically, they'd be W, A, S, and D.
3. The ball moves when you hold down the relevant key, and stops when you release it.
4. The ball uses KeyListener to manage its own movement; nothing should be in p5 functions such as ```setup()``` or ```draw()```, or even ```keyPressed()```. (```KeyListener``` is included in the ```KeyedUpBall``` repo.)
5. The ball should not go off the canvas. You should **not** use conditional logic to keep it there. (A hint: peruse the p5.js reference, [especially the Math section](http://p5js.org/reference/#group-Math).)
6. The ball's position should not be stored in InteractiveBall.x and InteractiveBall.y, but InteractiveBall.position.x and InteractiveBall.position.y. (You may, but you do not have to, use a p5.Vector for this.)
7. This is entailed by using KeyListener, but I will make it explicit for you: the ball must have different functions for each direction of movement.

##### ```BouncingBall```s
Create a sketch in which you create an arbitrary number of balls that bounce around the canvas. This should be a familiar problem by now. That said, these have some new and unfamiliar twists:
1. These should be translucent, showing how each ball passes through the other (hint: look at [the p5.js documentation for color](http://p5js.org/reference/#group-Color)).
2. Their positions and directions must be represented by [p5.Vector objects](http://p5js.org/reference/#/p5.Vector).
3. Their direction should be random when they are ```initialize()```ed, but each ball should have the same speed. (A hint: please consult the p5.Vector documentation closely; what functions let you change the direction of a vector?)
4. They should bounce off the edges of the canvas.

##### ```HUD```
A "HUD" is a "head's up display." Create a sketch in which some discrete thing happens over and over again (say, a ball bounces off the edge of the canvas, or the user clicks the mouse, or whatever).
1. Display a counter on the screen, anywhere you like. Each time this thing happens, the counter should increment by 1.
2. Also on the screen, there should be a timer counting up, displaying the time elapsed since the sketch started running. To do this, use the ```Timer``` provided in the ```HUD``` repo.
