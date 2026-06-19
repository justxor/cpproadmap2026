# 🗺️ Полный роадмап C++ 2026 — от нуля до профессионала

> **Автор:** сгенерировано с помощью Claude AI  
> **Актуальность:** C++11/14/17/20/23  
> **Уровни:** от абсолютного новичка до эксперта

---

## Общая структура пути

```
Уровень 0: Фундамент и настройка окружения   (1-2 недели)
Уровень 1: Базовый C++                       (2-3 месяца)
Уровень 2: Средний C++ / ООП / STL           (3-4 месяца)
Уровень 3: Продвинутый C++                   (4-6 месяцев)
Уровень 4: Экспертный / Специализация        (бесконечно)
```

---

## Уровень 0 — Фундамент

### Настройка окружения

**Компиляторы:** GCC (Linux/Mac), MSVC (Windows), Clang (все платформы)

```bash
# Ubuntu/Debian
sudo apt install g++ clang build-essential cmake git
# macOS
brew install gcc llvm cmake
# Проверить версию
g++ --version && clang++ --version
```

### ✅ Шаблон для компиляции — всегда используй эти флаги

```bash
# Разработка — максимум предупреждений + санитайзеры
g++ -std=c++20 -Wall -Wextra -Wpedantic -Wshadow -Wconversion -g -fsanitize=address,undefined -o program main.cpp

# Продакшен — оптимизация
g++ -std=c++20 -O2 -DNDEBUG -o program main.cpp
```

---

## Уровень 1 — Базовый C++ (2–3 месяца)

### 1.1 Типы данных — best practices

```cpp
#include <cstdint>
#include <iostream>

int main() {
    // Используй точные типы когда размер важен
    int32_t  health = 100;
    uint64_t score  = 1'000'000'000ULL; // апостроф как разделитель (C++14)

    // auto убирает дублирование типа
    auto name = std::string{"Alice"};  // явно std::string, не const char*
    auto pi   = 3.14159265358979;      // double
    auto pif  = 3.14159f;             // float — суффикс f обязателен!

    // ПЛОХО — магические числа
    if (health < 20) std::cout << "critical\n";

    // ХОРОШО — именованные константы
    constexpr int32_t kCriticalHealth = 20;
    if (health < kCriticalHealth) std::cout << "critical\n";

    // ВАЖНО: Инициализируй всегда! Неинициализированная переменная = UB
    int x{};    // value-init = 0
    int y = 0;  // явно
    // int z;   <- мусор в памяти, UB при чтении

    return 0;
}
```

### 1.2 Функции — best practices

```cpp
#include <string>
#include <string_view>

// string_view для readonly строк — не копирует
void print_name(std::string_view name) {
    std::cout << "Name: " << name << "\n";
}

// [[nodiscard]] — компилятор предупредит если результат проигнорирован
[[nodiscard]] int compute(int x) { return x * x; }

// noexcept — явное обещание не бросать исключения
int safe_add(int a, int b) noexcept { return a + b; }

// Возврат нескольких значений — через struct, не через out-параметры
struct DivResult { int quotient; int remainder; };

DivResult divmod(int a, int b) {
    return {a / b, a % b};
}

// ПЛОХО — out-параметры через указатель, непонятно что куда
void divmod_bad(int a, int b, int* q, int* r) {
    *q = a / b; *r = a % b;
}

int main() {
    print_name("Alice");

    // compute(42); <- предупреждение компилятора о [[nodiscard]]
    auto result = compute(42);

    auto [q, r] = divmod(17, 5); // C++17 structured bindings
    std::cout << q << " rem " << r << "\n"; // 3 rem 2
}
```

### 1.3 Указатели и ссылки — разбор опасных мест

```cpp
// ПЛОХО — висячая ссылка
const std::string& bad_ref() {
    std::string local = "hello";
    return local; // UB! local уничтожен после return
}

// ПЛОХО — забыли передать по ссылке, лишняя копия
void modify_bad(std::vector<int> v) { // копирует весь вектор!
    v.push_back(42); // изменяет копию, не оригинал
}

// Правило выбора способа передачи:
// | Тип              | Передавай как         |
// | int, double, ... | по значению (T)       |
// | только читать    | const T&              |
// | нужно изменить   | T&                    |
// | передать владение| unique_ptr<T>         |

void read_vec(const std::vector<int>& v) { /* читаем */ }
void modify_vec(std::vector<int>& v)     { v.push_back(42); }

// Проверяй указатель перед использованием
void safe_use(const int* ptr) {
    if (ptr == nullptr) return;
    std::cout << *ptr << "\n";
}
```

