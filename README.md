# Ask-Javascript-Interview-Question
Q1. What is JavaScript? Answer: JavaScript is a programming language that allows you to implement complex things on web pages, such as dynamic content, animations, and user interactions.

// This script changes the text of a button when it's clicked
document.getElementById("myButton").onclick = function() {
    this.innerHTML = "Clicked!";
};

Q2. What are the different data types in JavaScript?
Answer: JavaScript has several data types including:

Primitive Types: String, Number, Boolean, Null, Undefined, Symbol, and BigInt.
Non-Primitive Types: Object (which includes arrays and functions).
Example: 
let a = "Hello"; // String
let b = 42;      // Number
let c = true;    // Boolean
let d = null;    // Null
let e;           // Undefined
let f = Symbol("id"); // Symbol
let g = 123456789012345678901234567890n; // BigInt
let h = {name: "Alice"}; // Object

Q3. Explain == vs. ===.
Answer: == compares two values for equality after converting both values to a common type (type coercion). === compares two values for equality without type conversion (strict equality).

Example:
console.log(5 == '5');  // true, because '5' is converted to 5
console.log(5 === '5'); // false, because they are of different types

Q4. What is null vs. undefined?
Answer: null is an assignment value that represents no value or no object. undefined means a variable has been declared but has not yet been assigned a value.

Example:
let a; // a is undefined
let b = null; // b is null

Q5. What is a closure in JavaScript?
Answer: A closure is a function that retains access to its lexical scope, even when the function is executed outside that scope.

Example: 
function outer() {
    let count = 0;
    return function inner() {
        count++;
        return count;
    };
}
const counter = outer();
console.log(counter()); // 1
console.log(counter()); // 2


Q6. Explain the concept of "hoisting".
Answer: Hoisting is JavaScript's default behavior of moving declarations (not initializations) to the top of the current scope.

Example:
console.log(x); // undefined
var x = 5;
// JavaScript interprets it as:
// var x;
// console.log(x);
// x = 5;

Q7. What are JavaScript Promises?
Answer: Promises represent a value that may be available now, or in the future, or never. They are used to handle asynchronous operations.

Example:
let promise = new Promise(function(resolve, reject) {
    setTimeout(() => resolve("Done!"), 1000);
});
promise.then(result => console.log(result)); // "Done!" after 1 second

Q8. What is the difference between let, const, and var?
Answer:

var is function-scoped and can be re-declared.
let is block-scoped and cannot be re-declared within the same scope.
const is block-scoped and must be initialized during declaration; it cannot be reassigned.
Example:

var x = 10;
let y = 20;
const z = 30;

x = 15; // allowed
y = 25; // allowed
z = 35; // error, assignment to constant variable

Q9. Explain this keyword in JavaScript.
Answer: this refers to the object that is currently executing the code. Its value depends on where it is used.

Example:

const obj = {
    name: 'Alice',
    greet: function() {
        console.log(this.name);
    }
};
obj.greet(); // 'Alice', because `this` refers to `obj`

Q10. What is event delegation?
Answer: Event delegation is a technique that allows you to handle events at a higher level in the DOM rather than adding event listeners to individual elements.

Example:
document.querySelector("#parent").addEventListener("click", function(e) {
    if (e.target && e.target.nodeName == "BUTTON") {
        console.log("Button clicked", e.target);
    }
});

Q11. What is async/await in JavaScript?
Answer: async/await is syntactic sugar built on Promises that allows for writing asynchronous code in a more synchronous manner.

Example:
async function fetchData() {
    let response = await fetch('https://api.example.com/data');
    let data = await response.json();
    console.log(data);
}
fetchData();

Q12. What is a callback function?
Answer: A callback function is a function passed into another function as an argument, which is then invoked inside the outer function to complete some kind of routine or action.

Example:
function greet(name, callback) {
    console.log('Hello ' + name);
    callback();
}
function sayGoodbye() {
    console.log('Goodbye!');
}
greet('Alice', sayGoodbye);

Q13. Explain the concept of the prototype in JavaScript.
Answer: Every JavaScript object has a prototype. A prototype is also an object. All JavaScript objects inherit their properties and methods from their prototype.

Example:

