# 🗺️ Полный роадмап C++ 2026 — от нуля до профессионала

> **Автор:** сгенерировано с помощью Claude AI  
> **Актуальность:** C++11/14/17/20/23  
> **Уровни:** от абсолютного новичка до эксперта

---

## Общая структура пути

```
Уровень 0: Фундамент и настройка окружения    (1-2 недели)
Уровень 1: Базовый C++                         (2-3 месяца)
Уровень 2: Средний C++ / ООП / STL             (3-4 месяца)
Уровень 3: Продвинутый C++                     (4-6 месяцев)
Уровень 4: Экспертный / Специализация          (бесконечно)
```

---

## Уровень 0 — Фундамент

### Настройка окружения

**Компиляторы:** GCC (Linux/Mac), MSVC (Windows), Clang (все платформы)

**IDE и редакторы:**
- **CLion** — лучший для обучения (платный, есть студенческая лицензия)
- **VS Code** + расширение C/C++ + CMake Tools — бесплатно
- **Visual Studio** — на Windows

**Установка на Linux/Mac:**
```bash
# GCC
sudo apt install g++ build-essential   # Ubuntu/Debian
brew install gcc                        # macOS

# CMake
sudo apt install cmake
brew install cmake
```

**Первый файл:**
```cpp
#include <iostream>

int main() {
    std::cout << "Hello, C++!" << std::endl;
    return 0;
}
```

**Компиляция:**
```bash
g++ -std=c++20 -Wall -Wextra -o hello hello.cpp
./hello
```

> Флаги `-Wall -Wextra` показывают предупреждения — всегда используй их!

---

## Уровень 1 — Базовый C++ (C++11/14/17)

### 1.1 Типы данных и переменные

```cpp
#include <iostream>
#include <string>

int main() {
    // Целочисленные типы
    int a = 42;
    long long big = 9'000'000'000LL;  // апостроф — разделитель (C++14)
    unsigned int positive = 100u;

    // Вещественные
    double pi = 3.14159265358979;
    float small = 3.14f;

    // Символы и строки
    char c = 'A';
    std::string s = "Hello";

    // Логический
    bool flag = true;

    // auto — вывод типа (используй часто!)
    auto x = 42;       // int
    auto y = 3.14;     // double

    // sizeof — размер типа в байтах
    std::cout << sizeof(int) << "\n";      // обычно 4
    std::cout << sizeof(double) << "\n";   // обычно 8

    return 0;
}
```

**Используй `int64_t`, `uint32_t` из `<cstdint>` когда нужен точный размер:**
```cpp
#include <cstdint>
int64_t score = 1'000'000'000'000LL;  // гарантированно 64 бита
```

### 1.2 Управляющие конструкции

```cpp
#include <iostream>

int main() {
    int x = 10;

    // if / else if / else
    if (x > 0) std::cout << "positive\n";
    else if (x < 0) std::cout << "negative\n";
    else std::cout << "zero\n";

    // switch
    switch (x) {
        case 10: std::cout << "ten\n"; break;
        default: std::cout << "other\n";
    }

    // for
    for (int i = 0; i < 5; ++i) std::cout << i << " ";

    // while
    int n = 5;
    while (n > 0) std::cout << n-- << " ";

    // range-based for (C++11) — используй по умолчанию!
    std::string word = "hello";
    for (char ch : word) std::cout << ch << " ";

    return 0;
}
```

### 1.3 Функции

```cpp
#include <iostream>
#include <string>
#include <tuple>

// Базовая функция
int add(int a, int b) { return a + b; }

// Параметры по умолчанию
void greet(const std::string& name, const std::string& prefix = "Hello") {
    std::cout << prefix << ", " << name << "!\n";
}

// Перегрузка функций
double multiply(double a, double b) { return a * b; }
int    multiply(int a, int b)       { return a * b; }

// Возврат нескольких значений (C++17)
std::tuple<int, int> divmod(int a, int b) {
    return {a / b, a % b};
}

// Рекурсия
long long factorial(int n) {
    return (n <= 1) ? 1 : n * factorial(n - 1);
}

int main() {
    greet("Alice");           // Hello, Alice!
    greet("Bob", "Hi");      // Hi, Bob!

    auto [quot, rem] = divmod(17, 5);  // C++17 structured bindings
    std::cout << quot << " " << rem << "\n";  // 3 2

    std::cout << factorial(10) << "\n";  // 3628800
    return 0;
}
```

