# C++ in Embedded Systems Interview Questions
> Modern C++ (C++11, C++14, C++17, C++20) in Embedded Context  
> Microcontrollers, RTOS, Real-Time Systems  
> M.Tech Graduate + 10 Years Experience  
> Basic → Intermediate → Advanced → Expert → War Stories

---

## INTRODUCTION: WHY C++ IN EMBEDDED?

**Arguments FOR C++ in embedded:**
- Object-oriented design reduces complexity
- RAII (Resource Acquisition Is Initialization) — automatic cleanup
- Templates enable zero-cost abstractions
- Type safety vs C
- Standard library (though limited in embedded)

**Arguments AGAINST C++ in embedded:**
- Code size overhead (vtable, exceptions, RTTI)
- Memory overhead (hidden allocations, temporaries)
- Latency unpredictability (exceptions, dynamic allocation)
- Learning curve steeper than C
- Older embedded systems don't support C++

**Embedded C++ Reality:**
- Subset of C++ is acceptable: no RTTI, no exceptions, no dynamic allocation
- Core language features (classes, templates, const) are valuable
- Bare-metal/RTOS projects still prefer C, but C++ gaining adoption in modern systems

---

## BASIC C++ FUNDAMENTALS

### 1. Classes & Objects

1. What is a class in C++?
   - Follow-up: Encapsulation — data (members) + methods (behavior).
   - Follow-up: Access levels: `public`, `private`, `protected`.
   - Follow-up: Constructor and destructor.
   - Follow-up: `this` pointer — implicit reference to current object.

2. How to define a simple class?
   ```cpp
   class GPIO {
   private:
       uint32_t port;
       uint32_t pin;
   
   public:
       GPIO(uint32_t p, uint32_t n) : port(p), pin(n) {}
       
       void set_output() {
           // Configure as output
       }
       
       void write(bool value) {
           if (value) {
               // Set pin high
           } else {
               // Set pin low
           }
       }
   };
   ```

3. What is constructor and destructor?
   - Follow-up: Constructor: initializes object, called when created.
   - Follow-up: Destructor: cleans up object, called when destroyed.
   - Follow-up: Embedded gotcha: static objects created at startup, destroyed (maybe) at shutdown.
   ```cpp
   class UART {
   public:
       UART(uint32_t baud) {
           // Initialize UART hardware
       }
       
       ~UART() {
           // Disable UART, free resources
       }
   };
   ```

4. What is initializer list?
   - Follow-up: More efficient than assignment in constructor body.
   - Follow-up: Required for `const` members, references, base classes.
   ```cpp
   class Timer {
   private:
       const uint32_t period;
   
   public:
       Timer(uint32_t p) : period(p) {}  // initializer list
   };
   ```

5. What is member function (method)?
   - Follow-up: Function inside class, operates on class members.
   - Follow-up: `const` member function: cannot modify members.
   ```cpp
   class Sensor {
   private:
       uint16_t last_value;
   
   public:
       void read() {  // non-const, can modify
           last_value = read_adc();
       }
       
       uint16_t get_value() const {  // const, cannot modify
           return last_value;
       }
   };
   ```

---

### 2. Memory Management in Embedded C++

6. What is dynamic memory allocation in C++?
   - Follow-up: `new` and `delete` operators.
   - Follow-up: In embedded, **prefer to avoid** — stack allocation is safer.
   ```cpp
   int *p = new int(42);  // heap allocation
   delete p;              // must manually free
   ```

7. Why is `new`/`delete` dangerous in embedded?
   - Follow-up: Heap fragmentation — memory allocated/freed, creates holes.
   - Follow-up: Unpredictable latency — heap operations not O(1).
   - Follow-up: Memory leak risk — if delete forgotten, memory leaked.
   - Follow-up: Real-time violation — cannot guarantee allocation will succeed.

8. What is stack vs heap in embedded?
   - Follow-up: Stack: automatic, fast, fixed size (typically 4KB–64KB).
   - Follow-up: Heap: manual, slow, limited size.
   - Follow-up: **Embedded best practice:** allocate all at startup, never allocate at runtime.

9. How to avoid dynamic allocation?
   - Follow-up: Pre-allocate fixed-size buffers:
   ```cpp
   class DataBuffer {
   private:
       static const int MAX_SIZE = 256;
       uint8_t data[MAX_SIZE];
       int length;
   
   public:
       DataBuffer() : length(0) {}
       
       void add(uint8_t byte) {
           if (length < MAX_SIZE) {
               data[length++] = byte;
           }
       }
   };
   ```
   - Follow-up: Use placement `new` (no dynamic allocation):
   ```cpp
   class Buffer {
   private:
       uint8_t buffer[256];
   
   public:
       void *operator new(size_t size, void *ptr) {
           return ptr;  // use pre-allocated memory
       }
   };
   
   // Usage
   Buffer *b = new(&preallocated_memory) Buffer();
   ```

