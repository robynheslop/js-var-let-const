# `var`, `let` & `const`

- Introduced in ES6
- Two new ways to declare a variable

## `var`

### Scope

Two different types of scope:

- Function scope
- Global scope

```javascript
function getName() {
    var myName = 'robyn';
    return myName;
}
getName();
console.log(myName); // Uncaught ReferenceError: myName is not defined
```

`myName` is scoped to the `getName` function, therefore it's only accessible inside the `getName` function (or any functions nested within it).

```javascript
function getName() {
    
    var myName = 'robyn';
    
    function logName(){
        console.log(myName); // myName is accessible here. 
    }

    logName();

}
getName();
console.log(myName); // Uncaught ReferenceError: myName is not defined
```

What about `block scope`?

```javascript
function sayHelloToYourInstructors(instructors){
    var greetings = [];
    for (var index = 0; index < instructors.length; index++) {
        var instructor = instructors[index];
        greetings.push(`Hello ${instructor}!`);   
    }
    console.log(index); // Logs: 3 
    console.log(instructor); // Logs: 'Elise'
    console.log(greetings); // Logs: ['Hello Rami','Hello Robyn','Hello Elise']
    return greetings;
}
sayHelloToYourInstructors(['Rami','Robyn','Elise']);
console.log(index); // Uncaught ReferenceError: index is not defined
console.log(instructor); // Uncaught ReferenceError: instructor is not defined
console.log(greetings); // Uncaught ReferenceError: greetings is not defined
```

### Hoisting

Recall that any variable declared using `var` are initialized with the value of `undefined` during the _creation phase_ of an execution context. This is what we refer to as **hoisting**.

```javascript
function sayHelloToYourInstructors(instructors){
    console.log(index); // Logs: undefined
    console.log(instructor); // Logs: undefined
    console.log(greetings); // Logs: undefined 
    var greetings = [];
    for (var index = 0; index < instructors.length; index++) {
        var instructor = instructors[index];
        greetings.push(`Hello ${instructor}!`);   
    }
    console.log(index); // Logs: 3 
    console.log(instructor); // Logs: 'Elise'
    console.log(greetings); // Logs: ['Hello Rami','Hello Robyn','Hello Elise']
    return greetings;
}
```

## `var` vs `let`

### Scope

Variables declared with `var` are _function scoped_, whereas those declared with `let` are _block scoped_.

```javascript
{ // <---- The start of the block

// Everything between the {} is block scoped

} // <---- The end of the block
```

```javascript
function sayHelloToYourInstructors(instructors){

    var greetings = [];
    for (var index = 0; index < instructors.length; index++) {
        var instructor = instructors[index];
        greetings.push(`Hello ${instructor}!`);   
    }
    console.log(index); // Logs: 3 
    console.log(instructor); // Logs: 'Elise'
    console.log(greetings); // Logs: ['Hello Rami','Hello Robyn','Hello Elise']
    return greetings;
}
```

Variables declared with `let` are _block scoped_, not _function scoped_. This means that they will not be accessible outside of the block they are declared in.

```javascript
function sayHelloToYourInstructors(instructors){
    
    let greetings = [];
    for (let index = 0; index < instructors.length; index++){
        
        let instructor = instructors[index];
        greetings.push(`Hello ${instructor}!`);   
    }
    console.log(index); // Logs: Uncaught ReferenceError: index is not defined
    console.log(instructor); // Logs: Uncaught ReferenceError: instructor is not defined
    console.log(greetings); // Logs: ['Hello Rami','Hello Robyn','Hello Elise'] 
    return greetings;
}
```

### Hoisting

Variables declared with `let` are not hoisted. This means you'll get a `Reference` error if you try to access a variable declared with `let` before it's declaration.

```javascript
function sayHelloToYourInstructors(instructors){
    console.log(index); // Logs: undefined
    console.log(instructor); // Logs: undefined
    console.log(greetings); // Logs: undefined 
    var greetings = [];
    for (var index = 0; index < instructors.length; index++) {
        var instructor = instructors[index];
        greetings.push(`Hello ${instructor}!`);   
    }
    console.log(index); // Logs: 3 
    console.log(instructor); // Logs: 'Elise'
    console.log(greetings); // Logs: ['Hello Rami','Hello Robyn','Hello Elise']
    return greetings;
}
```

```javascript
function sayHelloToYourInstructors(instructors){
    console.log(index); // Logs: Uncaught ReferenceError: index is not defined
    console.log(instructor); // Logs: Uncaught ReferenceError: instructor is not defined
    console.log(greetings); // Logs: Uncaught ReferenceError: greetings is not defined
    let greetings = [];
    for (let index = 0; index < instructors.length; index++) {
        let instructor = instructors[index];
        greetings.push(`Hello ${instructor}!`);   
    }
    console.log(index); // Logs: Uncaught ReferenceError: index is not defined
    console.log(instructor); // Logs: Uncaught ReferenceError: instructor is not defined
    console.log(greetings); // Logs: ['Hello Rami','Hello Robyn','Hello Elise']
    return greetings;
}
```

## `let` vs `const`

### Scope

Variables declared with `const` are _block scoped_, like those declared with `let`.

### Hoisting

Variables declared with `const`, like those declared with `let` are not hoisted.

### Assignment

Variables declared with `const` cannot have their value reassigned

```javascript
const robynsAge = 16;
 
// Does Robyn age?
robynsAge = robynsAge +1;
// Uncaught TypeError: Assignment to constant variable.

// No she does not! :D - What a timeless, ageless youth! Waw. 
```

(For those of you that don't know what a Trapper Keeper is)

![Robyns Trapper Keeper](./assets/images/trapper-keeper.jpg)

```javascript
const robynsTrapperKeeper = {
    color: 'Hot Pink',
    owner: 'Robyn Heslop',
    level: 'Over 9000'
};
// Can I nick it?
robynsTrapperKeeper.owner = 'Rami Ruhayel'; // Yes I can!

// What if I try to swap it out for another, less excellent Trapper Keeper?
robynsTrapperKeeper = {
    color: 'Beige',
    owner: 'Dick Dirgler',
    level: 'Low AF'
} // Uncaught TypeError: Assignment to constant variable.
```

## When should I use `var`, `let` and `const`?

`var` should be a-_var_-ded. (Hyuk, Hyuk, Hyuk! Thanks Robyn...)

Use `const` unless you have a good reason *not* to! An example of a situation where `const` is not appropriate is when declaring a counter to be used in `for` loop:

```javascript
for (const i = 0; i < 3; i++){

}
// Uncaught TypeError: Assignment to constant variable.
```

Instead, you should use `let`:

```javascript
for (let i = 0; i < 3; i++){

}
```

Basically, for variable that don't need their value to change, use `const`, otherwise use `let`. Don't ever use `var` again! ;)

## Recap

| type    | scope    | hoisted  |
|---------|----------|----------|
| `var`   | function | ✅       |
| `let`   | block    | ❌       |
| `const` | block    | ❌       |

## References

[ui.dev - var, let and const](https://ui.dev/var-let-const/)
