# Putting things Together
##### (Or, how to start building complexity.)

Today's coding lesson is largely about how to put components together to get the behaviors and outcomes you want. We're going to be using everything we've learned so far, and largely figuring out how to put those things together. We'll learn some new things along the way, but these are largely not conceptual but rather syntactical.

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

Be very, very careful concatenating or nesting conditions. They become a total bear to figure out if something goes wrong. Above, actually prefer the first version (two lines of code, two ```if``` statements) to the second (a single line of code, two conditions in a single ```if``` statement). It's more readable and it's more useful in a stack trace (see below).

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