### 1.4 Указатели и ссылки — ВАЖНЕЙШАЯ тема

```cpp
#include <iostream>

int main() {
    int x = 42;

    // Ссылка — псевдоним переменной
    int& ref = x;
    ref = 100;
    std::cout << x << "\n";  // 100

    // Указатель — хранит адрес
    int* ptr = &x;
    std::cout << *ptr << "\n";  // разыменование — 100
    *ptr = 200;
    std::cout << x << "\n";  // 200

    // nullptr (C++11) — не NULL!
    int* empty = nullptr;
    if (empty == nullptr) std::cout << "pointer is null\n";

    // const ссылка — читать без копирования (стандартный паттерн)
    auto print = [](const std::string& s) {
        std::cout << s << "\n";
    };

    return 0;
}
```

> **Правило:** Передавай большие объекты по `const&`, маленькие типы (int, double) — по значению.

### 1.5 std::vector и массивы

```cpp
#include <iostream>
#include <vector>
#include <array>
#include <algorithm>

int main() {
    // std::array — фиксированный размер (C++11)
    std::array<int, 5> arr = {1, 2, 3, 4, 5};

    // std::vector — динамический массив, используй по умолчанию
    std::vector<int> v = {3, 1, 4, 1, 5, 9, 2, 6};

    v.push_back(7);     // добавить в конец
    v.emplace_back(8);  // добавить на месте (эффективнее)
    v.pop_back();       // удалить последний
    v.reserve(100);     // зарезервировать память заранее

    // Доступ
    std::cout << v[0] << "\n";    // без проверки границ
    std::cout << v.at(0) << "\n"; // с проверкой, бросает исключение

    // Сортировка
    std::sort(v.begin(), v.end());
    std::sort(v.begin(), v.end(), std::greater<int>{});

    // Итерация
    for (const auto& elem : v) std::cout << elem << " ";

    // 2D вектор
    std::vector<std::vector<int>> matrix(3, std::vector<int>(3, 0));
    matrix[1][1] = 5;

    return 0;
}
```

### 1.6 Строки

```cpp
#include <iostream>
#include <string>
#include <sstream>

int main() {
    std::string s = "Hello, World!";

    std::cout << s.length() << "\n";       // 13
    std::cout << s.substr(0, 5) << "\n";   // Hello
    std::cout << s.find("World") << "\n";  // 7

    s.replace(7, 5, "C++");  // Hello, C++!
    s += " Great!";           // конкатенация

    // Конвертация
    int n = std::stoi("42");
    std::string ns = std::to_string(42);

    // std::string_view (C++17) — легковесный просмотр, не копирует
    std::string_view sv = s;
    std::cout << sv.substr(0, 5) << "\n";  // не выделяет память!

    return 0;
}
```

### Где учить уровень 1

| Ресурс | Тип | Ссылка |
|--------|-----|--------|
| learncpp.com | Бесплатный курс | https://www.learncpp.com |
| C++ Primer (Lippman) | Книга | Классика для новичков |
| The Cherno | YouTube | Очень доходчиво |
| cppreference.com | Справочник | Открывай всегда |
| Codeforces Div.4/3 | Практика | Задачи по алгоритмам |


---

## Уровень 2 — Средний C++

### 2.1 ООП — Классы и объекты

