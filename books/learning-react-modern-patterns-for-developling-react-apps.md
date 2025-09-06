# Learning React - Modern Patterns for Developling React Apps

- Chapter 2. JavaScript for React
  - Declaring Variables
    - The `const` Keyword
    - The `let` Keyword
    - Template Strings
  - Creating Functions
    - Function Declarations
    - Function Expressions
    - Default Parameters
    - Arrow Functions
  - Compiling JavaScript
  - Objects and Arrays
    - Destructuring Objects

---

# Chapter 2. JavaScript for React
## Declaring Variables
### The `const` Keyword
works:
```
var pizza = true;
pizza = false;
console.log(pizza); // false
```

does not work:
```
const pizza = true;
pizza = false;
```

### The `let` Keyword
```
var topic = "JavaScript";
if (topic) {
  var topic = "React";
  console.log("block", topic); // block React
}

console.log("global", topic); // global React
```

With the `let` keyword, we can scope a variable to any code block.
```
var topic = "JavaScript";
if (topic) {
  let topic = "React";
  console.log("block", topic); // React
}

console.log("global", topic); // JavaScript
```

Always outputs `This is box #5`:
```
var div, container = document.getElementById("container");
for (var i = 0; i < 5; i++) {
  div = document.createElement("div");
  div.onclick = function() {
    alert("This is box #" + i);
  };

  container.appendChild(div);
}
```

Works fine:
```
const container = document.getElementById("container");
let div;
for (let i = 0; i < 5; i++) {
  div = document.createElement("div");
  div.onclick = function() {
    alert("This is box #: " + i);
  };
  container.appendChild(div);
}
```

### Template Strings
```
console.log(lastName + "," + firstName + " " + middleName);
```

template:
```
console.log(`${lastName}, ${firstName} ${middleName}`);
```

## Creating Functions
### Function Declarations
```
function logCompliment() {
  console.log("You're doing great!");
}

logCompliment();
```

### Function Expressions
```
const logCompliment = function() {
    console.log("You're doing great!");
};

logCompliment();
```

works:
```
// Invoking the function before it's declared
hey();

// Function Declaration
function hey() {
  alert("hey!");
}
```

does not work:
```
// Invoking the function before it's declared
hey();

// Function Expression
const hey = function() {
  alert("hey!");
};
```

#### Passing Arguments
```
const logCompliment = function(firstName, message) {
  console.log(`${firstName}: ${message}`);
};

logCompliment("Molly", "You're so cool");
```

#### Function Returns
```
const createCompliment = function(firstName, message) {
    return `${firstName}: ${message}`;
};

createCompliment("Molly", "You're cool");
```

### Default Parameters
```
function logActivity(name = "Shane McConkey", activity = "skiing") {
    console.log(`${name} loves ${activity}`);
}

logActivity("Alisher");
```

Default arguments can be any type, not just strings:
```
const defaultPerson = {
    name: {
        first: "Shane",
        last: "McConkey",
    },
    favActivity: "skiing"
};

function logActivity(person = defaultPerson) {
    console.log(`${person.name.first} loves ${person.favActivity}`);
}

logActivity();
```

### Arrow Functions
```
const lordify = function(firstName) {
    return `${firstName} of Canterbury`;
};

console.log(lordify("Dale"));
```

With an arrow, we can simplify the syntax tremendously:
```
const lordify = firstName => `${firstName} of Canterbury`;

console.log(lordify("Dale"));
```

More than one argument:
```
const lordify = (firstName, land) => `${firstName} of ${land}`;

console.log(lordify("Dale", "Canterbury"));
```

If there are multiple lines, you'll use curly braces:
```
const lordify = (firstName, land) => {
    if(!firstName) {
        throw new Error("A firstName is required to lordify");
    }

    if(!land) {
        throw new Error("A lord must have a land");
    }

    return `${firstName} of ${land}`;
};

console.log(lordify("Kelly"));
console.log(lordify("Kelly", "Sonoma"));
```

#### Returning Objects
```
const person = (firstName, lastName) => ({
    first: firstName,
    last: lastName
});

console.log(person("Flad", "Hanson"));
```

#### Arrow Functions and Scope
```
const tahoe = {
    mountains: ["Freel", "Rose", "Tallac", "Rubicon", "Silver"],
    print: function(delay = 1000) {
        setTimeout(() => {
            console.log(this.mountains.join(", "));
        }, delay);
    }
};

tahoe.print();
```

Does not work:
```
const tahoe = {
    mountains: ["Freel", "Rose", "Tallac", "Rubicon", "Silver"],
    print: (delay = 1000) => {
        setTimeout(() => {
            console.log(this.mountains.join(", "));
        }, delay);
    }
};

tahoe.print(); // throws error
```

