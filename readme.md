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

## 2-The Double Equals Operator
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

Based on this condition, if one of the operands is an object, we need to convert it into a primitive value. This is where the ToPrimitive algorithm comes into the picture. We need to convert `x` which is `[]` to a primitive value. In this case, the **ToPrimitive(x)** is converting the array to a string using `toString()`:
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