function Person(name) {
    this.name = name;
}
Person.prototype.sayHello = function() {
    console.log("Hello, I'm " + this.name);
};
const alice = new Person("Alice");
alice.sayHello(); // "Hello, I'm Alice"

Q14. What is the map() function in JavaScript?
Answer: map() creates a new array populated with the results of calling a provided function on every element in the calling array.

Example:

const numbers = [1, 2, 3, 4];
const doubled = numbers.map(n => n * 2);
console.log(doubled); // [2, 4, 6, 8]


Q15. What is the filter() function in JavaScript?
Answer: filter() creates a new array with all elements that pass the test implemented by the provided function.

Example:

const numbers = [1, 2, 3, 4, 5];
const evenNumbers = numbers.filter(n => n % 2 === 0);
console.log(evenNumbers); // [2, 4]

Q16. What is the reduce() function in JavaScript?
Answer: reduce() executes a reducer function (that you provide) on each element of the array, resulting in a single output value.

Example:

const numbers = [1, 2, 3, 4];
const sum = numbers.reduce((accumulator, currentValue) => accumulator + currentValue, 0);
console.log(sum); // 10

Q17. Explain the concept of currying in JavaScript.
Answer: Currying is a technique in functional programming where a function is transformed into a series of functions, each taking a single argument.

Example:

function add(a) {
    return function(b) {
        return a + b;
    };
}
const addFive = add(5);
console.log(addFive(3)); // 8

Q18. What is the difference between slice() and splice()?
Answer:

slice() returns a shallow copy of a portion of an array into a new array without modifying the original array.
splice() changes the contents of an array by removing or replacing existing elements and/or adding new elements.
Example:

let arr = [1, 2, 3, 4, 5];
console.log(arr.slice(1, 3)); // [2, 3]
console.log(arr); // [1, 2, 3, 4, 5]

arr.splice(1, 2);
console.log(arr); // [1, 4, 5]

Q19. What is an IIFE (Immediately Invoked Function Expression)?
Answer: An IIFE is a JavaScript function that runs as soon as it is defined.

Example:
(function() {
    console.log("This is an IIFE");
})();

Q20. What is JSON?
Answer: JSON (JavaScript Object Notation) is a lightweight data interchange format that's easy for humans to read and write and easy for machines to parse and generate.

Example:
const obj = { name: "Alice", age: 25 };
const jsonString = JSON.stringify(obj);
console.log(jsonString); // '{"name":"Alice","age":25}'

const newObj = JSON.parse(jsonString);
console.log(newObj); // {name: "Alice", age: 25}

Q21. What is the difference between synchronous and asynchronous code in JavaScript?
Answer:

Synchronous code is executed line by line, with each line blocking the next one until it finishes.
Asynchronous code allows other code to run while waiting for some operations to complete, using callbacks, promises, or async/await.
Example:

// Synchronous
console.log('Start');
console.log('End');

// Asynchronous
console.log('Start');
setTimeout(() => console.log('Async'), 1000);
console.log('End');

Q22. What is the spread operator in JavaScript?
Answer: The spread operator (...) allows an iterable to expand in places where zero or more arguments or elements are expected.

Example:
const arr = [1, 2, 3];
const newArr = [...arr, 4, 5];
console.log(newArr); // [1, 2, 3, 4, 5]

Q23. What is destructuring in JavaScript?
Answer: Destructuring is a syntax that allows you to unpack values from arrays or properties from objects into distinct variables.

Example:

const person = { name: "Alice", age: 25 };
const { name, age } = person;
console.log(name, age); // "Alice", 25

Q24. What is the typeof operator?
Answer: The typeof operator returns a string indicating the type of the operand.

Example:

console.log(typeof 42);       // "number"
console.log(typeof 'hello');  // "string"
console.log(typeof true);     // "boolean"
console.log(typeof {});       // "object"
console.log(typeof undefined);// "undefined"

Q25. What is NaN in JavaScript?
Answer: NaN stands for "Not-a-Number" and represents a value that is not a legal number. It is a special type of Number in JavaScript.

Example:

console.log(0 / 0);        // NaN
console.log(typeof NaN);   // "number"
console.log(isNaN(NaN));   // true

