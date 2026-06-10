# C++ Errors, Debugging, and Problem-Solving Interview Questions
> Compilation Errors, Runtime Errors, Logic Errors, Memory Issues, Debugging Techniques  
> M.Tech Graduate + 10 Years Experience  
> Basic → Intermediate → Advanced → Expert → War Stories

---

## PART 1: COMPILATION ERRORS

### Basic Compilation Errors

1. What is a compilation error?
   - Follow-up: Error detected by compiler during compilation phase.
   - Follow-up: Code does not compile, no executable generated.
   - Follow-up: Must be fixed before running program.
   - Follow-up: vs runtime error: happens during execution.

2. What is "undefined reference" error?
   - Follow-up: Linker cannot find symbol definition.
   - Follow-up: Function/variable declared but not defined.
   - Follow-up: Example:
   ```cpp
   // header.h
   void my_function();
   
   // main.cpp
   int main() {
       my_function();  // declared but never defined
       return 0;
   }
   
   // Error: undefined reference to `my_function()'
   ```
   - Follow-up: Fix: define function or link with library containing it.
   ```cpp
   // utils.cpp
   void my_function() {
       // implementation
   }
   
   // Compile both: g++ main.cpp utils.cpp -o program
   ```

3. What causes "multiple definition" error?
   - Follow-up: Symbol defined in multiple translation units.
   - Follow-up: Example:
   ```cpp
   // header.h
   void my_function() {  // WARNING: defined in header
       printf("hello\n");
   }
   
   // file1.cpp
   #include "header.h"
   
   // file2.cpp
   #include "header.h"  // ERROR: my_function defined twice
   ```
   - Follow-up: Fix: use `inline` or put in separate .cpp file.
   ```cpp
   // header.h
   inline void my_function() {  // OK: inline allows multiple definitions
       printf("hello\n");
   }
   ```

4. What is "redeclaration" error?
   - Follow-up: Variable/function declared twice in same scope.
   - Follow-up: Example:
   ```cpp
   int x = 5;
   int x = 10;  // ERROR: redeclaration
   
   void foo();
   void foo();  // OK: declaration is allowed multiple times
   void foo();
   
   void foo() {}  // OK: definition once
   ```

5. What is "incomplete type" error?
   - Follow-up: Using type before it's fully defined.
   - Follow-up: Example:
   ```cpp
   struct Node;  // forward declaration (incomplete)
   
   void process(Node *n) {  // OK: pointer to incomplete type
       // n->value = 5;  // ERROR: incomplete type
   }
   
   struct Node {
       int value;
   };
   ```

6. What is "type mismatch" or "invalid conversion" error?
   - Follow-up: Assigning incompatible types.
   - Follow-up: Example:
   ```cpp
   int x = 3.14;       // implicit conversion float->int, might warn
   int *p = new double[10];  // ERROR: type mismatch
   
   void foo(int x);
   foo(3.14);          // implicit conversion, compiles but might warn
   foo("hello");       // ERROR: const char* -> int
   ```

7. What is "no matching function" error?
   - Follow-up: Function call doesn't match any overload.
   - Follow-up: Example:
   ```cpp
   void print(int x);
   void print(const char *s);
   
   print(3.14);  // ERROR: no matching overload for double
   
   // Fix: add overload
   void print(double x);
   ```

8. What is "private member access" error?
   - Follow-up: Accessing private member from outside class.
   - Follow-up: Example:
   ```cpp
   class MyClass {
   private:
       int value;
   };
   
   int main() {
       MyClass obj;
       obj.value = 5;  // ERROR: value is private
   }
   ```

9. What is "virtual function override" issue?
   - Follow-up: Override function doesn't match base signature.
   - Follow-up: Example:
   ```cpp
   class Base {
   public:
       virtual void foo(int x);
   };
   
   class Derived : public Base {
   public:
       void foo(double x);  // ERROR: doesn't override (different signature)
   };
   
   // Fix: use override keyword to catch errors
   class Derived : public Base {
   public:
       void foo(int x) override;  // OK
   };
   ```

10. What is "missing return statement" warning?
    - Follow-up: Function should return value but doesn't.
    - Follow-up: Example:
    ```cpp
    int get_value() {
        if (condition) {
            return 42;
        }
        // ERROR: missing return for else case
    }
    
    // Fix:
    int get_value() {
        if (condition) {
            return 42;
        }
        return 0;  // default return
    }
    ```

---

### Intermediate Compilation Errors

11. What is "forward declaration" problem?
    - Follow-up: Using class/function before declaring it.
    - Follow-up: Example:
    ```cpp
    int main() {
        foo();  // ERROR: foo not declared
        return 0;
    }
    
    void foo() {
        printf("hello\n");
    }
    
    // Fix: forward declare
    void foo();  // declaration
    
    int main() {
        foo();  // OK
        return 0;
    }
    
    void foo() {  // definition
        printf("hello\n");
    }
    ```

12. What is "header guard" and why it matters?
    - Follow-up: Prevents multiple inclusion of same header.
    - Follow-up: Example of problem:
    ```cpp
    // myheader.h (no guards)
    int global_var;
    
    // file1.cpp
    #include "myheader.h"
    
    // file2.cpp
    #include "myheader.h"  // ERROR: global_var defined twice
    
    // Fix: add header guards
    #ifndef MYHEADER_H
    #define MYHEADER_H
    
    int global_var;
    
    #endif
    
    // Or use pragma (non-standard but widely supported)
    #pragma once
    
    int global_var;
    ```

13. What is "circular include" problem?
    - Follow-up: header A includes B, B includes A.
    - Follow-up: Example:
    ```cpp
    // a.h
    #include "b.h"
    
    class A {
        B *b;
    };
    
    // b.h
    #include "a.h"  // ERROR: circular dependency
    
    class B {
        A *a;
    };
    
    // Fix: forward declare instead of include
    // a.h
    class B;  // forward declaration
    
    class A {
        B *b;  // OK: pointer to incomplete type
    };
    
    // b.h
    class A;  // forward declaration
    
    class B {
        A *a;  // OK
    };
    ```

14. What is "name collision" or "ambiguous symbol" error?
    - Follow-up: Same symbol defined in multiple namespaces/scopes.
    - Follow-up: Example:
    ```cpp
    namespace NS1 {
        void print(int x);
    }
    
    namespace NS2 {
        void print(int x);
    }
    
    using namespace NS1;
    using namespace NS2;
    
    print(5);  // ERROR: ambiguous (NS1::print or NS2::print?)
    
    // Fix: specify namespace
    NS1::print(5);  // OK
    ```

15. What is "template instantiation" error?
    - Follow-up: Template error only visible when instantiated.
    - Follow-up: Example:
    ```cpp
    template <typename T>
    T divide(T a, T b) {
        return a / b;  // OK for numeric types
    }
    
    int main() {
        divide<std::string>("hello", "world");  // ERROR: string doesn't have operator/
    }
    ```

16. What is "const mismatch" error?
    - Follow-up: Trying to pass non-const where const expected.
    - Follow-up: Example:
    ```cpp
    void print_const(const char *s);
    
    char *str = "hello";
    print_const(str);  // OK: non-const to const allowed
    
    const char *cstr = "world";
    void modify(char *s);  // expects non-const
    modify(cstr);  // ERROR: const to non-const not allowed
    ```

17. What is "static variable in header" problem?
    - Follow-up: Static variables in headers create multiple copies.
    - Follow-up: Example:
    ```cpp
    // header.h
    static int counter = 0;  // OK but creates separate variable in each .cpp
    
    // file1.cpp
    #include "header.h"
    counter++;  // increments file1's copy
    
    // file2.cpp
    #include "header.h"
    counter++;  // increments file2's copy (different from file1)
    
    // Fix: use extern
    // header.h
    extern int counter;  // declaration
    
    // main.cpp
    int counter = 0;  // definition
    ```

18. What is "missing include" error?
    - Follow-up: Using symbol without including header.
    - Follow-up: Example:
    ```cpp
    int main() {
        std::vector<int> v;  // ERROR: vector not declared
        return 0;
    }
    
    // Fix:
    #include <vector>
    
    int main() {
        std::vector<int> v;  // OK
        return 0;
    }
    ```

19. What is "ambiguous operator overload" error?
    - Follow-up: Multiple overloads could apply.
    - Follow-up: Example:
    ```cpp
    class X {};
    
    X operator+(int, X);
    X operator+(X, int);
    
    int main() {
        X x;
        x + 5;  // ERROR: ambiguous (could call either overload)
    }
    ```

20. What is "narrowing conversion" warning?
    - Follow-up: Implicit conversion that loses information.
    - Follow-up: Example:
    ```cpp
    double d = 3.14;
    int x = d;  // WARNING: narrowing conversion (loses fractional part)
    
    // C++11 aggregate initialization catches this
    int arr[] = {d};  // ERROR: narrowing conversion
    ```

---

### Advanced Compilation Errors

21. What is "ODR violation" (One Definition Rule)?
    - Follow-up: Defining something in multiple translation units incorrectly.
    - Follow-up: Example:
    ```cpp
    // utils.h
    int global = 5;  // defined in header
    
    // file1.cpp
    #include "utils.h"  // global defined here
    
    // file2.cpp
    #include "utils.h"  // ERROR: global defined again
    
    // Fix:
    // utils.h
    extern int global;  // declaration only
    
    // utils.cpp
    int global = 5;  // definition
    ```

22. What is "friend function" compilation issue?
    - Follow-up: Friend declaration might not declare function.
    - Follow-up: Example:
    ```cpp
    class MyClass {
    private:
        int value;
    
        friend void print(MyClass &obj);  // declares friend function
    };
    
    // Must still define it
    void print(MyClass &obj) {
        printf("%d\n", obj.value);
    }
    ```

23. What is "default member initializer" issue?
    - Follow-up: In-class member initialization only in C++11+.
    - Follow-up: Example:
    ```cpp
    // C++98 (ERROR)
    class MyClass {
        int x = 5;  // ERROR: not allowed
    };
    
    // C++11+ (OK)
    class MyClass {
        int x = 5;  // OK: default member initializer
    };
    ```

24. What is "explicit constructor" issue?
    - Follow-up: Prevents implicit conversions.
    - Follow-up: Example:
    ```cpp
    class MyClass {
    public:
        MyClass(int x);  // allows implicit conversion
    };
    
    MyClass obj = 5;  // OK: implicit conversion
    
    class MyClass2 {
    public:
        explicit MyClass2(int x);  // prevents implicit
    };
    
    MyClass2 obj2 = 5;  // ERROR: explicit prevents this
    MyClass2 obj3(5);   // OK: explicit call
    ```

25. What is "deleted function" (C++11)?
    - Follow-up: Explicitly forbid copy/move.
    - Follow-up: Example:
    ```cpp
    class NonCopyable {
    public:
        NonCopyable() = default;
        NonCopyable(const NonCopyable &) = delete;  // forbid copy
        NonCopyable &operator=(const NonCopyable &) = delete;
    };
    
    NonCopyable obj1;
    NonCopyable obj2 = obj1;  // ERROR: copy deleted
    ```

---

## PART 2: RUNTIME ERRORS

### Basic Runtime Errors

26. What is a "segmentation fault" (SIGSEGV)?
    - Follow-up: Accessing invalid memory address.
    - Follow-up: Program crashes immediately.
    - Follow-up: Example:
    ```cpp
    int *p = nullptr;
    *p = 5;  // SEGFAULT: dereference null pointer
    
    int *arr = new int[10];
    arr[100] = 5;  // SEGFAULT: buffer overflow
    delete[] arr;
    arr[5] = 10;  // SEGFAULT: use-after-free
    ```

27. What is "null pointer dereference"?
    - Follow-up: Dereferencing pointer that is null.
    - Follow-up: Example:
    ```cpp
    int *p = nullptr;
    int x = *p;  // CRASH: dereference nullptr
    
    // Fix:
    int *p = new int(5);
    if (p) {
        int x = *p;  // safe
    }
    delete p;
    ```

28. What is "buffer overflow"?
    - Follow-up: Writing beyond allocated buffer.
    - Follow-up: Example:
    ```cpp
    char buffer[10];
    strcpy(buffer, "hello world");  // CRASH: "hello world" is 11 chars
    
    // Fix:
    strncpy(buffer, "hello world", sizeof(buffer) - 1);
    buffer[sizeof(buffer) - 1] = '\0';
    ```

29. What is "stack overflow"?
    - Follow-up: Allocating too much on stack, exhausting stack space.
    - Follow-up: Example:
    ```cpp
    int large_array[1000000];  // might overflow stack (typically 4-64KB)
    
    // Fix:
    int *large_array = new int[1000000];  // heap allocation
    delete[] large_array;
    ```

30. What is "double free"?
    - Follow-up: Deleting same pointer twice.
    - Follow-up: Example:
    ```cpp
    int *p = new int(5);
    delete p;
    delete p;  // CRASH: double free
    
    // Fix:
    int *p = new int(5);
    delete p;
    p = nullptr;
    // now safe to check/delete
    if (p) delete p;
    ```

31. What is "memory leak"?
    - Follow-up: Allocating memory but never freeing.
    - Follow-up: Example:
    ```cpp
    void process_data() {
        int *data = new int[1000];
        // use data
        return;  // LEAK: data not freed
    }
    
    // Fix:
    void process_data() {
        int *data = new int[1000];
        // use data
        delete[] data;
    }
    
    // Or use RAII:
    void process_data() {
        std::unique_ptr<int[]> data(new int[1000]);
        // use data
    }  // auto-freed on scope exit
    ```

32. What is "uninitialized variable"?
    - Follow-up: Using variable before assigning value.
    - Follow-up: Example:
    ```cpp
    int x;  // uninitialized
    printf("%d\n", x);  // garbage value
    
    // Fix:
    int x = 0;  // initialized
    printf("%d\n", x);
    ```

33. What is "wild pointer"?
    - Follow-up: Pointer pointing to invalid/deallocated memory.
    - Follow-up: Example:
    ```cpp
    int *p = new int(5);
    delete p;
    *p = 10;  // CRASH: p is wild pointer (use-after-free)
    ```

34. What is "array out of bounds"?
    - Follow-up: Accessing array element beyond allocated size.
    - Follow-up: Example:
    ```cpp
    int arr[5] = {1, 2, 3, 4, 5};
    arr[10] = 99;  // CRASH: out of bounds (undefined behavior)
    
    // Fix:
    if (index >= 0 && index < 5) {
        arr[index] = 99;
    }
    ```

35. What is "format string vulnerability"?
    - Follow-up: Using untrusted string as format string.
    - Follow-up: Example:
    ```cpp
    char user_input[256];
    scanf("%s", user_input);
    printf(user_input);  // DANGER: user_input might contain %x, %s
    
    // Fix:
    printf("%s\n", user_input);
    ```

---

### Intermediate Runtime Errors

36. What is "iterator invalidation"?
    - Follow-up: Using iterator after container modified.
    - Follow-up: Example:
    ```cpp
    std::vector<int> v = {1, 2, 3};
    auto it = v.begin();
    
    v.push_back(4);  // might invalidate iterators
    *it;  // CRASH: iterator invalidated
    
    // Fix:
    std::vector<int> v = {1, 2, 3};
    auto it = v.begin();
    // don't modify v
    *it;  // safe
    ```

37. What is "dangling pointer"?
    - Follow-up: Pointer to object that's been destroyed.
    - Follow-up: Example:
    ```cpp
    int *get_pointer() {
        int x = 5;
        return &x;  // DANGER: returns pointer to local variable
    }
    
    int main() {
        int *p = get_pointer();
        *p;  // CRASH: x destroyed, p is dangling
    }
    
    // Fix:
    int *get_pointer() {
        return new int(5);  // allocate on heap
    }
    ```

38. What is "exception during destruction"?
    - Follow-up: Exception thrown in destructor (very bad).
    - Follow-up: Example:
    ```cpp
    class Resource {
    public:
        ~Resource() {
            if (some_error) {
                throw std::runtime_error("cleanup failed");  // DANGER
            }
        }
    };
    
    // Fix:
    class Resource {
    public:
        ~Resource() noexcept {
            if (some_error) {
                log_error("cleanup failed");  // never throw
            }
        }
    };
    ```

39. What is "resource leak in exception"?
    - Follow-up: Exception prevents cleanup code from running.
    - Follow-up: Example:
    ```cpp
    void process() {
        int *data = new int[1000];
        
        if (some_error) {
            throw std::runtime_error("error");  // LEAK: data not freed
        }
        
        delete[] data;
    }
    
    // Fix: use RAII
    void process() {
        std::unique_ptr<int[]> data(new int[1000]);
        
        if (some_error) {
            throw std::runtime_error("error");  // data auto-freed
        }
    }  // auto-freed
    ```

40. What is "type casting error"?
    - Follow-up: Incorrect reinterpret_cast causes undefined behavior.
    - Follow-up: Example:
    ```cpp
    int x = 42;
    double d = *reinterpret_cast<double *>(&x);  // DANGER: reinterpret as wrong type
    
    // Fix: use proper cast
    double d = static_cast<double>(x);
    ```

41. What is "heap corruption"?
    - Follow-up: Writing outside allocated block damages heap metadata.
    - Follow-up: Example:
    ```cpp
    int *arr = new int[5];
    arr[10] = 99;  // writes past end, corrupts heap
    delete[] arr;  // CRASH: corrupted heap
    ```

42. What is "race condition"?
    - Follow-up: Multiple threads access shared data without synchronization.
    - Follow-up: Example:
    ```cpp
    int shared_counter = 0;
    
    void increment() {
        shared_counter++;  // not atomic, race condition
    }
    
    // Thread 1 and 2 call increment() simultaneously
    // Counter might end up as 1 instead of 2
    
    // Fix:
    std::atomic<int> shared_counter(0);
    
    void increment() {
        shared_counter++;  // atomic, safe
    }
    ```

43. What is "deadlock" in multithreading?
    - Follow-up: Two threads waiting for each other (circular dependency).
    - Follow-up: Example:
    ```cpp
    Mutex m1, m2;
    
    void thread_A() {
        m1.lock();
        sleep(1);
        m2.lock();  // wait for m2
    }
    
    void thread_B() {
        m2.lock();
        sleep(1);
        m1.lock();  // wait for m1 (DEADLOCK)
    }
    
    // Fix: always lock in same order
    void thread_A() {
        m1.lock();
        m2.lock();
    }
    
    void thread_B() {
        m1.lock();
        m2.lock();
    }
    ```

44. What is "undefined behavior"?
    - Follow-up: Code that compiler makes no guarantees about.
    - Follow-up: Might work, might crash, might do anything.
    - Follow-up: Examples: dereference null, out of bounds, use-after-free.

---

### Advanced Runtime Errors

45. What is "ABI incompatibility"?
    - Follow-up: Calling convention/object layout mismatch between libraries.
    - Follow-up: Example:
    ```cpp
    // compiled with -fabi-version=2
    void process(MyClass obj);
    
    // linked with library compiled with -fabi-version=6
    // might call wrong function or read wrong members (CRASH)
    ```

46. What is "floating-point exception"?
    - Follow-up: Division by zero, invalid operation (FPE signal).
    - Follow-up: Example:
    ```cpp
    int x = 10;
    int y = 0;
    int result = x / y;  // FPE: integer division by zero
    
    // Fix:
    if (y != 0) {
        int result = x / y;
    }
    ```

47. What is "stack smashing"?
    - Follow-up: Buffer overflow on stack corrupts return address.
    - Follow-up: Example:
    ```cpp
    void vulnerable() {
        char buffer[10];
        gets(buffer);  // reads unlimited input, buffer overflow
    }
    
    // Fix:
    void safe() {
        char buffer[10];
        fgets(buffer, sizeof(buffer), stdin);  // bounded read
    }
    ```

---

## PART 3: LOGIC ERRORS

### 48. What is a logic error?
    - Follow-up: Code compiles and runs but produces wrong result.
    - Follow-up: Hardest to debug.
    - Follow-up: Examples: wrong algorithm, incorrect condition, off-by-one.

49. What is "off-by-one error"?
    - Follow-up: Loop iterates one too many or one too few times.
    - Follow-up: Example:
    ```cpp
    // WRONG: prints 0-10 (11 values)
    for (int i = 0; i <= 10; i++) {
        printf("%d\n", i);
    }
    
    // CORRECT: prints 0-9 (10 values)
    for (int i = 0; i < 10; i++) {
        printf("%d\n", i);
    }
    ```

50. What is "wrong operator" error?
    - Follow-up: Using = instead of ==, or wrong comparison.
    - Follow-up: Example:
    ```cpp
    if (x = 5) {  // assigns, not compares
        // always true
    }
    
    if (x == 5) {  // correct
        // true if x equals 5
    }
    ```

51. What is "missing break statement"?
    - Follow-up: Fall-through in switch statement.
    - Follow-up: Example:
    ```cpp
    switch (x) {
        case 1:
            printf("one");
            // missing break, falls through to case 2
        case 2:
            printf("two");
            break;
    }
    
    // if x == 1, prints "onetwo" (probably not intended)
    ```

52. What is "incorrect loop condition"?
    - Follow-up: Loop never executes or runs forever.
    - Follow-up: Example:
    ```cpp
    // WRONG: never executes (x starts at 5, condition is < 5)
    for (int x = 5; x < 5; x++) {
        printf("%d\n", x);
    }
    
    // CORRECT:
    for (int x = 0; x < 5; x++) {
        printf("%d\n", x);
    }
    
    // INFINITE LOOP:
    for (int x = 0; x < 5; x--) {  // x decreases, never reaches 5
        printf("%d\n", x);
    }
    ```

53. What is "wrong type coercion"?
    - Follow-up: Implicit type conversion produces wrong result.
    - Follow-up: Example:
    ```cpp
    int x = 10, y = 3;
    double result = x / y;  // WRONG: integer division (result = 3.0)
    
    double result = (double)x / y;  // CORRECT: (result = 3.333...)
    ```

54. What is "incorrect operator precedence"?
    - Follow-up: Operator order not as expected.
    - Follow-up: Example:
    ```cpp
    int result = 2 + 3 * 4;  // is it (2+3)*4=20 or 2+(3*4)=14?
    // Answer: 2 + (3*4) = 14 (multiplication has higher precedence)
    
    // Use parentheses for clarity:
    int result = 2 + (3 * 4);
    ```

---

## PART 4: MEMORY ISSUES

### Basic Memory Debugging

55. What is "memory leak detection"?
    - Follow-up: Tools: valgrind, AddressSanitizer (ASAN), Dr. Memory.
    - Follow-up: Example with valgrind:
    ```bash
    g++ -g program.cpp -o program
    valgrind --leak-check=full ./program
    ```

56. What is AddressSanitizer (ASAN)?
    - Follow-up: Compiler-based memory error detector.
    - Follow-up: Detects: out-of-bounds, use-after-free, double-free, leaks.
    - Follow-up: Usage:
    ```bash
    g++ -fsanitize=address -g program.cpp -o program
    ./program
    ```

57. What is "memory profiling"?
    - Follow-up: Track memory usage over time.
    - Follow-up: Tools: valgrind --tool=massif, heaptrack.
    - Follow-up: Identify memory leaks and excessive allocation.

58. What is "valgrind" and how to use?
    - Follow-up: Memory debugging and profiling tool.
    - Follow-up: Common commands:
    ```bash
    valgrind --leak-check=full ./program  # check for leaks
    valgrind --tool=massif ./program      # profile memory
    valgrind --track-origins=yes ./program  # find uninit var origin
    ```

59. What is valgrind output?
    - Follow-up: "LEAK SUMMARY": shows leaked memory.
    - Follow-up: "HEAP SUMMARY": total allocations.
    - Follow-up: "ERROR SUMMARY": errors found (0 is good).

---

## PART 5: DEBUGGING TECHNIQUES

### 60. What is "print debugging"?
    - Follow-up: Insert printf statements to trace execution.
    - Follow-up: Example:
    ```cpp
    void process(int x) {
        printf("process called with x=%d\n", x);
        x = x * 2;
        printf("after multiply: x=%d\n", x);
    }
    ```
    - Follow-up: Advantages: simple, works everywhere.
    - Follow-up: Disadvantages: clutters code, slow, must recompile.

61. What is "logging"?
    - Follow-up: Better than print debugging: log to file, configurable levels.
    - Follow-up: Example:
    ```cpp
    #define LOG_DEBUG(fmt, ...) printf("[DEBUG] " fmt "\n", __VA_ARGS__)
    #define LOG_ERROR(fmt, ...) fprintf(stderr, "[ERROR] " fmt "\n", __VA_ARGS__)
    
    void process(int x) {
        LOG_DEBUG("process called with x=%d", x);
        if (x < 0) {
            LOG_ERROR("invalid x: %d", x);
            return;
        }
    }
    ```

62. What is "GDB (GNU Debugger)"?
    - Follow-up: Interactive debugger for Linux/embedded.
    - Follow-up: Common commands:
    ```bash
    gdb ./program          # start debugger
    (gdb) break main       # set breakpoint at main
    (gdb) run              # start program
    (gdb) step             # step one line
    (gdb) next             # step over function
    (gdb) continue         # resume execution
    (gdb) print x          # print variable x
    (gdb) backtrace        # show call stack
    (gdb) watch x          # break on x change
    (gdb) quit             # exit debugger
    ```

63. How to compile for debugging?
    - Follow-up: Include debug symbols: -g flag.
    - Follow-up: Disable optimization for accurate debugging: -O0.
    ```bash
    g++ -g -O0 program.cpp -o program
    gdb ./program
    ```

64. What is "remote debugging"?
    - Follow-up: Debug embedded system from host PC.
    - Follow-up: Using gdbserver on target:
    ```bash
    # On target (embedded)
    gdbserver :9999 ./program
    
    # On host
    arm-none-eabi-gdb ./program
    (gdb) target remote 192.168.1.100:9999
    (gdb) break main
    (gdb) continue
    ```

65. What is "core dump"?
    - Follow-up: Image of process memory when it crashes.
    - Follow-up: Analyze post-mortem.
    - Follow-up: Enable core dumps:
    ```bash
    ulimit -c unlimited   # allow core dumps
    ./program             # crashes, generates core
    gdb ./program core    # analyze crash
    ```

66. What is "assert"?
    - Follow-up: Catch programming errors during development.
    - Follow-up: Disabled in release builds (NDEBUG).
    ```cpp
    #include <cassert>
    
    void process(int x) {
        assert(x > 0);  // crash if x <= 0 (debug only)
        // use x
    }
    ```

67. What is "static analysis"?
    - Follow-up: Check code without running it.
    - Follow-up: Tools: cppcheck, clang-analyzer.
    - Follow-up: Catches common mistakes early.
    ```bash
    cppcheck program.cpp
    clang --analyze program.cpp
    ```

68. What is "code coverage"?
    - Follow-up: Measure which lines of code execute during tests.
    - Follow-up: Tools: gcov, llvm-cov.
    - Follow-up: Goal: find untested code paths.
    ```bash
    g++ --coverage program.cpp test.cpp -o test
    ./test
    gcov program.cpp  # generate coverage report
    ```

---

## PART 6: EMBEDDED-SPECIFIC ERRORS

### 69. What is "stack overflow" in embedded?
    - Follow-up: Stack exhausted due to deep recursion or large locals.
    - Follow-up: Example:
    ```cpp
    void recursive(int n) {
        if (n == 0) return;
        int large_array[1000];  // 4KB per call
        recursive(n - 1);  // CRASH if stack too small
    }
    ```
    - Follow-up: Fix: use heap or reduce stack size.

70. What is "RTOS task crash"?
    - Follow-up: Task accesses null or invalid pointer.
    - Follow-up: Symptoms: task hangs, system resets, watchdog trigger.
    - Follow-up: Debug: add logging, use JTAG debugger.

71. What is "interrupt handler crash"?
    - Follow-up: ISR has bug (dereference nullptr, infinite loop).
    - Follow-up: Very hard to debug (ISR runs asynchronously).
    - Follow-up: Fix: keep ISR minimal, move work to task.

72. What is "hardware timeout"?
    - Follow-up: Code waits for hardware that never responds.
    - Follow-up: Example:
    ```cpp
    // HANG: wait for UART data forever
    while (!UART_RxReady()) {}
    uint8_t data = UART_Receive();
    
    // Fix: add timeout
    uint32_t timeout = get_ticks() + 1000;  // 1 second
    while (!UART_RxReady() && get_ticks() < timeout) {}
    if (get_ticks() >= timeout) {
        // handle timeout error
    }
    ```

73. What is "watchdog reset"?
    - Follow-up: Embedded system resets due to watchdog timer.
    - Follow-up: Indicates system hanging or stuck in ISR.
    - Follow-up: Fix: kick watchdog periodically, find deadlock.

74. What is "stack corruption" in embedded?
    - Follow-up: Task writes beyond its stack, corrupts other task's data.
    - Follow-up: Symptoms: random task crashes, memory corruption.
    - Follow-up: Fix: increase task stack size, use stack guard.

75. What is "integer overflow"?
    - Follow-up: Arithmetic result exceeds type range.
    - Follow-up: Example:
    ```cpp
    uint8_t x = 250;
    x += 10;  // overflow: result is 4 (wraps around)
    
    // Fix: check before operation or use wider type
    uint16_t x = 250;
    x += 10;  // OK: result is 260
    ```

76. What is "division by zero" check?
    - Follow-up: Check before dividing.
    - Follow-up: Example:
    ```cpp
    // WRONG:
    int result = 100 / divisor;
    
    // CORRECT:
    if (divisor != 0) {
        int result = 100 / divisor;
    } else {
        // handle error
    }
    ```

77. What is "null pointer check" best practice?
    - Follow-up: Always check pointer before use.
    - Follow-up: Example:
    ```cpp
    void process(MyClass *obj) {
        if (!obj) {
            LOG_ERROR("obj is null");
            return;
        }
        obj->do_something();
    }
    ```

---

## PART 7: DEBUGGING TOOLS & TECHNIQUES

### 78. What is "strace"?
    - Follow-up: Linux tool to trace system calls.
    - Follow-up: Shows every syscall program makes.
    - Follow-up: Useful for understanding OS-level issues.
    ```bash
    strace -e trace=open,read,write ./program
    ```

79. What is "ltrace"?
    - Follow-up: Trace library function calls.
    - Follow-up: Similar to strace but for libc functions.
    ```bash
    ltrace ./program
    ```

80. What is "perf" (Linux profiler)?
    - Follow-up: Performance profiler, see which functions consume most time.
    - Follow-up: Usage:
    ```bash
    perf record ./program
    perf report
    ```

81. What is "Clang/LLVM sanitizers"?
    - Follow-up: AddressSanitizer (ASAN): detect heap errors.
    - Follow-up: ThreadSanitizer (TSAN): detect race conditions.
    - Follow-up: MemorySanitizer (MSAN): detect use-of-uninitialized-memory.
    - Follow-up: UndefinedBehaviorSanitizer (UBSAN): undefined behavior.
    - Follow-up: Usage:
    ```bash
    clang++ -fsanitize=address program.cpp -o program
    clang++ -fsanitize=thread program.cpp -o program
    ./program
    ```

82. What is "CppCheck" static analyzer?
    - Follow-up: Free static analyzer for C/C++.
    - Follow-up: Finds common mistakes, memory leaks, style issues.
    - Follow-up: Usage:
    ```bash
    cppcheck --enable=all program.cpp
    ```

83. What is "Clang Static Analyzer"?
    - Follow-up: More powerful than CppCheck, uses AST analysis.
    - Follow-up: Usage:
    ```bash
    scan-build g++ program.cpp
    ```

84. What is "splint"?
    - Follow-up: Lint tool for C/C++.
    - Follow-up: Checks for: null pointers, buffer overflow, memory leak.
    - Follow-up: Usage:
    ```bash
    splint program.c
    ```

85. What is "Valgrind memcheck"?
    - Follow-up: Detects: memory leaks, invalid accesses, use-after-free.
    - Follow-up: Very accurate but slow (100x slower).
    - Follow-up: Usage:
    ```bash
    valgrind --leak-check=full ./program
    ```

86. What is "Valgrind Helgrind"?
    - Follow-up: Thread error detector in valgrind.
    - Follow-up: Finds data races, deadlocks.
    - Follow-up: Usage:
    ```bash
    valgrind --tool=helgrind ./program
    ```

87. What is "JTAG debugging"?
    - Follow-up: Hardware debugger for embedded systems.
    - Follow-up: Sets breakpoints in hardware, inspects registers/memory.
    - Follow-up: Tools: OpenOCD, J-Link, ST-Link.
    - Follow-up: Usage:
    ```bash
    openocd -f interface/stlink-v2.cfg -f target/stm32f4x.cfg
    arm-none-eabi-gdb
    (gdb) target remote localhost:3333
    (gdb) load firmware.elf
    ```

---

## PART 8: DEBUGGING STRATEGIES

### 88. How to approach debugging?
    1. **Reproduce** the bug consistently.
    2. **Isolate** the problem (narrow down location).
    3. **Hypothesize** the cause.
    4. **Test** the hypothesis.
    5. **Fix** the root cause (not symptom).
    6. **Verify** the fix doesn't break anything else.

89. What is "binary search debugging"?
    - Follow-up: Divide code in half, determine which half has bug.
    - Follow-up: Repeat until bug isolated to small function.
    - Follow-up: Example:
    ```cpp
    // Suspect bug in this function
    void process_data() {
        step1();
        // add breakpoint here
        step2();
        step3();
        step4();
    }
    
    // Run with breakpoint, inspect data after step1
    // If correct, bug is in step2-4
    // Move breakpoint, repeat
    ```

90. What is "rubber duck debugging"?
    - Follow-up: Explain code line-by-line to rubber duck (or someone).
    - Follow-up: Often discovers bug while explaining.
    - Follow-up: Effective technique, really works.

91. What is "instrumentation" for debugging?
    - Follow-up: Add code to measure behavior (timing, values).
    - Follow-up: Example:
    ```cpp
    uint32_t start = get_time();
    some_function();
    uint32_t duration = get_time() - start;
    printf("Function took %u ms\n", duration);
    ```

92. How to handle flaky bugs?
    - Follow-up: Bug happens intermittently, hard to reproduce.
    - Follow-up: Causes: timing issues, uninitialized variables, race conditions.
    - Follow-up: Fix: enable all warnings, use sanitizers, add logging, stress test.

93. What is "git bisect" for debugging?
    - Follow-up: Binary search through git history to find breaking commit.
    - Follow-up: Usage:
    ```bash
    git bisect start
    git bisect bad HEAD        # current commit is bad
    git bisect good v1.0       # v1.0 was good
    # git checks out middle commit
    ./test.sh                  # test if good or bad
    git bisect good            # if this commit good
    # repeat until bug commit found
    git bisect reset
    ```

---

## PART 9: COMMON C++ MISTAKES & SOLUTIONS

### 94. Mistake: Comparing floating-point with ==
    - Follow-up: Floating-point precision issues.
    - Follow-up: WRONG:
    ```cpp
    if (x == 0.1) {  // might never be true
        // ...
    }
    ```
    - Follow-up: CORRECT:
    ```cpp
    const double EPSILON = 1e-9;
    if (abs(x - 0.1) < EPSILON) {
        // ...
    }
    ```

95. Mistake: Using = in if condition
    - Follow-up: Accidental assignment instead of comparison.
    - Follow-up: WRONG:
    ```cpp
    if (x = 5) {  // assigns x to 5, condition always true
        // ...
    }
    ```
    - Follow-up: CORRECT:
    ```cpp
    if (x == 5) {  // compares x to 5
        // ...
    }
    ```

96. Mistake: Not checking function return value
    - Follow-up: Function returns error code, ignored.
    - Follow-up: WRONG:
    ```cpp
    FILE *f = fopen("file.txt", "r");
    fscanf(f, "%d", &x);  // f might be null
    ```
    - Follow-up: CORRECT:
    ```cpp
    FILE *f = fopen("file.txt", "r");
    if (!f) {
        LOG_ERROR("cannot open file");
        return;
    }
    fscanf(f, "%d", &x);
    fclose(f);
    ```

97. Mistake: Modifying container while iterating
    - Follow-up: Iterator invalidated, undefined behavior.
    - Follow-up: WRONG:
    ```cpp
    std::vector<int> v = {1, 2, 3};
    for (auto it = v.begin(); it != v.end(); ++it) {
        if (*it == 2) {
            v.erase(it);  // DANGER: invalidates it
        }
    }
    ```
    - Follow-up: CORRECT:
    ```cpp
    std::vector<int> v = {1, 2, 3};
    for (auto it = v.begin(); it != v.end(); ) {
        if (*it == 2) {
            it = v.erase(it);  // erase returns next iterator
        } else {
            ++it;
        }
    }
    ```

98. Mistake: Accessing string with operator[] without bounds check
    - Follow-up: Buffer overflow if index out of range.
    - Follow-up: WRONG:
    ```cpp
    std::string s = "hello";
    char c = s[10];  // undefined behavior
    ```
    - Follow-up: CORRECT:
    ```cpp
    std::string s = "hello";
    if (10 < s.length()) {
        char c = s[10];
    }
    ```

99. Mistake: Using pointer to local variable
    - Follow-up: Dangling pointer after function returns.
    - Follow-up: WRONG:
    ```cpp
    int *get_pointer() {
        int x = 5;
        return &x;  // x destroyed when function returns
    }
    ```
    - Follow-up: CORRECT:
    ```cpp
    int *get_pointer() {
        return new int(5);  // allocate on heap
    }
    
    int *ptr = get_pointer();
    delete ptr;  // caller must delete
    ```

100. Mistake: Forgetting to free allocated memory
     - Follow-up: Memory leak.
     - Follow-up: WRONG:
     ```cpp
     void process() {
         int *data = new int[1000];
         // use data
         return;  // leak: never freed
     }
     ```
     - Follow-up: CORRECT:
     ```cpp
     void process() {
         std::unique_ptr<int[]> data(new int[1000]);
         // use data
     }  // auto-freed on scope exit
     ```

---

## EXPERT WAR STORIES / DEBUGGING EXPERIENCES

1. "I had random crashes that only happened in release builds (not debug), what causes this?"
   - **Answer:** Debug build disables optimizations, changes memory layout. Likely bug: uninitialized variable, undefined behavior, stack smash, use-after-free. Enable ASAN or sanitizers in release to catch.

2. "My embedded system resets randomly every few hours, how do I find the cause?"
   - **Answer:** Add watchdog logging. Add assert in ISRs. Check stack usage (usually stack overflow). Increase stack size. Use JTAG to set breakpoint at reset vector. Profile memory allocations.

3. "Memory leak detector shows huge leak, but code looks correct, why?"
   - **Answer:** Static objects not destroyed (acceptable). Global variables. Singleton pattern. False positive (static allocated memory). Use suppressions or leak-check=summary.

4. "My RTOS task hangs in one function, but no debugger breakpoint works, what do I do?"
   - **Answer:** Task might be in ISR or infinite loop. Add logging before/after suspect lines. Check if spinlock or interrupt contention. Use logic analyzer to see signals. Add timeout assert.

5. "Valgrind shows "still reachable" leaks, should I fix them?"
   - **Answer:** "Still reachable" are blocks referenced at program end, usually fine. "Definitely lost" and "indirectly lost" must be fixed. Configure valgrind to ignore "still reachable".

6. "I fixed one buffer overflow bug but crashes still happen elsewhere, am I missing something?"
   - **Answer:** Likely multiple bugs. Enable ASAN or valgrind for all tests. One overflow might corrupt heap metadata, causing later crash in unrelated code.

7. "My function works in unit tests but crashes in integration test, why?"
   - **Answer:** Integration test uses different data (larger, edge case). Different order of operations (race condition). Mock doesn't match real hardware. Use real hardware for integration test.

8. "Thread sanitizer shows race condition but code has mutex, am I missing something?"
   - **Answer:** Mutex not protecting both accesses. Mutex acquired/released in different order (ABBA deadlock). Mutex destroyed before thread exits. Review locking carefully.

9. "GDB shows wrong source line during debugging, am I doing something wrong?"
   - **Answer:** Likely optimization flag (-O2, -O3). Compile with -O0 -g for debugging. Linker script might move code. Check if code matches binary version.

10. "Embedded system works on bench but fails in the field, what could cause this?"
    - **Answer:** Temperature-dependent bugs (clock too fast when hot). Voltage-dependent bugs (flaky memory). EMI from environment. Timing bugs that don't manifest at bench clock speed. Add temperature monitoring, reduce clock, shield from EMI.

---

## DEBUGGING CHECKLIST

### Before starting debugging:
- ☐ Reproduce bug consistently
- ☐ Record error message, stack trace, or symptoms
- ☐ Note when it started (recent change?)
- ☐ Compile with debug symbols (-g)
- ☐ Enable warnings (-Wall -Wextra -Werror)
- ☐ Enable sanitizers (ASAN, UBSAN)

### During debugging:
- ☐ Use gdb or IDE debugger
- ☐ Set breakpoint before crash
- ☐ Inspect variables and stack
- ☐ Use valgrind for memory issues
- ☐ Use static analyzer (cppcheck, clang-analyzer)
- ☐ Add logging for async code (ISR, threads)

### After fix:
- ☐ Verify bug is fixed
- ☐ Verify no new bugs introduced
- ☐ Add regression test
- ☐ Run full test suite
- ☐ Check binary size didn't explode
- ☐ Document root cause and fix

---

## C++ ERROR QUICK REFERENCE

| Error Type | Common Cause | Debug Tool | Fix |
|-----------|-------------|-----------|-----|
| Segmentation Fault | Null pointer, buffer overflow | gdb, ASAN | Add null checks, bounds checking |
| Undefined Reference | Missing definition | linker | Link with correct object file |
| Stack Overflow | Deep recursion, large locals | valgrind | Reduce stack usage, use heap |
| Memory Leak | Never freed memory | valgrind | Use smart pointers or free |
| Race Condition | Unsynchronized threads | ThreadSanitizer | Add mutex, atomic |
| Deadlock | Circular lock dependency | strace | Lock in same order |
| Logic Error | Wrong algorithm | gdb, assert | Review algorithm, add tests |
| Type Mismatch | Wrong cast | compiler | Use correct type, static_cast |

---

*This comprehensive guide covers C++ errors and debugging from basics to expert level, suitable for M.Tech graduate with 10+ years experience in embedded systems.*