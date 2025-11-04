# 100 Java Mistakes and How to Avoid Them

- Managing code quality
- 2. Expressions
  - [2.1. Mistake #1: Incorrect assumptions about numeric operator precedence](#21-mistake-1-incorrect-assumptions-about-numeric-operator-precedence)
  - [2.2. Mistake #2: Missing parentheses in conditions](#22-mistake-2-missing-parentheses-in-conditions)
  - [2.3. Mistake #3: Accidental concatenation instead of addition](#23-mistake-3-accidental-concatenation-instead-of-addition)
  - 2.4. Mistake #4: Multiline string literals
  - 2.5. Mistake #5: Unary plus
  - 2.6. Mistake #6: Implicit type conversion in conditional expressions
  - 2.7. Mistake #7: Using non-short-circuit logic operators
  - 2.8. Mistake #8: Confusing `&&` and `||`
  - 2.9. Mistake #9: Incorrectly using variable arity calls
  - 2.10. Mistake #10: Conditional operators and variable arity calls
  - 2.11. Mistake #11: Ignoring an important return value
  - 2.12. Mistake #12: Not using a newly created object
  - 2.13. Mistake #13: Binding a method reference to the wrong method
  - 2.14. Mistake #14: Using the wrong in a method reference

## 2.1. Mistake #1: Incorrect assumptions about numeric operator precedence
### Binary shift
wrong:
```
static int pack() {
  int xmin = 2;
  return xmin << 1 + 2;
}
```

correct:
```
static int pack() {
  int xmin = 2;
  return (xmin << 1) + 2;
}
```

### Bitwise operators
wrong:
```
static int updateBits() {
  int bits = 2;
  return bits & 0b00001111 + 1;
}
```

correct:
```
static int updateBits() {
  int bits = 2;
  return (bits & 0b00001111) + 1;
}
```
or:
```
static int updateBits() {
  int bits = 2;
  return bits & 0b00001111 | 1;
}
```

## 2.2. Mistake #2: Missing parentheses in conditions
### && and || precedence
wrong:
```
if (index >= 0 && str.charAt(index) == ' ' || str.charAt(index) == '\t') { ... }
```

correct:
```
if (index >= 0 && (str.charAt(index) == ' ' || str.charAt(index) == '\t')) { ... }
```
or:
```
static boolean isWhitespace(char ch) {
  return ch == ' ' || ch == '\t';
}

if (index >= 0 && isWhitespace(str.charAt(index))) { ... }
```

### Conditional operator and addition
wrong:
```
int capacity = str.length() + indent < 0 ? 0 : index;
```

correct:
```
int capacity = str.length() + (indent < 0 ? 0 : index);
```
or:
```
int capacity = str.length() + Math.max(indent, 0);
```

### The conditional operator and `null` check
wrong:
```
return "Value: " + value != null ? value : "(unknown)";
```

correct:
```
String displayedValue = value != null ? value : "(unknown)";
return "Value: " + displayedValue;
```
or:
```
return String.format("Value: %s", value != null ? value : "(unknown)");
```
or:
```
// since Java 9
return "Value: " + Objects.requireNonNullElse(value, "(unknown)");
```
or:
```
// since Java 15
return "Value: %s".formatted(value != null ? value : "(unknown)");
```

## 2.3. Mistake #3: Accidental concatenation instead of addition
wrong:
```
String entryName = "Entry#" + index + 1;
```

correct:
```
String entryName = "Entry#" + (index + 1);
```
or:
```
int adjustedIndex = index + 1;
String entryName = "Entry#" + adjustedIndex;
```
or:
```
String entryName = String.format("Entry#%d", index + 1);
```

## 2.4. Mistake #4: Multiline string literals












































