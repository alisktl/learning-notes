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

#### 2.1.1. The `@DisplayName` annotation
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

#### 2.1.2. The `@Disabled` annotation
Disabled class:
```
import org.junit.jupiter.api.Disabled;
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.assertTrue;
import static org.junit.jupiter.api.Assertions.assertFalse;

@Disabled("Feature is still under construction.")
public class DisabledClassTest {
    private SUT systemUnderTest = new SUT("Our system under test");

    @Test
    public void testRegularWork() {
        boolean canReceiveRegularWork = systemUnderTest.canReceiveRegularWork();
        assertTrue(canReceiveRegularWork);
    }

    @Test
    public void testAdditionalWork() {
        boolean canReceiveAdditionalWork = systemUnderTest.canReceiveAdditionalWork();
        assertFalse(canReceiveAdditionalWork);
    }
}
```

Disabled methods:
```
import org.junit.jupiter.api.Disabled;
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.assertFalse;
import static org.junit.jupiter.api.Assertions.assertTrue;

public class DisabledMethodsTest {
    private SUT systemUnderTest = new SUT("Our system under test");

    @Test
    @Disabled
    public void testRegularWork() {
        boolean canReceiveRegularWork = systemUnderTest.canReceiveRegularWork();
        assertTrue(canReceiveRegularWork);
    }

    @Test
    @Disabled("Feature still under construction.")
    public void testAdditionalWork() {
        boolean canReceiveAdditionalWork = systemUnderTest.canReceiveAdditionalWork();
        assertFalse(canReceiveAdditionalWork);
    }
}
```

### 2.2. Nested tests
`Gender.java`:
```
public enum Gender {
    MALE,
    FEMALE,
    OTHER
}
```

`Customer.java`:
```
import java.util.Date;

public class Customer {
    private final Gender gender;
    private final String firstName;
    private final String lastName;
    private final String middleName;
    private final Date becomeCustomerDate;

    public Customer(Builder builder) {
        this.gender = builder.gender;
        this.firstName = builder.firstName;
        this.lastName = builder.lastName;
        this.middleName = builder.middleName;
        this.becomeCustomerDate = builder.becomeCustomerDate;
    }

    public Gender getGender() {
        return gender;
    }

    public String getFirstName() {
        return firstName;
    }

    public String getLastName() {
        return lastName;
    }

    public String getMiddleName() {
        return middleName;
    }

    public Date getBecomeCustomerDate() {
        return becomeCustomerDate;
    }

    public static class Builder {
        private final Gender gender;
        private final String firstName;
        private final String lastName;
        private String middleName;
        private Date becomeCustomerDate;

        public Builder(final Gender gender, final String firstName, final String lastName) {
            this.gender = gender;
            this.firstName = firstName;
            this.lastName = lastName;
        }

        public Builder withMiddleName(final String middleName) {
            this.middleName = middleName;
            return this;
        }

        public Builder withBecomeCustomerDate(final Date becomeCustomerDate) {
            this.becomeCustomerDate = becomeCustomerDate;
            return this;
        }

        public Customer build() {
            return new Customer(this);
        }
    }
}
```

`NestedTestsTest.java`:
```
import org.junit.jupiter.api.Nested;
import org.junit.jupiter.api.Test;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;
import static org.junit.jupiter.api.Assertions.assertAll;
import static org.junit.jupiter.api.Assertions.assertEquals;

public class NestedTestsTest {
    private static final String FIRST_NAME = "John";
    private static final String LAST_NAME = "Smith";

    @Nested
    class BuilderTest {
        private String MIDDLE_NAME = "Michael";

        @Test
        void customerBuilder() throws ParseException {
            SimpleDateFormat sdf = new SimpleDateFormat("MM-dd-yyyy");
            Date customerDate = sdf.parse("04-21-2019");

            Customer customer = new Customer.Builder(Gender.MALE, FIRST_NAME, LAST_NAME)
                    .withMiddleName(MIDDLE_NAME)
                    .withBecomeCustomerDate(customerDate).build();

            assertAll(() -> {
                assertEquals(Gender.MALE, customer.getGender());
                assertEquals(FIRST_NAME, customer.getFirstName());
                assertEquals(LAST_NAME, customer.getLastName());
                assertEquals(MIDDLE_NAME, customer.getMiddleName());
                assertEquals(customerDate, customer.getBecomeCustomerDate());
            });
        }
    }
}
```































