# 🗺️ Полный роадмап C++ 2026 — от нуля до профессионала

> **Автор:** сгенерировано с помощью Claude AI
> **Актуальность:** C++11/14/17/20/23
> **Уровни:** от абсолютного новичка до эксперта

---

## 📑 Содержание

- [Общая структура пути](#общая-структура-пути)
- [Уровень 0 — Фундамент](#уровень-0--фундамент)
- [Уровень 1 — Базовый C++](#уровень-1--базовый-c-23-месяца)
- [Уровень 2 — Средний C++](#уровень-2--средний-c-34-месяца)
- [Уровень 3 — Продвинутый C++](#уровень-3--продвинутый-c-46-месяцев)
- [Уровень 4 — Экспертный / Специализация](#уровень-4--экспертный--специализация)
- [Типичные ошибки новичков](#типичные-ошибки-новичков)
- [Инструменты профессионального разработчика](#инструменты-профессионального-разработчика)
- [Лучшие ресурсы для изучения](#лучшие-ресурсы-для-изучения)
- [Дорожная карта по времени](#дорожная-карта-по-времени)
- [Глоссарий](#глоссарий)
- [📚 Теория C++ — углублённый раздел](#-теория-c--углублённый-раздел)

---

## Общая структура пути

```
Уровень 0: Фундамент и настройка окружения (1-2 недели)
Уровень 1: Базовый C++ (2-3 месяца)
Уровень 2: Средний C++ / ООП / STL (3-4 месяца)
Уровень 3: Продвинутый C++ (4-6 месяцев)
Уровень 4: Экспертный / Специализация (бесконечно)
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
    int32_t health = 100;
    uint64_t score = 1'000'000'000ULL; // апостроф как разделитель (C++14)

    // auto убирает дублирование типа
    auto name = std::string{"Alice"}; // явно std::string, не const char*
    auto pi = 3.14159265358979; // double
    auto pif = 3.14159f; // float — суффикс f обязателен!

    // ХОРОШО — именованные константы
    constexpr int32_t kCriticalHealth = 20;
    if (health < kCriticalHealth) std::cout << "critical\n";

    // ВАЖНО: Инициализируй всегда! Неинициализированная переменная = UB
    int x{}; // value-init = 0
    int y = 0; // явно

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

// Возврат нескольких значений — через struct
struct DivResult { int quotient; int remainder; };

DivResult divmod(int a, int b) {
    return {a / b, a % b};
}

int main() {
    print_name("Alice");
    auto result = compute(42);
    auto [q, r] = divmod(17, 5); // C++17 structured bindings
    std::cout << q << " rem " << r << "\n"; // 3 rem 2
}
```

### 1.3 Указатели и ссылки — разбор опасных мест

```cpp
// Правило выбора способа передачи:
// | Тип              | Передавай как     |
// | int, double, ... | по значению (T)   |
// | только читать    | const T&          |
// | нужно изменить   | T&                |
// | передать владение| unique_ptr<T>     |

void read_vec(const std::vector<int>& v) { /* читаем */ }
void modify_vec(std::vector<int>& v) { v.push_back(42); }

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
    std::vector<int> v;
    v.reserve(1000); // избегаем realloc
    for (int i = 0; i < 1000; ++i) v.push_back(i);

    int sum = std::accumulate(v.begin(), v.end(), 0);
    std::sort(v.begin(), v.end(), std::greater<int>{});

    // erase-remove idiom
    v.erase(std::remove_if(v.begin(), v.end(), [](int x){ return x%2!=0; }), v.end());

    std::erase_if(v, [](int x) { return x % 2 != 0; }); // C++20
}
```

### Где учить уровень 1

| Ресурс | Тип | Ссылка |
|---|---|---|
| learncpp.com | Бесплатный курс | https://www.learncpp.com |
| C++ Primer (Lippman) | Книга | Классика для новичков |
| The Cherno | YouTube | Очень доходчиво |
| cppreference.com | Справочник | Открывай всегда |

---

## Уровень 2 — Средний C++ (3–4 месяца)

### 2.1 ООП — как правильно проектировать классы

```cpp
class BankAccount {
public:
    explicit BankAccount(std::string owner, double initial_balance = 0.0)
        : owner_(std::move(owner)), balance_(initial_balance) {
        if (initial_balance < 0)
            throw std::invalid_argument("Balance cannot be negative");
    }

    virtual ~BankAccount() = default; // обязателен для полиморфных классов

    const std::string& owner() const { return owner_; }
    double balance() const { return balance_; }

    virtual void deposit(double amount) {
        if (amount <= 0) throw std::invalid_argument("Amount must be positive");
        balance_ += amount;
    }
private:
    std::string owner_;
    double balance_;
};

class SavingsAccount : public BankAccount {
public:
    SavingsAccount(std::string owner, double balance, double rate)
        : BankAccount(std::move(owner), balance), interest_rate_(rate) {}

    void deposit(double amount) override { // override проверяется компилятором
        BankAccount::deposit(amount);
        BankAccount::deposit(amount * interest_rate_);
    }
private:
    double interest_rate_;
};
```

### 2.2 Умные указатели — паттерны использования

```cpp
#include <memory>

// unique_ptr — единственный владелец (90% случаев)
auto ptr = std::make_unique<MyClass>(args...);
auto ptr2 = std::move(ptr); // ptr = nullptr

// shared_ptr — подсчёт ссылок
auto sp1 = std::make_shared<MyClass>(args...);

// weak_ptr разрывает циклические зависимости
struct Department { std::vector<std::weak_ptr<Employee>> members; };
struct Employee { std::shared_ptr<Department> dept; };
```

### 2.3 Шаблоны (Templates)

```cpp
template<typename T>
T clamp(T val, T lo, T hi) {
    return val < lo ? lo : (val > hi ? hi : val);
}

// C++20 концепты
template<typename T>
concept Numeric = std::integral<T> || std::floating_point<T>;

template<Numeric T>
T square(T x) { return x * x; }

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

std::optional<double> safe_sqrt(double x) {
    if (x < 0) return std::nullopt;
    return std::sqrt(x);
}

double r = safe_sqrt(-1.0).value_or(0.0);

if (auto result = safe_sqrt(4.0)) {
    std::cout << *result << "\n"; // 2.0
}
```

### Где учить уровень 2

| Ресурс | Тип |
|---|---|
| Effective Modern C++ (Scott Meyers) | Книга — обязательно |
| C++ Templates: The Complete Guide | Книга — шаблоны |
| LeetCode Medium | Практика |
| Back to Basics на CppCon | YouTube |

---

## Уровень 3 — Продвинутый C++ (4–6 месяцев)

### 3.1 Move семантика и rvalue references

```cpp
void process(const std::string& s) { /* lvalue */ }
void process(std::string&& s) { std::string local = std::move(s); }

// Perfect forwarding — сохраняет категорию значения
template<typename T>
void wrapper(T&& arg) { process(std::forward<T>(arg)); }

std::string s = "hello";
auto s2 = std::move(s); // s теперь пусто — не используй после move!
```

### 3.2 Многопоточность — правильные паттерны

```cpp
#include <thread>
#include <mutex>
#include <condition_variable>
#include <atomic>

template<typename T>
class ThreadSafeQueue {
public:
    void push(T value) {
        {
            std::lock_guard<std::mutex> lock(mutex_);
            queue_.push(std::move(value));
        }
        cv_.notify_one();
    }
    T pop() {
        std::unique_lock<std::mutex> lock(mutex_);
        cv_.wait(lock, [this] { return !queue_.empty(); });
        T value = std::move(queue_.front());
        queue_.pop();
        return value;
    }
private:
    mutable std::mutex mutex_;
    std::condition_variable cv_;
    std::queue<T> queue_;
};
```

### 3.3 RAII — продвинутые паттерны (PImpl)

```cpp
// PImpl — скрывает детали реализации, ускоряет компиляцию
class Engine {
public:
    explicit Engine(int cylinders);
    ~Engine();
    Engine(Engine&&) noexcept;
    Engine& operator=(Engine&&) noexcept;
    void start();
private:
    struct Impl;
    std::unique_ptr<Impl> impl_;
};
```

### 3.4 Метапрограммирование — практичные случаи

```cpp
#include <type_traits>

template<typename T>
std::string to_string_smart(T val) {
    if constexpr (std::is_same_v<T, bool>) {
        return val ? "true" : "false";
    } else if constexpr (std::is_floating_point_v<T>) {
        return std::to_string(val);
    } else {
        return std::string(val);
    }
}
```

### Где учить уровень 3

| Ресурс | Тип |
|---|---|
| CppCon на YouTube | Конференция |
| C++ Weekly (Jason Turner) | YouTube |
| Concurrency in Action (Williams) | Книга |

---

## Уровень 4 — Экспертный / Специализация

### 4.1 Выбор специализации

| Направление | Ключевые темы | Где применяется |
|---|---|---|
| Gamedev | ECS, рендеринг, физика, SIMD | Unreal Engine, Unity C++ |
| Системное ПО | OS internals, драйверы, ядро | Linux, embedded |
| HPC / Финансы | SIMD, memory layout, lock-free | Алготрейдинг |
| Компиляторы | LLVM, AST, IR, codegen | Clang, GCC |
| Embedded | RTOS, bare-metal, HAL | Arduino, STM32 |

### 4.2 Cache-friendly код (SoA vs AoS)

```cpp
// ХОРОШО: Structure of Arrays — линейный доступ, cache friendly
struct Particles {
    std::vector<float> x, y, z;
    std::vector<float> vx, vy, vz;
};

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

std::string s = std::format("Hello, {}! Score: {:.2f}", "Alice", 98.5);

auto result = nums
    | std::views::filter([](int x) { return x % 2 == 0; })
    | std::views::transform([](int x) { return x * x; })
    | std::views::take(3);
```

### 4.4 Корутины (C++20/23)

```cpp
#include <generator> // C++23

std::generator<int> fibonacci() {
    int a = 0, b = 1;
    while (true) {
        co_yield a;
        auto c = a + b; a = b; b = c;
    }
}
```

---

## Типичные ошибки новичков

```cpp
// ОШИБКА 1: Забытый virtual деструктор -> UB при delete через Base*
// ИСПРАВЛЕНИЕ: virtual ~Base() = default;

// ОШИБКА 2: Срезка объекта (slicing)
void process(Base obj);        // копирует только Base-часть!
void process(const Base& obj); // ИСПРАВЛЕНИЕ

// ОШИБКА 3: Итератор инвалидирован при push_back во время обхода -> UB

// ОШИБКА 4: Signed/unsigned сравнение
for (size_t i = 0; i < v.size(); ++i) // правильно

// ОШИБКА 5: Неинициализированные переменные
int x{}; // value-initialization = 0
```

---

## Инструменты профессионального разработчика

### Отладка и профилирование

```bash
# GDB — отладчик
gdb ./myprogram

# Valgrind — утечки памяти
valgrind --leak-check=full ./myprogram

# AddressSanitizer — быстрее Valgrind
g++ -fsanitize=address -g ./myprogram

# ThreadSanitizer — гонки данных
g++ -fsanitize=thread -g ./myprogram

# Perf — профилировщик производительности
perf record ./myprogram && perf report
```

### Статический анализ

```bash
clang-tidy myfile.cpp -- -std=c++20
cppcheck --enable=all src/
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
Месяц 1-2:   Синтаксис, типы, управляющие конструкции, функции
Месяц 3-4:   Указатели, ссылки, массивы, строки, std::vector
Месяц 5-6:   ООП, классы, наследование, полиморфизм
Месяц 7-8:   STL контейнеры, алгоритмы, итераторы
Месяц 9-10:  Умные указатели, move-семантика, шаблоны
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

## 📚 Теория C++ — углублённый раздел

> Этот раздел дополняет практический роадмап теоретической базой. Здесь собраны ключевые концепции языка с акцентом на «почему так устроено», а не только «как использовать». Каждый подраздел сопровождается коротким примером кода.

### Т.1 Модель памяти и время жизни объектов

В C++ каждый объект имеет storage duration: automatic (стек), static (глобальные и static), thread (thread_local) и dynamic (куча). Локальные объекты уничтожаются в порядке, обратном созданию; обращение к объекту вне окна «конструктор завершён → деструктор не начат» — undefined behavior; временные объекты живут до конца полного выражения, кроме продления жизни const-ссылкой.

```cpp
const std::string& bad() {
    std::string local = "hello";
    return local;            // UB: local уничтожен после return
}

const std::string& ok = std::string{"hi"}; // жизнь временного продлена до конца ok
```

### Т.2 RAII — фундамент управления ресурсами

RAII связывает время жизни ресурса с временем жизни объекта на стеке: захват в конструкторе, освобождение в деструкторе — даже при исключении. Поэтому в идиоматичном C++ почти не нужны ручные delete, fclose или unlock.

```cpp
void write_log(const std::string& msg) {
    std::lock_guard<std::mutex> lock(mtx); // захват
    std::ofstream file("log.txt", std::ios::app);
    file << msg << '\n';
}   // file закрыт, mtx разблокирован — автоматически, даже при исключении
```

### Т.3 Правило пяти и правило нуля

Если класс управляет ресурсом, он определяет пять функций: деструктор, копирующие и перемещающие конструктор и присваивание. Лучшая практика — правило нуля: делегировать управление ресурсами RAII-обёрткам, тогда компилятор сгенерирует всё сам.

```cpp
// Правило нуля: ничего не пишем руками — vector и string всё умеют
class Document {
    std::string title_;
    std::vector<std::string> lines_;
}; // копирование, перемещение и удаление сгенерированы корректно
```

### Т.4 Move-семантика и категории значений

lvalue имеет имя и адрес, prvalue — чистое временное, xvalue — «истекающий» объект. Move «крадёт» внутренности вместо копирования. std::move сам ничего не перемещает — это лишь приведение к rvalue-ссылке.

```cpp
std::vector<int> make() {
    std::vector<int> v(1'000'000);
    return v;                    // перемещение/NRVO, не копия
}
std::vector<int> a = make();
std::vector<int> b = std::move(a); // a теперь пуст — использовать нельзя
```

### Т.5 Шаблоны и обобщённое программирование

Шаблоны инстанцируются во время компиляции под конкретные типы. Концепты (C++20) задают читаемые ограничения вместо SFINAE, if constexpr выбирает ветвь при компиляции, variadic templates обрабатывают переменное число аргументов.

```cpp
template<typename T>
concept Addable = requires(T a, T b) { { a + b } -> std::same_as<T>; };

template<Addable T>
T sum(T a, T b) { return a + b; } // ошибка компиляции, если T не Addable
```

### Т.6 Полиморфизм: динамический и статический

Динамический полиморфизм — через vtable, выбор функции в рантайме (косвенный вызов, нет инлайна). Статический (шаблоны, CRTP) разрешается при компиляции: нет затрат рантайма, но больше кода.

```cpp
// Динамический
struct Shape { virtual double area() const = 0; virtual ~Shape() = default; };
struct Circle : Shape { double r; double area() const override { return 3.14*r*r; } };

// Статический (CRTP)
template<class D> struct Printable { void print() const { static_cast<const D*>(this)->impl(); } };
```

### Т.7 Const-корректность и constexpr

const выражает намерение «не изменять». Const-методы не меняют наблюдаемое состояние; mutable разрешает менять отдельные поля (кэш) в const-методе. constexpr переносит вычисление в этап компиляции.

```cpp
constexpr int factorial(int n) {
    return n <= 1 ? 1 : n * factorial(n - 1);
}
constexpr int f5 = factorial(5); // 120 вычислено при компиляции

class Cache {
    mutable int cached_ = -1; // меняется даже в const-методе
public:
    int get() const { if (cached_ < 0) cached_ = compute(); return cached_; }
};
```

### Т.8 Undefined Behavior — почему это опасно

UB — ситуации без определённого поведения: разыменование нулевого/висячего указателя, переполнение знакового целого, выход за границы, гонки данных. Компилятор предполагает, что UB не происходит, и агрессивно оптимизирует код. Ловят UB санитайзеры (ASan, UBSan) и Valgrind.

```cpp
int arr[3] = {1, 2, 3};
int x = arr[3];          // UB: выход за границы

int* p = nullptr;
int y = *p;              // UB: разыменование nullptr

// Сборка с проверками: g++ -fsanitize=address,undefined -g main.cpp
```

### Т.9 Многопоточность и модель памяти C++11

С C++11 язык имеет формальную модель памяти. Гонка данных (одновременный доступ без синхронизации, где есть запись) — это UB. Синхронизация — мьютексы, std::atomic и memory_order. Высокоуровневые средства: std::async, std::thread, std::jthread, future/promise.

```cpp
std::atomic<uint64_t> counter{0};

void worker() {
    for (int i = 0; i < 1000; ++i)
        counter.fetch_add(1, std::memory_order_relaxed); // без гонки
}

auto fut = std::async(std::launch::async, [] { return heavy(); });
int result = fut.get(); // блокирует до готовности
```

### Т.10 Эволюция стандартов

Таблица ниже кратко описывает, что каждый стандарт добавил в язык:

| Стандарт | Ключевые нововведения |
|---|---|
| C++11 | auto, лямбды, move-семантика, умные указатели, потоки, range-based for, nullptr, constexpr |
| C++14 | обобщённые лямбды, вывод возвращаемого типа через auto, переменные-шаблоны |
| C++17 | structured bindings, if constexpr, std::optional/variant/any, файловая система, параллельные алгоритмы STL |
| C++20 | концепты, ranges, модули, корутины, оператор трёхстороннего сравнения (<=>), std::span |
| C++23 | std::expected, std::mdspan, std::generator, улучшения ranges, deducing this |

### Т.11 Как пользоваться этим разделом

Теорию лучше изучать параллельно с практикой из уровней роадмапа. Возвращайтесь к разделам при конкретной проблеме: непонятная ошибка шаблона — Т.5; падение с повреждением памяти — Т.1 и Т.8; вопрос «копируется или перемещается» — Т.4. Понимание «почему» делает чтение чужого кода и проектирование собственного осознаннее.

---

*Этот роадмап создан для репозитория cpproadmap2026*
*Стандарты: C++11, C++14, C++17, C++20, C++23*