```cpp
#include <iostream>
#include <string>

class Animal {
private:
    std::string name_;
    int age_;

protected:
    double weight_;

public:
    // Конструктор с initializer list
    Animal(std::string name, int age, double weight)
        : name_(std::move(name))
        , age_(age)
        , weight_(weight)
    {
        std::cout << "Animal created: " << name_ << "\n";
    }

    virtual ~Animal() {
        std::cout << "Animal destroyed: " << name_ << "\n";
    }

    // Геттеры — const метод не меняет объект
    const std::string& name() const { return name_; }
    int age() const { return age_; }

    // Виртуальный метод — для полиморфизма
    virtual std::string sound() const { return "..."; }

    // Перегрузка оператора
    bool operator<(const Animal& other) const {
        return age_ < other.age_;
    }

    // Статический метод
    static std::string kingdom() { return "Animalia"; }
};

class Dog : public Animal {
    std::string breed_;
public:
    Dog(std::string name, int age, double weight, std::string breed)
        : Animal(std::move(name), age, weight)
        , breed_(std::move(breed))
    {}

    // override — явно указываем переопределение
    std::string sound() const override { return "Woof!"; }
    const std::string& breed() const { return breed_; }
};

int main() {
    Dog d("Rex", 3, 25.0, "German Shepherd");

    // Полиморфизм через указатель на базовый класс
    Animal* ptr = &d;
    std::cout << ptr->sound() << "\n";  // Woof!
    std::cout << Animal::kingdom() << "\n";
    return 0;
}
```

### 2.2 Правило пяти (Rule of Five)

Если управляешь ресурсом вручную — определи все пять:

```cpp
class Buffer {
    char* data_;
    size_t size_;
public:
    explicit Buffer(size_t size)
        : data_(new char[size]), size_(size) {}

    ~Buffer() { delete[] data_; }                        // 1. Деструктор

    Buffer(const Buffer& o)                              // 2. Конструктор копирования
        : data_(new char[o.size_]), size_(o.size_) {
        std::memcpy(data_, o.data_, size_);
    }

    Buffer& operator=(const Buffer& o) {                 // 3. Копирование =
        if (this != &o) {
            delete[] data_;
            data_ = new char[o.size_];
            size_ = o.size_;
            std::memcpy(data_, o.data_, size_);
        }
        return *this;
    }

    Buffer(Buffer&& o) noexcept                          // 4. Конструктор перемещения
        : data_(o.data_), size_(o.size_) {
        o.data_ = nullptr; o.size_ = 0;
    }

    Buffer& operator=(Buffer&& o) noexcept {             // 5. Перемещение =
        if (this != &o) {
            delete[] data_;
            data_ = o.data_; size_ = o.size_;
            o.data_ = nullptr; o.size_ = 0;
        }
        return *this;
    }
};
```

> **На практике:** Используй `std::vector`, `std::string`, `std::unique_ptr` — они реализуют это за тебя.

### 2.3 Умные указатели — забудь про raw new/delete

```cpp
#include <memory>

// unique_ptr — единственный владелец
auto p1 = std::make_unique<MyClass>(args...);
// p1 автоматически удаляется при выходе из scope
// нельзя скопировать, только переместить:
auto p2 = std::move(p1);  // p1 = nullptr

// shared_ptr — подсчёт ссылок
auto sp1 = std::make_shared<MyClass>(args...);
{
    auto sp2 = sp1;  // счётчик = 2
}  // sp2 уничтожен, счётчик = 1
// sp1 уничтожен, счётчик = 0 — объект удалён

// weak_ptr — наблюдатель без владения
std::weak_ptr<MyClass> wp = sp1;
if (auto locked = wp.lock()) {   // получаем shared_ptr если объект жив
    locked->doSomething();
}
```

> **Правило:** 90% случаев — `unique_ptr`. `shared_ptr` только при реальном совместном владении.

### 2.4 Шаблоны (Templates)

