# JavaScript
Something something programs the behavior of web pages

## Basics

Nek basic HTML in CSS shit
```javascript
document.getElementById("demo").innerHTML = "Hello JavaScript";

// Spremenimo CSS
document.getElementById("demo").style.fontSize = "35px";

console.log()
```

**External scripts**
```html
<script src="myScript.js"></script>
```

Buttons
```html
<button type="button" onclick="document.write(5 + 6)">Try it</button>
```


**Asynchronous** functions that have to wait for another function before excecuting

### Variables
**var** idk basic variables can be redeclared, don't have block scope

**let** variables cannot be redeclared, it provides **Block Scope** (variables declared inside a {} block cannot be accessed from outside the block)

```javascript
{
  let x = 2;
}
// x can NOT be used here

{
  var x = 2;
}
// x CAN be used here
```

**const** variables cannot be redeclared or reasigned and have a **block scope**. But it does not define a constant value... 

```javascript
// You can create a constant array:
const cars = ["Saab", "Volvo", "BMW"];

// You can change an element:
cars[0] = "Toyota";
z
// You can add an element:
cars.push("Audi");

// But you can NOT reassing the array:
cars = ["Toyota", "Volvo", "Audi"];    // ERROR

```


### Map
Well ja je Dict
```javascript
// Create a Map
const fruits = new Map([
  ["apples", 500],
  ["bananas", 300],
  ["oranges", 200]
]);
fruits.set("apples", 100);
fruits.get("apples");
fruits.size;
fruits.delete("apples");
fruits.has("apples");

// List all entries
let text = "";
fruits.forEach (function(value, key) {
  text += key + ' = ' + value;
})

// List all entries
let text = "";
for (const x of fruits.entries()) {
  text += x;
}
```

### Callback functions
A callback is a function passsed as an argument to another function.
Are used in asynchronous functions
```javascript
function myDisplayer(some) {
  document.getElementById("demo").innerHTML = some;
}

function myCalculator(num1, num2, myCallback) {
  let sum = num1 + num2;
  myCallback(sum);
}

myCalculator(5, 5, myDisplayer);
```

Be aware of callback hell

### Promises

```javascript
function myDisplayer(some) {
  document.getElementById("demo").innerHTML = some;
}

let myPromise = new Promise(function(myResolve, myReject) {
  let x = 0;

// The producing code (this may take some time)

  if (x == 0) {
    myResolve("OK");
  } else {
    myReject("Error");
  }
});

myPromise.then(
  function(value) {myDisplayer(value);},
  function(error) {myDisplayer(error);}
);
```

### async
Keyword async before a function makes the function return a promise.
```javascript
async function myFunction() {
  return "Hello";
}
myFunction().then(
  function(value) {myDisplayer(value);},
  function(error) {myDisplayer(error);}
);
```

### await 
await before a function makes the function wait for a promise:
```javascript
async function myDisplay() {
  let myPromise = new Promise(function(resolve, reject) {
    resolve("I love You !!");
  });
  document.getElementById("demo").innerHTML = await myPromise;
}

myDisplay();
```
## Tricks
Placing scripts at the bottom of the **\<body\>** element improves the display speed, because script interpretation slows down the display.

