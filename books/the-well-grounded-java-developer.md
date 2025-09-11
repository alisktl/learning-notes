# Java для опытных разработчиков [The Well-Grounded Java Developer], 2nd Edition

# Глава 1: Современный язык Java
## 1.3. Расширенное выведение типов (синтаксис `var`)
Выведение типов может сделать вывод, что `s` имеет тип `String`:
```
Function<String, Integer> lengthFn = s -> s.length();
```

Полный пример:
```
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        Function<String, Integer> lengthFn = s -> s.length();

        String s = "Hello World";
        System.out.println("Length: " + lengthFn.apply(s));
    }
}
```

Появилось выведение типов локальных переменных (LVTI, Local Variable Type Inference), которое также обозначается `var`:
```
var names = new ArrayList<String>();
```

Полный пример:
```
import java.util.ArrayList;

public class Main {
    public static void main(String[] args) {
        var names = new ArrayList<String>();
        names.add("First");

        System.out.println(names.getFirst());
    }
}
```