```cpp
#include <iostream>
#include <vector>
#include <concepts>  // C++20

// Шаблонная функция
template<typename T>
T max_val(T a, T b) { return (a > b) ? a : b; }

// Шаблонный класс
template<typename T>
class Stack {
    std::vector<T> data_;
public:
    void push(T val) { data_.push_back(std::move(val)); }
    void pop()       { data_.pop_back(); }
    const T& top() const { return data_.back(); }
    bool empty() const   { return data_.empty(); }
    size_t size() const  { return data_.size(); }
};

// Variadic templates (C++11) + fold expression (C++17)
template<typename... Args>
void print_all(Args&&... args) {
    (std::cout << ... << args) << "\n";
}

// Концепты (C++20) — ограничения на шаблонные параметры
template<typename T>
requires std::integral<T>
T gcd(T a, T b) {
    while (b) { a %= b; std::swap(a, b); }
    return a;
}

int main() {
    std::cout << max_val(3, 7) << "\n";        // 7
    std::cout << max_val(3.14, 2.72) << "\n";  // 3.14

    Stack<std::string> ss;
    ss.push("hello"); ss.push("world");
    std::cout << ss.top() << "\n";  // world

    print_all("a=", 42, " b=", 3.14);  // a=42 b=3.14
    std::cout << gcd(48, 18) << "\n";  // 6
    return 0;
}
```

### 2.5 STL контейнеры и алгоритмы

```cpp
#include <map>
#include <unordered_map>
#include <set>
#include <queue>
#include <algorithm>
#include <numeric>
#include <ranges>  // C++20

int main() {
    // map — отсортированный, O(log n)
    std::map<std::string, int> scores;
    scores["Alice"] = 95; scores["Bob"] = 87;
    for (const auto& [name, score] : scores)  // structured binding
        std::cout << name << ": " << score << "\n";

    // unordered_map — хэш-таблица, O(1) средне
    std::unordered_map<std::string, int> fast;
    fast.reserve(100);
    fast["key"] = 42;

    // priority_queue — куча
    std::priority_queue<int> maxHeap;
    std::priority_queue<int, std::vector<int>, std::greater<int>> minHeap;
    maxHeap.push(3); maxHeap.push(1); maxHeap.push(4);
    std::cout << maxHeap.top() << "\n";  // 4

    // Алгоритмы STL
    std::vector<int> v = {5, 2, 8, 1, 9, 3, 7, 4, 6};

    // Поиск
    auto it = std::find(v.begin(), v.end(), 8);

    // Подсчёт
    int cnt = std::count_if(v.begin(), v.end(), [](int x) { return x > 5; });

    // Аккумуляция
    int sum = std::accumulate(v.begin(), v.end(), 0);  // 45

    // Ranges (C++20) — ленивые, composable
    auto result = v
        | std::views::filter([](int x) { return x % 2 == 0; })
        | std::views::transform([](int x) { return x * x; });

    for (int x : result) std::cout << x << " ";  // 4 64 16 36
    return 0;
}
```

### 2.6 Лямбды

```cpp
#include <functional>

int main() {
    // Базовая лямбда
    auto greet = [](const std::string& name) {
        std::cout << "Hello, " << name << "!\n";
    };

    // Захват по значению
    int mult = 3;
    auto times = [mult](int x) { return x * mult; };

    // Захват по ссылке
    auto times_ref = [&mult](int x) { return x * mult; };

    // Обобщённая лямбда (C++14)
    auto add = [](auto a, auto b) { return a + b; };
    std::cout << add(1, 2) << " " << add(1.5, 2.5) << "\n";  // 3 4

    // std::function — для хранения любого callable
    std::function<int(int, int)> op = [](int a, int b) { return a + b; };
    op = [](int a, int b) { return a * b; };

    // Сортировка по нескольким критериям
    struct Person { std::string name; int age; };
    std::vector<Person> people = {{"Charlie", 25}, {"Alice", 30}, {"Bob", 25}};
    std::sort(people.begin(), people.end(), [](const Person& a, const Person& b) {
        return a.age != b.age ? a.age < b.age : a.name < b.name;
    });
    return 0;
}
```

### 2.7 Обработка ошибок

