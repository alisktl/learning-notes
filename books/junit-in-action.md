# jUnit in Action. 3rd Edition

## 1. jUnit jump-start
Add minimal dependencies:
```
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter-api</artifactId>
    <version>6.0.1</version>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter-engine</artifactId>
    <version>6.0.1</version>
    <scope>test</scope>
</dependency>
```

or:
```
testImplementation("org.junit.jupiter:junit-jupiter-api:6.0.1")
testImplementation("org.junit.jupiter:junit-jupiter-engine:6.0.1")
```

Calculator.java:
```
public class Calculator {
    public double add(double number1, double number2) {
        return number1 + number2;
    }
}
```

CalculatorTest.java:
```
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.assertEquals;

public class CalculatorTest {

    @Test
    public void testAdd() {
        Calculator calc = new Calculator();
        double result = calc.add(10, 50);
        assertEquals(60, result, 0);
    }
}
```
