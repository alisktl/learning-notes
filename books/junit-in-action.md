# jUnit in Action. 3rd Edition

## 1. jUnit jump-start
### 1.1. Add minimal dependencies:
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

### 1.2. Simple example
`Calculator.java`:
```
public class Calculator {
    public double add(double number1, double number2) {
        return number1 + number2;
    }
}
```

`CalculatorTest.java`:
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

## 2. Exploring core jUnit
### 2.1. Core annotations
`SUT.java`:
```
public class SUT {

    public SUT(String text) {
        System.out.println("SUT(): " + text);
    }

    public void close() {
        System.out.println("SUT:close()");
    }

    public boolean canReceiveRegularWork() {
        return true;
    }

    public boolean canReceiveAdditionalWork() {
        return false;
    }
}
```

`ResourceForAllTests.java`:
```
public class ResourceForAllTests {

    public ResourceForAllTests(String text) {
        System.out.println("ResourceForAllTests(): " + text);
    }

    public void close() {
        System.out.println("ResourceForAllTests:close()");
    }
}
```

`SUTTest.java`:
```
import org.junit.jupiter.api.*;
import static org.junit.jupiter.api.Assertions.assertFalse;
import static org.junit.jupiter.api.Assertions.assertTrue;

public class SUTTest {

    private static ResourceForAllTests resourceForAllTests;
    private SUT systemUnderTest;

    @BeforeAll
    static void setupClass() {
        resourceForAllTests = new ResourceForAllTests("Our resource for all tests");
    }

    @AfterAll
    static void tearDownClass() {
        resourceForAllTests.close();
    }

    @BeforeEach
    void setUp() {
        systemUnderTest = new SUT("Our system under test");
    }

    @AfterEach
    void tearDown() {
        systemUnderTest.close();
    }

    @Test
    void testRegularWork() {
        boolean canReceiveRegularWork = systemUnderTest.canReceiveRegularWork();
        assertTrue(canReceiveRegularWork);
    }

    @Test
    void testAdditionalWork() {
        boolean canReceiveAdditionalWork = systemUnderTest.canReceiveAdditionalWork();
        assertFalse(canReceiveAdditionalWork);
    }
}
```

- The method annotated with `@BeforeAll` is executed once: before all tests. This method needs to be static unless the entire test class is annotated with `@TestInstance(Lifecycle.PER_CLASS)`.
- The method annotated with `@BeforeEach` is executed before each test. In our case, it will be executed twice.
- The two methods annotated with `@Test` are executed independently.
- The method annotated with `@AfterEach` is executed after each test. In our case, it will be executed twice.
- The method annotated with `@AfterAll` is executed once: after all tests. This method needs to be static unless the entire test class is annotated with `@TestInstance(Lifecycle.PER_CLASS)`.
- To run this test class, you can execute the following from the command line:
```
mvn -Dtest=SUTTest.java clean install
```

### 2.2. The `@DisplayName` annotation
`SUT.java`:
```
public class SUT {

    public SUT() {
        System.out.println("SUT()");
    }

    public String hello() {
        return "Hello";
    }

    public String talk() {
        return "How are you?";
    }

    public String bye() {
        return "Bye";
    }
}
```

`DisplayNameTest.java`:
```
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.assertEquals;

@DisplayName("Test class showing the @DisplayName annotation.")
public class DisplayNameTest {
    private SUT systemUnderTest = new SUT();

    @Test
    @DisplayName("Our system under test says hello.")
    void testHello() {
        assertEquals("Hello", systemUnderTest.hello());
    }

    @Test
    @DisplayName("\uD83D\uDE31")
    void testTalking() {
        assertEquals("How are you?", systemUnderTest.talk());
    }

    @Test
    void testBye() {
        assertEquals("Bye", systemUnderTest.bye());
    }
}
```
