### 1.4 std::vector — правильное использование

```cpp
#include <vector>
#include <algorithm>
#include <numeric>

int main() {
    // reserve() если знаешь размер заранее — избегает realloc
    std::vector<int> v;
    v.reserve(1000);
    for (int i = 0; i < 1000; ++i) v.push_back(i);

    // emplace_back вместо push_back для объектов — конструирует на месте
    std::vector<std::pair<int,int>> pairs;
    pairs.emplace_back(1, 2);

    // STL алгоритмы вместо ручных циклов
    int sum  = std::accumulate(v.begin(), v.end(), 0);
    auto it  = std::find(v.begin(), v.end(), 42);
    bool has = (it != v.end());
    int  cnt = std::count_if(v.begin(), v.end(), [](int x){ return x%2==0; });

    std::sort(v.begin(), v.end(), std::greater<int>{}); // убывание

    // erase-remove idiom — удалить все нечётные
    v.erase(std::remove_if(v.begin(), v.end(), [](int x){ return x%2!=0; }), v.end());

    // C++20 — то же самое проще:
    std::erase_if(v, [](int x) { return x % 2 != 0; });
}
```

### Где учить уровень 1

| Ресурс | Тип | Ссылка |
|---|---|---|
| learncpp.com | Бесплатный курс | https://www.learncpp.com |
| C++ Primer (Lippman) | Книга | Классика для новичков |
| The Cherno | YouTube | Очень доходчиво |
| cppreference.com | Справочник | Открывай всегда |
| Codeforces Div.4/3 | Практика | Задачи по алгоритмам |

---

## Уровень 2 — Средний C++ (3–4 месяца)

### 2.1 ООП — как правильно проектировать классы

```cpp
#include <string>
#include <stdexcept>

class BankAccount {
public:
    // explicit — запрет неявного преобразования
    explicit BankAccount(std::string owner, double initial_balance = 0.0)
        : owner_(std::move(owner))   // move, не копирование
        , balance_(initial_balance)
    {
        if (initial_balance < 0)
            throw std::invalid_argument("Balance cannot be negative");
    }

    // Виртуальный деструктор — обязателен для полиморфных классов
    virtual ~BankAccount() = default;

    // Геттеры — const метод не меняет объект
    const std::string& owner()   const { return owner_; }
    double             balance() const { return balance_; }

    virtual void deposit(double amount) {
        if (amount <= 0) throw std::invalid_argument("Amount must be positive");
        balance_ += amount;
    }

    virtual bool withdraw(double amount) {
        if (amount <= 0) throw std::invalid_argument("Amount must be positive");
        if (amount > balance_) return false;
        balance_ -= amount;
        return true;
    }

    // Перегрузка << через friend
    friend std::ostream& operator<<(std::ostream& os, const BankAccount& acc) {
        return os << acc.owner_ << ": $" << acc.balance_;
    }

private:
    std::string owner_;
    double      balance_;
};

// override — явно, компилятор проверит что мы действительно переопределяем
class SavingsAccount : public BankAccount {
public:
    SavingsAccount(std::string owner, double balance, double rate)
        : BankAccount(std::move(owner), balance), interest_rate_(rate) {}

    void deposit(double amount) override {
        BankAccount::deposit(amount);
        BankAccount::deposit(amount * interest_rate_); // + проценты
    }

private:
    double interest_rate_;
};
```

### 2.2 Умные указатели — паттерны использования