```cpp
#include <stdexcept>
#include <optional>  // C++17

// Исключения
double safe_divide(double a, double b) {
    if (b == 0.0) throw std::invalid_argument("Division by zero");
    return a / b;
}

void exception_demo() {
    try {
        std::cout << safe_divide(10, 2) << "\n";
        std::cout << safe_divide(10, 0) << "\n";
    } catch (const std::invalid_argument& e) {
        std::cerr << "Error: " << e.what() << "\n";
    } catch (const std::exception& e) {
        std::cerr << "Exception: " << e.what() << "\n";
    }
}

// std::optional (C++17) — функция может не вернуть значение
std::optional<int> parse_int(const std::string& s) {
    try { return std::stoi(s); }
    catch (...) { return std::nullopt; }
}

void optional_demo() {
    auto val = parse_int("42").value_or(0);
    std::cout << val << "\n";  // 42

    if (auto result = parse_int("abc")) {
        std::cout << *result << "\n";
    } else {
        std::cout << "parse failed\n";
    }
}
```

### Где учить уровень 2

| Ресурс | Тип |
|--------|-----|
| Effective Modern C++ (Scott Meyers) | Книга — обязательно |
| C++ Templates: The Complete Guide | Книга — шаблоны |
| LeetCode Medium | Практика |
| Codeforces Div.2 A-C | Практика |
| Back to Basics на CppCon | YouTube |


---

## Уровень 3 — Продвинутый C++

### 3.1 Move семантика и rvalue references

```cpp
#include <iostream>
#include <string>
#include <vector>

void process(const std::string& s) {   // принимает lvalue и rvalue
    std::cout << "copy: " << s << "\n";
}
void process(std::string&& s) {        // только rvalue
    std::cout << "move: " << s << "\n";
    std::string local = std::move(s);  // "крадём" содержимое
}

// Perfect forwarding — сохраняет категорию значения
template<typename T>
void wrapper(T&& arg) {
    process(std::forward<T>(arg));
}

int main() {
    std::string s = "hello";
    process(s);             // copy (lvalue)
    process("world");       // move (rvalue — временный)
    process(std::move(s));  // move (явное перемещение, s становится пустым)

    // vector избегает копирований благодаря move
    std::vector<std::string> vec;
    std::string big(1000000, 'x');
    vec.push_back(std::move(big));  // O(1), не O(n)!
    return 0;
}
```

### 3.2 Многопоточность

```cpp
#include <thread>
#include <mutex>
#include <atomic>
#include <future>
#include <vector>

// Mutex для защиты общих данных
std::mutex mtx;
int shared_counter = 0;

void increment(int times) {
    for (int i = 0; i < times; ++i) {
        std::lock_guard<std::mutex> lock(mtx);  // RAII
        ++shared_counter;
    }
}

// atomic для простых операций
std::atomic<int> atomic_counter{0};
void atomic_inc(int times) {
    for (int i = 0; i < times; ++i) ++atomic_counter;
}

// future/async — результат из другого потока
int heavy_computation(int n) {
    std::this_thread::sleep_for(std::chrono::milliseconds(100));
    return n * n;
}

int main() {
    // Создание потоков
    std::vector<std::thread> threads;
    for (int i = 0; i < 4; ++i)
        threads.emplace_back(increment, 1000);
    for (auto& t : threads) t.join();
    std::cout << shared_counter << "\n";  // 4000

    // async — параллельное выполнение
    auto future = std::async(std::launch::async, heavy_computation, 42);
    std::cout << "Doing other work...\n";
    int result = future.get();  // блокирует до готовности
    std::cout << "Result: " << result << "\n";  // 1764
    return 0;
}
```

### 3.3 RAII и паттерны проектирования

```cpp
// RAII — весь C++ строится на этом принципе
class FileHandle {
    FILE* file_;
public:
    explicit FileHandle(const char* path, const char* mode)
        : file_(fopen(path, mode)) {
        if (!file_) throw std::runtime_error("Cannot open file");
    }
    ~FileHandle() { if (file_) fclose(file_); }
    // Некопируемый:
    FileHandle(const FileHandle&) = delete;
    FileHandle& operator=(const FileHandle&) = delete;
    FILE* get() { return file_; }
};

// Singleton (thread-safe с C++11)
class Logger {
    Logger() = default;
public:
    static Logger& instance() {
        static Logger inst;  // гарантированно thread-safe
        return inst;
    }
    void log(const std::string& msg) {
        std::cout << "[LOG] " << msg << "\n";
    }
    Logger(const Logger&) = delete;
    Logger& operator=(const Logger&) = delete;
};

// Factory
std::unique_ptr<Shape> make_shape(const std::string& type, double a, double b = 0) {
    if (type == "circle")    return std::make_unique<Circle>(a);
    if (type == "rectangle") return std::make_unique<Rectangle>(a, b);
    throw std::invalid_argument("Unknown: " + type);
}
```