10. What is `static` in C++?
    - Follow-up: Global scope: visible to all translation units (extern) or one file (internal linkage).
    - Follow-up: Class scope: shared by all instances, not instance-specific.
    - Follow-up: Function scope: persists across calls.
    - Follow-up: Embedded use: global hardware objects (UART1, GPIO, Timer).
    ```cpp
    class UART {
    public:
        static UART uart1;
        static UART uart2;
    };
    
    UART UART::uart1(USART1_BASE, 115200);
    UART UART::uart2(USART2_BASE, 9600);
    ```

---

### 3. Const Correctness

11. What is `const` in C++?
    - Follow-up: Variable: value cannot be changed.
    - Follow-up: Pointer: can declare const pointer, pointer to const, or both.
    - Follow-up: Method: const method cannot modify object state.

12. Const variations?
    ```cpp
    int x = 10;
    
    const int *p1 = &x;      // pointer to const int (can change p1, not *p1)
    int *const p2 = &x;      // const pointer to int (can change *p2, not p2)
    const int *const p3 = &x; // const pointer to const int (cannot change either)
    
    const int &r = x;        // const reference (cannot change x through r)
    ```

13. Why `const` in embedded?
    - Follow-up: Prevents accidental modification.
    - Follow-up: Compiler can place in flash (read-only): `const int table[] = {...};`
    - Follow-up: Optimization hint: compiler knows value won't change.
    - Follow-up: Best practice: mark everything `const` by default, remove only if needed.

14. `volatile` vs `const`?
    - Follow-up: `volatile`: value can change asynchronously (hardware register, ISR).
    - Follow-up: `const volatile`: cannot modify, but value changes externally (read-only register).
    - Follow-up: Not same as `const` — volatile disables compiler optimizations.
    ```cpp
    volatile uint32_t *status_reg = (uint32_t *)0x40000000;
    uint32_t status = *status_reg;  // compiler must read (not cached)
    
    const volatile uint32_t *counter = (uint32_t *)0x40000010;
    uint32_t c1 = *counter;
    uint32_t c2 = *counter;  // compiler must read again (not same as c1)
    ```

---

### 4. RAII (Resource Acquisition Is Initialization)

15. What is RAII?
    - Follow-up: Fundamental C++ idiom: tie resource lifecycle to object lifetime.
    - Follow-up: Acquire in constructor, release in destructor.
    - Follow-up: Guarantees cleanup even if exception thrown (in throw-enabled systems).
    - Follow-up: Most powerful feature of C++ for safety.

16. RAII example in embedded?
    ```cpp
    class CriticalSection {
    private:
        uint32_t old_prim;
    
    public:
        CriticalSection() {
            old_prim = __get_BASEPRI();  // save interrupt priority
            __set_BASEPRI(240);           // disable interrupts
        }
        
        ~CriticalSection() {
            __set_BASEPRI(old_prim);      // restore
        }
    };
    
    // Usage
    {
        CriticalSection cs;  // interrupts disabled
        // critical code here
    }  // cs destroyed, interrupts restored (even if exception thrown)
    ```

17. RAII for locks and mutexes?
    ```cpp
    class Mutex {
    public:
        void lock() { /* acquire lock */ }
        void unlock() { /* release lock */ }
    };
    
    class ScopedLock {
    private:
        Mutex &m;
    
    public:
        ScopedLock(Mutex &mutex) : m(mutex) {
            m.lock();
        }
        
        ~ScopedLock() {
            m.unlock();
        }
    };
    
    // Usage
    Mutex data_lock;
    
    void access_shared_data() {
        ScopedLock lock(data_lock);  // lock acquired
        // access data
    }  // lock automatically released
    ```

18. RAII for file/device handles?
    ```cpp
    class FileHandle {
    private:
        int fd;
    
    public:
        FileHandle(const char *name) {
            fd = open(name, O_RDWR);
        }
        
        ~FileHandle() {
            if (fd >= 0) close(fd);
        }
        
        int read(void *buf, int len) {
            return ::read(fd, buf, len);
        }
    };
    
    // Usage
    {
        FileHandle f("/dev/ttyS0");
        f.read(buffer, 256);
    }  // file automatically closed
    ```

---

## INTERMEDIATE C++

### 5. Inheritance & Polymorphism

19. What is inheritance?
    - Follow-up: Child class inherits members and methods from parent.
    - Follow-up: Access levels: `public` (normal inheritance), `private` (not common).
    - Follow-up: Constructor/destructor call order: parent first (constructor), child first (destructor).