```cpp
#include <memory>
#include <vector>

// unique_ptr — единственный владелец (90% случаев)
auto ptr = std::make_unique<MyClass>(args...);
// ptr автоматически удаляется при выходе из scope
// нельзя скопировать, только переместить:
auto ptr2 = std::move(ptr); // ptr = nullptr

// shared_ptr — подсчёт ссылок (только при реальном совместном владении)
auto sp1 = std::make_shared<MyClass>(args...);
{
    auto sp2 = sp1; // счётчик = 2
} // sp2 уничтожен, счётчик = 1

// weak_ptr разрывает циклические зависимости
struct Employee;
struct Department {
    std::vector<std::weak_ptr<Employee>> members; // не владеем!
};
struct Employee {
    std::shared_ptr<Department> dept; // владеем
};

// Принимай unique_ptr по значению = берёшь владение
void consume(std::unique_ptr<Node> node) { /* владеем */ }

// Принимай raw pointer = только используем, не владеем
void read_node(const Node* node) {
    if (node) std::cout << node->value << "\n";
}
```

### 2.3 Шаблоны (Templates)

```cpp
// Простой шаблон функции
template<typename T>
T clamp(T val, T lo, T hi) {
    return val < lo ? lo : (val > hi ? hi : val);
}

// C++20 концепты — читаемее SFINAE
template<typename T>
concept Numeric = std::integral<T> || std::floating_point<T>;

template<Numeric T>
T square(T x) { return x * x; }

// CRTP — статический полиморфизм (быстрее виртуальных функций)
template<typename Derived>
class Printable {
public:
    void print() const {
        static_cast<const Derived*>(this)->print_impl();
    }
};

class Circle : public Printable<Circle> {
public:
    explicit Circle(double r) : radius(r) {}
    void print_impl() const {
        std::cout << "Circle(r=" << radius << ")\n";
    }
private:
    double radius;
};

// Variadic templates + fold expression (C++17)
template<typename... Args>
void print_all(Args&&... args) {
    (std::cout << ... << args) << "\n";
}
```

### 2.4 Обработка ошибок — правильная стратегия

```cpp
#include <optional>
#include <variant>

// std::optional — функция может не иметь результата
std::optional<double> safe_sqrt(double x) {
    if (x < 0) return std::nullopt;
    return std::sqrt(x);
}

// value_or — дефолт если пусто
double r = safe_sqrt(-1.0).value_or(0.0);

// if + разыменование
if (auto result = safe_sqrt(4.0)) {
    std::cout << *result << "\n"; // 2.0
}

// std::variant — тип-or-ошибка без исключений (C++17)
using ParseResult = std::variant<int, std::string>;

ParseResult parse_positive(const std::string& s) {
    try {
        int n = std::stoi(s);
        if (n < 0) return std::string{"Expected positive, got " + s};
        return n;
    } catch (...) {
        return std::string{"Not a number: " + s};
    }
}

// Иерархия своих исключений
class AppError     : public std::runtime_error { using std::runtime_error::runtime_error; };
class NetworkError : public AppError           { using AppError::AppError; };
class TimeoutError : public NetworkError       { using NetworkError::NetworkError; };
```

### Где учить уровень 2

| Ресурс | Тип |
|---|---|
| Effective Modern C++ (Scott Meyers) | Книга — обязательно |
| C++ Templates: The Complete Guide | Книга — шаблоны |
| LeetCode Medium | Практика |
| Codeforces Div.2 A-C | Практика |
| Back to Basics на CppCon | YouTube |

---

## Уровень 3 — Продвинутый C++ (4–6 месяцев)

### 3.1 Move семантика и rvalue references

```cpp
void process(const std::string& s) { /* lvalue */ }
void process(std::string&& s) {      /* rvalue */
    std::string local = std::move(s); // "крадём" содержимое
}

// Perfect forwarding — сохраняет категорию значения
template<typename T>
void wrapper(T&& arg) {
    process(std::forward<T>(arg));
}

// Когда использовать std::move:
// 1. Передаём объект в функцию и он нам больше не нужен
// 2. В конструкторе/операторе перемещения
// 3. vector использует move при resize — перемещение не копирование

std::string s = "hello";
auto s2 = std::move(s); // s теперь пусто — не используй после move!

// ПЛОХО — move в return мешает NRVO
std::string make() {
    std::string r = "result";
    return r;        // ХОРОШО: NRVO сработает
    // return std::move(r); // ПЛОХО: мешает NRVO
}
```

### 3.2 Многопоточность — правильные паттерны

