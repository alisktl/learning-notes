# Learning React - Modern Patterns for Developling React Apps

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





