20. Simple inheritance example?
    ```cpp
    class Peripheral {
    protected:
        uint32_t base_addr;
    
    public:
        Peripheral(uint32_t addr) : base_addr(addr) {}
        
        virtual void init() = 0;  // pure virtual
        virtual ~Peripheral() {}
    };
    
    class UART : public Peripheral {
    public:
        UART(uint32_t addr) : Peripheral(addr) {}
        
        void init() override {
            // Initialize UART-specific
        }
    };
    
    class Timer : public Peripheral {
    public:
        Timer(uint32_t addr) : Peripheral(addr) {}
        
        void init() override {
            // Initialize timer-specific
        }
    };
    ```

21. What is virtual function?
    - Follow-up: Function that can be overridden in child class.
    - Follow-up: Allows polymorphism — call child's implementation through parent pointer.
    - Follow-up: Cost: virtual table (vtable) pointer per object, indirect function call.

22. Virtual function cost in embedded?
    - Follow-up: Extra pointer per object (8 bytes on 64-bit).
    - Follow-up: Indirect function call (1–2 extra CPU cycles typically).
    - Follow-up: VTable pointer initialization in constructor.
    - Follow-up: For real-time: acceptable if calls not in hot loop.

23. Virtual vs non-virtual dispatch?
    ```cpp
    // Virtual: runtime polymorphism (slow, flexible)
    Peripheral *p = get_peripheral();
    p->init();  // calls correct child implementation
    
    // Non-virtual: compile-time (fast, rigid)
    UART uart(0x40000000);
    uart.init();  // compiler knows it's UART::init()
    ```

24. When to use virtual in embedded?
    - Follow-up: **Use sparingly** — RTOS abstraction layers, device driver architecture.
    - Follow-up: Avoid in hot loops (ISR, tight control loops).
    - Follow-up: Profile before using — measure cost.

25. What is pure virtual function?
    - Follow-up: No implementation in base class, derived must implement.
    - Follow-up: Base class is abstract (cannot instantiate).
    ```cpp
    class Sensor {
    public:
        virtual uint16_t read() = 0;  // pure virtual
        virtual ~Sensor() {}
    };
    
    // Sensor s;  // ERROR: abstract class
    
    class TemperatureSensor : public Sensor {
    public:
        uint16_t read() override {
            return adc_read();
        }
    };
    ```

---

### 6. Templates

26. What are templates in C++?
    - Follow-up: Generic programming — write code for any type.
    - Follow-up: Compile-time specialization — compiler generates code for each type used.
    - Follow-up: Zero-cost abstraction — no runtime overhead.
    - Follow-up: Powerful in embedded — reusable device driver code.

27. Simple template example?
    ```cpp
    template <typename T>
    class Queue {
    private:
        static const int MAX = 256;
        T buffer[MAX];
        int head, tail;
    
    public:
        Queue() : head(0), tail(0) {}
        
        void push(const T &item) {
            buffer[head] = item;
            head = (head + 1) % MAX;
        }
        
        T pop() {
            T item = buffer[tail];
            tail = (tail + 1) % MAX;
            return item;
        }
    };
    
    // Usage
    Queue<uint8_t> byte_queue;
    Queue<int16_t> sensor_queue;
    
    byte_queue.push(0xFF);
    int16_t val = sensor_queue.pop();
    ```

28. Template specialization?
    - Follow-up: Provide specialized implementation for specific type.
    ```cpp
    template <typename T>
    void print_value(T val) {
        // generic implementation
    }
    
    template <>
    void print_value<float>(float val) {
        // specialized for float
        printf("%.2f", val);
    }
    ```

29. Template meta-programming in embedded?
    - Follow-up: Compile-time computation, generates efficient code.
    ```cpp
    // Compile-time power of 2
    template <int N>
    struct Pow2 {
        static const int value = 2 * Pow2<N-1>::value;
    };
    
    template <>
    struct Pow2<0> {
        static const int value = 1;
    };
    
    int x = Pow2<10>::value;  // computed at compile-time: 1024
    ```

30. Variadic templates?
    - Follow-up: Templates with variable number of arguments.
    ```cpp
    template <typename T>
    void log(T val) {
        printf("%d ", val);
    }
    
    template <typename T, typename... Args>
    void log(T val, Args... rest) {
        printf("%d ", val);
        log(rest...);  // recursive call
    }
    
    log(1, 2, 3, 4, 5);  // prints: 1 2 3 4 5
    ```

---

### 7. Smart Pointers (Limited Use in Embedded)

