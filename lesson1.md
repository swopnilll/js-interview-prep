# Call, Apply and Bind

```js
let nameObj = {
  firstName: "Swopnil",
  lastName: "Acharya",
  printFullName: () => {
    console.log(this);
    console.log(`${this.firstName} ${this.lastName}`);
  },
};

nameObj.printFullName();
```

```terminal
// Output
{}
undefined undefined
```

#### Step-by-Step Execution:

1.  **Object Creation:**

    -   `nameObj` is an object with three properties: `firstName`, `lastName`, and `printFullName`.
    -   `printFullName` is an arrow function.
2.  **Calling `printFullName`:**

    -   When you call `nameObj.printFullName()`, you might expect `this` inside the `printFullName` method to refer to `nameObj`. However, because `printFullName` is an arrow function, it does not have its own `this`. Instead, it inherits `this` from the surrounding context where `nameObj` was defined, which is the global scope in a Node.js environment (or window in a browser).
3.  **`this` in Global Scope:**

    -   In Node.js, the global scope's `this` is the `global` object. However, when not in strict mode, `this` can be an empty object `{}`.
4.  **Printing `this`:**

    -   The first `console.log(this);` inside `printFullName` prints `{}` because `this` refers to the global scope in Node.js, which appears as an empty object in this context.
5.  **Accessing `firstName` and `lastName`:**

    -   `console.log(`${this.firstName} ${this.lastName}`);` tries to access `firstName` and `lastName` properties on `this`. Since `this` is not `nameObj` but the global scope, and there are no `firstName` or `lastName` properties in the global scope, it prints `undefined undefined`.

In JavaScript, the behavior of the `this` keyword can be a bit tricky, especially when dealing with arrow functions. Let's break down why the output is `{}` and `undefined undefined` in your example.

### Understanding `this` in Different Contexts

1.  **Arrow Functions and `this`:** Arrow functions (`() => {}`) do not have their own `this` context. Instead, they inherit `this` from the surrounding lexical scope at the time the arrow function is defined. This is different from regular functions (`function() {}`), which have their own `this` context.

2.  **`this` in Methods:**

    -   In regular functions defined as methods within an object, `this` typically refers to the object the method is called on.
    -   In arrow functions defined as methods within an object, `this` does not refer to the object. Instead, it refers to the lexical scope in which the arrow function was defined, which could be the global scope if defined at the top level.
  
### Using Regular Function 

```js
let nameObj = {
  firstName: "Swopnil",
  lastName: "Acharya",
  printFullName: function () {
    console.log(this);
    console.log(`${this.firstName} ${this.lastName}`);
  },
};

nameObj.printFullName();
```


```zsh
Output: 

{
  firstName: 'Swopnil',
  lastName: 'Acharya',
  printFullName: [Function: printFullName]
}
Swopnil Acharya
```

# Call


In JavaScript, objects can "borrow" methods from other objects using the call, apply, or bind methods. This is a powerful feature that allows for more flexible and reusable code. Let's go through your example step by step to understand how this works.

```js
let nameObj = {
  firstName: "Swopnil",
  lastName: "Acharya",
  printFullName: function () {
    console.log(this);
    console.log(`${this.firstName} ${this.lastName}`);
  },
};

nameObj.printFullName();

let nameObj2 = {
  firstName: "Pratigya",
  lastName: "Sharma",
};

// Function Borrowing
nameObj.printFullName.call(nameObj2);

```

```zsh
Output: 


{
  firstName: 'Swopnil',
  lastName: 'Acharya',
  printFullName: [Function: printFullName]
}
Swopnil Acharya
{ firstName: 'Pratigya', lastName: 'Sharma' }
Pratigya Sharma
```

### Function Borrowing Using `call`

You then borrow the `printFullName` method from `nameObj` and call it with `nameObj2` as its context:


```js
nameObj.printFullName.call(nameObj2);
```

#### How `call` Works

-   The `call` method allows you to call a function with a specified `this` value and arguments provided individually.
-   Syntax: `function.call(thisArg, arg1, arg2, ...)`
    -   `thisArg` is the value of `this` provided for the call to the function.
    -   `arg1, arg2, ...` are the arguments passed to the function.

#### Execution of `printFullName.call(nameObj2)`

