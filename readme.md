Hi! Below, you'll find a compilation of valuable JavaScript interview questions designed to be a helpful resource during your interview preparations.

## A Closer Look at the + and - Operators
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