31. What are smart pointers?
    - Follow-up: RAII wrappers around raw pointers.
    - Follow-up: Automatic deletion when last reference goes away.
    - Follow-up: `unique_ptr`: sole owner, move semantics.
    - Follow-up: `shared_ptr`: reference counted, multiple owners.
    - Follow-up: **Embedded warning:** reference counting adds overhead, avoid in real-time.

32. `unique_ptr` example?
    ```cpp
    class DataBuffer {
    private:
        std::unique_ptr<uint8_t[]> data;
    
    public:
        DataBuffer(int size) : data(new uint8_t[size]) {}
        
        // Destructor: data automatically deleted
        // Cannot copy (unique)
        
        DataBuffer(DataBuffer &&other) = default;  // move allowed
    };
    
    // Usage
    {
        std::unique_ptr<DataBuffer> buf(new DataBuffer(256));
    }  // buf destroyed, data freed automatically
    ```

33. When to use smart pointers in embedded?
    - Follow-up: **Rarely** — most embedded systems pre-allocate.
    - Follow-up: `unique_ptr` acceptable if allocating once at startup.
    - Follow-up: **Avoid `shared_ptr`** in real-time (reference counting overhead).
    - Follow-up: Raw pointers to pre-allocated objects is clearer in embedded.

---

### 8. Operators Overloading

34. What is operator overloading?
    - Follow-up: Define custom behavior for operators (+, -, [], (), etc.).
    - Follow-up: Makes classes act like built-in types.
    - Follow-up: **Embedded warning:** can hide complexity, use sparingly.

35. Simple operator overloading example?
    ```cpp
    class Angle {
    private:
        int degrees;
    
    public:
        Angle(int d) : degrees(d) {}
        
        Angle operator+(const Angle &other) const {
            return Angle((degrees + other.degrees) % 360);
        }
        
        Angle &operator+=(const Angle &other) {
            degrees = (degrees + other.degrees) % 360;
            return *this;
        }
        
        bool operator==(const Angle &other) const {
            return degrees == other.degrees;
        }
    };
    
    Angle a(10), b(20);
    Angle c = a + b;  // uses overloaded operator+
    ```

36. Operator overloading to avoid?
    - Follow-up: **Avoid** user-defined conversion operators (confusion).
    - Follow-up: **Avoid** overloading `new`/`delete` (unless placement new).
    - Follow-up: **Keep semantics** close to built-in types (a + b should intuitively combine a and b).

---

## ADVANCED C++

### 9. Modern C++ Features (C++11 and beyond)

37. What is auto keyword?
    - Follow-up: Type deduction from initializer.
    - Follow-up: Reduces boilerplate, improves readability.
    ```cpp
    std::vector<int> v = {1, 2, 3};
    auto iter = v.begin();  // deduce: std::vector<int>::iterator
    auto val = v[0];        // deduce: int
    ```

38. What is range-based for loop?
    - Follow-up: Simplified iteration over containers.
    ```cpp
    std::vector<int> v = {1, 2, 3};
    
    for (int x : v) {
        printf("%d\n", x);
    }
    
    // Also works with arrays
    int arr[] = {1, 2, 3};
    for (int x : arr) {
        printf("%d\n", x);
    }
    ```

39. What is move semantics and rvalue references?
    - Follow-up: `&&` = rvalue reference (temporary, can be moved).
    - Follow-up: `std::move()` = cast to rvalue.
    - Follow-up: Move assignment/constructor: steal resources from temporary.
    - Follow-up: Avoids expensive copies.
    ```cpp
    class Buffer {
    private:
        uint8_t *data;
        int size;
    
    public:
        Buffer(int s) : size(s), data(new uint8_t[s]) {}
        
        // Move constructor
        Buffer(Buffer &&other) : size(other.size), data(other.data) {
            other.data = nullptr;
            other.size = 0;
        }
        
        ~Buffer() {
            delete[] data;
        }
    };
    
    // Usage
    Buffer b1(256);
    Buffer b2 = std::move(b1);  // move, don't copy
    ```