```cpp
#include <thread>
#include <mutex>
#include <condition_variable>
#include <atomic>
#include <future>

// Thread-safe очередь — классический паттерн producer/consumer
template<typename T>
class ThreadSafeQueue {
public:
    void push(T value) {
        {
            std::lock_guard<std::mutex> lock(mutex_);
            queue_.push(std::move(value));
        }
        cv_.notify_one(); // уведомить после unlock
    }

    T pop() {
        std::unique_lock<std::mutex> lock(mutex_);
        cv_.wait(lock, [this] { return !queue_.empty(); }); // ждём с предикатом
        T value = std::move(queue_.front());
        queue_.pop();
        return value;
    }

    bool try_pop(T& value) {
        std::lock_guard<std::mutex> lock(mutex_);
        if (queue_.empty()) return false;
        value = std::move(queue_.front());
        queue_.pop();
        return true;
    }

private:
    mutable std::mutex      mutex_;
    std::condition_variable cv_;
    std::queue<T>           queue_;
};

// atomic — для счётчиков без мьютекса
class Statistics {
public:
    void record_request() noexcept { ++total_requests_; }
    void record_error()   noexcept { ++total_errors_; }
    double error_rate() const noexcept {
        auto total = total_requests_.load();
        if (total == 0) return 0.0;
        return static_cast<double>(total_errors_.load()) / total;
    }
private:
    std::atomic<uint64_t> total_requests_{0};
    std::atomic<uint64_t> total_errors_{0};
};

// async — параллельное выполнение
auto future = std::async(std::launch::async, []{ return heavy_computation(); });
std::cout << "Doing other work...\n";
int result = future.get(); // блокирует до готовности
```

### 3.3 RAII — продвинутые паттерны

```cpp
// ScopeGuard — выполнить действие при выходе из scope
class ScopeGuard {
public:
    explicit ScopeGuard(std::function<void()> f) : fn_(std::move(f)) {}
    ~ScopeGuard() { if (active_) fn_(); }
    void dismiss() { active_ = false; }
    ScopeGuard(const ScopeGuard&) = delete;
    ScopeGuard& operator=(const ScopeGuard&) = delete;
private:
    std::function<void()> fn_;
    bool active_ = true;
};

void database_transaction(Database& db) {
    db.begin_transaction();
    ScopeGuard rollback_guard([&db]{ db.rollback(); }); // выполнится при любом выходе

    db.execute("UPDATE users SET active=1 WHERE id=42");
    db.commit();
    rollback_guard.dismiss(); // успех — отменяем rollback
}

// PImpl — скрывает детали реализации, ускоряет компиляцию
class Engine {
public:
    explicit Engine(int cylinders);
    ~Engine(); // определён в .cpp где Impl известен
    Engine(Engine&&) noexcept;
    Engine& operator=(Engine&&) noexcept;
    void start();
    int  rpm() const;
private:
    struct Impl;
    std::unique_ptr<Impl> impl_; // детали только в .cpp
};
```

### 3.4 Метапрограммирование — практичные случаи

```cpp
#include <type_traits>
#include <concepts>
#include <array>

// constexpr — вычисления при компиляции
constexpr std::array<int, 10> make_squares() {
    std::array<int, 10> arr{};
    for (int i = 0; i < 10; ++i) arr[i] = i * i;
    return arr;
}
constexpr auto SQUARES = make_squares(); // вычисляется при компиляции!

// if constexpr — ветвление на уровне типов (C++17)
template<typename T>
std::string to_string_smart(T val) {
    if constexpr (std::is_same_v<T, bool>) {
        return val ? "true" : "false";
    } else if constexpr (std::is_floating_point_v<T>) {
        return std::to_string(val);
    } else if constexpr (std::is_integral_v<T>) {
        return std::to_string(val);
    } else {
        return std::string(val);
    }
}

// Концепты (C++20) — описывают требования к типу
template<typename T>
concept Sortable = requires(T container) {
    container.begin();
    container.end();
    { container.size() } -> std::convertible_to<size_t>;
};

template<Sortable Container>
void sort_and_print(Container& c) {
    std::sort(c.begin(), c.end());
    for (const auto& elem : c) std::cout << elem << " ";
    std::cout << "\n";
}
```

