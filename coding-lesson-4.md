# Putting things Together
##### (Or, how to start building complexity.)

Today's coding lesson is largely about how to put components together to get the behaviors and outcomes you want, or rather, it's geared towards starting you in that direction. We're going to be using everything we've learned so far, and largely figuring out how to put those things together. We'll learn some new things along the way, but these are largely not conceptual but rather syntactical.

If you're feeling like you need to do some catching up, I once again recommend very highly [Dan Shiffman's introduction to p5.js videos](https://www.youtube.com/playlist?list=PLRqwX-V7Uu6Zy51Q-x9tMWIv9cueOFTFA).

##### So first, a few language lessons...

### Conditionals, revisited
#### ANDs and ORs
We've met some conditions already, for example, in ```ball```:

```javascript
if (this.x < this.size / 2) this.horizontalBounce();
if (this.x > width - this.size / 2) this.horizontalBounce();
```

These two ```if``` statements can, however, be combined into a single line of code:

```javascript
if (this.x < this.size / 2 || this.x > width - this.size / 2) this.horizontalBounce();
```

The ```||``` in the middle means OR. Reading this in English, we'd get: if the x is less than half the size OR the x is greater than the width of the canvas minus half the size, call ```horizontalBounce()```.

Meanwhile, if you want to put an AND in the middle of conditions, you'd use ```&&```.

Be very, very careful concatenating or nesting conditions. They become a total bear to figure out if something goes wrong. Above, prefer the first version (two lines of code, two ```if``` statements) to the second (a single line of code, two conditions in a single ```if``` statement). It's more readable and it's more useful in a stack trace (see below).

#### NOTs
Negation is typically used in two situations: to test for nonequality (not-equal-to, which is different from inequality, which is greater-than-less-than). In Javascript, to test for equality, we use three equals signs: ```===```. Negation, meanwhile, is specified by an exclamation point (or bang): ```!```. Not equals, then is this: ```!==```.

#### ```else```
Finally, there's an additional constituent of an ```if``` statement: ```else```. Whereas ```if``` gives you a code block to run if the condition is truthy, if you follow that ```if``` with an ```else```, you also get a code block to run if the condition is falsy.

Take a simple, but nontrivial example:

```javascript
if (this.x < width - this.radius) {
  // is called if the condition is truthy: if the ball is to the left of the right edge
  this.x += this.speed.x;
} else {
  // is called if the condition is falsy: if the ball is at or beyond the right edge
  this.x = width - this.radius; // sets it exactly at the right edge.
}
```

What this code does is increase ```x``` by ```speed``` as long as ```x``` is less than the ```width``` less the ```radius```. So far, so familiar. But, if this isn't true, and ```x``` is greater than that, this code sets ```x``` to the right side of the ```canvas```, ```width - this.radius```.

This can be incredibly useful! So use it.

But: **I encourage you to keep your conditional logic as simple as possible.** This is because it's a hellish pain in the ass to debug if you get something wrong. It will also take you a while to figure out how to keep it simple.

##### Testing variable definitions
Also, because ```undefined``` is falsy, while most assigned values in variables are truthy (except zeroes and empty strings), you can also use a conditional to discover if a value has been assigned or not. This is incredibly common to specify default values. So if a variable ```foo``` has been defined, as say, ```var foo = "bar";```, then the condition ```(foo)``` will come back truthy. Meanwhile, if, say, you want to do something if ```foo``` hasn't been defined, you can write (for example):

```javascript
var foo;
if (!foo) foo = "default";
```

In this example, ```foo``` has been declared but not defined. Because ```foo``` is ```undefined```, that means it evaluates to falsy. By then negating that falsy, we come up with a truthy, and that runs the code.

That said, in this example, we obviously know whether or not ```foo``` has been defined. Testing variables in this way is much, much more common in functions to test if arguments exist:

```javascript
var foo = function(bar) {
  if (!bar) bar = "default";
  return bar;
};
```

Here we see a ```return``` statement; we'll get to that in a bit.

For now, we see a pattern that's quite common: if you call ```foo()``` without an argument, the local variable ```bar``` will be undefined. If you fail to pass an argument, the function assigns a default value! This can be very useful, indeed.

#### The ternary operator
This pattern is so common that there's a simpler way to do this, called the **ternary operator**. It's called that for old-school coding reasons, but it's super simple. In place of the most recent example above, we could simply write:

```javascript
var foo = function(bar) {
  bar = (bar) ? bar : "default";
  return bar;
};
```

The ternary operator really lies in the question mark. It goes like this: ```(condition) ? "value if true" : "value if false"```. In this case, this ```bar = (bar) ? bar : "default";``` does the following: if ```bar``` evaluates to truthy (it is defined, and not ```false``` or zero or an empty string) it stays ```bar```, making this the equivalent of ```bar = bar;```, which is perfectly good code, if redundant. If, however, ```bar``` is ```undefined``` (or zero or an empty string), this line will instead give us ```bar = "default";```.

This is the same as above, and you need not use the ternary operator if it feels confusing. But it's worth knowing about. In some ways, it's a bit more straightforward than the ```if``` statement, since that question mark is actually like asking javascript a question: condition? yes : no. A trick, though: the condition needs to be in parentheses, like all conditions in JavaScript. (With the unfortunate single exception of the ```for``` loop, which, while in parentheses, also has other bits of gobbledigook on either side.)

I've prepared [a conditionals quick reference](http://github,com/eng7006/blob/master/conditionals.md), for your ease of use.

### ```return``` statements
Let's go back to what a function is: effectively, it is a named block of code, so you can invoke that code whenever you want. Functions have arguments, which are effectively local variables that, rather than being declared using the ```var``` keyword, are assigned values when the function is invoked. For example:

```javascript
var foo = function(bar) {
  console.log(bar);
};

foo("AHOY!!!");
```
That line, ```foo("AHOY!!!");``` is exactly equivalent to the following:

```javascript
var bar = "AHOY!!!";
console.log(bar); // AHOY!!!
```

So functions can take arguments, whose local variable names are listed in the parentheses in the function declaration. Similar, functions can ```return``` values, which means, send a value back out to the line of code where they're invoked. To take a similarly trivial example:

```javascript
var ahoy = function(matey) {
  return "AHOY!!! " + matey;
};

var arrr = function(meMate) {
  return "Arrrrrrr " + meMate;
};

var pirateSalutation = ahoy("Blackbeard");
var pirateFrustration = arrr("Ahab");

console.log(pirateSalutation); // AHOY!!! Blackbeard
console.log(pirateFrustration); // Arrrrrrr Ahab
```

We've already used implicit return statements; objects' constructor functions all end implicitly with ```return this```, sending the newly constructed object out back into the world, so that it can be assigned to a variable, so that it can live on in your JavaScript program.

Examples of useful ```return``` statements make far more sense in context, so here's a simplified version of one of the bits of code we'll be using this week, a ```Timer```.

```javascript
var Timer = function () {
  this.initialize();
};

Timer.prototype = {

  initialize: function() {
    this.init = new Date();
    this.now = new Date();
  },

  restart: function() {
    this.initialize();
  },

  update: function() {
    this.now = new Date();
  },

  millisecondsElapsed: function() {
    this.update();
    return this.now.getTime() - this.init.getTime();
  },

  secondsElapsed: function() {
    return Math.floor(this.millisecondsElapsed() / 1000);
  },

  minutesElapsed: function() {
    return Math.floor(this.secondsElapsed() / 60);
  },

  getPrettyElapsedTime: function() {
    return this.minutesElapsed() + this.getPrettySeconds();
  },

  getPrettySeconds: function() {
    var secondCounter = this.secondsElapsed() % 60;
    if (secondCounter < 10) secondCounter = "0" + secondCounter;
    return ":" + secondCounter;
  }
};
```
This ```Timer``` simply counts up. (Here's an exercise: how might it count down?)

The primary use case for this object is to create a new ```Timer``` when you want the counting to begin, and then ask it to give you the pretty elapsed time:

```javascript
var timer = new Timer();
// wait some duration of time, say 23 seconds
text(timer.getPrettyElapsedTime(), 20, 40); // 0:23
```
This will print the elapsed time in your standard format, 0:03 after three seconds, 1:03 after sixty-three.

Each of these has return values: ```getPrettyElapsedTime()``` ```return```s ```minutesElapsed()``` plus ```getPrettySeconds()```, each of which ```return``` the minutes and seconds elapsed since the ```Timer``` was ```initialize()```ed. (Here's a question: why will ```getPrettyElapsedTime()``` always return a ```string```?)

(Other questions we might have here: What does ```Math.floor()``` do? What does ```Date.getTime()``` do? Why do we have to bother with milliseconds?)

(The real version of ```Timer``` has additional functions—like ```restart()```, ```pause()```, and ```unpause()```—and some internal logic to support them.)

### Dealing With Errors
As the software you write gets more complicated, you're more prone to have errors and not quite know how to figure out what is going on. Three things:

**When troubleshooting, the JavaScript console is your friend:** One of the reasons we're using Chrome is because its debugging tools are much, much more robust than other browsers'. If you're having difficulties, open the JavaScript console.

**When you have an error that stops the program, or nothing seems to be happening:** Look at the console. Are there any errors? If there are, check out the **stack trace**. The stack trace tells you which line of code is the offending one. It will look kind of like this:

```
game.js:17
Uncaught ReferenceError: thisWillMakeAnError is not defined
game.display @ game.js:17
draw @ sketch.js:86
p5.redraw @ p5.js:15945
(anonymous function) @ p5.js:11734
(anonymous function) @ p5.js:11646
p5 @ p5.js:11906
_globalInit @ p5.js:8409
```
This tells you that the line of code in ```game.js``` caused the Uncaught Reference Error, that in particular, you're trying to refer to something that hasn't been defined (in this case, ```thisWillMakeAnError```), and that it's in the ```game.display()``` function in ```sketch.js```, which is called by the ```draw()``` function in ```sketch.js``` on line 86.

I suggest you break things on purpose once in a while to discover how you might figure out what's going wrong. p5.js can be infuriating sometimes, since it doesn't throw errors unless you do something really egregious; usually it just doesn't draw the thing with a mistake. This is not especially useful behavior in the long run, but error handling is actually an interesting and complex topic. (You can see some very basic error handling in the ```listen()``` function in ```KeyListener```, from this "week's" dailies.)

**When things aren't working, but you can't figure out why, and there aren't errors:** Leave yourself messages. You can send messages out to the console that aren't errors by passing strings to ```console.log()```. This is most commonly used to tell yourself the value of a variable. For example, if a shape isn't going where you think it should, you might add lines like the following to your display function:
```javascript
console.log("Drawing ellipse at: " + this.position.x + ", " + this.position.y);
console.log("With direction " + this.direction.x + ", " + this.direction.y);
```
Of course, with ```draw()``` called approximately 60x per second, that's a lot of text to go through in the console, but it can be very helpful information, indeed. Usually you don't have to go through it so closely. (And again, you can see an example of sending messages to the console log in the ```listen()``` function in ```KeyListener```.)

### Design
A great deal of programming isn't about coming up with your own solutions to problems you encounter, since most problems you have aren't novel. It's about using tools developed by other people (in this case, mostly me) and patching them together to solve the problem at hand.

This process is greatly eased by what we call "design." We've met some design principles already: don't repeat yourself; keep your code as readable as possible; try to keep all functions to less than five lines of code; every object we make that interacts with p5 has an ```update()``` and a ```display()``` function. Design specifies ways of doing things that will make your life much, much easier.

Here's a p5.js-specific design principle: try to keep ```setup()``` and ```draw()``` (and any other redefined p5.js functions, like ```mouseClicked()``` or ```keyPressed()```) as dead simple as possible, delegating work to the one or two other objects that they know about.

And here is the more general version of that particular principle, which is super important: try to keep objects from having to know about what goes on inside other objects. Any given object will end up mashing a whole bunch of stuff around, often with many internal functions. Sometimes you can't keep something from having to know about the insides of something else. But it's best to keep your private parts private (the same goes for objects). And if you must show your private parts to somebody else, it's best to be very picky about who sees them.

#### Conventions: ```update()```, ```display()```, and ```initialize()```
One way to do this is to make sure that all your objects conform to a convention (this also helps with collaboration). The convention we will use this semester, which you've already met, is that just about every object that we use will end up having three functions defined on them, which do conventional things: ```update()```, ```display()```, and ```initialize()```.

We've met ```update()``` and ```display()``` before:

* ```display()``` takes care of drawing anything that needs to be drawn
* ```update()``` manages any state transitions between frames (say, adding 1 to x to animate a thing)
* ```initialize()``` is like p5.js's ```setup()``` function, and is typically only called once when the object is created or put to use

Not all of our objects will do this, but many of them will. And it's occasionally okay to go beyond the convention by "exposing" additional functions, or not including functions that don't seem necessary. That said, ```display()```, ```update()```, and ```initialize()``` are so common that it may well make sense to start every object this way:

```javascript
var MyObject = function() {};

MyObject.prototype = {

  initialize: function() {},

  update: function() {},

  display: function() {}

};
```
This way, even if it turns out you don't need, say, ```initialize()```, it's still there, and won't throw an error if you call it by accident.

##### Position and Direction
Another convention it is worth using is representing an object's position and direction using vectors. We've seen this in the past. You can create your own vector objects if you want, but it's convenient to use p5.Vector. Here's a simple example:

```javascript
var Circle = function(x, y, radius) {
  this.position = new p5.Vector(x, y);
  this.radius = radius;
};

Circle.prototype = {

  initialize: function(directionX, directionY) {
    directionX = (directionX) ? directionX : 1; // note these are optional arguments
    directionY = (directionY) ? directionY : -1;
    this.direction = new p5.Vector(directionX, directionY);
  },

  update: function() {
    this.position.add(this.direction);
  },

  display: function() {
    ellipse(this.position.x, this.position.y, this.radius, this.radius);
  }
};
```
This way you always know where an object is and where it's headed by referring to the same things. This is super useful when you're bringing multiple types of objects together, and you won't need to read the documentation/comments/code deeply to know how to get at these fundamental properties of an object.

#### What needs to know about what?
Let's say you're building a game. (Actually, you are building a game.) And the idea is that you have a player piece that moves, and some game pieces that the player is trying to capture, and all of this is orchestrated using p5.js. p5.js should know about the game, the game needs to know about the game pieces, but the pieces actually don't need to know about each other (with one teensie little exception, maybe).

When approaching a design problem like this, you should work it both ways: start building up from the bottom, and then also work it top down as you put your components together into a whole.

Instead of telling you about this process, this week, as an exercise in putting things together, I'm going to cause you to go through the process of building this game, each step is one of your dailies for the next two weeks:

#### Dailies
The dailies are substantially separate to start, but I will ask you to put them (or something like them) together at the end. I am introducing only three dailies now, but will demonstrate the end product you'll be building at the beginning. We'll build this over two or three weeks, depending on folks' progress.

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

The difficulty in this daily lies in figuring out how [p5.Vector](http://p5js.org/reference/#/p5.Vector) works. We will almost certainly go over that in the following lab, but I would like for you to work on this on your own before working with me on it.

Also, if you create all your ```BouncingBalls``` at the same point on the screen, they fan out in this really pretty way.

##### ```HUD```
A "HUD" is a "head's up display." Create a sketch in which some discrete thing happens over and over again (say, a ball bounces off the edge of the canvas, or the user clicks the mouse, or whatever).
1. Display a counter on the screen, anywhere you like. Each time this thing happens, the counter should increment by 1.
2. Also on the screen, there should be a timer counting up, displaying the time elapsed since the sketch started running. To do this, use the ```Timer``` provided in the ```HUD``` repo.
