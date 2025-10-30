# 100 Java Mistakes and How to Avoid Them

## Mistake #1: Incorrect assumptions about numeric operator precedence
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

## Mistake #2: Missing parentheses in conditions
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

## Mistake #3: Accidental concatenation instead of addition











































