# 🗺️ Полный роадмап C++ 2026 — от нуля до профессионала

> **Автор:** сгенерировано с помощью Claude AI
> **Актуальность:** C++11/14/17/20/23
> **Уровни:** от абсолютного новичка до эксперта

---

## 📖 О чём этот роадмап

Это не просто список тем, а пошаговый маршрут превращения новичка в профессионального C++-разработчика. Каждый уровень даёт не только перечень концепций, но и **рабочие примеры кода с пояснениями «как правильно» и «как неправильно»**, чтобы вы сразу видели идиоматичный современный C++, а не устаревшие практики из учебников 2000-х годов.

Роадмап построен по принципу «теория рядом с практикой»: сначала вы изучаете концепцию на коде из соответствующего уровня, а затем углубляете понимание в разделе [📚 Теория C++](#-теория-c--от-нуля-до-профи-с-примерами), где каждая тема разобрана от базовой интуиции до тонкостей, важных на собеседованиях и в продакшене. Такой подход экономит месяцы: вы не зубрите оторванные факты, а понимаете, **почему** язык устроен именно так.

**Как пользоваться:** идите по уровням сверху вниз, не перепрыгивая. Пишите каждый пример руками, ломайте его, смотрите на ошибки компилятора — именно так формируется интуиция. Параллельно решайте задачи на платформах из раздела ресурсов и читайте теорию по мере появления вопросов.

---

## 🔗 Полезные ресурсы и каналы

Подборка обучающих ресурсов, которые помогут прокачаться не только в C++, но и в смежных областях — базах данных, машинном обучении и .NET:

- 🖥 **[С++ Академия](https://t.me/+Cf3VMuCxqv9kYzIy)** — самый крупный обучающий ресурс в Telegram, посвящённый С++.
- 🧠 **[Machine learning](https://t.me/+IikNImh3FuNmM2Ji)** — показываем на примере, как использовать AI, который может генерировать готовые базы данных и код; разбираем всё, что нужно знать в области ИИ.
- 🖥 **[SQLHUB](https://t.me/+CUy_lIu9GKZlMGEy)** — вся база по работе с SQL.
- 🔝 **[Подборка для С#/.NET](https://t.me/addlist/u15AMycxRMowZmRi)** — самые полезные ресурсы для С#/.Net разработчиков.

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
- [📚 Теория C++ — от нуля до профи с примерами](#-теория-c--от-нуля-до-профи-с-примерами)

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
    auto name = std::string{"Alice"};   // явно std::string, не const char*
    auto pi = 3.14159265358979;          // double
    auto pif = 3.14159f;                 // float — суффикс f обязателен!

    // ПЛОХО — магические числа
    if (health < 20) std::cout << "critical\n";

    // ХОРОШО — именованные константы
    constexpr int32_t kCriticalHealth = 20;
    if (health < kCriticalHealth) std::cout << "critical\n";

    // ВАЖНО: Инициализируй всегда! Неинициализированная переменная = UB
    int x{};      // value-init = 0
    int y = 0;    // явно
    // int z;     <- мусор в памяти, UB при чтении

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

int main() {
    print_name("Alice");
    auto result = compute(42);
    auto [q, r] = divmod(17, 5);  // C++17 structured bindings
    std::cout << q << " rem " << r << "\n"; // 3 rem 2
}
```

### 1.3 Указатели и ссылки — разбор опасных мест

```cpp
// ПЛОХО — висячая ссылка
const std::string& bad_ref() {
    std::string local = "hello";
    return local;  // UB! local уничтожен после return
}

// ПЛОХО — забыли передать по ссылке, лишняя копия
void modify_bad(std::vector<int> v) { // копирует весь вектор!
    v.push_back(42);                   // изменяет копию, не оригинал
}

// Правило выбора способа передачи:
// | Тип               | Передавай как     |
// | int, double, ...  | по значению (T)   |
// | только читать     | const T&          |
// | нужно изменить    | T&                |
// | передать владение | unique_ptr<T>     |

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
    // reserve() если знаешь размер заранее — избегает realloc
    std::vector<int> v;
    v.reserve(1000);
    for (int i = 0; i < 1000; ++i) v.push_back(i);

    // emplace_back вместо push_back для объектов — конструирует на месте
    std::vector<std::pair<int,int>> pairs;
    pairs.emplace_back(1, 2);

    // STL алгоритмы вместо ручных циклов
    int sum = std::accumulate(v.begin(), v.end(), 0);
    auto it = std::find(v.begin(), v.end(), 42);
    bool has = (it != v.end());
    int cnt = std::count_if(v.begin(), v.end(), [](int x){ return x%2==0; });

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
| С++ Академия | Telegram | https://t.me/+Cf3VMuCxqv9kYzIy |
| Codeforces Div.4/3 | Практика | Задачи по алгоритмам |

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
    auto pi = 3.14159265358979;       // double
    auto pif = 3.14159f;              // float — суффикс f обязателен!

    // ПЛОХО — магические числа
    if (health < 20) std::cout << "critical\n";

    // ХОРОШО — именованные константы
    constexpr int32_t kCriticalHealth = 20;
    if (health < kCriticalHealth) std::cout << "critical\n";

    // ВАЖНО: Инициализируй всегда! Неинициализированная переменная = UB
    int x{}; // value-init = 0
    int y = 0; // явно
    // int z; <- мусор в памяти, UB при чтении

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

int main() {
    print_name("Alice");
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
    v.reserve(1000); // избегает realloc
    for (int i = 0; i < 1000; ++i) v.push_back(i);

    int sum = std::accumulate(v.begin(), v.end(), 0);
    auto it = std::find(v.begin(), v.end(), 42);
    int cnt = std::count_if(v.begin(), v.end(), [](int x){ return x%2==0; });

    std::sort(v.begin(), v.end(), std::greater<int>{}); // убывание

    // C++20 — удалить все нечётные
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
| С++ Академия | Telegram | https://t.me/+Cf3VMuCxqv9kYzIy |


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
        : owner_(std::move(owner))  // move, не копирование
        , balance_(initial_balance)
    {
        if (initial_balance < 0)
            throw std::invalid_argument("Balance cannot be negative");
    }

    // Виртуальный деструктор — обязателен для полиморфных классов
    virtual ~BankAccount() = default;

    const std::string& owner() const { return owner_; }
    double balance() const { return balance_; }

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

    friend std::ostream& operator<<(std::ostream& os, const BankAccount& acc) {
        return os << acc.owner_ << ": $" << acc.balance_;
    }

private:
    std::string owner_;
    double balance_;
};

// override — компилятор проверит что мы действительно переопределяем
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
auto ptr2 = std::move(ptr); // ptr = nullptr, нельзя скопировать

// shared_ptr — подсчёт ссылок (только при реальном совместном владении)
auto sp1 = std::make_shared<MyClass>(args...);
{
    auto sp2 = sp1; // счётчик = 2
}                   // sp2 уничтожен, счётчик = 1

// weak_ptr разрывает циклические зависимости
struct Employee;
struct Department {
    std::vector<std::weak_ptr<Employee>> members; // не владеем!
};
struct Employee {
    std::shared_ptr<Department> dept;             // владеем
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
    void print_impl() const { std::cout << "Circle(r=" << radius << ")\n"; }
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

double r = safe_sqrt(-1.0).value_or(0.0); // дефолт если пусто

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
class AppError : public std::runtime_error { using std::runtime_error::runtime_error; };
class NetworkError : public AppError { using AppError::AppError; };
class TimeoutError : public NetworkError { using NetworkError::NetworkError; };
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

## Уровень 2 — Средний C++ (3–4 месяца)

### 2.1 ООП — как правильно проектировать классы

```cpp
#include <string>
#include <stdexcept>

class BankAccount {
public:
    explicit BankAccount(std::string owner, double initial_balance = 0.0)
        : owner_(std::move(owner)), balance_(initial_balance) {
        if (initial_balance < 0)
            throw std::invalid_argument("Balance cannot be negative");
    }

    virtual ~BankAccount() = default; // виртуальный деструктор обязателен

    const std::string& owner() const { return owner_; }
    double balance() const { return balance_; }

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

private:
    std::string owner_;
    double balance_;
};

class SavingsAccount : public BankAccount {
public:
    SavingsAccount(std::string owner, double balance, double rate)
        : BankAccount(std::move(owner), balance), interest_rate_(rate) {}

    void deposit(double amount) override {
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

// shared_ptr — подсчёт ссылок (только при реальном совместном владении)
auto sp1 = std::make_shared<MyClass>(args...);

// weak_ptr разрывает циклические зависимости
struct Department { std::vector<std::weak_ptr<Employee>> members; };
struct Employee  { std::shared_ptr<Department> dept; };

// Принимай unique_ptr по значению = берёшь владение
void consume(std::unique_ptr<Node> node) { /* владеем */ }
```

### 2.3 Шаблоны (Templates)

```cpp
template<typename T>
T clamp(T val, T lo, T hi) {
    return val < lo ? lo : (val > hi ? hi : val);
}

// C++20 концепты — читаемее SFINAE
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

// Иерархия своих исключений
class AppError : public std::runtime_error { using std::runtime_error::runtime_error; };
class NetworkError : public AppError { using AppError::AppError; };
```

### Где учить уровень 2

| Ресурс | Тип |
|---|---|
| Effective Modern C++ (Scott Meyers) | Книга — обязательно |
| C++ Templates: The Complete Guide | Книга — шаблоны |
| LeetCode Medium | Практика |
| Back to Basics на CppCon | YouTube |
| SQLHUB (базы данных) | https://t.me/+CUy_lIu9GKZlMGEy |


---

## Уровень 3 — Продвинутый C++ (4–6 месяцев)

### 3.1 Move семантика и rvalue references

```cpp
void process(const std::string& s) { /* lvalue */ }
void process(std::string&& s) {     /* rvalue */
    std::string local = std::move(s); // "крадём" содержимое
}

// Perfect forwarding — сохраняет категорию значения
template<typename T>
void wrapper(T&& arg) {
    process(std::forward<T>(arg));
}

std::string s = "hello";
auto s2 = std::move(s); // s теперь пусто — не используй после move!

// ПЛОХО — move в return мешает NRVO
std::string make() {
    std::string r = "result";
    return r;             // ХОРОШО: NRVO сработает
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
#include <queue>

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

private:
    mutable std::mutex mutex_;
    std::condition_variable cv_;
    std::queue<T> queue_;
};

// atomic — для счётчиков без мьютекса
std::atomic<uint64_t> counter{0};
counter.fetch_add(1, std::memory_order_relaxed);

// async — параллельное выполнение
auto future = std::async(std::launch::async, []{ return heavy_computation(); });
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
    ScopeGuard rollback_guard([&db]{ db.rollback(); });
    db.execute("UPDATE users SET active=1 WHERE id=42");
    db.commit();
    rollback_guard.dismiss(); // успех — отменяем rollback
}

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
    std::unique_ptr<Impl> impl_; // детали только в .cpp
};
```

### 3.4 Метапрограммирование — практичные случаи

```cpp
#include <type_traits>
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
    } else if constexpr (std::is_arithmetic_v<T>) {
        return std::to_string(val);
    } else {
        return std::string(val);
    }
}

// Концепты (C++20)
template<typename T>
concept Sortable = requires(T c) {
    c.begin(); c.end();
    { c.size() } -> std::convertible_to<size_t>;
};
```

### 3.5 CMake — правильная структура проекта

```cmake
cmake_minimum_required(VERSION 3.20)
project(MyProject VERSION 1.0.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

option(BUILD_TESTS "Build tests" ON)
option(ENABLE_ASAN "Enable AddressSanitizer" OFF)

add_library(project_warnings INTERFACE)
target_compile_options(project_warnings INTERFACE
    -Wall -Wextra -Wpedantic -Wshadow -Wconversion
    $<$<CONFIG:Debug>:-g>
    $<$<CONFIG:Release>:-O2 -DNDEBUG>
)

if(ENABLE_ASAN)
    target_compile_options(project_warnings INTERFACE -fsanitize=address,undefined)
    target_link_options(project_warnings INTERFACE -fsanitize=address,undefined)
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

## Уровень 3 — Продвинутый C++ (4–6 месяцев)

### 3.1 Move семантика и rvalue references

```cpp
void process(const std::string& s) { /* lvalue */ }
void process(std::string&& s) {       /* rvalue */
    std::string local = std::move(s); // "крадём" содержимое
}

// Perfect forwarding — сохраняет категорию значения
template<typename T>
void wrapper(T&& arg) { process(std::forward<T>(arg)); }

std::string s = "hello";
auto s2 = std::move(s); // s теперь пусто — не используй после move!

// ПЛОХО — move в return мешает NRVO
std::string make() {
    std::string r = "result";
    return r; // ХОРОШО: NRVO сработает
}
```

### 3.2 Многопоточность — правильные паттерны

```cpp
#include <thread>
#include <mutex>
#include <condition_variable>
#include <atomic>
#include <future>

template<typename T>
class ThreadSafeQueue {
public:
    void push(T value) {
        { std::lock_guard<std::mutex> lock(mutex_); queue_.push(std::move(value)); }
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

// async — параллельное выполнение
auto future = std::async(std::launch::async, []{ return heavy_computation(); });
int result = future.get();
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
    ScopeGuard rollback_guard([&db]{ db.rollback(); });
    db.execute("UPDATE users SET active=1 WHERE id=42");
    db.commit();
    rollback_guard.dismiss(); // успех — отменяем rollback
}
```

### 3.4 Метапрограммирование — практичные случаи

```cpp
#include <type_traits>
#include <array>

constexpr std::array<int, 10> make_squares() {
    std::array<int, 10> arr{};
    for (int i = 0; i < 10; ++i) arr[i] = i * i;
    return arr;
}
constexpr auto SQUARES = make_squares(); // вычисляется при компиляции!

// if constexpr — ветвление на уровне типов (C++17)
template<typename T>
std::string to_string_smart(T val) {
    if constexpr (std::is_same_v<T, bool>) return val ? "true" : "false";
    else if constexpr (std::is_arithmetic_v<T>) return std::to_string(val);
    else return std::string(val);
}
```

### 3.5 CMake — правильная структура проекта

```cmake
cmake_minimum_required(VERSION 3.20)
project(MyProject VERSION 1.0.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

add_library(project_warnings INTERFACE)
target_compile_options(project_warnings INTERFACE
    -Wall -Wextra -Wpedantic -Wshadow -Wconversion
    $<$<CONFIG:Debug>:-g>
    $<$<CONFIG:Release>:-O2 -DNDEBUG>)

add_subdirectory(src)
```

### Где учить уровень 3

| Ресурс | Тип |
|---|---|
| CppCon на YouTube | Конференция — отличные доклады |
| C++ Weekly (Jason Turner) | YouTube — практические трюки |
| Concurrency in Action (Williams) | Книга — многопоточность |
| LeetCode Hard, Codeforces Div.1 | Практика |
| Machine learning (ИИ для кода) | https://t.me/+IikNImh3FuNmM2Ji |


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

void update_positions(Particles& p, float dt) {
    const size_t n = p.x.size();
    for (size_t i = 0; i < n; ++i) {
        p.x[i] += p.vx[i] * dt;  // 3 непрерывных массива — cache friendly!
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

// Ranges pipeline — ленивые цепочки без промежуточных векторов
std::vector<int> nums = {1,2,3,4,5,6,7,8,9,10};
auto result = nums
    | std::views::filter([](int x){ return x % 2 == 0; })
    | std::views::transform([](int x){ return x * x; })
    | std::views::take(3);
for (int x : result) std::cout << x << " "; // 4 16 36

for (int i : std::views::iota(1, 11)) std::cout << i << " "; // 1..10
```

### 4.4 Корутины (C++20/23)

```cpp
#include <generator> // C++23

std::generator<int> fibonacci() {
    int a = 0, b = 1;
    while (true) {
        co_yield a;
        auto c = a + b;
        a = b; b = c;
    }
}

for (int fib : fibonacci() | std::views::take(10))
    std::cout << fib << " "; // 0 1 1 2 3 5 8 13 21 34
```

---

## Типичные ошибки новичков

```cpp
// ОШИБКА 1: Забытый virtual деструктор
class Base { };
class Derived : public Base { ~Derived() { /* cleanup */ } };
Base* ptr = new Derived();
delete ptr; // UB! деструктор Derived не вызовется
// ИСПРАВЛЕНИЕ: class Base { public: virtual ~Base() = default; };

// ОШИБКА 2: Срезка объекта (object slicing)
void process(Base obj) { obj.do_something(); } // копирует только Base-часть!
// ИСПРАВЛЕНИЕ: void process(const Base& obj)

// ОШИБКА 3: Итератор инвалидирован при push_back
std::vector<int> v = {1,2,3,4,5};
for (auto it = v.begin(); it != v.end(); ++it) {
    if (*it == 3) v.push_back(99); // UB! реаллокация инвалидирует итераторы
}
// ИСПРАВЛЕНИЕ: собери что добавить после цикла

// ОШИБКА 4: Signed/unsigned сравнение
for (int i = 0; i < v.size(); ++i) // warning! v.size() — unsigned
// ИСПРАВЛЕНИЕ: for (size_t i = 0; ...) или range-based for

// ОШИБКА 5: Не инициализированные переменные
int x;     // мусорное значение! UB при чтении
int x{};   // ИСПРАВЛЕНИЕ: value-initialization = 0
```

---

## Инструменты профессионального разработчика

### Отладка и профилирование

```bash
gdb ./myprogram                              # отладчик
valgrind --leak-check=full ./myprogram       # утечки памяти
g++ -fsanitize=address -g ./myprogram        # AddressSanitizer
g++ -fsanitize=thread -g ./myprogram         # ThreadSanitizer — гонки данных
perf record ./myprogram && perf report       # профилировщик
heaptrack ./myprogram                        # профилировщик памяти
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

### Онлайн-ресурсы

| Ресурс | Описание |
|---|---|
| learncpp.com | Лучший бесплатный курс для начинающих |
| cppreference.com | Главный справочник по стандарту |
| godbolt.org | Compiler Explorer — смотри ассемблер |
| cppinsights.io | Видишь что делает компилятор с шаблонами |
| quick-bench.com | Бенчмаркинг прямо в браузере |

### Telegram-каналы

| Канал | Описание |
|---|---|
| [С++ Академия](https://t.me/+Cf3VMuCxqv9kYzIy) | Крупнейший обучающий ресурс по C++ |
| [Machine learning](https://t.me/+IikNImh3FuNmM2Ji) | AI на практике: генерация кода и баз данных |
| [SQLHUB](https://t.me/+CUy_lIu9GKZlMGEy) | Вся база по работе с SQL |
| [С#/.NET подборка](https://t.me/addlist/u15AMycxRMowZmRi) | Ресурсы для С#/.Net разработчиков |

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
    char name[32]; // мусор в кэше при обработке позиций
};

// ХОРОШО: Structure of Arrays — линейный доступ к каждому полю
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

std::vector<int> nums = {1,2,3,4,5,6,7,8,9,10};
auto result = nums
    | std::views::filter([](int x) { return x % 2 == 0; })
    | std::views::transform([](int x) { return x * x; })
    | std::views::take(3);
for (int x : result) std::cout << x << " "; // 4 16 36
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

for (int fib : fibonacci() | std::views::take(10))
    std::cout << fib << " "; // 0 1 1 2 3 5 8 13 21 34
```

---

## Типичные ошибки новичков

```cpp
// ОШИБКА 1: Забытый virtual деструктор
class Base { };                       // нет virtual ~Base()
class Derived : public Base { };
Base* ptr = new Derived();
delete ptr; // UB! деструктор Derived не вызовется
// ИСПРАВЛЕНИЕ: class Base { public: virtual ~Base() = default; };

// ОШИБКА 2: Срезка объекта (object slicing)
void process(Base obj);          // копирует только Base-часть!
// ИСПРАВЛЕНИЕ: void process(const Base& obj);

// ОШИБКА 3: Итератор инвалидирован при push_back
for (auto it = v.begin(); it != v.end(); ++it)
    if (*it == 3) v.push_back(99); // UB! реаллокация инвалидирует итераторы

// ОШИБКА 4: Signed/unsigned сравнение
for (int i = 0; i < v.size(); ++i)   // warning! v.size() — unsigned
// ИСПРАВЛЕНИЕ: for (size_t i = 0; i < v.size(); ++i)

// ОШИБКА 5: Не инициализированные переменные
int x;       // мусор! UB при чтении
int x2{};    // value-initialization = 0
```

---

## Инструменты профессионального разработчика

```bash
# GDB — отладчик
gdb ./myprogram

# Valgrind — утечки памяти
valgrind --leak-check=full ./myprogram

# AddressSanitizer — быстрее Valgrind
g++ -fsanitize=address -g ./myprogram

# ThreadSanitizer — гонки данных
g++ -fsanitize=thread -g ./myprogram

# clang-tidy — линтер и анализатор
clang-tidy myfile.cpp -- -std=c++20

# clang-format — форматирование
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

### Telegram-каналы

| Канал | Описание |
|---|---|
| [С++ Академия](https://t.me/+Cf3VMuCxqv9kYzIy) | Крупнейший обучающий ресурс по C++ |
| [Machine learning](https://t.me/+IikNImh3FuNmM2Ji) | AI для генерации кода и баз данных |
| [SQLHUB](https://t.me/+CUy_lIu9GKZlMGEy) | Вся база по работе с SQL |
| [С#/.NET подборка](https://t.me/addlist/u15AMycxRMowZmRi) | Ресурсы для C#/.Net разработчиков |

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
Месяц 1-2:  Синтаксис, типы, управляющие конструкции, функции
Месяц 3-4:  Указатели, ссылки, массивы, строки, std::vector
Месяц 5-6:  ООП, классы, наследование, полиморфизм
Месяц 7-8:  STL контейнеры, алгоритмы, итераторы
Месяц 9-10: Умные указатели, move-семантика, шаблоны
Месяц 11-12:Многопоточность, исключения, RAII
Год 2:      Углублённые шаблоны, метапрограммирование, C++20
Год 2-3:    Специализация по выбранному направлению
```

---

## Глоссарий

| Термин | Объяснение |
|---|---|
| UB | Undefined Behavior — поведение не определено стандартом, опасно |
| RAII | Resource Acquisition Is Initialization — ресурс берётся в конструкторе, освобождается в деструкторе |
| lvalue/rvalue | lvalue — есть имя, rvalue — временный объект |
| ODR | One Definition Rule — каждая сущность имеет ровно одно определение |
| ABI | Application Binary Interface — бинарная совместимость между модулями |
| TMP | Template Metaprogramming — программирование на шаблонах |
| SFINAE | Substitution Failure Is Not An Error — ошибка подстановки шаблона не ошибка |
| CRTP | Curiously Recurring Template Pattern — статический полиморфизм |
| SoA/AoS | Structure of Arrays / Array of Structures — паттерны layout памяти |

---

## 📚 Теория C++ — от нуля до профи с примерами

> Этот раздел даёт глубокое понимание того, **почему** язык устроен именно так. Каждая тема идёт от простой интуиции к тонкостям уровня профи и сопровождается рабочими примерами. Изучайте параллельно с практикой из уровней роадмапа: столкнулись с проблемой — вернитесь к нужному пункту.

### Т.1 Модель памяти и время жизни объектов

В C++ каждый объект имеет **storage duration** (длительность хранения): *automatic* (стек, локальные переменные), *static* (глобальные и static-переменные), *thread* (thread_local) и *dynamic* (куча). Объект «жив» с момента завершения работы конструктора до начала работы деструктора; обращение к нему вне этого окна — undefined behavior. Локальные объекты уничтожаются в порядке, **обратном** созданию. Временные объекты живут до конца полного выражения, кроме случая продления жизни привязкой к const-ссылке.

```cpp
#include <iostream>
#include <string>

struct Tracer {
    std::string name;
    Tracer(std::string n) : name(std::move(n)) { std::cout << "+ " << name << "\n"; }
    ~Tracer() { std::cout << "- " << name << "\n"; }
};

const std::string& dangling() {
    std::string local = "temp";
    return local;          // ❌ UB: local умрёт здесь
}

int main() {
    Tracer a{"a"};         // создаётся первым
    Tracer b{"b"};         // создаётся вторым
    // Продление жизни временного объекта const-ссылкой:
    const std::string& ext = std::string("hello"); // живёт до конца main
    std::cout << ext << "\n";
}   // деструкторы вызовутся в порядке: b, затем a
```

**Профи-уровень:** понимание времени жизни — основа диагностики «случайных» падений. Висячие ссылки/указатели, use-after-free и dangling возвращаемые значения — все они нарушают окно жизни объекта. Санитайзер ASan ловит большинство таких ошибок в рантайме.

### Т.2 RAII — фундамент управления ресурсами

RAII (Resource Acquisition Is Initialization) связывает время жизни ресурса с временем жизни объекта на стеке: ресурс захватывается в конструкторе и **гарантированно** освобождается в деструкторе — даже при выбросе исключения, благодаря раскрутке стека. Именно поэтому в идиоматичном C++ почти нет ручных delete/fclose/unlock.

```cpp
#include <cstdio>
#include <stdexcept>

// RAII-обёртка над C-файлом
class File {
public:
    explicit File(const char* path, const char* mode)
        : f_(std::fopen(path, mode)) {
        if (!f_) throw std::runtime_error("cannot open file");
    }
    ~File() { if (f_) std::fclose(f_); } // освобождение гарантировано
    File(const File&) = delete;          // запрещаем копирование ресурса
    File& operator=(const File&) = delete;
    std::FILE* get() const { return f_; }
private:
    std::FILE* f_;
};

void write_log() {
    File log("app.log", "a");   // открыли
    std::fprintf(log.get(), "started\n");
    throw std::runtime_error("boom"); // даже здесь файл закроется!
}   // ~File() вызовется при раскрутке стека
```

**Профи-уровень:** стандартные RAII-обёртки — std::unique_ptr (память), std::fstream (файлы), std::lock_guard / std::scoped_lock (мьютексы), std::jthread (C++20, поток с автоприсоединением). RAII превращает управление ресурсами из ручной дисциплины в гарантию, проверяемую компилятором.

### Т.3 Правило пяти и правило нуля

Если класс **напрямую** управляет ресурсом, он должен корректно определить пять специальных функций: деструктор, копирующий конструктор, копирующее присваивание, перемещающий конструктор и перемещающее присваивание. Но лучшая практика — **правило нуля**: проектировать классы так, чтобы они не управляли ресурсами вручную, а делегировали это RAII-обёрткам.

```cpp
#include <memory>
#include <vector>
#include <string>

// ❌ Правило пяти: вручную управляем сырой памятью (нужно всё определить)
class Buffer {
public:
    explicit Buffer(size_t n) : size_(n), data_(new int[n]) {}
    ~Buffer() { delete[] data_; }
    Buffer(const Buffer& o) : size_(o.size_), data_(new int[o.size_]) {
        std::copy(o.data_, o.data_ + size_, data_);
    }
    Buffer& operator=(Buffer o) { swap(o); return *this; } // copy-and-swap
    Buffer(Buffer&& o) noexcept { swap(o); }
    void swap(Buffer& o) noexcept { std::swap(size_, o.size_); std::swap(data_, o.data_); }
private:
    size_t size_ = 0;
    int* data_ = nullptr;
};

// ✅ Правило нуля: ресурсами управляют члены, компилятор сам сгенерирует всё
class Buffer2 {
    std::vector<int> data_;       // вектор сам управляет памятью
    std::string name_;            // строка сама управляет памятью
public:
    Buffer2(size_t n, std::string name) : data_(n), name_(std::move(name)) {}
    // никаких ~, copy, move писать не нужно — всё корректно автоматически
};
```

**Профи-уровень:** если вы пишете деструктор вручную — это сигнал пересмотреть дизайн в сторону правила нуля. Перемещающие операции помечайте noexcept, иначе контейнеры (std::vector при reallocation) будут копировать вместо перемещения.

### Т.4 Move-семантика и категории значений

**Категории значений** описывают, чем является выражение: *lvalue* имеет имя и адрес, *prvalue* — чистое временное значение, *xvalue* — «истекающий» объект, ресурсы которого можно забрать. Move-семантика позволяет «украсть» внутренности у объекта, который скоро будет уничтожен, вместо дорогого глубокого копирования. Важно: **std::move сам по себе ничего не перемещает** — это лишь приведение к rvalue-ссылке, разрешающее перемещение.

```cpp
#include <string>
#include <vector>
#include <utility>

std::vector<std::string> make() {
    std::vector<std::string> v;
    v.push_back("a");
    return v;               // перемещается (или NRVO) — без копии
}

void demo() {
    std::string s = "очень длинная строка...";
    std::string s2 = std::move(s);  // украли буфер у s, s теперь пуст
    // s использовать НЕЛЬЗЯ (кроме переприсваивания)

    std::vector<std::string> v;
    std::string item = "x";
    v.push_back(item);              // КОПИЯ (item ещё нужен)
    v.push_back(std::move(item));   // ПЕРЕМЕЩЕНИЕ (item больше не нужен)
}
```

**Профи-уровень:** реальное перемещение происходит только там, где определены перемещающие операции; иначе тихо выполняется копирование. Perfect forwarding (T&& + std::forward) сохраняет категорию значения при передаче через шаблон. Не пишите `return std::move(local)` — это мешает NRVO.


---

## 📚 Теория C++ — от нуля до профи с примерами

> Этот раздел даёт теоретическую базу, без которой невозможно вырасти из «человека, пишущего код» в «человека, проектирующего системы». Каждая тема разобрана от простой интуиции до тонкостей, а **рядом с объяснением идёт код**, который можно скопировать в [godbolt.org](https://godbolt.org) и потрогать руками. Изучайте теорию параллельно с практикой из уровней роадмапа, а не отдельно.

### Т.1 Модель памяти и время жизни объектов

**Интуиция:** каждый объект где-то «живёт», и у этого жилья есть срок. В C++ есть четыре вида storage duration: *automatic* (стек, локальные переменные), *static* (глобальные/static), *thread* (`thread_local`) и *dynamic* (куча). Объект «жив» с момента, когда отработал его конструктор, и до момента, когда начал работать деструктор. Обращение к нему вне этого окна — undefined behavior.

```cpp
#include <iostream>

struct Loud {
    int id;
    Loud(int i) : id(i) { std::cout << "ctor " << id << "\n"; }
    ~Loud()             { std::cout << "dtor " << id << "\n"; }
};

void demo() {
    Loud a(1);          // automatic — на стеке
    Loud b(2);
    {                   // вложенный scope
        Loud c(3);
    }                   // dtor 3 здесь — c уничтожен раньше
    // выход из demo()  -> dtor 2, dtor 1 (обратный порядок!)
}

// ОПАСНО: висячая ссылка на временный объект
const std::string& danger() {
    return std::string("temp"); // временный объект умрёт сразу — UB
}

// ОК: продление жизни временного через const-ссылку
const std::string& ok = std::string("lives until 'ok' dies");
```

**Профи-нюанс:** временные объекты живут до конца полного выражения (точки с запятой), *кроме* случая привязки к `const`-ссылке (или к rvalue-ссылке) — тогда их жизнь продлевается до жизни ссылки. Это не работает «через слой»: если функция возвращает ссылку на временный, продление не наступает.

### Т.2 RAII — фундамент управления ресурсами

**Интуиция:** «захватил ресурс в конструкторе — освободи в деструкторе». Тогда ресурс освобождается **автоматически и гарантированно**, даже если выбросилось исключение (благодаря раскрутке стека). Именно поэтому в идиоматичном C++ почти нет ручных `delete`, `fclose`, `unlock`.

```cpp
// ПЛОХО — ручное управление, утечка при исключении
void bad() {
    int* p = new int[100];
    risky();          // если бросит исключение — delete[] не вызовется (утечка!)
    delete[] p;
}

// ХОРОШО — RAII через умный указатель
void good() {
    auto p = std::make_unique<int[]>(100);
    risky();          // бросит исключение? p всё равно освободится при раскрутке стека
}                     // память освобождена автоматически
```

### Т.3 Правило пяти и правило нуля

**Интуиция:** если класс сам управляет ресурсом (память, файл, сокет), он обычно обязан правильно определить *пять* функций: деструктор, копирующие конструктор/присваивание, перемещающие конструктор/присваивание. Но лучшая практика — **правило нуля**: не управлять ресурсами вручную вообще, а делегировать это RAII-обёрткам. Тогда компилятор сам сгенерирует все пять корректно.

```cpp
// Правило НУЛЯ — ничего писать не надо, всё корректно по умолчанию
class User {
    std::string name_;          // сам управляет своей памятью
    std::vector<int> scores_;   // сам управляет своей памятью
    // ни деструктора, ни копирования писать НЕ нужно — компилятор справится
};

// Правило ПЯТИ — нужно ТОЛЬКО если держишь сырой ресурс
class Buffer {
public:
    explicit Buffer(size_t n) : data_(new int[n]), size_(n) {}
    ~Buffer() { delete[] data_; }                                  // 1
    Buffer(const Buffer& o) : data_(new int[o.size_]), size_(o.size_) {
        std::copy(o.data_, o.data_ + size_, data_);                // 2
    }
    Buffer& operator=(const Buffer&) = delete;                     // 3 (упрощённо)
    Buffer(Buffer&& o) noexcept : data_(o.data_), size_(o.size_) { // 4
        o.data_ = nullptr; o.size_ = 0;
    }
    Buffer& operator=(Buffer&&) = delete;                          // 5
private:
    int* data_;
    size_t size_;
};
```

### Т.4 Move-семантика и категории значений

**Интуиция:** *lvalue* — у выражения есть имя и адрес (`x`); *prvalue* — чистое временное значение (`x + 1`, `42`); *xvalue* — «истекающий» объект, у которого можно забрать кишки. Move-семантика позволяет «украсть» внутренности у объекта, который скоро умрёт, вместо дорогого копирования. **Главное заблуждение:** `std::move` сам ничего не перемещает — это лишь приведение к rvalue-ссылке, дающее *разрешение* на перемещение.

```cpp
std::vector<int> big(1'000'000, 42);

auto copy = big;             // КОПИЯ: выделяется новый миллион интов
auto moved = std::move(big); // MOVE: просто переставили указатели — O(1)
// big теперь в "moved-from" состоянии: валиден, но пуст. Не читай его значение!

// std::move == static_cast<T&&> — никакого "движения" сам по себе нет
template<typename T>
T&& my_move(T& x) { return static_cast<T&&>(x); }
```

**Профи-нюанс:** перемещение реально происходит только если у типа определены move-операции. Иначе компилятор тихо делает копию. Помечайте move-конструкторы `noexcept` — иначе `std::vector` при росте предпочтёт копировать ради строгой гарантии исключений.

### Т.5 Шаблоны и обобщённое программирование

**Интуиция:** шаблон — это инструкция компилятору «как сгенерировать код под конкретный тип». Генерация (инстанцирование) происходит во время компиляции. Современный C++ делает шаблоны читаемее: **концепты** задают понятные ограничения вместо громоздких SFINAE, **if constexpr** выбирает ветвь на этапе компиляции, **fold-выражения** обрабатывают переменное число аргументов.

```cpp
// Старый способ ограничить тип — SFINAE (нечитаемо)
template<typename T,
         typename = std::enable_if_t<std::is_integral_v<T>>>
T old_sum(T a, T b) { return a + b; }

// C++20 — концепт: коротко и сообщения об ошибках понятные
template<std::integral T>
T sum(T a, T b) { return a + b; }

// Variadic + fold expression: сумма любого числа аргументов
template<typename... Args>
auto total(Args... xs) { return (xs + ...); } // (a + (b + (c + ...)))

total(1, 2, 3, 4); // 10
```

### Т.6 Полиморфизм: динамический и статический

**Интуиция:** *динамический* полиморфизм (виртуальные функции) выбирает нужную реализацию **во время выполнения** через таблицу vtable — гибко, но есть цена косвенного вызова и нельзя заинлайнить. *Статический* полиморфизм (шаблоны, CRTP) разрешается **при компиляции** — нулевая цена в рантайме, но больше кода и сложнее ошибки.

```cpp
// Динамический — решение в рантайме
struct Shape { virtual double area() const = 0; virtual ~Shape() = default; };
struct Square : Shape { double s; double area() const override { return s*s; } };

void print_area(const Shape& sh) { std::cout << sh.area(); } // vtable-вызов

// Статический (CRTP) — решение при компиляции, можно заинлайнить
template<typename D>
struct ShapeS { double area() const { return static_cast<const D*>(this)->area_impl(); } };
struct SquareS : ShapeS<SquareS> {
    double s;
    double area_impl() const { return s*s; }
};
```

### Т.7 Const-корректность и constexpr

**Интуиция:** `const` выражает намерение «не менять» — это помогает и компилятору (оптимизации), и читателю. `mutable` разрешает менять отдельное поле даже в `const`-методе (кэш, мьютекс). `constexpr` идёт дальше — переносит вычисление из рантайма в **компиляцию**.

```cpp
class Cache {
public:
    int get() const {              // const-метод: логически не меняет объект
        if (!ready_) {
            value_ = compute();    // но mutable-поля менять можно
            ready_ = true;
        }
        return value_;
    }
private:
    mutable int value_ = 0;        // mutable: исключение из const
    mutable bool ready_ = false;
    int compute() const { return 42; }
};

// constexpr — факториал считается на этапе компиляции
constexpr int factorial(int n) { return n <= 1 ? 1 : n * factorial(n - 1); }
constexpr int F5 = factorial(5);   // 120 — в бинарнике уже константа, не вычисление
static_assert(F5 == 120);
```

### Т.8 Undefined Behavior — почему это опасно

**Интуиция:** UB — это ситуации, для которых стандарт *намеренно* не задаёт поведения: разыменование nullptr/висячего указателя, переполнение знакового int, выход за границы массива, гонки данных. Опасность в том, что компилятор **вправе считать, что UB никогда не случается**, и агрессивно оптимизирует на этом основании — поэтому симптомы проявляются непредсказуемо и далеко от причины.

```cpp
// Компилятор может удалить проверку, считая что UB не бывает
int foo(int* p) {
    int x = *p;        // если сюда дошли — p НЕ nullptr (иначе уже было бы UB)
    if (p == nullptr)  // ...поэтому компилятор вправе ВЫРЕЗАТЬ эту проверку!
        return -1;
    return x;
}

// Знаковое переполнение — тоже UB (а беззнаковое определено как wrap-around)
int x = INT_MAX;
int y = x + 1;         // UB! не полагайся на "обернётся в минус"

// Лови UB санитайзерами:
// g++ -fsanitize=address,undefined -g main.cpp
```

### Т.9 Многопоточность и модель памяти C++11

**Интуиция:** *гонка данных* — это когда два потока обращаются к одной ячейке памяти без синхронизации и хотя бы один пишет. Это **UB**. Синхронизация достигается мьютексами, атомиками и порядками памяти (`memory_order`).

```cpp
#include <thread>
#include <mutex>
#include <atomic>

// ПЛОХО — гонка данных, UB
long bad_counter = 0;
void race() { for (int i=0;i<100000;++i) ++bad_counter; } // несколько потоков = мусор

// ХОРОШО (вариант 1) — атомик
std::atomic<long> ok_counter{0};
void with_atomic() { for (int i=0;i<100000;++i) ok_counter.fetch_add(1); }

// ХОРОШО (вариант 2) — мьютекс + RAII-замок
std::mutex m;
long guarded = 0;
void with_mutex() {
    for (int i=0;i<100000;++i) {
        std::lock_guard<std::mutex> lk(m); // освободит замок автоматически
        ++guarded;
    }
}
```

### Т.10 Эволюция стандартов

Что добавил каждый стандарт:

| Стандарт | Ключевые нововведения |
|---|---|
| C++11 | auto, лямбды, move-семантика, умные указатели, потоки, range-based for, nullptr, constexpr |
| C++14 | обобщённые лямбды, вывод возвращаемого типа через auto, переменные-шаблоны |
| C++17 | structured bindings, if constexpr, std::optional/variant/any, файловая система, параллельные алгоритмы STL |
| C++20 | концепты, ranges, модули, корутины, оператор <=>, std::span |
| C++23 | std::expected, std::mdspan, std::generator, улучшения ranges, deducing this |

### Т.11 Как пользоваться этим разделом

Возвращайтесь сюда, когда упёрлись в конкретную проблему: непонятная ошибка шаблона → Т.5; падение с повреждением памяти → Т.1 и Т.8; вопрос «копируется или перемещается» → Т.4; гонки в многопотоке → Т.9. Понимание «почему» делает чтение чужого кода и проектирование собственного гораздо осознаннее.

---

*Этот роадмап создан для репозитория cpproadmap2026*
*Стандарты: C++11, C++14, C++17, C++20, C++23*
