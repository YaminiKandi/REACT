### JavaScript Refresher
1. let and const
2. Arrow Functions
3. Exports and Imports
4. Understanding Classes
5. Classes, Properties & Methods
6. Spread and Rest Operator
7. Destructuring
8. Reference and Primitive Types

#### Understanding `let` and `const`:
* JavaScript only knows `var`
* With ES6, a version of javascript, two different keywords were introduced, let and const.
* var still works, but use of let and const is highly recommended.
* Let - new var, creates a variable that really is variable.
* const - creates a constant value, something which we assign only once and never change.
* const never receives a new value.

```js
let myName = 'Yam';
console.log(myName);    // Yam
myName = 'Yamini'
console.log(myName);    // Yamini
```
```js
const myName = 'Yam';
console.log(myName);    // Yam
myName = 'Yamini'
console.log(myName);
// TypeError: Assignment to constant variable
```

#### Arrow Functions:
* Arrow functions are a different way of creating functions in JavaScript. 
* Besides a shorter syntax, they offer advantages when it comes to keeping the scope of 'this' keyword.

```js
// Normal Function Syntax
function myfunc(){
    .......
}
// Arrow Function Syntax
const myfunc = () => {
    .......
}
```
```js
function printMyName(name){
    console.log(name)
}
print(printMyName('Yam'))   // Yam

const printMyName = (name) => {
    console.log(name)
}
print(printMyName('Yam'))   // Yam
```
```js
const multiply = num => num * 2
console.log(multiply(4))    // 8
```

#### Exports and Imports:

<table>
<tr>
<td> person.js </td> <td> utility.js </td>
</tr>
<tr>
<td>

```js
const person = {
    name: 'Yam'
}
export default person
```

</td>
<td>

```js
export const clean = () => {..}
export const baseData = 10;
```

</td>
</tr>
</table>
<table>
<tr>
<td> app.js </td>
<tr>
<td>

```js
// Default export
import person from './person.js'
import prs from './person.js'
// Imports default and only export of the file
// Name in the receiving file is upto us

// Named export
import {baseData} from './utility.js'
import {clean} from './utility.js'
import {clean as Clean} from './utility.js'
import * as bundled from './utility.js'
```

</td>
</tr>
</table>

#### Understanding Classes:
* A Class can have both Properties and Methods.
* Classes are a feature which basically replace constructorfunctions and prototypes.
* We can define blueprints for JavaScript objects with them.

```js
class Person {
    name = 'Yam'            // Property
    call = () => {...}      // Method
}
```
* Usage
```js
const myPerson = new Person()
myPerson.call()
console.log(myPerson.name)
// Constructor Functions
```
* Inheritance
```js
class Person extends Master
```

```js
// Examples
class Person {
    constructor() {
        this.name = 'Yam';
    }
    printMyName(){
        console.log(this.name)
    }
}
const person = new Person();
person.printMyName();   // Yam

// Inheritance
class Human {
    constructor() {
        this.gender = 'female';
    }
    printGender(){
        console.log(this.gender);
    }
}
class Person extends Human{
    constructor() {
        super();    // keyword that initialize parent constructor
        this.name = 'Yam';
    }
    printMyName(){
        console.log(this.name)
    }
}
const person = new Person();
person.printMyName();   // Yam
person.printGender();   // female
```
```js
class Human {
    constructor() {
        this.gender = 'male';
    }
    printGender(){
        console.log(this.gender);
    }
}
class Person extends Human{
    constructor() {
        super();
        this.name = 'Yam';
        this.gender = 'female';
    }
    printMyName(){
        console.log(this.name)
    }
}
const person = new Person();
person.printMyName();   // Yam
person.printGender();   // female
```

#### Classes, Properties and Methods:
<table>
<tr><td>Properties</td><td>Methods</td></tr>
<tr>
<td>Properties are like 'variables attached to classes/objects'</td>
<td>Methods are like 'functions attached to classes/objects'</td>
</tr>
<tr>
<td>

