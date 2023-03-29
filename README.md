# TKOM 23L
Student: **Mikalai Stelmakh**

Language used: **Python**
## Functionality

### Specifications
Main specifications of the language:
- **Typing**: weak, dynamic
- **Mutability**: mutable by default
- **Passing the arguments**: The arguments to the function are passed by its value, so the function only gets the copy of the object.

### Data types
There will be only a few data types:
- **Booleans.** There are two boolean values: `True` and `False`.
- **Numbers.** The language will have only two kinds of numbers: `int` and `float`.
- **Strings.** The strings are enclosed in double quotes. Escape character (`\`) allows to insert special characters in strings.
- **Nil.** Represents the absence of a value.

### Expressions

#### Arithmetic
There are four operators:

|      name      | sign |      type     |
|----------------|------|---------------|
| Addition       |   +  |  Infix        |
| Substraction   |   -  |  Infix/Prefix |
| Miltiplication |   *  |  Infix        |
| Division       |   /  |  Infix        |

All of these operators can work on numbers. Addition can also be used on strings to concatenate them.

Because of the fact, that the language is weakly typed, some operations can be also performed on different operand types by coercing them to common type supported by the operator.

See [examples](#arithmetic-1).

#### Comparison

There are some operators that always return a Boolean result.

|          name         | sign |
|-----------------------|------|
| Less than             |  <   |
| Less than or equal    |  <=  |
| Greater than          |  >   |
| Greater than or equal |  >=  |
| Equal                 |  ==  |
| Not equal             |  !=  |

The comparison rules can be roughly summarized as follows:
1. If the operands have the same type, they are compared as follows:
    - Number: The values are compared.
    - String: The values are compared character by character, using the Unicode value of each character.
    - Boolean: The boolean is converted to a number. true is converted to 1, and false is converted to 0. Then they are compared as numbers.

2. If operator is `==` and one of the operands is `nil`, the other must also be `nil` to return true. Otherwise return false.

3. Because the language is weakly typed, when comparing values of different types, the interpreter will attempt to coerce the values to a common type using the type coersion rules:
    - If one of the operands is `nil`, convert `nil` to 0. Then compare two operands again.
    - If one of the operands is a Boolean, convert the boolean to a number: true is converted to 1, and false is converted to 0. Then compare two operands again.
    - Number to string: the string is converted to a number if possible. If the string cannot be converted to a number, the comparison will always return false.

See [examples](#comparison-1).

#### Logical operators

There are three logical operators used:
- **!** - the NOT operator, returns `false` if its operand is `true`, and vice versa.
- **and** - determines if two values are both `true`. It doesn't evaluate the right operand if the left one is `false`.
- **or** - determines if either of two values (or both) are `true`. It doesn't evaluate the right operand if the left one is `true`.

See [examples](#comparison-1).

#### Grouping

To change the order of precedence in arithmetic expressions, the parentheses `()` can be used to group sub-expressions. The expressions inside parentheses are evaluated first before the rest of the expression, regardless of the usual operator precedence.

### Statements

The language uses a semicolon (`;`) to mark the end of the statement.

To pack a series of statements, the **block** (`{}`) syntax is used.

### Variables

The keyword `var` is used to declare a variable. The variable without a value defaults to `nil`.

Because the variables are mutable by default, their value can be changed. To declare a constant variable, that can not be modified, use the keyword `const`.

Because of the dynamic typing, the data types of variables are determined at runtime, rather than at compile time. It means that there is no need to specify the data type of a variable explicitly when declaring it, and its data type can be changed by assigning a value of a different type to it.

Variable can be redefined.

See [examples](#declaration).

#### Variable Scopes

Variable from the outer scope can be modified inside the block, but if the variable is declared inside the block, it is not visible outside of it.

See [examples](#scopes).

### Comments

Comment starts with a double-slash (`//`) symbol and ends with the new line.

See [example](#comments-1).

### Control flow

There are two ways of controlling the flow of the program:

**if** - executes statement or block of statements based on the condition. See [example](#if).

**while** - executes statement or block of statements while the condition evaluates to `true`. See [example](#while).

The scope of the variables described [here](#variable-scopes).

### Functions

The function declaration looks like this:
```Rust
fn getSum(a, b) {
    return a + b;
}
```

Where `fn` is the keyword to declare a function.

The code after the `return` statement is not executed. If the function doesn't have a `return` statement, it returns `nil`.

To call a function use the following syntax:
```javascript
someFunction(5, 5);

// Or without arguments
someFunction();
```

Function can be passed to another function, as well as it can be declared in another function. See [example](#closure).

Functions and variables are stored in the same namespace, so it is not possible to declare a function and a variable with the same name within the same scope and use them both. The same goes for redefining a function. The latest definition overrides previous ones. See [example](#overriding).

#### Function Scopes

Scopes does work [the same way](#variable-scopes) they do in blocks, but here we have one more case to consider - when the variable is passed as the argument to the function. See [example](#scopes-1).

## Code examples

### Hello world
```javascript
print("hello world");
```

### Arithmetic

```javascript
// Basic arithmetics
8 / 4 * 1 + 1               = 3
8 / 4 * (1 + 1)             = 4
8 / (4 * (1 + 1))           = 1
15.5 / 7.75                 = 2.0
2.36 - 5 * -1               = 7.36
```

### Type coercion
```javascript
// On Booleans
true + true                 = 2     // true is coerced to 1
false + false               = 0     // false if coerced to 0
true + false = false + true = 1

// On Numbers
4 + 6                       = 10
"5" + 3                     = "53"
7 + "2"                     = "72"

// Operator "-" is used only for numerical substraction
7 - "2" = "7" - 2           = 5     // string is coerced to number
10 - ""                     = 10    // empty string is coerced to 0
10 - "foo"                          // error

// On Strings
"hello" + "world"           = "helloworld"
"The answer is: " + 12      = "The answer is: 12"
"The answer is " + true     = "The answer is true"
```

### Comparison

```javascript
5 == "5";                           // true
5 != 10;                            // true
5 < 10;                             // true
10 > "5";                           // true
5 <= 5;                             // true
10 >= 10;                           // true
(5 == 5) and (10 == 10);            // true
(5 == 5) or (10 == 5);              // true
!(5 == 10);                         // true
1 == "1" == true                    // true
1 < 1 == true                       // false
true == 2 > 1                       // true
(2 >= 1) and (1 >= 0)               // true
2 >= 1 >= 0                         // error (not grammatically correct)
```

### Variables

#### Declaration

```javascript
var a = "string";
var b;                              // nil

const a = 5;
a = 6                               // error

var variable = "one";
print(variable)                     // "one"
var variable = 2;
print(variable)                     // 2
```

#### Scopes

```javascript
var a = 1;
var b = 2;

{
    var a = 5;
    print(a);           // 5

    b = 3;

    var c = 10;
}

print(a);               // 1
print(b);               // 3
print(c);               // Error
```

### Comments

```javascript
// Comment starts with a double-slash (`//`) symbol and ends with the new line.

var number = -5;
var maxNumber = 10;
var minNumber = 0;

// Limit the number
if (number > maxNumber) {
    number = maxNumber;     // If greater than the high limit, set to max value
}
if (number < minNumber>) {
    number = minNumber;     // If less than the low limit, set to min value
}
```

### If

```javascript
var a;

if (someCondition or !otherCondition) a = true;
else a = false;

if (someCondition == false and otherCondition) {
    a = true;
    print(a);
} else {
    a = false;
    print(a);
}
```

### While

```javascript
var a = 0;

while (a < 10) a = a + 1;

while (a < 20) {
    a = a + 1;
    print(a)
}

```

### Functions

#### Declaration

```Rust
fn getSum(a, b) {
    return a + b;
}
```

#### Closure

```Rust
fn getSum(a, b) {
    var outside = a + b;

    fn inner() {
        return outside + 1
    }

    return inner();
}

fn wrapper(a) {
    return a;
}

wrapper(getSum)(5, 3)   // returns 9
```

#### Recursion

```javascript
fn fib(n) {
    if (n <= 1) return n;
    return fib(n - 2) + fib(n - 1);
}

var i = 0;
while (i < 20) {
    print(fib(i));
}
```

#### Overriding

```javascript
fn name() {
    print("John");
}

name();                 // "John"

fn name() {
    print("Jane");
}

name();                 // "Jane"

// or: name = "Doe"
var name = "Doe";
print(name);            // "Doe"
```

#### Scopes

```javascript
var a = 0;
var b = 0;

fn add(value) {
    a = 1;
    value = 1;
    var b = 10;
    print(b);               // 10
    fn inner() {
        print(b);           // 10
        var b = 20;
        print(b);           // 20
    }
    print(b);               // 10
    c = 1;                  // Error (c not declared)
}

fn main() {
    var c = 0;
    var value = 0;
    add(value);
    print(a);               // 1 (the value was modified inside the function)
    print(b);               // 0 (b was declared in the local scope of the function, so it didn't change)
    print(value);           // 0 (arguments are passed by value, not by reference)
}
```

## Grammar

The grammar is in [this](./grammar.ebnf) file.

## Error handling

If the error was found while scanning the source - the lexer reports an error, but it doesn't stop, it keeps scanning.

If the error was found while parsing - the parser reports an error and uses a technique called *error recovery* to try and continue parsing the code.

The error message structure looks like this:
```
<Error Type>: <Error message>

    <line number> | <place where error occured>
```

For example:
```
ZeroDivisionError: division by zero

    15 | var number = 15 / 0;
```

## How to run

To run the interpreter use following syntax:
```sh
python3 interpreter.py [filepath]
```

Where `interpreter.py` is a path to the interpreter and `filepath` is a path to the file with the source code.

If `filepath` is not given, the interpreter will be run interactively.

## Implementation

#### Error handling

#### Streams

#### Lexer

#### Parser

Example tree:
```
```

#### Interpreter

## Testing