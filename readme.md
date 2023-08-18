Hi! Below, you'll find a compilation of valuable JavaScript interview questions designed to be a helpful resource during your interview preparations.

## 1-A Closer Look at the + and - Operators
```js
console.log(1 + '1' - 1);
```

Can you guess the behavior of JavaScript's `+` and `-` operators in situations like the one above?
When JavaScript encounters `1 + '1'`, it processes the expression using the + operator. One interesting property of the `+` operator is that it prefers string concatenation when one of the operands is a string. In our case, `'1'` is a string, so JavaScript implicitly coerces the numeric value `1` into a string. Consequently, `1 + '1'` becomes `'1' + '1'`, resulting in the string `'11'`.
Now, our equation is `'11' - 1`. The behavior of the `-` operator is quite the opposite. It prioritizes numeric subtraction, regardless of the types of operands. When the operands are not of the number type, JavaScript performs implicit coercion to convert them into numbers. In this case, `'11'` is converted to the numeric value `11`, and the expression simplifies to `11 - 1`.
Putting it all together:
```bash
'11' - 1 = 11 - 1 = 10
```
## 2-Object Coercion
```js
const obj = {
  valueOf: () => 42,
  toString: () => 27
};
console.log(obj + '');
```

One interesting aspect is how JavaScript handles the conversion of objects into primitive values, such as strings, numbers, or booleans. This is an interesting question which tests whether you know how coercion works with Objects.

This conversion is crucial when working with objects in scenarios like string concatenation or arithmetic operations. To achieve this, JavaScript relies on two special methods: `valueOf` and `toString`.

### valueOf and toString

The `valueOf` method is a fundamental part of JavaScript's object conversion mechanism. When an object is used in a context that requires a primitive value, JavaScript first looks for the `valueOf` method within the object. In cases where the `valueOf` method is either absent or doesn't return an appropriate primitive value, JavaScript falls back to the `toString` method. This method is responsible for providing a string representation of the object.

### Understanding the Output
Returning to our original code snippet:
```js
const obj = {
  valueOf: () => 42,
  toString: () => 27
};
console.log(obj + '');
```
When we run this code, the object `obj` is converted to a primitive value. In this case, the `valueOf` method returns `42`, which is then implicitly converted to a string due to the concatenation with an empty string. Consequently, the output of the code will be:
```bash
42
```
However, in cases where the valueOf method is either absent or doesn't return an appropriate primitive value, JavaScript falls back to the `toString` method. Let's modify our previous example:
```js
const obj = {
  toString: () => 27
};
console.log(obj + '');
```
Here, we've removed the `valueOf` method, leaving only the `toString` method, which returns the number `27`. In this scenario, JavaScript will resort to the `toString` method for object conversion.

## 3-The Double Equals Operator
```js
console.log([] == ![]);
```

This one is a bit complex. So, what do you think will be the output? Let's evaluate this step by step. Let's first start by seeing the types of both operands:
```js
typeof([]) // "object"
typeof(![]) // "boolean"
```

For `[]` it is an object, which is understandable. As everything in JavaScript is an object including arrays and functions. But how does the operand `![]` has a type of boolean? Let's try to understand this. When you use `!` with a primitive value, the following conversions happen:
- **Falsy Values**: If the original value is a falsy value (such as `false`, `0`, `null`, `undefined`, `NaN`, or an empty string `''`), applying `!` will convert it to `true`.
- **Truthy Values**: If the original value is a truthy value (any value that is not falsy), applying `!` will convert it to `false`.

In our case `[]` is an empty array, which is a truthy value in JavaScript. Since `[]` is truthy, `![]` becomes false. So our expression becomes:

```js
[] == ![]
[] == false
```

Now let's move ahead and understand the `==` operator. Whenever 2 values are compared using `==` operator, JavaScript performs the **Abstract Equality Comparison Algorithm**. The algorithm has the following steps:
![Abstract Equality Comparison Algorithm](/images/abstractEquality.png "Abstract Equality Comparison Algorithm")

As you can see this algorithm takes into account the types of the compared values and performs necessary conversions. 
For our case, let's denote `x` as `[]` and `y` as `![]`. We inspected the types of `x` and `y` and found `x` as an object and `y` as boolean. Since `y` is a boolean and `x` is an object, condition 7 from the abstract equality comparison algorithm is applied:
> If Type(y) is Boolean, return the result of the comparison x == ToNumber(y).

Meaning if one of the types is a boolean, we need to convert it into a number before comparison. What's the value of **ToNumber(y)**? As we saw, `[]` is a truthy value, negating makes it `false`. As a result, **Number(false)** is `0`. 
```js
[] == false
[] == Number(false)
[] == 0
```

Now we have the comparison `[] == 0` and this time condition 8 comes into play:
> If Type(x) is either String or Number and Type(y) is Object, return the result of the comparison x == ToPrimitive(y).