### 3.4 Метапрограммирование и type traits

```cpp
#include <type_traits>
#include <iostream>

// SFINAE (C++11) — Substitution Failure Is Not An Error
template<typename T>
typename std::enable_if<std::is_integral<T>::value, T>::type
only_integral(T val) { return val * 2; }

// if constexpr (C++17) — ветвление на этапе компиляции
template<typename T>
void type_info(T val) {
    if constexpr (std::is_integral_v<T>) {
        std::cout << "integral: " << val << "\n";
    } else if constexpr (std::is_floating_point_v<T>) {
        std::cout << "float: " << val << "\n";
    } else {
        std::cout << "other type\n";
    }
}

// constexpr — вычисления на этапе компиляции
constexpr int fibonacci(int n) {
    if (n <= 1) return n;
    return fibonacci(n-1) + fibonacci(n-2);
}

int main() {
    type_info(42);     // integral: 42
    type_info(3.14);   // float: 3.14
    type_info("hi");   // other type

    constexpr int fib10 = fibonacci(10);  // вычисляется при компиляции!
    std::cout << fib10 << "\n";  // 55
    return 0;
}
```

### 3.5 CMake — система сборки

```cmake
# CMakeLists.txt
cmake_minimum_required(VERSION 3.20)
project(MyProject VERSION 1.0.0)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Исполняемый файл
add_executable(main
    src/main.cpp
    src/utils.cpp
)

# Библиотека
add_library(mylib STATIC
    src/mylib.cpp
)
target_include_directories(mylib PUBLIC include/)

# Линковка
target_link_libraries(main PRIVATE mylib)

# Включить предупреждения
target_compile_options(main PRIVATE -Wall -Wextra -Wpedantic)
```

```bash
# Сборка
mkdir build && cd build
cmake .. -DCMAKE_BUILD_TYPE=Release
cmake --build . -j$(nproc)
```

### 3.6 Тестирование с Google Test

```cpp
// tests/test_math.cpp
#include <gtest/gtest.h>
#include "mylib.hpp"

TEST(MathTest, AddPositive) {
    EXPECT_EQ(add(2, 3), 5);
    EXPECT_EQ(add(0, 0), 0);
}

TEST(MathTest, DivisionByZero) {
    EXPECT_THROW(safe_divide(1, 0), std::invalid_argument);
}

TEST(VectorTest, PushBack) {
    std::vector<int> v;
    v.push_back(42);
    ASSERT_EQ(v.size(), 1);
    EXPECT_EQ(v[0], 42);
}
```

### Где учить уровень 3

| Ресурс | Тип |
|--------|-----|
| CppCon на YouTube | Конференция — отличные доклады |
| C++ Weekly (Jason Turner) | YouTube — практические трюки |
| Concurrency in Action (Williams) | Книга — многопоточность |
| LeetCode Hard, Codeforces Div.1 | Практика |
| open-source проекты на GitHub | Практика |


---

## Уровень 4 — Экспертный C++ / Специализация

### 4.1 Выбор специализации

| Направление | Ключевые темы | Где применяется |
|-------------|---------------|-----------------|
| **Gamedev** | ECS, рендеринг, физика, SIMD | Unreal Engine, Unity C++ |
| **Системное ПО** | OS internals, драйверы, ядро | Linux, embedded |
| **HPC / Финансы** | SIMD, memory layout, lock-free | Алготрейдинг, simulations |
| **Компиляторы** | LLVM, AST, IR, codegen | Clang, GCC |
| **Embedded** | RTOS, bare-metal, HAL | Arduino, STM32 |
| **Сети** | async I/O, epoll, IOCP | Серверы, брокеры |

