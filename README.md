# cmonkey
This is an implementation of the monkey language in C.
Monkey is a programming language created by Thorsten Ball in the book
[Writing an interpreter in go](https://interpreterbook.com/).

[**EDIT**]
Now this also includes a bytecode compiled version of monkey based on the
second book by the author [Writing a compiler in go](https://compilerbook.com).

## BUILDING
Requires GNU make to build the code.
Tested mostly on Linux and NetBSD, bug reports welcome for other
operating systems.

To build:
- clone or download the code from github
- run `make`
- Binaries are generated in bin/

## TESTS
Tests are implemented in files ending with \_tests.c. No frameworks are used to write tests. Tests
are built with the normal build and can be executed by running each of the test programs one by one.
For example:

`bin/parser_tests` - will execute the parser tests

## Running as REPL
execute `bin/monkey`

![demo](https://github.com/abhinav-upadhyay/cmonkey/blob/master/repl.png)

**Multiline expressions in the REPL**

It's straightforward to run single line code in the REPL. For more
complex code which requires multiple lines, `\` can be used at the
end of the line to indicate continuation so that the REPL waits for
you to finish the code. The last line not containing `\` at the end
will be treated as the end of the code after which the evaluation will be performed.

## Running monkey programs
To execute a monkey program saved in a file `hello_world.mnk`

`bin/monkey hello_world.mnk`

## Language Features

### Supported data types
Monkey supports following datatypes natively:
- integers
- strings
- booleans
- arrays
- dictionary or hash
- functions (functions are first class citiznes in monkey)

### Variable bindings

```
let x = 10;
let str = "hello, world!";
let bool = x == 10;
```

### Arithmetic operators
Monkey supports following arithmetic operators which are similar to other programming languages:

- \+ (addition) 
- \- (subtraction)
- \* (multiplication)
- \/ (division)
- % (mod)

### Comparision operators
Monkey supports following comparison operators
- == (equals)
- != (not equals)
- \> (greater than)
- < (less than)

### Logical operators
Monkey supports following logical operators
- && (and)
- || (or)
- ! (not)

### Creating string literals
```
let s = "hello world";
```
We can also do indexing on string objects, for example:
```
>> let s1 = "hello world";
>> let c0 = s1[0]
>> c0
h
>> s1[2]
l
```

### Creating array literals
Monkey arrays can contains objects of any type supported by monkey

```
let add = fn(a, b) { a + b};
let arr = [1, "two", "3", true, false, add]
```

We can access arrays by using positive indices from `0` to `n - ` (`n` being the length of the array)
```
let first_v = arr[0]
let last_v = arr[len(arr) - 1]
```

### Creating monkey dictionaries
We can use integers, strings and boolean types as keys in dictionaries in monkey
```
let d = {"foo": 1, "bar": 2, true: 3, false: 4, 1: 5};
```

Values can be accessed from dictionaries as follows:

```
let one = d["foo"];
let two = d["bar"];
let three = d[true];
let five = d[1];
```

### Functions
```
let factorial = fn(n) {
    if (n == 0) {
        return 1;
    }
    return n * factorial(n - 1);
}
```

Calling functions
```
>> factorial(3)
6
```

### Higher order functions

```
let map = fn(arr, f) {
    let iter = fn(arr, accumulated) {
        if (len(arr) == 0) {
            accumulated
        } else {
            iter(rest(arr), push(accumulated, f(first(arr))))
        }
    };
    iter(arr, []);
};
```

```
>> let a = [1, 2, 3, 4];
>> let double = fn(x) { x * 2 };
>> map(a, double);
[2, 4, 6, 8]
```

### if expressions
`if` statements can produce values in monkey and thus called expressions.
```
let x = 10;
let y = 0;
if (x == 10) {
    x
} else {
    y
}
```

The above would if expression would produce the value 10

### While expressions
Similar to `if` expressions, while loops can also be used as expressions in monkey.
```
let x = 10;
while (x < 100) {
    let x = x * 2;
    x;
}
```

### Builtin functions

**len**
`len` returns the length of the object, it is supported for strings, arrays and dictionaries

```
>> let s = "hello";
>> len(s)
5

>> let arr = [1, 2, 3];
>> len(arr)
3

>> let dict = {"foo": 1, "bar": 2};
>> len(dict)
2
```

**first**

`first` returns the first element from an array, it's not supported for other types

```
>> let arr = [1, 2, 3]
>> first(arr)
1
```

**last**

`last` returns the last element from an array, it's not supported for other types
```
>> let arr = [1, 2, 3]
>> last(arr)
3
```

**rest**

`rest` returns a new array which is a copy of the given array except its first element

```
>> let arr = [1, 2, 3, 4]
>> rest(arr)
[2, 3, 4]

>> rest(arr(arr))
[3, 4]
```

**push**

`push` returns a new array by copying the old array and adds new element at its end

```
>> let arr = [1, 2, 3]
>> push(arr, 4)
[1, 2, 3, 4]
```

**puts**

`puts` prints the value of a monkey object on stdout

```
>> let s= "hello";
>> puts(s)
hello
null

>> let arr = [1, 2, 3];
>> puts(arr)
[1, 2, 3]
null
```

**type**

`type` prints the type of the monkey object

```
>> let arr = [1, 2, 3]
>> type(arr)
ARRAY

>> let i = 10
>> type(i)
INTEGER

>> let s = "string"
>> type(s)
STRING

>> let d = {"foo": "bar"};
>> type(d)
HASH

>> type(true)
>> BOOLEAN
```