Based on this condition, if one of the operands is an object, we need to convert it into a primitive value. This is where the **ToPrimitive** algorithm comes into the picture. We need to convert `x` which is [] to a primitive value. Arrays are objects in JavaScript. And we saw earlier that when converting objects to primitives, `valueOf` and `toString` methods come into play. In this case, `valueOf` returns the array itself which is not a valid primitive value. As a result, we move to `toString` for an output. Applying the `toString` method to an empty array results in obtaining an empty string, which is a primitive:
```js
[] == 0
[].toString() == 0
"" == 0
```

Converting the empty array to a string gives us an empty string, `"" ` and now we face the comparison: `"" == 0`.
Now that one of the operands is of the type string and the other one is of the type number,  condition 5 holds:
> If Type(x) is String and Type(y) is Number, return the result of the comparison ToNumber(x) == y.

Hence, we need to convert the empty string, `""` to a number, which gives us a `0`.

```js
"" == 0
ToNumber("") == 0
0 == 0
```

Finally, both operands have the same type and condition 1 holds. As both have the same value, the final output is: 
```js
0 == 0 // true
```

## 4-Understanding Object Keys
When working with objects in JavaScript, it's important to grasp how keys are treated and assigned within the context of other objects. Consider the following code snippet and take some time to guess the output:
```js
let a = {};
let b = { key: 'test' };
let c = { key: 'test' };
a[b] = '123';
a[c] = '456';
console.log(a);
```
At first glance, it might seem like this code should produce an object a with two distinct key-value pairs. However, the outcome is quite different due to JavaScript's handling of object keys.
JavaScript employs the default `toString()` method to convert object keys into strings. But why? In JavaScript, object keys are always strings, or they are automatically converted to strings via implicit coercion. When you use any value other than a string (e.g., a number, object, or symbol) as a key in an object, JavaScript will internally convert that value to its string representation before using it as a key.
Consequently, when we use objects `b` and `c` as keys in the object `a`, both are transformed into the same string representation: `[object Object]`. Due to this behavior, the second assignment, `a[b] = '123';` will overwrite the first assignment `a[c] = '456';`. Let's break down the code step by step:
1. `let a = {};`: Initializes an empty object `a`.
2. `let b = { key: 'test' };`: Creates an object `b` with a property `key` having the value `'test'`.
3. `let c = { key: 'test' };`: Defines another object `c` with the same structure as `b`.
4. `a[b] = '123';`: Sets the value `'123'` to the property with key `[object Object]` in object `a`.
5. `a[c] = '456';`: Updates the value to `'456'` for the same property with key `[object Object]` in object `a`, replacing the previous value.

Both assignments utilize the identical key string `[object Object]`. As a result, the second assignment overwrites the value set by the first assignment. When we log the object `a`, we observe the following output:
```bash
{ '[object Object]': '456' }
```

## 5-Closures
This is one of the most famous interview questions asked related to closures:

```js
const arr = [10, 12, 15, 21];
for (var i = 0; i < arr.length; i++) {
  setTimeout(function() {
    console.log('Index: ' + i + ', element: ' + arr[i]);
  }, 3000);
}
```

If you know the output, then well and good. So let's try to understand this snippet. At the face value, it looks that this snippet would give us the output of:

```bash
Index: 0, element: 10
Index: 1, element: 12
Index: 2, element: 15
Index: 3, element: 21
```
But this is not the case over here. Due to the concept of closures and how JavaScript handles variable scope, the actual output will be different. When the `setTimeout` callbacks are executed after the delay of 3000 milliseconds, they will all reference the same variable `i`, which will have a final value of `4` after the loop has completed. As a result, the output of the code will be:

```bash
Index: 4, element: undefined
Index: 4, element: undefined
Index: 4, element: undefined
Index: 4, element: undefined
```

This behavior occurs because the `var` keyword does not have block scope, and the `setTimeout` callbacks capture the reference to the same `i` variable. When the callbacks execute, they all see the final value of `i`, which is `4`, and try to access `arr[4]`, which is `undefined`.

To achieve the desired output, you can use the `let` keyword to create a new scope for each iteration of the loop, ensuring that each callback captures the correct value of `i`:

```js
const arr = [10, 12, 15, 21];
for (let i = 0; i < arr.length; i++) {
  setTimeout(function() {
    console.log('Index: ' + i + ', element: ' + arr[i]);
  }, 3000);
}
```

With this modification, you will get the expected output:

```bash
Index: 0, element: 10
Index: 1, element: 12
Index: 2, element: 15
Index: 3, element: 21
```

Using `let` creates a new binding for `i` in each iteration, ensuring that each callback refers to the correct value.Often, developers have become familiar with the solution involving the `let` keyword. However, interviews can sometimes take a step further and challenge you to solve the problem without using `let`. In such cases, an alternative approach involves creating a closure by immediately invoking a function(IIFE) inside the loop. This way, each function call has its own copy of `i`. Here's how you can do it:

