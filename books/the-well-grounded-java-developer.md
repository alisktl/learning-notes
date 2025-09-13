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

## 1.5. Небольшие изменения в Java 11
### 1.5.1. Фабрики коллекций (JEP 213)
```
int[] numbers = {1, 2, 3};

Set<String> set = Set.of("a", "b", "c");

var list = List.of("x", "y");

var m1 = Map.of("a", "b");
var m2 = Map.of("a", "b", "c", "d");
var m3 = Map.ofEntries(
    Map.entry("a", "b"),
    Map.entry("c", "d"));
```

Фвбричные методы порождают экземпляры неизменяемых типов.

### 1.5.3. HTTP/2 (Java 11)
Синхронный Http запрос:
```
var client = HttpClient.newBuilder().build();

var uri = new URI("https://www.google.com");
var request = HttpRequest.newBuilder(uri).GET().build();

var response = client.send(
    request,
    HttpResponse.BodyHandlers.ofString(
        Charset.defaultCharset()));

System.out.println(response.body());
```

```
var client = HttpClient.newBuilder().build();

var uri = new URI("https://www.google.com");
var request = HttpRequest.newBuilder(uri).GET().build();

var handler = HttpResponse.BodyHandlers.ofString();
CompletableFuture.allOf(
    client.sendAsync(request, handler)
        .thenAccept((resp) -> System.out.println(resp.body())),
    client.sendAsync(request, handler)
        .thenAccept((resp) -> System.out.println(resp.body())),
    client.sendAsync(request, handler)
        .thenAccept((resp) -> System.out.println(resp.body()))
).join();
```

### 1.5.4. Однофайловое выполнение (JEP 330)
Save it as a file `HTTP2Check`:
```
#!/usr/bin/java --source 11

import java.net.URI;
import java.net.URISyntaxException;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.util.concurrent.CompletableFuture;

public class HTTP2Check {
    public static void main(String[] args) throws URISyntaxException {
        var client = HttpClient.newBuilder().build();

        var uri = new URI("https://www.google.com");
        var request = HttpRequest.newBuilder(uri).GET().build();

        var handler = HttpResponse.BodyHandlers.ofString();
        CompletableFuture.allOf(
                client.sendAsync(request, handler)
                        .thenAccept((resp) -> System.out.println(resp.body()))
        ).join();
    }
}
```

run in terminal:
```
./HTTP2Check
```







