and

```
const tahoe = {
    mountains: ["Freel", "Rose", "Tallac", "Rubicon", "Silver"],
    print: function(delay = 1000) {
        setTimeout(function() {
            console.log(this.mountains.join(", "));
        }, delay);
    }
};

tahoe.print();
```

## Objects and Arrays
### Destructuring Objects
```
const sandwich = {
    bread: "dutch crunch",
    meat: "tuna",
    cheese: "swiss",
    toppings: ["lettuce", "tomato", "mustard"]
};

const {bread, meat} = sandwich;
console.log(bread, meat);
```

Do not change the original sandwich:
```
const sandwich = {
    bread: "dutch crunch",
    meat: "tuna",
    cheese: "swiss",
    toppings: ["lettuce", "tomato", "mustard"]
};

let { bread, meat } = sandwich;
bread = "garlic";
meat = "turkey";

console.log(bread, meat);
console.log(sandwich.bread, sandwich.meat);
```

We can also destructure incoming function arguments. Instead of:
```
const lordify = regularPerson => {
    console.log(`${regularPerson.firstname} of Canterbury`);
};

const regularPerson = {
    firstname: "Bill",
    lastname: "Wilson"
};

lordify(regularPerson);
```

we may write:
```
const lordify = ({ firstname }) => {
    console.log(`${firstname} of Canterbury`);
};

const regularPerson = {
    firstname: "Bill",
    lastname: "Wilson"
};

lordify(regularPerson);
```

another example:
```
const lordify = ({ spouse: { firstname } }) => {
    console.log(`${firstname} of Canterbury`);
};

const regularPerson = {
    firstname: "Bill",
    lastname: "Wilson",
    spouse: {
        firstname: "Phil",
        lastname: "wilson"
    }
};

lordify(regularPerson);
```

### Destructuring Arrays
```
const [firstAnimal] = ["Horse", "Mouse", "Cat"];

console.log(firstAnimal);
```

```
const [, , thirdAnimal] = ["Horse", "Mouse", "Cat"];

console.log(thirdAnimal);
```

### Object Literal Enhancement
*Object Literal Enhancement* – is the opposite of destructuring.

```
const name = "Tallac";
const elevation = 9738;

const funHike = { name, elevation };

console.log(funHike);
```

We can also create object methods with object literal enhancement or restructuring:
```
const name = "Tallac";
const elevation = 9738;

const print = function() {
    console.log(`Mt. ${this.name} is ${this.elevation} feet tall`);
};

const funHike = { name, elevation, print };

funHike.print();
```

### The Spread Operator
```
const peaks = ["Tallac", "Ralston", "Rose"];
const canyons = ["Ward", "Blackwood"];
const tahoe = [...peaks, ...canyons];

console.log(tahoe.join(", "));
```

Reverse original array & last element:
```
const peaks = ["Tallac", "Ralston", "Rose"];
const [last] = peaks.reverse();

console.log(last);
console.log(peaks.join(", "));
```

Do not reverse the original array & last element:
```
const peaks = ["Tallac", "Ralston", "Rose"];
const [last] = [...peaks].reverse();

console.log(last);
console.log(peaks.join(", "));
```

The spread operator can also be used to get the remaining items in the array:
```
const lakes = ["Donner", "Marlette", "Fallen Leaf", "Cascade"];

const [first, ...others] = lakes;
console.log(others.join(", "));
```

We can also use the three-dot syntax to collect function arguments as an array:
```
function directions(...args) {
    let [start, ...remaining] = args;
    let [finish, ...stops] = remaining.reverse();

    console.log(`drive through ${args.length} towns`);
    console.log(`start in ${start}`);
    console.log(`the destination is ${finish}`);
    console.log(`stopping ${stops.length} times in between`);
}

directions("Truckee", "Tahoe City", "Sunnyside", "Homewood", "Tahoma");
```

The spread operator can also be used for objects:
```
const morning = {
    breakfast: "oatmeal",
    lunch: "peanut butter and jelly"
};

const dinner = "mac and cheese";

const backpackingMeals = {
    ...morning,
    dinner
};

console.log(backpackingMeals);
```

## Asynchronous JavaScript
### Simple Promises with Fetch
```
fetch('https://api.randomuser.me/?nat=US&results=1').then(res =>
    console.log(res.json())
);
```

We can chain together `then` functions to handle a promise that has been successfully resolved:
```
fetch('https://api.randomuser.me/?nat=US&results=1')
    .then(res => res.json())
    .then(json => json.results)
    .then(console.log)
    .catch(console.error);
```



