### 4.2 Продвинутые темы C++20/23

```cpp
// Корутины (C++20)
#include <coroutine>
#include <generator>  // C++23

std::generator<int> fibonacci() {
    int a = 0, b = 1;
    while (true) {
        co_yield a;
        auto c = a + b;
        a = b; b = c;
    }
}

void coro_demo() {
    for (int fib : fibonacci() | std::views::take(10)) {
        std::cout << fib << " ";
    }
    // 0 1 1 2 3 5 8 13 21 34
}

// Модули (C++20) — замена заголовочным файлам
// mymodule.ixx
export module mymodule;
export namespace math {
    int add(int a, int b) { return a + b; }
}

// main.cpp
import mymodule;
int main() {
    std::cout << math::add(3, 4) << "\n";
}

// std::span (C++20) — невладеющий вид на массив
#include <span>
void process(std::span<int> data) {
    for (int& x : data) x *= 2;
}
int arr[] = {1, 2, 3, 4, 5};
process(arr);  // работает с C-массивом
std::vector<int> v = {1, 2, 3};
process(v);    // и с вектором
```

### 4.3 Оптимизации и производительность

```cpp
// Cache-friendly data layout
// Плохо: Array of Structures (AoS)
struct BadParticle {
    float x, y, z;   // позиция
    float vx, vy, vz; // скорость
    float mass;
    float lifetime;
};
std::vector<BadParticle> particles;  // при обработке позиций — пропуски кэша

// Хорошо: Structure of Arrays (SoA)
struct GoodParticles {
    std::vector<float> x, y, z;
    std::vector<float> vx, vy, vz;
    std::vector<float> mass;
    std::vector<float> lifetime;
};  // при обработке позиций — линейный доступ, кэш доволен

// Избегай ветвлений в горячем коде
// Плохо:
for (auto& p : particles) {
    if (p.lifetime > 0) p.x += p.vx;  // branch prediction miss
}
// Лучше — разделить живых и мёртвых заранее

// SIMD (через intrinsics или библиотеки)
#include <immintrin.h>
void add_arrays_avx(float* a, float* b, float* result, int n) {
    for (int i = 0; i < n; i += 8) {
        __m256 va = _mm256_load_ps(a + i);
        __m256 vb = _mm256_load_ps(b + i);
        _mm256_store_ps(result + i, _mm256_add_ps(va, vb));
    }
}
```

---

## Инструменты профессионального разработчика

### Отладка и профилирование

```bash
# GDB — отладчик
gdb ./myprogram
(gdb) break main
(gdb) run
(gdb) next / step / continue
(gdb) print variable
(gdb) backtrace

# Valgrind — утечки памяти
valgrind --leak-check=full ./myprogram

# AddressSanitizer — быстрее Valgrind, встроен в GCC/Clang
g++ -fsanitize=address -g ./myprogram

# ThreadSanitizer — гонки данных
g++ -fsanitize=thread -g ./myprogram

# Perf — профилировщик производительности
perf record ./myprogram
perf report

# Heaptrack — профилировщик памяти
heaptrack ./myprogram
```

### Статический анализ

```bash
# clang-tidy — линтер и анализатор
clang-tidy myfile.cpp -- -std=c++20

# cppcheck — свободный статический анализатор
cppcheck --enable=all src/

# clang-format — форматирование кода
clang-format -i -style=Google src/
```

---

## Дорожная карта по времени

```
Месяц 1-2:  Синтаксис, типы, управляющие конструкции, функции
Месяц 3-4:  Указатели, ссылки, массивы, строки, std::vector
Месяц 5-6:  ООП, классы, наследование, полиморфизм
Месяц 7-8:  STL контейнеры, алгоритмы, итераторы
Месяц 9-10: Умные указатели, move-семантика, шаблоны
Месяц 11-12: Многопоточность, исключения, RAII
Год 2:      Углублённые шаблоны, метапрограммирование, C++20
Год 2-3:    Специализация по выбранному направлению
```