### 3.5 CMake — правильная структура проекта

```
MyProject/
├── CMakeLists.txt
├── src/
│   ├── CMakeLists.txt
│   ├── main.cpp
│   └── engine/
│       ├── engine.hpp
│       └── engine.cpp
├── tests/
│   └── test_engine.cpp
└── third_party/
    └── googletest/
```

```cmake
cmake_minimum_required(VERSION 3.20)
project(MyProject VERSION 1.0.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

option(BUILD_TESTS  "Build tests"             ON)
option(ENABLE_ASAN  "Enable AddressSanitizer" OFF)

add_library(project_warnings INTERFACE)
target_compile_options(project_warnings INTERFACE
    -Wall -Wextra -Wpedantic -Wshadow -Wconversion
    $<$<CONFIG:Debug>:-g>
    $<$<CONFIG:Release>:-O2 -DNDEBUG>
)

if(ENABLE_ASAN)
    target_compile_options(project_warnings INTERFACE -fsanitize=address,undefined)
    target_link_options(project_warnings    INTERFACE -fsanitize=address,undefined)
endif()

add_subdirectory(src)
if(BUILD_TESTS)
    enable_testing()
    add_subdirectory(tests)
endif()
```

### Где учить уровень 3

| Ресурс | Тип |
|---|---|
| CppCon на YouTube | Конференция — отличные доклады |
| C++ Weekly (Jason Turner) | YouTube — практические трюки |
| Concurrency in Action (Williams) | Книга — многопоточность |
| LeetCode Hard, Codeforces Div.1 | Практика |
| open-source проекты на GitHub | Практика |

---

## Уровень 4 — Экспертный / Специализация

### 4.1 Выбор специализации

| Направление | Ключевые темы | Где применяется |
|---|---|---|
| Gamedev | ECS, рендеринг, физика, SIMD | Unreal Engine, Unity C++ |
| Системное ПО | OS internals, драйверы, ядро | Linux, embedded |
| HPC / Финансы | SIMD, memory layout, lock-free | Алготрейдинг, simulations |
| Компиляторы | LLVM, AST, IR, codegen | Clang, GCC |
| Embedded | RTOS, bare-metal, HAL | Arduino, STM32 |
| Сети | async I/O, epoll, IOCP | Серверы, брокеры |

### 4.2 Cache-friendly код (SoA vs AoS)

```cpp
// ПЛОХО: Array of Structures — при обходе позиций тащим лишнее
struct ParticleBad {
    float x, y, z;
    float vx, vy, vz;
    float mass, lifetime;
    char name[32]; // мусор в кэше при обработке позиций
};

// ХОРОШО: Structure of Arrays — линейный доступ к каждому полю
struct Particles {
    std::vector<float> x, y, z;
    std::vector<float> vx, vy, vz;
    std::vector<float> mass;
    std::vector<float> lifetime;
};

// Обновление позиций — идёт по 3 непрерывным массивам, cache friendly!
void update_positions(Particles& p, float dt) {
    const size_t n = p.x.size();
    for (size_t i = 0; i < n; ++i) {
        p.x[i] += p.vx[i] * dt;
        p.y[i] += p.vy[i] * dt;
        p.z[i] += p.vz[i] * dt;
    }
}
```

### 4.3 std::format и Ranges (C++20)

```cpp
#include <format>
#include <ranges>

// std::format — безопасная замена printf
std::string s = std::format("Hello, {}! Score: {:.2f}", "Alice", 98.5);

// Для своих типов — специализируй formatter
struct Point { double x, y; };

template<>
struct std::formatter<Point> {
    constexpr auto parse(std::format_parse_context& ctx) { return ctx.begin(); }
    auto format(const Point& p, std::format_context& ctx) const {
        return std::format_to(ctx.out(), "({:.2f}, {:.2f})", p.x, p.y);
    }
};

Point pt{1.5, 2.7};
std::cout << std::format("Point: {}\n", pt); // Point: (1.50, 2.70)

// Ranges pipeline — ленивые цепочки без промежуточных векторов
std::vector<int> nums = {1,2,3,4,5,6,7,8,9,10};

auto result = nums
    | std::views::filter([](int x) { return x % 2 == 0; })
    | std::views::transform([](int x) { return x * x; })
    | std::views::take(3);

for (int x : result) std::cout << x << " "; // 4 16 36

// views::iota — диапазон без вектора
for (int i : std::views::iota(1, 11)) std::cout << i << " "; // 1..10
```