```js
// ES6
constructor(){
  this.myProperty = 'value'
}
```

</td>
<td>

```js
// ES6
myMethod() {...}
```

</td>
</tr>
<tr>
<td>

```js
// ES7
myProperty = 'value'
```

</td>
<td>

```js
// ES7
myMethod = () => {...}
```

</td>
</tr>
</table>

```js
// ES6/babel
class Human {
    gender = 'female';
    printGender = () => {
        console.log(this.gender);
    }
}
class Person extends Human {
    name = 'Yam';
    printMyName = () => {
        console.log(this.name)
    }
}
const person = new Person();
person.printMyName();   // Yam
person.printGender();   // female
```


#### Spread and Rest Operator:
* The spread and rest operators actually use the same
* syntax: `...`
<table>
<tr><td>Spread</td><td>Rest</td></tr>
<tr>
<td>Used to split up array elements or object properties</td>
<td>Used to merge a list of function arguments into an array</td>
</tr>
<tr>
<td>

```js
const newArray = [...oldArray, 1, 2]
const newObject = {...oldObject, newProp: 5}
```

</td>
<td>

```js
function sortArgs(...args) {
  return args.sort()
}
```

</td>
</tr>
</table>

```js
// Example
const numbers = [1, 2, 3, 4, 5];
const newNumbers = [...numbers, 6];
console.log(newNumbers);
// [1, 2, 3, 4, 5, 6]
const numbers = [1,2,3,4,5];
const newNumbers = [numbers, 6];
console.log(newNumbers);
// [[1, 2, 3, 4, 5], 6]

const person = {
  name: 'Yam',
}
const newPerson = {
  ...person,
  age: 28
}
console.log(newPerson);
/*
[object Object] {
  age: 28,
  name: "Yam"
}
*/
```
```js
// '...' - rest operator that merges the args into an array
// so we can use array methods like filter
const filter = (...args) => {
  return args.filter(el => el === 1);
}
console.log(filter(1, 2, 3))  // [1]
```

#### Destructuring:
* Easily extract array elements or object properties and store them in variables.
<table>
<tr><td>Array Destructuring</td><td>Object Destructuring</td></tr>
<tr>
<td>

```js
[a, b] = ['Hello', 'Yam']
console.log(a)  // Hello
console.log(b)  // Yam
```

</td>
<td>

```js
{name} = {name: 'Yam', age: 23}
console.log(name)  // Yam
console.log(age)  // undefined
```

</td>
</tr>
</table>

```js
const numbers = [1, 2, 3];
[num1, num2, num3] = numbers;
console.log(num1, num3); // 1 3
```

#### Reference and Primitive types:
* Numbers, strings, booleans, these are called primitive types.
* When ever we reassign or we store a variable in other variable, it will copy the value.

```js
// Primitive type
const number =  1;
const num2 = number;
console.log(num2) //1
```

* Objects and arrays are reference types.
* It will print the same value as the first person but it will not actually have copied the person instead.
* person, the object is stored in the memory and we store a pointer to that place in the memory. When we assign it to other person, that pointer will be copied not the value.

```js
// Reference type
const person = {
  name: 'Yamini',
  age: 23,
}
const secondPerson = person;
console.log(secondPerson)
/*
[object Object] {
  age: 23,
  name: "Yamini"
}
*/
const person = {
  name: 'Yamini',
  age: 23,
}
const secondPerson = person;
person.name = 'Yam'
console.log(secondPerson)
/*
[object Object] {
  age: 23,
  name: "Yam"
}
*/

// Immutable object

const person = {
  name: 'Yamini',
  age: 23,
}
const secondPerson = {
  ...person 
  // Copying the properties but not the entire object
}
person.name = 'Yam'
console.log(secondPerson)
/*
[object Object] {
  age: 23,
  name: "Yamini"
}
*/
```