```js
const arr = [10, 12, 15, 21];
for (var i = 0; i < arr.length; i++) {
  (function(index) {
    setTimeout(function() {
      console.log('Index: ' + index + ', element: ' + arr[index]);
    }, 3000);
  })(i);
}
```

In this code, the immediately invoked function `(function(index) { ... })(i);` creates a new scope for each iteration, capturing the current value of `i` and passing it as the `index` parameter. This ensures that each callback function gets its own separate `index` value, preventing the closure-related issue and giving you the expected output:

```bash
Index: 0, element: 10
Index: 1, element: 12
Index: 2, element: 15
Index: 3, element: 21
```
## 6-Duplicating Array Elements

Consider the following JavaScript code and try to find any issues in this code:

```js
function duplicate(array) {
  for (var i = 0; i < array.length; i++) {
    array.push(array[i]);
  }
  return array;
}

const arr = [1, 2, 3];
const newArr = duplicate(arr);
console.log(newArr);
```

In this code snippet we are required to create a new array containing the duplicated elements of the input array. Upon initial inspection, the code appears to aim for the creation of a new array, `newArr`, by duplicating each element from the original array `arr`. However, a critical issue arises within the `duplicate` function itself.

The `duplicate` function uses a loop to go through each item in the given array. But inside the loop, it tries to add a copy of each item at the end into the same array, using the `push()` method. This makes the array longer each time, creating a problem where the loop never stops. The part that checks if the loop should continue `(i < array.length)` always stays true because the array keeps getting bigger. This makes the loop go on forever, causing the program to get stuck.

To address the problem of the infinite loop caused by the growing array length, you can store the initial length of the array in a variable before entering the loop. Then, you can use this initial length as the limit for the loop iteration. This way, the loop will only run for the original elements in the array and won't be affected by the array's growth due to duplicates being added. Here is the modified version of the code:

```js
function duplicate(array) {
  var initialLength = array.length; // Store the initial length
  for (var i = 0; i < initialLength; i++) {
    array.push(array[i]); // Push a duplicate of each element
  }
  return array;
}

const arr = [1, 2, 3];
const newArr = duplicate(arr);
console.log(newArr);
```

By using the `initialLength` variable as the loop limit, it ensures that the loop runs only for the original elements and doesn't get caught in an infinite loop due to the array's expansion.
The output will show the duplicated elements at the end of the array, and the loop won't result in an infinite loop:

```bash
[1, 2, 3, 1, 2, 3]
```

## 4-Scopes
When writing JavaScript code, it's important to understand the concept of scope. Scope refers to the accessibility or visibility of variables within different parts of your code. Before we proceed with the example, if you're not familiar with hoisting and how JavaScript code is executed, you can learn about it from this [link](https://blog.devgenius.io/hoisting-in-javascript-c90f6d03d2df). This will help you understand how JavaScript code works in more detail.
Let's take a closer look at the code snippet:

```js
function foo() {
    console.log(a);
}
  
function bar() {
    var a = 3;
    foo();
}

var a = 5;
bar();
```

The code defines 2 functions foo() and bar() and a variable a with a value of 5. All these declarations happen in the global scope. Inside the bar() function, a variable a is declared and assigned the value 3. So when thebar() function is called, what value of a do you think it will print? 
When the JavaScript engine executes this code, the global variable a is declared and assigned the value 5. Then the bar() function is called. Inside the bar() function, a local variable a is declared and assigned the value 3. This local variable a is distinct from the global variable a. After that the foo() function is called from within the bar() function.

Inside the foo() function, the console.log(a) statement tries to log the value of a. Since there is no local variable a defined within the foo() function's scope, JavaScript looks up the scope chain to find the nearest variable named a. The scope chain refers to all the different scopes that a function has access to when it's trying to find and use variables.

Now, let's address the question of where JavaScript will search for the variable a. Will it look within the scope of the bar function, or will it explore the global scope? As it turns out, JavaScript will search in the global scope, and this behavior is driven by a concept called lexical scope.
Lexical scope essentially refers to the scope of a function or variable at the time it was written in the code. When we defined the foo function, it was given access to both its own local scope and the broader global scope. This characteristic remains consistent no matter where we call the foo function-whether it's inside the bar function, or if we export it to another module and run it there. Lexical scope is not determined where we call the function.
 
The upshot of this is that the output will always be the same: the value of a found in the global scope, which in this case is:

```bash
5
```

However, if we had defined the foo function within the bar function, a different scenario emerges:

```js
function bar() {
  var a = 3;

  function foo() {
    console.log(a);
  }
  
  foo();
}

var a = 5;
bar();
```

In this situation, the lexical scope of foo would encompass three distinct scopes: its own local scope, the scope of the bar function, and the global scope. Lexical scope is determined by where you place your code in the source code during compile time.
When this code runs, foo is situated within the bar function. This arrangement alters the scope dynamics. Now, when foo attempts to access the variable a, it will first search within its own local scope. Since it doesn't find a there, it will broaden its search to the scope of the bar function. Lo and behold, a exists there with the value 3. As a result, console statement would print:

```bash
3
```