40. What is `nullptr`?
    - Follow-up: Type-safe null pointer (vs C's `NULL`).
    - Follow-up: Prevents ambiguous overload resolution.
    ```cpp
    void func(int *p);
    void func(int x);
    
    func(NULL);      // ambiguous
    func(nullptr);   // clear: pointer version
    ```

41. What is initializer list?
    - Follow-up: `{...}` syntax for initialization.
    - Follow-up: Works with constructors, assignment.
    ```cpp
    class GPIO {
    private:
        std::vector<int> pins;
    
    public:
        GPIO(std::initializer_list<int> p) : pins(p) {}
    };
    
    GPIO led = {13, 14, 15};  // initializer list
    ```

42. What is `constexpr`?
    - Follow-up: Compile-time constant or function.
    - Follow-up: Value computed at compile-time if inputs are const.
    - Follow-up: Powerful for embedded: precompute lookup tables.
    ```cpp
    constexpr int square(int x) {
        return x * x;
    }
    
    constexpr int lookup[10] = {
        square(0), square(1), square(2), ...  // computed at compile-time
    };
    ```

43. What is `static_assert`?
    - Follow-up: Compile-time assertion.
    - Follow-up: Check constraints before compilation.
    ```cpp
    template <typename T>
    class Buffer {
        static_assert(sizeof(T) <= 256, "Type too large for buffer");
        T data[100];
    };
    ```

---

### 10. STL Containers (Limited Use in Embedded)

44. What are STL containers?
    - Follow-up: `vector`, `list`, `queue`, `map`, `set`, etc.
    - Follow-up: Dynamic allocation, variable size.
    - Follow-up: **Embedded rule:** avoid in real-time, pre-allocate fixed-size instead.

45. Which STL containers to avoid in embedded?
    - `vector`: dynamic allocation, reallocation on growth.
    - `map`, `set`: tree-based, non-deterministic insertion time.
    - `list`: pointer overhead.

46. Fixed-size alternative to vector?
    ```cpp
    template <typename T, int N>
    class FixedVector {
    private:
        T data[N];
        int count;
    
    public:
        FixedVector() : count(0) {}
        
        void push_back(const T &item) {
            if (count < N) {
                data[count++] = item;
            }
        }
        
        const T &operator[](int i) const {
            return data[i];
        }
        
        int size() const { return count; }
    };
    
    // Usage
    FixedVector<int, 100> v;
    v.push_back(42);
    ```

47. What about `std::array` (C++11)?
    - Follow-up: Fixed-size array wrapper, zero overhead.
    - Follow-up: Type-safe, bounds checking (at() method).
    ```cpp
    std::array<int, 100> arr;  // fixed size
    arr[0] = 42;
    int val = arr.at(0);  // bounds checked
    ```

---

### 11. Exceptions (Generally Avoided in Embedded)

48. What are exceptions in C++?
    - Follow-up: Error handling mechanism: throw, catch, try-catch.
    - Follow-up: **Embedded embedded:** generally **disabled** (CONFIG_EXCEPTIONS=0).
    - Follow-up: Reasons: code size, memory overhead, unpredictable latency.

49. Why exceptions are bad in embedded?
    - Follow-up: Exception throwing and unwinding adds latency.
    - Follow-up: Hidden control flow (hard to trace).
    - Follow-up: Stack unwinding can cause timing violations.
    - Follow-up: Exception handling tables add code size.

50. Exception alternatives in embedded?
    - Follow-up: Error codes (return int, out-parameter for result).
    - Follow-up: Asserts for programming errors.
    ```cpp
    class UART {
    public:
        bool read(uint8_t &data) {  // returns success/failure
            if (!is_data_available()) return false;
            data = receive_data();
            return true;
        }
    };
    
    uint8_t byte;
    if (!uart.read(byte)) {
        // handle error
    }
    ```

51. How to compile without exceptions?
    ```bash
    g++ -fno-exceptions -fno-rtti program.cpp -o program
    ```

---

## EXPERT LEVEL

### 12. Embedded-Specific Patterns

52. What is PIMPL (Pointer to Implementation)?
    - Follow-up: Hide implementation details, stable ABI.
    ```cpp
    // header file
    class Sensor {
    private:
        class Impl;
        std::unique_ptr<Impl> pimpl;
    
    public:
        Sensor();
        ~Sensor();
        uint16_t read();
    };
    
    // implementation file
    class Sensor::Impl {
    public:
        uint16_t read() {
            // actual implementation
        }
    };
    
    Sensor::Sensor() : pimpl(std::make_unique<Impl>()) {}
    uint16_t Sensor::read() { return pimpl->read(); }
    ```
    - Follow-up: Costs: extra pointer dereference, heap allocation.
    - Follow-up: Use only if ABI stability critical.

53. What is CRTP (Curiously Recurring Template Pattern)?
    - Follow-up: Static polymorphism (compile-time, zero runtime cost).
    - Follow-up: Alternative to virtual functions.
    ```cpp
    template <typename Derived>
    class Peripheral {
    public:
        void init() {
            static_cast<Derived *>(this)->init_impl();
        }
    };
    
    class UART : public Peripheral<UART> {
    public:
        void init_impl() {
            // UART-specific init
        }
    };
    
    class Timer : public Peripheral<Timer> {
    public:
        void init_impl() {
            // Timer-specific init
        }
    };
    
    // Usage: no virtual functions, compile-time dispatch
    UART uart;
    uart.init();  // calls UART::init_impl() directly
    ```

54. What is register wrapper pattern?
    - Follow-up: Type-safe access to hardware registers.
    ```cpp
    template <typename T, uint32_t Addr>
    class Register {
    public:
        void write(T value) {
            *reinterpret_cast<volatile T *>(Addr) = value;
        }
        
        T read() const {
            return *reinterpret_cast<volatile const T *>(Addr);
        }
        
        Register &operator=(T value) {
            write(value);
            return *this;
        }
        
        operator T() const {
            return read();
        }
    };
    
    // Usage
    Register<uint32_t, 0x40000000> control_reg;
    Register<uint32_t, 0x40000004> status_reg;
    
    control_reg = 0x123;
    uint32_t status = status_reg;  // clean syntax
    ```

55. What is hardware abstraction layer (HAL) in C++?
    - Follow-up: Abstract interface for hardware (GPIO, UART, Timer, SPI, etc.).
    - Follow-up: Single HAL header, multiple implementations per platform.
    ```cpp
    // hal.h
    class GPIO {
    public:
        virtual void set_output() = 0;
        virtual void write(bool) = 0;
        virtual bool read() const = 0;
        virtual ~GPIO() {}
    };
    
    // stm32_hal.cpp
    class STM32_GPIO : public GPIO {
    private:
        uint32_t port, pin;
    
    public:
        STM32_GPIO(uint32_t p, uint32_t n) : port(p), pin(n) {}
        
        void set_output() override { /* STM32 specific */ }
        void write(bool val) override { /* STM32 specific */ }
        bool read() const override { /* STM32 specific */ }
    };
    
    // esp32_hal.cpp
    class ESP32_GPIO : public GPIO {
        // ESP32 specific implementation
    };
    ```

56. How to handle GPIO with type-safe ports/pins?
    ```cpp
    template <int Port, int Pin>
    class GPIO_Pin {
    private:
        static const uint32_t port_base = PORT_BASE[Port];
        static const uint32_t pin_mask = (1 << Pin);
    
    public:
        void set_output() {
            configure_output(port_base, pin_mask);
        }
        
        void write(bool value) {
            if (value) {
                set_high(port_base, pin_mask);
            } else {
                set_low(port_base, pin_mask);
            }
        }
        
        bool read() const {
            return is_high(port_base, pin_mask);
        }
    };
    
    // Usage: no runtime cost for port/pin selection
    GPIO_Pin<GPIOA, 5> led;
    led.set_output();
    led.write(true);
    ```

57. What is state machine implementation in C++?
    ```cpp
    template <typename State>
    class StateMachine {
    private:
        State *current_state;
    
    public:
        StateMachine(State *initial) : current_state(initial) {}
        
        void on_event(const Event &evt) {
            State *next = current_state->handle_event(evt);
            if (next) {
                current_state->exit();
                current_state = next;
                current_state->enter();
            }
        }
    };
    
    class IdleState {
    public:
        State *handle_event(const Event &evt) {
            if (evt.type == START) return new RunningState();
            return nullptr;
        }
    };
    
    class RunningState {
    public:
        State *handle_event(const Event &evt) {
            if (evt.type == STOP) return new IdleState();
            return nullptr;
        }
    };
    ```

---

### 13. Type Safety & Design

58. What is enum class (scoped enum)?
    - Follow-up: Type-safe enumeration (vs C-style enum).
    - Follow-up: Prevents accidental integer conversion.
    ```cpp
    // Old C style
    enum State { IDLE, RUNNING, STOPPED };
    int x = IDLE;  // implicitly converts to int
    
    // C++11 scoped enum
    enum class State { IDLE, RUNNING, STOPPED };
    // State s = 0;  // ERROR: not implicitly convertible
    State s = State::IDLE;  // explicit
    ```

59. What is type traits?
    - Follow-up: Compile-time type information.
    - Follow-up: Enable/disable code based on type properties.
    ```cpp
    template <typename T>
    struct is_signed { static const bool value = false; };
    
    template <>
    struct is_signed<int> { static const bool value = true; };
    
    template <>
    struct is_signed<signed char> { static const bool value = true; };
    
    // Usage
    if (is_signed<int>::value) {
        // handle signed int
    }
    ```

60. What is SFINAE (Substitution Failure Is Not An Error)?
    - Follow-up: Powerful metaprogramming technique.
    - Follow-up: Enable template only for certain types.
    ```cpp
    template <typename T>
    typename std::enable_if<std::is_integral<T>::value>::type
    print_value(T val) {
        printf("%d\n", val);  // only for integral types
    }
    
    template <typename T>
    typename std::enable_if<std::is_floating_point<T>::value>::type
    print_value(T val) {
        printf("%.2f\n", val);  // only for floating-point types
    }
    ```

61. What is concepts (C++20)?
    - Follow-up: More readable alternative to SFINAE.
    - Follow-up: Defines requirements on template parameters.
    ```cpp
    template <typename T>
    concept Numeric = std::is_integral_v<T> || std::is_floating_point_v<T>;
    
    template <Numeric T>
    T add(T a, T b) {
        return a + b;
    }
    ```

---

### 14. Performance Optimization

62. How to write performant C++ for embedded?
    - Follow-up: Inline small functions (use `inline` or let compiler decide).
    - Follow-up: Avoid virtual functions in hot loops.
    - Follow-up: Pre-allocate memory.
    - Follow-up: Use const and constexpr.
    - Follow-up: Profile, profile, profile.

63. What is inlining?
    - Follow-up: Function code substituted directly at call site.
    - Follow-up: No function call overhead, but larger code.
    - Follow-up: `inline` is hint to compiler (compiler decides).
    ```cpp
    inline int max(int a, int b) {
        return (a > b) ? a : b;
    }
    
    // Compiler might inline:
    int m = max(5, 3);  // becomes: int m = (5 > 3) ? 5 : 3;
    ```

64. Compiler optimization flags?
    ```bash
    -O0  # no optimization (debug)
    -O1  # basic optimization
    -O2  # aggressive optimization (recommended)
    -O3  # maximum optimization (may increase code size)
    -Os  # optimize for size
    
    # Other useful flags
    -flto              # Link-Time Optimization (recompile and optimize across files)
    -march=cortex-m4   # optimize for specific CPU
    -ffunction-sections # separate section per function (for dead code removal)
    -Wl,--gc-sections  # linker garbage collection
    ```

65. How to measure performance?
    - Follow-up: Cycle counter (DWT on ARM): count CPU cycles.
    - Follow-up: GPIO toggle + oscilloscope: measure timing externally.
    - Follow-up: Profiler: identify hot spots.
    ```cpp
    uint32_t start = DWT_CYCCNT;
    some_function();
    uint32_t cycles = DWT_CYCCNT - start;
    printf("Took %u cycles\n", cycles);
    ```

---

### 15. Testing in Embedded C++

66. How to test embedded C++ code?
    - Follow-up: Unit testing on PC (Google Test, Catch2).
    - Follow-up: Mock hardware interfaces.
    - Follow-up: Integration testing on target.
    ```cpp
    #include <gtest/gtest.h>
    
    class MockGPIO {
    public:
        MOCK_METHOD(void, write, (bool));
        MOCK_METHOD(bool, read, ());
    };
    
    class LED {
    private:
        GPIO &gpio;
    
    public:
        LED(GPIO &g) : gpio(g) {}
        
        void on() { gpio.write(true); }
        void off() { gpio.write(false); }
    };
    
    TEST(LEDTest, OnWritesHigh) {
        MockGPIO mock;
        EXPECT_CALL(mock, write(true));
        
        LED led(mock);
        led.on();
    }
    ```

67. How to handle hardware in tests?
    - Follow-up: Dependency injection: pass hardware object to class.
    - Follow-up: Create mock objects for testing.
    - Follow-up: Real hardware testing on target.

---

## EXPERT WAR STORIES / DEEP QUESTIONS

1. "You said virtual functions cost extra pointer dereference — but modern CPUs have branch prediction, so is it really slower?"
   - **Answer:** Yes, still slower. Branch prediction helps, but cache miss and indirect prediction less accurate than direct call. Measure: typically 10–20% slower.

2. "You said use `const` everywhere — but sometimes compiler doesn't optimize out the const check, why?"
   - **Answer:** Compiler sees `const` as API contract, not optimization promise. Use `constexpr` if you need compile-time value.

3. "You said avoid `new`/`delete` — but what if I use pool allocator (pre-allocate all at startup)?"
   - **Answer:** OK if pool never exhausted. But still adds complexity. Fixed-size arrays simpler for embedded.

4. "You said templates generate code for each type — doesn't that bloat the binary?"
   - **Answer:** Yes, template instantiation = code bloat. Use sparingly in embedded. Profile binary size.

5. "You said move semantics avoid copies — but my object is 8 bytes, moving costs same as copying, right?"
   - **Answer:** Correct. Move beneficial for large objects (strings, vectors). For small objects, compiler optimizes away anyway (RVO).

6. "You said RAII guarantees cleanup — but what if destructor throws exception?"
   - **Answer:** Destructors should never throw. If they do, program terminates. Mark with `noexcept` to prevent.
   ```cpp
   class Resource {
   public:
       ~Resource() noexcept {
           // cleanup code, must not throw
       }
   };
   ```

7. "You said fixed-size vector template — but I'm not sure maximum size, what do I do?"
   - **Answer:** Design wrong. Embedded systems must have bounded requirements. If unbounded, redesign algorithm or use PC.

8. "You said static variables in embedded — but mine is initialized by hardware ISR, how do I order initialization?"
   - **Answer:** Static initialization happens before main(). If ISR needs it, you have race condition. Solution: lazy initialization check in ISR.
   ```cpp
   static bool initialized = false;
   
   void UART_ISR(void) {
       if (!initialized) {
           uart_state.init();
           initialized = true;
       }
       // ... rest of ISR
   }
   ```

9. "You said avoid STL containers in embedded — but I use `std::array` and it's zero overhead?"
   - **Answer:** `std::array` is fine (fixed-size, no dynamic allocation). STL containers to avoid: `vector`, `list`, `map` (dynamic allocation, unpredictable latency).

10. "You said use CRTP for static polymorphism — but my code got 10x more complex, was it worth it?"
    - **Answer:** CRTP is advanced technique. Use only if virtual function overhead proven to be problem. Simpler design first.

11. "You said profile binary size — but my embedded system has 1MB flash, what's the limit for C++ binary?"
    - **Answer:** Depends on requirements. C++ adds ~50KB–200KB overhead (standard library, RTTI disabled, no exceptions). Test on your system.

12. "You said constexpr functions are evaluated at compile-time — but my compiler still generated runtime code, why?"
    - **Answer:** `constexpr` doesn't guarantee compile-time eval. If called with runtime values, compiler generates runtime code. Use `if constexpr` in C++17 for guaranteed compile-time.
    ```cpp
    template <int N>
    void foo() {
        if constexpr (N > 5) {
            // code only instantiated if N > 5 at compile-time
        }
    }
    ```

---

## C++ EMBEDDED BEST PRACTICES SUMMARY

### DO:
- ✅ Use classes for hardware abstraction (GPIO, UART, Timer, SPI).
- ✅ Use `const` extensively.
- ✅ Use templates for reusable code (queues, buffers).
- ✅ Use RAII for resource management (locks, critical sections).
- ✅ Use `constexpr` for compile-time computation.
- ✅ Use `static_assert` for compile-time checks.
- ✅ Use fixed-size containers (`std::array`, custom templates).
- ✅ Use CRTP or register wrappers for type-safe hardware access.

### DON'T:
- ❌ Use exceptions (disable with `-fno-exceptions`).
- ❌ Use RTTI (`-fno-rtti`).
- ❌ Use dynamic allocation at runtime.
- ❌ Use STL containers with dynamic allocation.
- ❌ Overuse virtual functions (avoid in hot loops).
- ❌ Overuse templates (code bloat).
- ❌ Use `new`/`delete` unless very specialized (placement new OK).
- ❌ Ignore const-correctness.

### MEASURE:
- 📊 Binary size (every C++ feature has cost).
- 📊 Execution time (profile hot loops).
- 📊 Stack usage (RAII objects on stack, watch growth).
- 📊 Latency (jitter, context switches).

---

## EMBEDDED C++ COMPILATION

### Typical embedded C++ build:
```bash
# Compiler flags
CXXFLAGS = -std=c++17 \
           -mcpu=cortex-m4 \
           -mthumb \
           -O2 \
           -Wall -Werror \
           -fno-exceptions \
           -fno-rtti \
           -ffunction-sections \
           -fdata-sections \
           -Wl,--gc-sections

# Compile
arm-none-eabi-g++ $(CXXFLAGS) -c main.cpp -o main.o

# Link
arm-none-eabi-g++ $(CXXFLAGS) main.o -Wl,-Map=output.map -o firmware.elf

# Binary
arm-none-eabi-objcopy -O binary firmware.elf firmware.bin

# Size report
arm-none-eabi-size firmware.elf
```

---

## COMPARISON: C vs C++ IN EMBEDDED

| Feature | C | C++ |
|---------|---|-----|
| Learning curve | Easy | Steep |
| Binary size | Small | Medium (with care) |
| Latency | Predictable | Predictable (no exceptions/RTTI) |
| Code reuse | Limited | Excellent (templates, inheritance) |
| Type safety | Weak | Strong |
| Debugging | Easy | Harder (inlining, templates) |
| Community | Dominant | Growing |
| Real-time | Excellent | Good (with restrictions) |
| Hard real-time | Best choice | Acceptable |

**Recommendation:**
- **C:** Hard real-time (<10µs latency), resource-constrained (<100KB flash), simple applications.
- **C++:** Medium real-time (>100µs latency), modern systems (>1MB flash), complex applications, need code reuse.

---

*This document covers modern C++ in embedded systems from basics to expert level, suitable for M.Tech graduate with 10+ years experience transitioning from C to C++.*