# С++ ФПМИ лекции - основной поток

[timcodes](https://github.com/alisktl/learning-notes/blob/main/courses/mipt-course-c%2B%2B-timecodes.md)

1. [Основные типы и операции над ними.](#основные-типы-и-операции-над-ними)
2. [Объявления, определения и области видимости. Выражения и операторы.](#объявления-определения-и-области-видимости-выражения-и-операторы)
3. [Выражения и операторы. Управляющие конструкции. Ошибки компиляции.](#выражения-и-операторы-управляющие-конструкции-ошибки-компиляции)
4. [Runtime errors, undefined behaviour. Указатели и операции над ними.](#runtime-errors-undefined-behaviour-указатели-и-операции-над-ними)
5. [Виды памяти. Массивы. Перегрузка функций](#виды-памяти-массивы-перегрузка-функций)
6. [Указатели на функции. Ссылки. Константы](#указатели-на-функции-ссылки-константы)
7. [Приведения типов. Классы и структуры. Модификаторы доступа](#приведения-типов-классы-и-структуры-модификаторы-доступа)
8. [Конструкторы, деструкторы, оператор присваивания и "правило трех"](#конструкторы-деструкторы-оператор-присваивания-и-правило-трех)
9. [Перегрузка операторов, константные и статические методы](#перегрузка-операторов-константные-и-статические-методы)
10. [Указатели на члены. Енумы. Наследование (начало)]()

---

# Раздел №3
# Основные типы и операции над ними

## О разных компиляторах

Есть разные компиляторы, `clang` и `gcc` - основные компиляторы.

### **C** и **C++** компилятроов
- `clang`:
  - `clang` - **clang** компилятор для *C*
  - `clang++` - **clang** компилятор для *C++*
- `gcc`:
  - `gcc` - **GNU** компилятор для *C*
  - `g++` - **GNU** компилятор для *C++*

### Версии компилятора и версии языка
Версии компилятора и версии языка – это разные понятия.

Например, чтобы скомпилировать код с помощью **GNU** компилятора версии 10 и *C++* версии 20:
```
g++-10 -std=c++20 main.cpp
```

## Основные типы и операции над ними

### Целочисленные типы

Следующие типы имеют примерную длину (зависит от операционной системы и CPU):

- `short` – ≈2 bytes
- `int` – ≈4 bytes
- `long long` – ≈8 bytes
- `unsigned int` – ≈4 bytes
- `unsigned long long` – ≈8 bytes

Следующие типы имеют точную длину:
- `int8_t` – 1 byte
- `int16_t` – 2 bytes
- `int32_t` – 4 bytes
- `int64_t` – 8 bytes
- `int128_t` – 16 bytes
- `uint8_t` – 1 byte
- `uint16_t` – 2 bytes
- `uint32_t` – 4 bytes
- `uint64_t` – 8 bytes
- `uint128_t` – 16 bytes

### char
Тип данных `char` предназначен для хранения одного символа и занимает 1 байт памяти. Каждому символу соответствует 8-битное целое значение – ASCII-код. Таким образом тип *char* может использоваться для хранения целых чисел от `-128` до `127` или от `0` до `255` (в зависимости от того, какой из типов используется: `signed char` и `unsigned char`).

### Типы с плавающей точкой

Следующие типы имеют примерную длину (зависит от операционной системы и CPU):

- `float` – ~4 bytes
- `double` – ~8 bytes
- `long double` – ~16 bytes

[Дополнительный код](https://ru.wikipedia.org/wiki/%D0%94%D0%BE%D0%BF%D0%BE%D0%BB%D0%BD%D0%B8%D1%82%D0%B5%D0%BB%D1%8C%D0%BD%D1%8B%D0%B9_%D0%BA%D0%BE%D0%B4) – способ представления чисел с плвавющей точкой в компьютерах.

### Тип `bool` и логические операции
Логические операции:
- `&&`
- `||`
- `!`
- `!=`
- `==`

Побитовые операции:
- `&`
- `|`

### `std::string`
Объект типа `string` содержит последовательность символов типа *char*, которая может быть пустой.

```
std::string s = "abc";
s[0] == 'a';
'a' + s == "aabc";
s.size();
```

### `std::vector<>`

`vector` – динамический массив.

```
std::vector<int> = {1, 2 3 4 5};
v[3] == 4;
v.push_back(6);
v.pop_back();
```

### `std::set<int>` и `std::multiset<int>`, `std::map<std::string, int>` и `std::multimap<std::string, int>`
set:
```
std::set<int> s;
s.size();
s.insert(3);
s.erase(3);
s.count(2);

for (int x: s) {
  std::cout << x << '\n';
}
```

map:
```
std::map<std::string, int> m;
m["abc"] = 1;
m["aaa"] = 2;
std::cout << m["aaa"];
```

---

# Объявления, определения и области видимости. Выражения и операторы.

## Объявления и определения переменных
```
int a; // declaration and definition, because it has some value
int b = 5; // declaration and definition
```

## Объявления и определения функций
```
int f(int x); // declaration;

// definition
int f(int x) {
  return x + 1;
}
```

## Связь определения и объявления
**Rule**: Any definition is a declaration;

## Разница между определением и объявлением функций
В одной области видимости можно объявлять функцию много раз, определять функцию можно только один раз.

## Области видимости
Область видимости представляет часть программы, в пределах которой можно использовать объект. Как правило, область видимости ограничивается блоком кода, который заключается в фигурные скобки. В зависимости от области видимости создаваемые объекты могут быть глобальными, локальными или автоматическими.

### Глобальные объекты
Глобальные переменные определены в файле программы вне любой из функций или любого другого блока кода и могут использоваться любой функцией. Глобальные переменные существуют в течение всей жизни программы и уничтожаются лишь с завершением программы.

```
int x = 5;

int main() {
  std::cout << x << '\n';
}
```

Глобальные переменные можно переопределить в другой области видимости. Так же можно получить глобальную переменную с помощью оператора `::`:
```
int x = 3;

int main() {
  int x = 5;
  {
    int x = 10;
    std::cout << x << '\n'; // 10
    std::cout << ::x << '\n'; // 3
  }
  std::cout << x << '\n'; // 5
  std::cout << ::x << '\n'; // 3
}
```

## Пространства имен
Пространство имен позволяет сгруппировать функционал в отдельные контейнеры. Пространство имен представляет блок кода, который содержит набор компонентов (функций, классов и т.д.) и имеет некоторое имя, которое прикрепляется к каждому компоненту из этого пространства имен. Полное имя каждого компонента – это имя пространства имен, за которым следует оператор `::` (оператор области видимости или scope operator) и имя компонента.

**Глобальное пространство имен** – если пространство имен не указано, то по умолчанию применяется глобальное пространство имен. применяется по умолчанию, если пространство имен не было определено. Все имена в глобальном пространстве имен такие же, как вы их объявляете, без прикрепления имени пространства имен:
```
int x = 3;

int main() {
  std::cout << x << '\n'; // 3
  std::cout << ::x << '\n'; // 3
}
```

**Определение пространства имен.** Для определения пространства имен применяется ключевое слово namespace, за которым идет название имени пространства имен:
```
namespace new_namespace {
  int x = 10;

  int f(int y) {
    return y + 1;
  }
}

namespace new_namespace_2 {
  int f(int y);
}

int new_namespace_2::f(int y) {
  return y + 2;
}

int main() {
  std::cout << new_namespace::x << '\n'; // 10

  new_namespace::x = 13;
  std::cout << new_namespace::x << '\n'; // 13

  std::cout << new_namespace::f(10) << '\n'; // 11
  std::cout << new_namespace_2::f(10) << '\n'; // 12
}
```

## Директива `using`
### Использование #1: using-объявления для новых названий типов
```
int main() {
  using vi = std::vector<int>;
  vi v = {1, 2, 3, 4, 5};

  for (int a: v) {
    std::cout << a << ' ';
  }
}
```

```
using vi = std::vector<int>;

int main() {
  vi v = {1, 2, 3, 4, 5};

  for (int a: v) {
    std::cout << a << ' ';
  }
}
```

### Использование #2: using-объявления для внесения в локальную область
```
namespace N {
  int x = 3;
}

int main() {
  using N::x;
  std::cout << x << '\n'; // 3
}
```

### Использование #3: `using namespace`, и почему это плохо
```
namespace N {
  int x = 3;
}

int main() {
  using namespace N;
  std::cout << x << '\n'; // 3
}
```

## Выражения и операторы (expressions and operators)
TODO: add

## Арифметические и побитовые операторы. Оператора сравнения
TODO: add

## Логические операторы
TODO: add

## Инкремент и декремент
TODO: add

## Присваивания и составные присваивания
TODO: add

---

# Выражения и операторы. Управляющие конструкции. Ошибки компиляции

## Понятия `lvalue` и `rvalue`
TODO: add

## Тернарный оператор
```
int main() {
  int a {5};
  int b {8};
  int c = a > b ? a - b : a + b;
 
  std::cout << "c = " << c << std::endl; // c = 13
}
```

```
x < 2 ? x = 1 : ++x;
```
```
x = (a == 1 ? 1 : 2);
```

## Оператор запятая
OK:
```
x = 1, y = 5, ++z;
```
```
(x = 5, ++x) = 1;
```

not OK:
```
(x = 5, x++) = 1;
```

## оператор `sizeof`
Получение вол-ва байт переменной в памяти:
```
int x = 1;
std::cout << sizeof x << '\n' // 4
```

## Приоритет операторов
[C++ Operator Precedence](https://en.cppreference.com/w/cpp/language/operator_precedence.html)

## Управляющие конструкции (Control statements)
### *if*
```
if (bool-expr) statement; {}
[else statement;]
```

### *while*
```
while (bool-expr) statement;
```

### *while-do*
```
do statement; while (bool-expr);
```

### *for*
```
for (init-statement; bool-expr; expr) statement;
```

### *switch*
```
switch (x) {
  case 1: statements; break;
  case 2: statements; break;
  default: statements; break;
}
```

## Errors
### Compilation Error
TODO: add

---

# Runtime errors, undefined behaviour. Указатели и операции над ними.

## Errors (продолжение)
### Runtime Error
#### Segmentation Fault
```
int main() {
  std::vector<int> v(10);
  v[1000000000] = 123;
  std::cout << v[1000000000] << std::endl;
}
```
Произошла ошибка `segmentation fault` произошла по причине того, что программа вышла за пределы выделенного сегмента памяти.

### Undefined Behaviour
Неопределенное поведение происходит при
- обращении за границу массива
- переполнение int
и т.д.

#### Пример
```
int main() {
  for (int i = 0; i < 300; ++i) {
    std::cout << i << ' ' << i*12345678 << std::endl;
  }
}
```

Скомпилировать с флагом `-O2`, что дает оптимиззацию компиляции
```
g++ -O2 main.cpp
```

В результате будет выводиться бесконечное кол-во строк. Поскольку оптимизация работает примерно так:
- Компилятор предполагает, что i*12345678 не превысит INT_MAX;
- INT_MAX / 12345678 = 173
- Поскольку 300 > 173, то компилятор не проверяет `i < 300`, поскольку `i < 300 == true` при том, что компилятор думает, что i все равно больше 173

### Unspecified Behaviour
TODO: add

## Почему C++ быстрее остальных языков
Поскольку в *C++* нет лишних проверок (проверка переполнения int, проверка выхода за пределы массива и т.д. [в других языках есть проверка и выкидание ошибки, в C++ не выкидывает ошибки])

---
# Раздел №2

## Указатели, операции взятия адреса и разыменования
### Указатель, и указатель на указатель
```
int main() {
  int a = 2;
  int* p = &a;
  std::cout << p << '\n';

  int** pp = &p;
  std::cout << pp << '\n';

  std::cout << (pp - p) << '\n';
}
```

### Получение значения по адресу
```
int main() {
  int a = 2;
  int* p = &a;
  std::cout << p << " -> " << *p << '\n';

  int** pp = &p;
  std::cout << pp << " -> " << *pp << '\n';
  std::cout << pp << " -> " << *pp << " -> " << **pp << '\n';
}
```

## Реализация функции `swap`
```
void swap(int* a, int* b) {
  int temp = *a;
  *a = *b;
  *b = temp;
}

int main() {
  int a = 1;
  int b = 2;
  swap(&a, &b);
  std::cout << a << ' ' << b << std::endl;
}
```

## Арифметические операции над указателями
Арифметические операции над указателями:
- `++`
- `--`
- `+n`
- `-n`
- `a - b`

Допустим:
```
int a = 2;
int* p = &a;
```
Пусть, `p = 0x16d1d7104`, тогда `p + 1` равен `0x16d1d7108`, т.е. к адресу прибавилось число 4, поскольку тип int занимает 4 байта.

---

# Виды памяти. Массивы. Перегрузка функций

## `NULL` vs `nullptr`
- `NULL` – это наследие *C*, в качестве нулевого указателя используется *0*
- `nullptr` – это предпочтительны ключевое слово, введенное в *C++11* для описания константы нулевого указателя.

## Виды памяти
Существуют несколько видов памяти для C++. Основные из них – `data`, `text`, `stack`.

### `data` или `static memory`
Память `data` (или `static memory`) используется для хранения статичных данных, таких как статичные переменные и т.д. При запуске, еще до вызова *main()*, статические данные загружаются в `data`, и хранятся там до завершения программы.

### `text`
В этой памяти хранятся инструкии скомпилированного кода.

### `stack` или `автоматическая память`
Стэковая память используется для хранения переменных, созданных в течении работы программы. В стэковой памяти есть указатель, который указывает на первую свободную ячейку памяти. Когда программа выходит из области видимости, то указатель передвигается на область, на которой он был до момента входа в область видимости. Стэковая память имеет фиксированный объем, обычно - *8МБ* или *16МБ*.

## Адрес возврата
При вызове функции в стек заносится адрес возврата – адрес в памяти (*text memory*) следующей инструкции приостановленной программы и управление передается функции. При последующем вложенном или рекурсивном вызове, прерывании функции в стек заносится очередной адрес возврата и т. д.

При возврате из функции, адрес возврата снимается со стека и управление передается на следующую инструкцию приостановленной (под-)программы.

## Переполнение стека (**stack overflow**)
Переполнение стека возникает, когда в стековой памяти хранится больше информации, чем он может вместить. Обычно ёмкость стека задаётся при старте программы/потока. Когда указатель стека выходит за границы, программа аварийно завершает работу.

Простейший пример бесконечной рекурсии на *C*:
```
int foo() {
     return foo();
}
```

## Динамическая память
**Динамическая память** – это память, которая выделяется в *RAM* оперативной системой в *runtime*. В динамической памяти хранятся объекты, которые были созданы с потомщью оператора `new`;

### Оператор `new`
Примеры объектов, созданных в динамической памяти:
```
T* p = new T(...) // T - это какой-то класс
int* a = new int(3)
vector<int> b = {1, 2, 3, 4}; // vector хранится в динамической памяти
```

### Оператор `delete` и проблема утечка памяти
После того, как необходимость в объекте отпадает, то надо удалить объект (сообщить операционной системе, что надо определенный участок памяти отвязать от программы) с помощью `delete`:
```
delete a;
```

Если не удалять объект, то это приведет к утечке памяти (*momery leak*).

### `vector<>`
В векторе уже встроено удаление объекта из динамической памяти.

## Массивы в стиле *C*

### Создание массива в стековой памяти
Создание массива в стековой памяти:
```
int a[3] = {1, 2, 3};
```

### Создание массива в динамической памяти
```
int* b = new int[1000];
*(b+1) = 5; // то же самое, что и b[1] = 5;

int* c = b + 5; // 'c' указывает на элемент под индексом 5
c[-3] = 1; // то же самое, что и b[2] = 1;

b[3] = 4; // то же самое, что и *(b + 3) = 4;
4[b] = 5; // то же самое, что и *(4 + b) = 5;
```

### Удаление массива в динамической памяти
```
delete[] b;
```

### Массивы переменной длины (*variable length arrays*)
Не желательно создавать массивы переменной длины в стековой памяти:
```
int n;
std::cin >> n;

int a[n];
```
Поскольку это не прописано в спецификации.

## Array-to-pointer conversion
Мы можем работать с массивом как с указателем, но не наоборот:
```
int a[100];
int* b = new int[100];

b = a; // OK
a = b; // NOT OK
```

Но:
```
sizeof a == 400; // true -> поскольку массив 'a' занимает 400 байт
sizeof b == 4; // true -> поскольку указатель указывает только на первую ячейку памяти

b = a;
sizeof b == 4; // true -> даже после конверсии, 'b' остается указателем на первую ячейку
```

## Перегрузка функций
Можно создать одинаковые функции, но с разными параметрами:
```
int f(double) {
  return 1;
}

int f(int) {
  return 2;
}

int main() {
  std::cout << f(1) << ' ' << f(1.0) << std::endl;  // 2 1
}
```

Правила перегрузки можно посмотреть [здесь](https://en.cppreference.com/w/cpp/language/overload_resolution.html)

---

# Указатели на функции. Ссылки. Константы

## Функции с переменным числом аргументов и с аргументами по умолчанию

### Функции с переменным числом аргументов (*Variadic functions*)
TODO: add

### Функции c аргументами по умолчанию
```
double f(int x, double d = 0.5) {
  return x * d;
}

int main() {
  std::cout << f(10) << '\n';  // 5
  std::cout << f(10, 0.7) << '\n';  // 7
}
```

Нельзя перегружать функции c аргументами по умолчанию с меньшим кол-вом аргументов:
```
// Compilation error
double f(int x) {
  return x * 0.5;
}
```

Можно перегружать функции c аргументами по умолчанию с одинаковым кол-вом аргументов:
```
// Compiles
double f(double x, double d = 2) {
  return x * d;
}
```

## Указатели на функции, пример применения для сортировки
```
double f(int x, double d) {
    return x * d;
}

int main() {
  double (*p)(int, double) = &f;

  std::cout << p << std::endl;  // always prints '1' for function pointer
  std::cout << p(3, 4) << std::endl;  // 12
}
```

### Применение указателя на функцию как аргумент
```
// Сортировка по близости к числу 5
bool compare(int x, int y) {
    return abs(x - 5) < abs(y - 5);
}

int main() {
  int nums[10] = {5, 3, 6, 7, 10, 2, 3, 6, 7, 3};
  std::sort(nums, nums + 10, &compare);
  //std::sort(nums, nums + 10, compare);

  for (auto num: nums) {
    std::cout << num << ' ';
  }

  std::cout << std::endl;
}
```

## *inline* функции (Встраиваемые функции)
Ключевое слово *inline* встраивает машинный код функции не отдельно, а в линии, где оно будет вызвано.

**NOTE**: Не желательно использовать *inline*.

## Ссылки
```
int a = 5;
int& b = a;

vector<int> v = {1, 2, 3};
vector<int>& v2 = v;
```

На данный момент, мы должны воспринимать так: ссылки - это то же самое, что и переменная. Если изменяется одно, то и другое изменяется (вернее, не изменяется, а просто ссылается на один адрес в памяти).

## Особенности инициализации ссылок
```
void f(int a) {
  a += 1;
}

void f(int& a) {
  a += 1;
}

int a = 1;

f(a);  // Compilation error
int& r;  // Compilation error
int& r = 5;  // Compilation error
int&* p = &a;  // Compilation error
```

## Перегрузка функций с аргументами с сылками и без
**Compilation Error:**
```
void f(int a) {
  ...
}

void f(int& a) {
  ...
}
```

## Реализация *swap* с использованием ссылок
```
void swap(int& a, int& b) {
  int t = a;
  a = b;
  b = t;
}

int main() {
  int a = 4;
  int b = 6;
  swap(a, b);
  std::cout << "a = " << a << '\n';
  std::cout << "b = " << b << std::endl;
}
```

## Как компилятор реализовывает ссылки
```
void f(int &r);

int main() {
    int x = 0;
    int *p = &x;
    int &r = x;
    int y = 1;
    double d = 3.14;

    std::cout << &d << ' ' << &y << ' ' << &p << ' ' << &x << ' ' << '\n';
    f(x);
}

void f(int &r) {
    int x = 0;
    int *p = &x;
    std::cout << &r << '\n';
    std::cout << &x << ' ' << &p << '\n';

    std::cout << &p - 1 << ' ' << *(&p - 1) << '\n';
    std::cout << &p - 2 << ' ' << *(&p - 2) << '\n';
    std::cout << &p - 3 << ' ' << *(&p - 3) << '\n';

    for (int i = 4; i <= 200; i++) {
        std::cout << &p - i << ' ' << *(&p - i) << '\n';
    }
}
```

## Возвращение ссылок из функций, проблема *dangling reference*
```
int& f(int& x) {
  ++x;
  return x;
}

int& f(int x) {
  ++x;
  return x;
}

int main() {
  int a = 3;
  f(a);  // OK

  g(a);  // Undefined Bahaviour
}
```

## Почему в C++ не получается решить проблему ссылок так же, как в языках со сборкой мусора
TODO: add

## Константы, как правильно понимать концепцию констант
```
const int a = 5;
const vector<int> v = {1, 2, 3};
```

Константные типы – это другой тип, в котором отсутствуют операции изменения.

**Note:** `int` и `const int` – разные типы!

## Константные указатели и указатели на константу
### Указатели на константу
```
const int a = 5;
const int* p = &a;

++p; // OK
++*p; // Compilation Error
```

### Константные указатели
```
int b = 10;
int* const pp = &b;
++pp; // Compilation Error
```

### Можно сделать каст константный указатель на константный указатель на константу
```
const int* const ppp = pp; // Implicit cast (неявное привидение)
```

### Можно сделать каст константный указатель на константу на указатель на константу
```
const int* qq = ppp; // Because it's a copy
```

### Нельзя сделать каст константный указатель на константу на константный указатель
```
int* const q = ppp; // Compilation Error
```

## Константные ссылки
```
int a = 5;
int& r = a;
const int& cr = a; // OK
int& const ccr = a; // Illegal expression

++a;
std::cout << cr; // 6
```

## Три вида передачи аргументов: по значению, по ссылке и по константной ссылке
### По значению
```
void f(int a) {
  ...
}
```

### По ссылке
```
void f(vector<int> &a) {
  ...
}
```

### По константной ссылке
```
void f(const vector<int> &a) {
  ...
}
```

**Note:** Если вы передаете примитивный тип, то надо передавать по значению, поскольку хранение адреса требует больше памяти чем сам примитивный объект

## Почему константные ссылки могут продлевать жизнь объектов, а неконстантные – не могут
### Продление жизни объектов
```
const std::string& ref = std::string("hello");
std::cout << ref << '\n'; // "hello"
```

Без такого правила ссылочная переменная ref стала бы висячей (dangling reference) сразу после создания, потому что временный объект по умолчанию уничтожается в конце выражения, а мы пытались бы читать из уже освобождённой памяти. Правило «продления» позволяет безопасно держать ссылку на временный объект до конца жизни ссылки.

### Продление жизни объектов при передаче аргумента функции
```
void f(const int& a) {
    std::cout << a << std::endl;
}

int main() {
    int a = 12;

    f(12);
}
```

### Почему?
TODO: Add


# Приведения типов. Классы и структуры. Модификаторы доступа

## Вкратце о способах приведения типов в *C*
```
int a = 5;
double d = (double) d;
```

## `static_cast`
*static_cast* работает, если есть известное компилятору преобразование. Самое лучшее приведение типа
```
int a = 5;
double d = static_cast<double>(a);
```

## `reinterpret_cast`
*reinterpret_cast* позволяет преобразовать любой тип в любой тип. Это самый «низкоуровневый» из встроенных операторов приведения типов. В отличие от других кастов, он буквально переинтерпретирует биты одного объекта как объект другого типа, не меняя их содержимого. 
```
int a = 5;
double d2 = *reinterpret_cast<double*>(&a); // скорей всего будет не 5
```
Здесь каст происходит тупо. Допустим, *int* занимает *32* бита, а *double* занимает *64* бита, то каст берет *32* бита от *int*, затем еще берет соседние *32* бита. Из-за этого после преобразования, d2 будет скорей всего не числом *5*.

## `const_cast`
`const_cast` служит исключительно для добавления или удаления квалификаторов *const*.

### Добавление *const*
```
int x = 5;
const int& z = const_cast<const int&>(x);
```

Допустим, что есть две перегруженные функции, одна с *const*, тогда вызовется функция с *const*:
```
void f(int& a) {
  ...
}

// вызовется эта функция
void f(const int& a) {
  ...
}

int main() {
  int x = 5;
  const int& z = const_cast<const int&>(x);

  f(z);
}
```

### Удаление *const*
```
const int a = 5;
int& b = const_cast<int&>(a);
++b; // Возможно изменится (зависит от компилятора), но это UB (Undefined Bahaviour)
```

### Когда удаление *const* можно делать
Удаление *const* можно делать, если изначально он был не *const*:
```
int a = 5;
const int& b = const_cast<const int&>(a);
int& c = const_cast<int&>(b);
++c;
```

## *C*-стайл каст
Самый «универсальный» и самый «опасный» оператор приведения типов.
```
int i = 42;
double d = (double)i;    // классический С-стиль: явно указываем тип-приёмник
```

Когда компилятор встречает запись (T)expr (или T(expr)), он по стандарту пытается подобрать первый из следующих вариантов (в таком же порядке), который компилируется:
1. `const_cast<T>(expr)`
2. `static_cast<T>(expr)`
3. `reinterpret_cast<T>(expr)`
4. `const_cast<T>(static_cast<U>(expr))` или `static_cast<T>(const_cast<U>(expr))`
5. `dynamic_cast<T>(expr)`

---

# Раздел №3: Введение в ООП
## Классы и структуры, поля и методы

### Создание структуры:
```
struct S {
  int x;
  char c;
  std::string str;

  int f(double d) {
    std::cout << "Ahaha!" << d << '\n';
    return x + d;
  }
};

int main() {
  S s;
  std::cout << s.x << '\n';
  std::cout << s.str.size() << '\n';

  std::cout << s.f(3.14) << '\n';
}
```

### Создание класса:
```
class S {
public:
  int x;
  char c;
  std::string str;

  int f(double d) {
    std::cout << "Ahaha!" << d << '\n';
    return x + d;
  }
};

int main() {
  S s;
  std::cout << s.x << '\n';
  std::cout << s.str.size() << '\n';

  std::cout << s.f(3.14) << '\n';
}
```

### Различия класса и структуры
`struct` и `class` – это почти одно и то же, только в *struct* по умолчанию все функции и поля публичные, а в *class* по умолчанию все функции и поля приватные.

## Инициализация полей в структуре
```
struct S {
  int a = 1;
  int b = 2;
};
```

## Внутренние и локальные классы (структуры)
```
struct S {
  struct SS {
    int a = 1;
    int b = 2;
  };
};

int main() {
  struct Strange {
    int a = 3;
    int b = 4;
  };

  Strange strange;
  std::cout << strange.a << '\n';
  std::cout << strange.b << '\n';

  S::SS ss;
  std::cout << ss.a << '\n';
  std::cout << ss.b << '\n';
}
```

## Оператор стрелочка
```
struct S {
  int a = 1;
  int b = 2;
};

int main() {
  S s;
  S* p = &s;

  std::cout << p->a << '\n';
  std::cout << p->b << '\n';
}
```

## Агрегатная инициализация структур
```
struct S {
  int a;
  int b;
};

int main() {
  S s{1, 2};

  std::cout << s.a << '\n';
  std::cout << s.b << '\n';
}
```

## Определение метода вне тела структуры
```
struct S;

struct S {
  int a = 0;
  int b = 1;

  int f(double d);
};

int S::f(double d) {
  std::cout << "Ahaha!" << d << '\n';
  return a + d;
}

int main() {
  S s;

  std::cout << s.f(3.14) << std::endl;
}
```

## Ключевое слово `this`
Ключевое слово `this` – это оператор на текущий объект:
```
struct S;

struct S {
  int a = 2;
  int b = 3;

  S get() {
    std::cout << "Get Called!\n";
    return *this;
  }
};

int main() {
  S s;

  std::cout << s.get().a << std::endl;
}
```

## Размещение структур в памяти, эффекты от перестановки полей
Поля в структуре размещаются в том порядке, в котором они записаны:

### 24 байт
```
struct S {
  int a;
  double d;
  int b;
};

int main() {
  std::cout << sizeof(S) << '\n';
}
```

### 16 байт
```
struct S {
  int a;
  int b;
  double d;
};

int main() {
  std::cout << sizeof(S) << '\n';
}
```

## Понятие инкапсуляции, модификаторы доступа `private` и `public`
Инкапсуляция – механизм языка *C++*, ограничивающий доступ к составляющим объект компонентам (методам и атрибутам), делает их защищенными или приватными, то есть доступными только внутри объекта.

### Модификаторы доступа
- `public` – открытая часть класса
- `private` – скрытая от внешнего кода часть класса

```
struct S {
private:
  int a;
public:
  int b;
};
```

## Принцип "сначала разрешение перегрузки, потом проверка доступа"
```
class S {
private:
  int a;
  void f(int) {
    std::cout << 1;
  }

public:
  double d;
  void f(double) {
    std::cout << 2;
  }
};

int main() {
  S s;

  s.f(0);
}
```

## Функции-друзья и классы-друзья (Дружественные функции и классы)

### Дружественные функции
Дружественные функции – это функции, которые не являются членами класса, однако имеют доступ к его закрытым членам – переменным и функциям, которые имеют спецификатор `private`.
```
class S {
private:
  friend void g(int);

  int a;
  void f(int) {
    std::cout << 1;
  }

public:
  double d;
  void f(double) {
    std::cout << 2;
  }
};

void g(int) {
  S s;
  s.f(0);
}

int main() {
  g(0);
}
```

**Note:** Чем меньше дружественных функций, тем лучше.

### Дружественные классы
TODO: Add

## Понятие конструкторов, примеры простейших конструкторов
```
class Complex {
private:
  double re;
  double im;

public:
  Complex(double r, double i) {
    re = r;
    im = i;

    std::cout << "Constructor called\n";
  }
};

int main() {
  Complex c(1, 2);
}
```

## Списки инициализации полей в конструкторах
```
class Complex {
private:
  double re = 0.0;
  double im = 0.0;

public:
  Complex(double r, double i): re(r), im(i) {}
  Complex(double r): re(r) {}
};

int main() {
  Complex c1(1.2, 2.3);
  Complex c2(3.5);
}
```

**Node:** списки инициализации полей в конструкторах предпочтительней чем предыдущий вариант. Используя списки инициализации полей поля сразу инизиализируются аргументами, и не будут изначально инициализироваться дефолтными значениями. Эта разница сильнее заметна если использовать не примитивные типы в полях, а например *string* или *vector*.

---

# Конструкторы, деструкторы, оператор присваивания и "правило трех"

## Конструкторы по умолчанию
Конструктор по умолчанию создается, если других конструкторов нет.
```
class Complex {
private:
  double re = 0.0;
  double im = 0.0;
};

int main() {
  Complex c;
}
```

## Пример, когда нужен нетривиальный конструктор
```
class String {
private:
  char* str = nullptr;
  size_t sz = 0;

public:
  String(size_t n, char c): str(new char[n]), sz(n) {
    memset(str, c, n); // выделение ("захват") памяти
  }

  void print() {
    for (size_t i = 0; i < sz; ++i)
      std::cout << str[i];
    std::cout << '\n';
  }
};

int main() {
  String s(10, 'b');
  s.print();
}
```

## Понятие деструктора и пример, когда нужен нетривиальный деструктор
Деструктор вызывается, когда объект выходит из области видимости.
```
class String {
private:
  char* str = nullptr;
  size_t sz = 0;

public:
  String(size_t n, char c): str(new char[n]), sz(n) {
    memset(str, c, sz); // выделение ("захват") памяти
  }

  ~String() {
    std::cout << "Destructor called" << '\n';
    delete[] str;
  }

  void print() {
    for (size_t i = 0; i < sz; ++i)
      std::cout << str[i];
    std::cout << '\n';
  }
};

int main() {
  String s(10, 'a');
  {
    String s(10, 'b');
    s.print();
  }

  s.print();
}
```

## Порядок вызова конструкторов и деструкторов в разных ситуациях
Порядок вызова конструкторов и деструкторов такой же, как и у стека.

## Конструктор копирования и его особенности
```
class String {
private:
  char* str = nullptr;
  size_t sz = 0;

public:
  String(size_t n, char c): str(new char[n]), sz(n) {
    memset(str, c, sz); // выделение ("захват") памяти
  }

  String(const String& s): str(new char[s.sz]), sz(s.sz) {
    memcpy(str, s.str, sz);
  }

  ~String() {
    std::cout << "Destructor called" << '\n';
    delete[] str;
  }

  void print() {
    for (size_t i = 0; i < sz; ++i)
      std::cout << str[i];
    std::cout << '\n';
  }
};

int main() {
  String s(10, 'a');
  String s_copy = s;

  s_copy.print();
}
```

## Ключевые слова `default` и `delete`

### Ключевое слово `default`
Можно попросить компилятор создать конструктор по умолчанию.
```
class String {
private:
  char* str = nullptr;
  size_t sz = 0;

public:
  // конструктор по умолчанию
  String() = default;
  String(size_t n, char c): str(new char[n]), sz(n) {
    memset(str, c, sz); // выделение ("захват") памяти
  }

  // конструктор копирования по умолчанию
  String(String& s) = default;

  String(const String& s): str(new char[s.sz]), sz(s.sz) {
    memcpy(str, s.str, sz);
  }

  ~String() {
    std::cout << "Destructor called" << '\n';
    delete[] str;
  }

  void print() const {
    for (size_t i = 0; i < sz; ++i)
      std::cout << str[i];
    std::cout << '\n';
  }
};

int main() {
  String s1;
  s1.print();

  String s2(5, 'a');
  s2.print();

  String s3 = s2;
  s3.print();
}
```

Но в этом примере будет ошибка, поскольку,
```
String(String& s) = default;
```
это «поверхностный» (shallow) копирующий конструктор: он просто копирует значение указателя str и поле sz. В результате и у `s2`, и у `s3` одно и то же поле `str` указывает на один и тот же буфер в куче.

### Ключевое слово `delete`
Ключевое слово `delete` – запрещает создавать конструктор
```
class String {
private:
  char* str = nullptr;
  size_t sz = 0;

public:
  // Запретить создавать такой конструктор
  String() = delete;

  void print() const {
    for (size_t i = 0; i < sz; ++i)
      std::cout << str[i];
    std::cout << '\n';
  }
};

int main() {
  String s1;
  s1.print();
}
```

## Делегирующие конструкторы
```
class String {
private:
  char* str = nullptr;
  size_t sz = 0;

public:
  String() = delete;

  String(size_t n, char c): str(new char[n]), sz(n) {
    memset(str, c, sz);
  }

  // Делегирующий конструктор
  String(const String& s): String(s.sz, '\0') {
    memcpy(str, s.str, sz);
  }

  void print() const {
    for (size_t i = 0; i < sz; ++i)
      std::cout << str[i];
    std::cout << '\n';
  }
};

int main() {
  String s1(5, 'a');
  s1.print();

  String s2 = s1;
  s2.print();
}
```

## Оператор присваивания и его особенности
```
class String {
private:
    char* str = nullptr;
    size_t sz = 0;

public:
    String() = delete;

    String(size_t n, char c): str(new char[n]), sz(n) {
        memset(str, c, sz);
    }

    String(const String& s): String(s.sz, '\0') {
        memcpy(str, s.str, sz);
    }

    // Оператор присваивания
    String& operator=(const String& s) {
        std::cout << "operator= called" << '\n';

        delete[] str;
        str = new char[s.sz];
        sz = s.sz;
        memcpy(str, s.str, sz);
        return *this;
    }

    ~String() {
        std::cout << "Destructor called" << '\n';
        delete[] str;
    }

    void print() const {
        std::cout << str << '\n';
    }
};

int main() {
    String s1(5, 'a');
    String s2(5, 'b');

    // Присваивание
    s2 = s1;
    s2.print();
}
```

## Оператор присваивания: идиома `copy & swap`
```
class String {
private:
    char* str = nullptr;
    size_t sz = 0;

public:
    String() = delete;

    String(size_t n, char c): str(new char[n]), sz(n) {
        memset(str, c, sz);
    }

    // Оператор копирования
    String(const String& s): String(s.sz, '\0') {
        memcpy(str, s.str, sz);
    }

    void swap(String& s) {
      std::swap(str, s.str);
      std::swap(sz, s.sz);
    }

    // Оператор присваивания (Copy & Swap)
    String& operator=(const String& s) {
        std::cout << "operator= called" << '\n';

        // Копирование
        String copy = s;
        swap(copy);
        return *this;
    }

    ~String() {
        std::cout << "Destructor called" << '\n';
        delete[] str;
    }

    void print() const {
        std::cout << str << '\n';
    }
};

int main() {
    String s1(5, 'a');
    String s2(5, 'b');

    // Присваивание
    s2 = s1;
    s2.print();
}
```

## Rule of Three
Ранее, мы определяли два класса (структуры): Complex и String. Если для **Complex** можно использовать конструктор копирования, деструктор и оператор присваивания по умолчанию (*default*), то для **String** надо использовать нетривиальные конструктор копирования, деструктор и оператор присваивания.

[**Правило трех**](https://ru.wikipedia.org/wiki/%D0%9F%D1%80%D0%B0%D0%B2%D0%B8%D0%BB%D0%BE_%D1%82%D1%80%D1%91%D1%85_(C%2B%2B)) – если надо создать хотя бы один нетривиальный из трех (конструктор копирования, деструктор и оператор присваивания), то надо создать все три.

## Оператор присваивания `lvalue` и `rvalue`
### Оператор присваивания `lvalue`
```
String& operator=(const String& s) & {
  std::cout << "operator= called" << '\n';

  // Копирование
  String copy = s;
  swap(copy);
  return *this;
}
```

### Оператор присваивания `rvalue`
```
String& operator=(const String& s) && {
  std::cout << "operator= called" << '\n';

  // Копирование
  String copy = s;
  swap(copy);
  return *this;
}
```

---

# Перегрузка операторов, константные и статические методы
## Какие операторы можно перегружать
- `+`, `-`, `*`, `/`, `%`
- `>`, `<`, `==`, `<=`, `>=`
- `<<`, `>>`, `&`, `|`, `^`
- `&&`, `||`, `,`
- `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `<<=`, `>>=`, `^=`, `&=`, `|=`
- `()`, `[]`
- `*` [pointer], `&` [reference]

## Какие операторы нельзя перегружать
- `.`, `::`, `?:`

## Два способа перегрузить бинарный оператор
### Способ #1
```
class BigInt {
  BigInt operator+(const BigInt&); // binary operator
};

int main() {
  BigInt a;
  BigInt b;

  a + b;
  a.operator+(b);
}
```

### Способ #2
```
class BigInt {
  ...
};

BigInt operator+(const BigInt&, const BigInt&);

int main() {
  BigInt a;
  BigInt b;

  a + b;
}
```

### Пример: операторы `+` и `+=`
### Бинарный оператор `+`
```
class BigInt {
private:
  int a;

public:
  explicit BigInt(int a): a(a) {}

  BigInt operator+(const BigInt &other) const {
    return BigInt(a + other.a);
  }

  friend std::ostream &operator<<(std::ostream &os, const BigInt &obj) {
    return os << obj.a;
  }
};

int main() {
  BigInt a(10);
  BigInt b(13);

  BigInt c = a + b;
  std::cout << "Result: " << c << std::endl;
}
```

### Бинарный оператор `+=`
```
class BigInt {
private:
  int a;

public:
  explicit BigInt(int a): a(a) {}

  BigInt &operator+=(const BigInt &other) {
    a += other.a;
    return *this;
  }

  friend std::ostream &operator<<(std::ostream &os, const BigInt &obj) {
    return os << obj.a;
  }
};

int main() {
  BigInt a(10);
  BigInt b(13);

  a += b;
  std::cout << "Result: " << a << std::endl;
}
```

## Операторы сравнения: 0:15:20
```
class BigInt {
private:
  int a;

public:
  explicit BigInt(int a): a(a) {}

  bool operator<(const BigInt &other) const {
    return a < other.a;
  }

  bool operator>(const BigInt &other) const {
    return a > other.a;
  }

  friend std::ostream &operator<<(std::ostream &os, const BigInt &obj) {
    return os << obj.a;
  }
};

int main() {
  BigInt a(15);
  BigInt b(13);

  if (a < b) {
    std::cout << a << " < " << b << std::endl;
  }

  if (a > b) {
    std::cout << a << " > " << b << std::endl;
  }
}
```

## Spaceship operator
TODO: Add

## Операторы побитового сдвига
```
class BigInt {
private:
  int a;

  friend std::ostream& operator<<(std::ostream& out, const BigInt& x);

public:
  explicit BigInt(int a): a(a) {}
};

std::ostream& operator<<(std::ostream& out, const BigInt& x) {
  return out << x.a;
}

int main() {
  BigInt a(15);

  std::cout << a << std::endl;
}
```

## Остальные операторы
TODO: Add

## increment/decrement
```
class BigInt {
private:
  int a;

  friend std::ostream& operator<<(std::ostream& out, const BigInt& x);

public:
  explicit BigInt(int a): a(a) {}

  // prefix
  BigInt& operator++() {
    ++a;
    return *this;
  }

  // postfix
  BigInt operator++(int) {
    const BigInt copy = *this;
    ++a;
    return copy;
  }
};

std::ostream& operator<<(std::ostream& out, const BigInt& x) {
  return out << x.a;
}

int main() {
  BigInt a(15);

  std::cout << ++a << std::endl;
  std::cout << a++ << std::endl;
  std::cout << a << std::endl;
}
```

## Константные и неконстантные методы
```
class ArrayWrapper {
private:
  size_t sz = 0;
  int* arr = nullptr;

public:
  ArrayWrapper(size_t n, const int a): arr(new int[n]), sz(n) {
    std::fill_n(arr, sz, a);
  }

  size_t size() const {
    return sz;
  }

  void print() const {
    for (size_t i = 0; i < sz; i++) {
      std::cout << arr[i] << " ";
    }

    std::cout << std::endl;
  }
};

int main() {
  const ArrayWrapper a(15, 4);

  a.print();
}
```

### Что делают константные функции
В константных функциях полям присваивается `const`. Но есть один минус, `int* const arr` – это константный указатель, и можно изменить какой-нибудь элемент.

Как выглядит `ArrayWrapper` для константной функции:
```
class ArrayWrapper {
private:
  size_t sz = 0;
  int* arr = nullptr;
};
```

Пример того, как можно изменить элемент массива в константной функции:
```
class ArrayWrapper {
private:
  size_t sz = 0;
  int* arr = nullptr;

public:
  void print() const {
    arr[2] = 23; // OK
    sz += 1; // Error

    for (size_t i = 0; i < sz; i++) {
      std::cout << arr[i] << " ";
    }

    std::cout << std::endl;
  }
};
```

## `mutable` поля
`mutable` полями обозначаются поля, которые могут быть изменены в константных методах. Например, пусть у нас есьб дерево, и массив и рамер не могут быть изменены. И пусть у нас есть константный метод, который проходит по дереву, и для проходя по дереву нам нужен изменяемый указатель (*current_i*) на текущий элемент.
```
class Tree {
private:
  size_t sz = 0;
  int* arr = nullptr;
  mutable int current_i = 0;
};
```

## Статические поля
```
struct {
  static int x;
};

int main() {
  S::x = 5;
}
```

## Статические методы
```
struct {
  static void f();
};

int main() {
  S::f();
}
```

## Пример: `Singleton`
```
class C {
  static C* obj;
  C() {}

public:
  static C& GetObject() {
    if (obj) return *obj;
    obj = new C();
    return *obj;
  }
  static void destroy() {
    delete obj;
  }
};

C* C::obj = nullptr;

int main() {
  C c = C::GetObject();
}
```

## Custom type conversions
```
class BigInt {
  int a;

public:
  BigInt(int a): a(a) {}

  operator int() const {
    return a;
  }
}

int main() {
  int a = 123;

  // cast to BigInt
  BigInt b = static_cast<BigInt>(a);

  // cast to int
  int c = static_cast<int>(a);
  std::cout << c << std::endl;
}
```

## `explicit` keyword
Явно вызывать оператор или контсруктор.
```
class BigInt {
  int a;

public:
  explicit BigInt(int a): a(a) {}

  explicit operator int() const {
    return a;
  }
}

int main() {
  int a = 123;

  // cast to BigInt
  BigInt b = static_cast<BigInt>(a);

  // cast to int
  int c = static_cast<int>(a);
  std::cout << c << std::endl;
}
```

---

# Указатели на члены. Енумы. Наследование (начало)
## Указатели на члены (поле)
```
struct S {
    int x;
    double d;

    void f(char c);
};

int main() {
    int S::*p = &S::x;

    S s{5, 3.0};
    std::cout << sizeof(p) << ' ' << s.*p << std::endl;
}
```

## Указатели на методы
```
struct S {
    int x;
    double d;

    int f(char c) {
        return c - 'a';
    };
};

int main() {
    int (S::*pf)(char) = &S::f;

    S s{5, 3.0};
    std::cout << sizeof(pf) << ' ' << (s.*pf)('c') << std::endl;
}
```

Вызывать указатель на метод указателя на объект:
```
int main() {
    int (S::*pf)(char) = &S::f;

    S s{5, 3.0};
    S *ps = &s;

    std::cout << sizeof(pf) << ' ' << (ps->*pf)('c') << std::endl;
}
```

## Enum
```
enum class TaxiClass {
    ECONOM,
    COMFORT,
    COMFORT_PLUS,
    BUSINESS
};

int main() {
    TaxiClass taxi_comfort_class = TaxiClass::COMFORT;
    TaxiClass taxi_business_class = TaxiClass::BUSINESS;

    std::cout << static_cast<int>(taxi_comfort_class) << ' ' << static_cast<int>(taxi_business_class) << std::endl;
}
```

Можно так:
```
enum class TaxiClass {
    ECONOM = 1,
    COMFORT = 2,
    COMFORT_PLUS = 3,
    BUSINESS = 4
};
```

и так:
```
enum class TaxiClass:int16_t {
    ECONOM = 1,
    COMFORT = 2,
    COMFORT_PLUS = 3,
    BUSINESS = 4
};
```



