1.  **Borrowing the Method:**

    -   `nameObj.printFullName` refers to the `printFullName` method in `nameObj`.
2.  **Setting the Context:**

    -   `.call(nameObj2)` sets the `this` context of `printFullName` to `nameObj2`.
3.  **Executing the Method:**

    -   Inside `printFullName`, `this` now refers to `nameObj2`.
4.  **Printing `this`:**

    -   `console.log(this);` prints `nameObj2` because `this` is `nameObj2`.
5.  **Printing Full Name:**

    -   `console.log(`${this.firstName} ${this.lastName}`);` prints `Pratigya Sharma` because `this.firstName` is `Pratigya` and `this.lastName` is `Sharma`.


### Using The Call in a Better Way 

```js
let nameObj = {
  firstName: "Swopnil",
  lastName: "Acharya",
};

let printFullName = function () {
  console.log(this);
  console.log(`${this.firstName} ${this.lastName}`);
};

printFullName.call(nameObj);

let nameObj2 = {
  firstName: "Pratigya",
  lastName: "Sharma",
};

// Function Borrowing
printFullName.call(nameObj2);
```


## Apply 

```js
let nameObj = {
  firstName: "Swopnil",
  lastName: "Acharya",
};

let printFullName = function (place, interest) {
  console.log(
    `${this.firstName} ${this.lastName} is from ${place} likes ${interest}`
  );
};

printFullName.call(nameObj, "Kathmandu", "Working Hard");

let nameObj2 = {
  firstName: "Pratigya",
  lastName: "Sharma",
};

// Function Borrowing
printFullName.call(nameObj2, "Lalitpur", "Sleeping");

// Using Apply Approach

printFullName.apply(nameObj, ["Kathmandu", "Working Hard"]);

```

In JavaScript, both `call` and `apply` are methods that allow you to invoke a function with a specified `this` value and arguments. They are very similar but have a key difference in how they handle arguments.

### What is `apply`?

The `apply` method calls a function with a given `this` value, and arguments provided as an array (or an array-like object).

### Syntax

-   `function.apply(thisArg, [argsArray])`

    -   `thisArg`: The value of `this` provided for the call to the function.
    -   `[argsArray]`: An array or array-like object whose elements are used as the arguments for the function.

### Differences Between `call` and `apply`

1.  **Arguments Handling:**

    -   `call`: Arguments are passed individually.
    -   `apply`: Arguments are passed as an array.
2.  **Use Cases:**

    -   Use `call` when you have a fixed number of arguments and you know them beforehand.
    -   Use `apply` when you want to pass arguments as an array, which can be useful when you have a variable number of arguments or an array that needs to be spread out.


## Bind 

```js
let nameObj = {
  firstName: "Swopnil",
  lastName: "Acharya",
};

let printFullName = function (place, interest) {
  console.log(
    `${this.firstName} ${this.lastName} is from ${place} likes ${interest}`
  );
};

printFullName.call(nameObj, "Kathmandu", "Working Hard");

let nameObj2 = {
  firstName: "Pratigya",
  lastName: "Sharma",
};

// Function Borrowing
printFullName.call(nameObj2, "Lalitpur", "Sleeping");

// Using Apply Approach
printFullName.apply(nameObj, ["Kathmandu", "Working Hard"]);

// Using Bind
let printMyName = printFullName.bind(nameObj, "Kathmandu", "Working Hard");

printMyName();

```

The `bind` method in JavaScript creates a new function that, when called, has its `this` keyword set to the provided value, with a given sequence of arguments preceding any provided when the new function is called. This is different from `call` and `apply`, which immediately invoke the function.

### What is `bind`?

The `bind` method allows you to create a new function with a specific `this` context and predefined arguments.

### Syntax

-   `function.bind(thisArg, arg1, arg2, ...)`

    -   `thisArg`: The value to be used as `this` when the new function is called.
    -   `arg1, arg2, ...`: Arguments to be prepended to arguments provided when the new function is called.

### Differences Between `bind`, `call`, and `apply`

-   `call`: Immediately invokes the function with a specified `this` value and arguments passed individually.
-   `apply`: Immediately invokes the function with a specified `this` value and arguments passed as an array.
-   `bind`: Creates a new function with a specified `this` value and optionally predefined arguments, but does not immediately invoke the function
