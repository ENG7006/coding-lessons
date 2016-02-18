# A Conditional Cheat Sheet

### Test for:
* Equality: x is the same as y: ```x === y```
* Inequality: x is greater than y: ```x > y```
* Inequality:  x is less than y: ```x < y```
* You can combine equalities and inequalities: x is less than or equal to y: ```x <= y```

### Negation:
Negation typically uses an exclamation point (a bang): ```!```
* Not equal: x is not the same as y: ```x !== y```
* Do not use negation and inequalities. It doesn't work. Not greater than is the same as less than or equal to.
* You can negate things that can evaluate to truthy and falsy, including most variables we will be working with:

```javascript
if (variable) {
  //code in block runs only if variable is defined
}
if (!variable) {
  //code in block runs only if the variable is undefined
}
```

### Combining conditions:
To combine conditions, use an AND or an OR operator, ```&&``` and ```||```, respectively.
* AND: x is greater than y AND a is greater than b: ```x > y && a > b```
* OR: x is greater than y OR a is greater than b: ```x > y || a > b```

#### How AND and OR work:
The result of AND is true only if *both* conditions are true:
* ```true && true => true```
* ```true && false => false```
* ```false && true => false```
* ```false && false => false```

The result of OR is true if either one, or both, conditions are true:
* ```true || true => true```
* ```true || false => true```
* ```false || true => true```
* ```false || false => false```

### Conditionals in code:
Conditions show up in code in four situations, in which they *always go in parentheses*:

* In ```if``` statements: ```if (condition) { // code in code block runs once if condition is true }```
* In loops, e.g. in a ```while``` loop: ```while (condition) { // code in code block repeats as long as the condition is true }```
* In the ternary operator: ```(condition) ? first expression runs if true : second expression runs if false```
