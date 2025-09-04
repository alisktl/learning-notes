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





