### 4.4 Корутины (C++20)

```cpp
#include <coroutine>
#include <generator> // C++23

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
        std::cout << fib << " "; // 0 1 1 2 3 5 8 13 21 34
    }
}
```

---

## Типичные ошибки новичков

```cpp
// ОШИБКА 1: Забытый virtual деструктор
class Base { };
class Derived : public Base { ~Derived() { /* cleanup */ } };
Base* ptr = new Derived();
delete ptr; // UB! деструктор Derived не вызовется
// ИСПРАВЛЕНИЕ:
class Base { public: virtual ~Base() = default; };

// ОШИБКА 2: Срезка объекта (object slicing)
void process(Base obj) { obj.do_something(); } // копирует только Base-часть!
// ИСПРАВЛЕНИЕ:
void process(const Base& obj) { obj.do_something(); }

// ОШИБКА 3: Итератор инвалидирован при push_back
std::vector<int> v = {1,2,3,4,5};
for (auto it = v.begin(); it != v.end(); ++it) {
    if (*it == 3) v.push_back(99); // UB! реаллокация инвалидирует итераторы
}
// ИСПРАВЛЕНИЕ: собери что добавить после цикла

// ОШИБКА 4: Signed/unsigned сравнение
for (int i = 0; i < v.size(); ++i) // warning! v.size() — unsigned
// ИСПРАВЛЕНИЕ:
for (size_t i = 0; i < v.size(); ++i)
// или просто range-based for:
for (const auto& x : v) { /* ... */ }

// ОШИБКА 5: Не инициализированные переменные
int x; // мусорное значение! UB при чтении
// ИСПРАВЛЕНИЕ:
int x{};   // value-initialization = 0
int y = 0; // явно
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

**Онлайн-инструменты:**
- [godbolt.org](https://godbolt.org) — Compiler Explorer, видишь ассемблер
- [cppinsights.io](https://cppinsights.io) — что делает компилятор с шаблонами
- [quick-bench.com](https://quick-bench.com) — бенчмаркинг в браузере

---

## Лучшие ресурсы для изучения

### Книги (в порядке чтения)

| # | Книга | Уровень |
|---|---|---|
| 1 | C++ Primer — Lippman, Lajoie, Moo | Начинающий |
| 2 | A Tour of C++ — Bjarne Stroustrup | Начинающий+ |
| 3 | Effective Modern C++ — Scott Meyers | Средний |
| 4 | C++ Templates: The Complete Guide — Vandevoorde | Средний-продвинутый |
| 5 | C++ Concurrency in Action — Anthony Williams | Продвинутый |
| 6 | The C++ Programming Language — Stroustrup | Справочник |

### Онлайн-ресурсы

| Ресурс | Описание |
|---|---|
| learncpp.com | Лучший бесплатный курс для начинающих |
| cppreference.com | Главный справочник по стандарту |
| godbolt.org | Compiler Explorer — смотри ассемблер |
| cppinsights.io | Видишь что делает компилятор с шаблонами |
| quick-bench.com | Бенчмаркинг прямо в браузере |

### YouTube каналы

| Канал | Описание |
|---|---|
| The Cherno | Практический C++ для начинающих |
| CppCon | Конференционные доклады экспертов |
| C++ Weekly (Jason Turner) | Короткие трюки и новинки стандарта |
| javidx9 (One Lone Coder) | Gamedev на C++ |

### Платформы для практики

| Платформа | Описание |
|---|---|
| LeetCode | Алгоритмы и структуры данных |
| Codeforces | Олимпиадное программирование |
| HackerRank | Задачи по C++ специфически |
| Exercism | Задачи с ревью от ментора |

---

## Дорожная карта по времени

```
Месяц  1-2:  Синтаксис, типы, управляющие конструкции, функции
Месяц  3-4:  Указатели, ссылки, массивы, строки, std::vector
Месяц  5-6:  ООП, классы, наследование, полиморфизм
Месяц  7-8:  STL контейнеры, алгоритмы, итераторы
Месяц  9-10: Умные указатели, move-семантика, шаблоны
Месяц 11-12: Многопоточность, исключения, RAII
Год 2:       Углублённые шаблоны, метапрограммирование, C++20
Год 2-3:     Специализация по выбранному направлению
```

---

## Глоссарий

| Термин | Объяснение |
|---|---|
| UB | Undefined Behavior — поведение не определено стандартом, опасно |
| RAII | Resource Acquisition Is Initialization — ресурс берётся в конструкторе, освобождается в деструкторе |
| lvalue/rvalue | lvalue — есть имя, rvalue — временный объект |
| ODR | One Definition Rule — каждая сущность должна иметь ровно одно определение |
| ABI | Application Binary Interface — бинарная совместимость между модулями |
| TMP | Template Metaprogramming — программирование на шаблонах |
| SFINAE | Substitution Failure Is Not An Error — ошибка подстановки шаблона не ошибка |
| CRTP | Curiously Recurring Template Pattern — статический полиморфизм |
| SoA/AoS | Structure of Arrays / Array of Structures — паттерны layout памяти |

---

*Этот роадмап создан для репозитория cpproadmap2026*  
*Стандарты: C++11, C++14, C++17, C++20, C++23*


---

## 📚 Теория C++ — углублённый раздел

> Этот раздел дополняет практический роадмап теоретической базой. Здесь собраны ключевые концепции языка с акцентом на «почему так устроено», а не только «как использовать». Понимание теории отличает того, кто пишет код, от того, кто проектирует системы.
>
> ### Т.1 Модель памяти и время жизни объектов
>
> В C++ каждый объект имеет storage duration (длительность хранения): automatic (стек, локальные переменные), static (глобальные и static-переменные), thread (thread_local) и dynamic (куча, через new/delete или аллокаторы). Понимание того, когда объект создаётся и уничтожается — основа безопасного кода.
>
> Ключевые правила времени жизни: локальные объекты уничтожаются в порядке, обратном их созданию, при выходе из области видимости; объект считается «живым» с момента завершения работы конструктора до начала работы деструктора, а обращение к нему вне этого окна — undefined behavior; временные объекты живут до конца полного выражения, кроме случая продления жизни привязкой к const-ссылке.
>
> ### Т.2 RAII — фундамент управления ресурсами
>
> RAII (Resource Acquisition Is Initialization) связывает время жизни ресурса с временем жизни объекта на стеке. Ресурс захватывается в конструкторе и гарантированно освобождается в деструкторе — даже при выбросе исключения, благодаря раскрутке стека. Именно поэтому в идиоматичном C++ почти не встречается ручных delete, fclose или unlock: за это отвечают деструкторы std::unique_ptr, std::fstream и std::lock_guard. RAII превращает управление ресурсами из ручной дисциплины в гарантию, проверяемую компилятором.
>
> ### Т.3 Правило пяти и правило нуля
>
> Если класс напрямую управляет ресурсом, он обычно должен корректно определить пять специальных функций-членов: деструктор, копирующий конструктор, копирующее присваивание, перемещающий конструктор и перемещающее присваивание. Однако лучшая практика — правило нуля: проектировать классы так, чтобы они вообще не управляли ресурсами вручную, а делегировали это RAII-обёрткам (умным указателям, контейнерам). Тогда компилятор сгенерирует все пять функций правильно, и писать их руками не нужно.
>
> ### Т.4 Move-семантика и категории значений
>
> Категории значений описывают, чем является выражение: lvalue имеет имя и адрес, prvalue — чистое временное значение, xvalue — «истекающий» объект, ресурсы которого можно забрать. Move-семантика позволяет «украсть» внутренности у объекта, который скоро будет уничтожен, вместо дорогого глубокого копирования. Важно понимать: std::move сам по себе ничего не перемещает — это лишь приведение к rvalue-ссылке, разрешающее перемещение. Реальное перемещение происходит только там, где определены перемещающие операции; иначе тихо выполняется копирование.
>
> ### Т.5 Шаблоны и обобщённое программирование
>
> Шаблоны — это инструкции компилятору о том, как сгенерировать код под конкретные типы. Инстанцирование (подстановка типов и генерация кода) происходит во время компиляции, поэтому ошибки шаблонов исторически многословны. Современные подходы делают шаблоны читаемее: концепты (concepts, C++20) задают понятные ограничения на типы вместо громоздких SFINAE-конструкций; if constexpr выбирает ветвь кода на этапе компиляции; variadic templates вместе с fold-выражениями обрабатывают переменное число аргументов.
>
> ### Т.6 Полиморфизм: динамический и статический
>
> Динамический полиморфизм реализуется через виртуальные функции и таблицу виртуальных методов (vtable); конкретная функция выбирается во время выполнения, цена этого — косвенный вызов и невозможность инлайнинга. Статический полиморфизм (шаблоны, CRTP) разрешается на этапе компиляции: накладных расходов в рантайме нет, но растёт объём сгенерированного кода и усложняются сообщения об ошибках. Выбор между ними зависит от того, нужна ли гибкость именно во время выполнения программы.
>
> ### Т.7 Const-корректность и constexpr
>
> Ключевое слово const выражает намерение «не изменять» и помогает как компилятору в оптимизации, так и читателю в понимании кода. Const-методы не меняют наблюдаемое состояние объекта, а ключевое слово mutable разрешает изменять отдельные внутренние поля (например, кэш или мьютекс) даже внутри const-метода. Constexpr идёт ещё дальше: он переносит вычисление из времени выполнения во время компиляции, позволяя получать константы и даже сложные структуры данных без затрат в рантайме.
>
> ### Т.8 Undefined Behavior — почему это опасно
>
> Undefined Behavior (UB) — это ситуации, для которых стандарт намеренно не задаёт никакого поведения: разыменование нулевого или висячего указателя, переполнение знакового целого, выход за границы массива, гонки данных, нарушение строгого алиасинга. Опасность в том, что компилятор вправе предполагать, будто UB никогда не происходит, и агрессивно оптимизировать код на этом основании — из-за чего симптомы проявляются непредсказуемо и далеко от причины. Обнаруживать UB помогают санитайзеры AddressSanitizer и UndefinedBehaviorSanitizer, а также Valgrind.
>
> ### Т.9 Многопоточность и модель памяти C++11
>
> Начиная с C++11 язык имеет формальную модель памяти, описывающую корректность параллельного кода. Гонка данных — одновременный доступ нескольких потоков к одной ячейке памяти, где хотя бы один доступ является записью, без синхронизации — это undefined behavior. Синхронизация достигается мьютексами (std::mutex совместно с std::lock_guard или std::unique_lock), атомарными операциями (std::atomic) и явным указанием порядков памяти (memory_order). Высокоуровневые средства параллелизма — std::async, std::thread, std::jthread (C++20), а также future и promise для передачи результата между потоками.
>
> ### Т.10 Эволюция стандартов
>
> Таблица ниже кратко описывает, что каждый стандарт добавил в язык:
>
> | Стандарт | Ключевые нововведения |
> |---|---|
> | C++11 | auto, лямбды, move-семантика, умные указатели, потоки, range-based for, nullptr, constexpr |
> | C++14 | обобщённые лямбды, вывод возвращаемого типа через auto, переменные-шаблоны |
> | C++17 | structured bindings, if constexpr, std::optional/variant/any, файловая система, параллельные алгоритмы STL |
> | C++20 | концепты, ranges, модули, корутины, оператор трёхстороннего сравнения (<=>), std::span |
> | C++23 | std::expected, std::mdspan, std::generator, улучшения ranges, deducing this |
>
> ### Т.11 Как пользоваться этим разделом
>
> Теорию лучше изучать параллельно с практикой из уровней роадмапа, а не отдельно. Возвращайтесь к этим разделам, когда сталкиваетесь с конкретной проблемой: непонятная ошибка шаблона — перечитайте Т.5; падение с повреждением памяти — Т.1 и Т.8; вопрос о том, копируется объект или перемещается — Т.4. Понимание «почему» делает чтение чужого кода и проектирование собственного гораздо осознаннее.
>
> ---
> 