---

## Лучшие ресурсы для изучения

### Книги (в порядке чтения)

| # | Книга | Уровень |
|---|-------|---------|
| 1 | **C++ Primer** — Lippman, Lajoie, Moo | Начинающий |
| 2 | **A Tour of C++** — Bjarne Stroustrup | Начинающий+ |
| 3 | **Effective Modern C++** — Scott Meyers | Средний |
| 4 | **C++ Templates: The Complete Guide** — Vandevoorde | Средний-продвинутый |
| 5 | **C++ Concurrency in Action** — Anthony Williams | Продвинутый |
| 6 | **The C++ Programming Language** — Stroustrup | Справочник |

### Онлайн-ресурсы

| Ресурс | Описание |
|--------|----------|
| **learncpp.com** | Лучший бесплатный курс для начинающих |
| **cppreference.com** | Главный справочник по стандарту |
| **godbolt.org** | Compiler Explorer — смотри ассемблер |
| **cppinsights.io** | Видишь что делает компилятор с шаблонами |
| **quick-bench.com** | Бенчмаркинг прямо в браузере |

### YouTube каналы

| Канал | Описание |
|-------|----------|
| **The Cherno** | Практический C++ для начинающих |
| **CppCon** | Конференционные доклады экспертов |
| **C++ Weekly (Jason Turner)** | Короткие трюки и новинки стандарта |
| **javidx9 (One Lone Coder)** | Gamedev на C++ |

### Платформы для практики

| Платформа | Описание |
|-----------|----------|
| **LeetCode** | Алгоритмы и структуры данных |
| **Codeforces** | Олимпиадное программирование |
| **HackerRank** | Задачи по C++ специфически |
| **Exercism** | Задачи с ревью от ментора |

---

## Типичные ошибки новичков

```cpp
// ❌ Плохо: забытый virtual деструктор
class Base { };
class Derived : public Base { ~Derived() { /* cleanup */ } };
Base* ptr = new Derived();
delete ptr;  // UB! деструктор Derived не вызовется

// ✅ Хорошо:
class Base { public: virtual ~Base() = default; };

// ❌ Плохо: висячий указатель
int* ptr = new int(42);
delete ptr;
*ptr = 10;  // UB! используем удалённую память

// ✅ Хорошо: умные указатели
auto ptr = std::make_unique<int>(42);
// автоматически удаляется

// ❌ Плохо: возврат ссылки на локальную переменную
int& bad() {
    int x = 42;
    return x;  // UB! x уничтожен после возврата
}

// ❌ Плохо: signed/unsigned сравнение
std::vector<int> v = {1, 2, 3};
for (int i = 0; i < v.size(); ++i)  // warning! v.size() — unsigned
// ✅ Хорошо:
for (size_t i = 0; i < v.size(); ++i)
// или просто range-based for:
for (const auto& x : v)

// ❌ Плохо: не инициализированные переменные
int x;  // мусорное значение!
std::cout << x;  // UB!

// ✅ Хорошо:
int x = 0;
int y{};  // value-initialization, тоже 0
```

---

## Глоссарий

| Термин | Объяснение |
|--------|------------|
| **UB** | Undefined Behavior — поведение не определено стандартом, опасно |
| **RAII** | Resource Acquisition Is Initialization — ресурс берётся в конструкторе, освобождается в деструкторе |
| **lvalue/rvalue** | lvalue — есть имя, rvalue — временный объект |
| **ODR** | One Definition Rule — каждая сущность должна иметь ровно одно определение |
| **ABI** | Application Binary Interface — бинарная совместимость между модулями |
| **TMP** | Template Metaprogramming — программирование на шаблонах |
| **POD** | Plain Old Data — простые типы без конструкторов/деструкторов |
| **SFINAE** | Substitution Failure Is Not An Error — ошибка подстановки шаблона не ошибка |

---

*Этот роадмап создан для репозитория [cpproadmap2026](https://github.com/justxor/cpproadmap2026)*  
*Стандарты: C++11, C++14, C++17, C++20, C++23*
