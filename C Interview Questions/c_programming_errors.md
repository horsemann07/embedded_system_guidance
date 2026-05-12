# C Errors, Warnings & Debugging — Complete Question Set

> Covers: Compile-time, Link-time, Runtime, Logical, and Segmentation errors  
> With follow-up/counter questions for depth

---

## 1. COMPILE-TIME ERRORS

### Syntax Errors

1. What is a compile-time error?
   - Follow-up: Who catches it — preprocessor, compiler, or linker?
2. What is a syntax error? Give 5 examples.
   ```c
   int x = 10     // missing semicolon
   if (x = 5) {   // = instead of == (warning, not error, but still important)
   int 1var;       // invalid identifier
   int x = ;      // missing expression
   void foo(      // missing closing parenthesis
   ```
3. What happens if you forget `#include <stdio.h>` and call `printf()`?
   - Follow-up: In C89 vs C99 — implicit function declaration.
   - Follow-up: Why did C99 remove implicit function declarations?

### Type Errors

4. What is a type mismatch error?
   ```c
   int *p = 10;           // assigning int to pointer
   float f = "hello";     // assigning string to float
   ```
5. What is incompatible pointer type warning?
   ```c
   int x = 5;
   float *p = &x;  // incompatible pointer type
   ```
   - Follow-up: Why is this dangerous even if it compiles?
6. What happens when you pass wrong argument types to a function?
   - Follow-up: What if there is no prototype?

### Declaration Errors

7. What is "undeclared identifier" error?
8. What is "redefinition" error?
   ```c
   int x = 5;
   int x = 10;  // redefinition
   ```
   - Follow-up: Difference between redefinition and redeclaration.
9. What is "conflicting types" error?
   - Follow-up: When does this happen with function declarations?

### Preprocessor Errors

10. What is `#error` directive? When do you use it?
    ```c
    #ifndef CONFIG_BOARD
    #error "CONFIG_BOARD must be defined"
    #endif
    ```
11. What happens with circular `#include`?
    - Follow-up: How do header guards prevent it?
12. What is "macro redefined" warning?
13. What happens if `#include` file is not found?

### Compiler Warnings (Treated as Errors with `-Werror`)

14. What is the difference between a warning and an error?
15. Why should you compile with `-Wall -Wextra -Werror`?
16. Common important warnings:
    - Unused variable
    - Unused function
    - Implicit function declaration
    - Incompatible pointer type
    - Format string mismatch (`%d` with `float`)
    - Comparison between signed and unsigned
    - Missing return statement
    - Uninitialized variable usage
    - Follow-up: Which warnings can hide real bugs?
17. What is `-Wpedantic`? When should you use it?
18. What is `-Wshadow`?
    ```c
    int x = 5;
    void foo() {
        int x = 10;  // shadows outer x
    }
    ```
    - Follow-up: Why is variable shadowing dangerous?

---

## 2. LINK-TIME ERRORS

19. What is a link-time error?
    - Follow-up: Difference between compile-time and link-time error.
20. What is "undefined reference" error?
    ```
    undefined reference to `foo'
    ```
    - Follow-up: Common causes (missing `.c` file, missing library, typo in function name).
21. What is "multiple definition" error?
    ```
    multiple definition of `x'
    ```
    - Follow-up: Why does defining a variable in a header cause this?
    - Follow-up: How does `static` or `extern` fix it?
22. What is "undefined symbol" error?
    - Follow-up: How to use `nm` to check symbols in object file?
23. What happens if you declare `extern int x;` but never define `x`?
24. Difference between "unresolved external" and "undefined reference".
25. What is "duplicate symbol" error in static libraries?
    - Follow-up: How does the linker resolve symbols from `.a` files vs `.o` files?
26. What happens when library linking order is wrong?
    ```
    gcc main.o -lm -lfoo   # correct
    gcc -lfoo main.o -lm   # may fail
    ```
    - Follow-up: Why does order matter with static libraries?
27. What is a weak symbol? How does it affect linking?
    ```c
    __attribute__((weak)) void handler(void) { }
    ```
    - Follow-up: What happens if both weak and strong definitions exist?

---

## 3. RUNTIME ERRORS

### Segmentation Fault

28. What is a segmentation fault?
    - Follow-up: What causes it at OS level? (accessing unmapped memory, permission violation)
29. Common causes of segfault:
    - Dereferencing NULL pointer
    - Dereferencing dangling pointer
    - Buffer overflow (stack/heap)
    - Writing to string literal
    - Stack overflow
    - Accessing freed memory
    - Follow-up: How do you debug a segfault systematically?
30. What is `SIGSEGV` signal?
    - Follow-up: Can you catch it? Should you?
    ```c
    signal(SIGSEGV, handler);  // can catch but dangerous
    ```
31. What is a core dump? How do you analyze it?
    ```bash
    ulimit -c unlimited
    gcc -g program.c -o program
    ./program        # crashes, generates core
    gdb program core # analyze
    ```
    - Follow-up: What info does a core dump contain?
    - Follow-up: How do you read a backtrace from core dump?

### Bus Error

32. What is a bus error (`SIGBUS`)?
    - Follow-up: Difference between segfault and bus error.
    - Follow-up: When does misaligned access cause bus error?
    - Follow-up: On which architectures? (ARM Cortex-M0 vs x86)

### Memory Errors

33. What is a memory leak?
    - Follow-up: How do you detect it? (Valgrind, ASan, custom tracking)
    - Follow-up: Why is memory leak critical in embedded long-running systems?
34. What is double free?
    ```c
    int *p = malloc(sizeof(int));
    free(p);
    free(p);  // double free — undefined behavior
    ```
    - Follow-up: What actually happens internally? (heap corruption)
    - Follow-up: How to prevent? (set pointer to NULL after free)
35. What is use-after-free?
    ```c
    int *p = malloc(sizeof(int));
    free(p);
    *p = 10;  // use after free — UB
    ```
    - Follow-up: Why might it "work" sometimes?
36. What is heap corruption?
    - Follow-up: How does writing past allocated buffer corrupt heap metadata?
    - Follow-up: How do you detect heap corruption?
37. What is buffer overflow?
    ```c
    char buf[10];
    strcpy(buf, "this is way too long for the buffer");
    ```
    - Follow-up: Stack buffer overflow vs heap buffer overflow.
    - Follow-up: What is stack smashing?
    - Follow-up: What is a stack canary?
38. What is stack overflow?
    - Follow-up: How to detect in embedded without OS? (fill pattern, watermark checking)
    - Follow-up: What is the default stack size? Can you change it?

### Arithmetic Errors

39. What happens on division by zero?
    ```c
    int x = 10 / 0;   // UB for integers
    float y = 10.0 / 0.0;  // +Inf (IEEE 754)
    ```
    - Follow-up: Difference between integer and floating-point division by zero.
    - Follow-up: What is `SIGFPE`?
40. What is integer overflow?
    ```c
    int x = INT_MAX;
    x = x + 1;  // signed overflow — UB
    ```
    - Follow-up: Why is signed overflow UB but unsigned overflow wraps?
    - Follow-up: How to safely check for overflow before it happens?
41. What is floating-point precision error?
    ```c
    float f = 0.1 + 0.2;  // not exactly 0.3
    if (f == 0.3) { }      // may be false
    ```
    - Follow-up: Why should you never use `==` with floats?
    - Follow-up: What is epsilon comparison?

### Format String Errors

42. What happens with format string mismatch?
    ```c
    int x = 5;
    printf("%s", x);    // UB — expects string, gets int
    printf("%d", 3.14); // UB — expects int, gets double
    ```
    - Follow-up: What is format string vulnerability/attack?
    ```c
    char *user_input = get_input();
    printf(user_input);  // DANGEROUS — format string attack
    ```
    - Follow-up: How to fix? `printf("%s", user_input);`
43. What happens when `printf` and `scanf` format specifiers don't match argument types?
    - Follow-up: How does `-Wformat` help?

---

## 4. LOGICAL ERRORS

44. What is a logical error?
    - Follow-up: Why is it the hardest to debug?
45. Common logical errors:
    - Using `=` instead of `==`
    - Off-by-one error in loops
    - Wrong operator precedence assumption
    - Missing `break` in `switch`
    - Infinite loop (unintentional)
    - Wrong pointer arithmetic
    - Signed/unsigned comparison bug
    ```c
    unsigned int a = 5;
    int b = -1;
    if (a > b) { }  // b promoted to unsigned — becomes huge number
    ```
    - Follow-up: Why is signed/unsigned comparison dangerous?
46. What is an off-by-one error?
    ```c
    for (int i = 0; i <= n; i++)  // processes n+1 elements
    ```
    - Follow-up: How do you systematically avoid it?
47. What is a race condition?
    - Follow-up: How is it different from a logical error?
    - Follow-up: Can you have a race condition without threads? (ISR + main loop)
48. What is a deadlock? Can it happen in single-threaded embedded?
    - Follow-up: How do nested interrupt priorities create deadlock-like situations?

---

## 5. PREPROCESSOR-TIME ERRORS & GOTCHAS

49. Macro expansion bugs:
    ```c
    #define SQUARE(x) x * x
    SQUARE(3 + 1)  // expands to 3 + 1 * 3 + 1 = 7, not 16
    ```
    - Follow-up: How to fix? `#define SQUARE(x) ((x) * (x))`
    - Follow-up: Even with parentheses, what problem remains?
    ```c
    #define SQUARE(x) ((x) * (x))
    SQUARE(i++)  // i incremented twice — UB
    ```
50. Macro side effects with arguments evaluated multiple times.
    - Follow-up: Why are inline functions safer than macros?
51. `#define` vs `typedef` — what errors can each cause?
    ```c
    #define PTR int*
    PTR a, b;  // a is int*, b is int (NOT int*)

    typedef int* PTR;
    PTR a, b;  // both are int*
    ```
52. What is "token pasting" error?
    ```c
    #define VAR(n) var_##n
    VAR(1)  // var_1 — fine
    VAR(+)  // invalid token — error
    ```

---

## 6. ASSERTION ERRORS

53. What is `assert()`? When to use it?
    ```c
    assert(ptr != NULL);
    assert(index < MAX_SIZE);
    ```
    - Follow-up: What happens when assertion fails?
    - Follow-up: What does `NDEBUG` do?
    - Follow-up: Should you use `assert` in production embedded code?
54. What is `_Static_assert` (C11)?
    ```c
    _Static_assert(sizeof(int) == 4, "int must be 4 bytes");
    ```
    - Follow-up: When is it evaluated — compile time or runtime?
    - Follow-up: How is it useful for embedded portability?
55. Difference between `assert` and `_Static_assert`.
    - Follow-up: Can `_Static_assert` check a variable value?

---

## 7. EMBEDDED-SPECIFIC RUNTIME ERRORS

56. Watchdog timeout — what causes it?
    - Follow-up: Infinite loop in ISR? Blocking call in main loop?
57. Hard fault on ARM Cortex-M — common causes.
    - Follow-up: How do you decode the hard fault register?
    - Follow-up: What is a bus fault, usage fault, memory management fault?
58. Stack overflow in embedded (no MMU/MPU).
    - Follow-up: How to detect? (fill pattern `0xDEADBEEF`, MPU if available)
59. Priority inversion in RTOS.
    - Follow-up: What is priority inheritance?
60. DMA errors — buffer overrun, alignment issues.
    - Follow-up: Why must DMA buffers be cache-aligned on Cortex-M7?

---

## 8. DEBUGGING TOOLS & TECHNIQUES

61. How do you use `gdb` for debugging C programs?
    - Follow-up: Set breakpoint, watch variable, backtrace, step, next, print.
62. What is Valgrind? What errors does it detect?
    - Follow-up: Memory leak, invalid read/write, use-after-free, uninitialized value.
63. What is AddressSanitizer (ASan)?
    - Follow-up: How to enable? `gcc -fsanitize=address`
    - Follow-up: ASan vs Valgrind — performance and coverage difference?
64. What is UndefinedBehaviorSanitizer (UBSan)?
    - Follow-up: `gcc -fsanitize=undefined`
65. What is `strace` / `ltrace`?
    - Follow-up: How to trace system calls and library calls?
66. How do you use `objdump` and `readelf`?
    - Follow-up: Dump disassembly, check sections, check symbols.
67. What is `nm`? How to check if a symbol exists in object file?
68. Embedded debugging:
    - JTAG/SWD
    - Serial print debugging
    - Logic analyzer
    - LED toggling
    - Follow-up: When is printf debugging acceptable vs not?

---

## 9. ERROR HANDLING PATTERNS IN C

69. Return code pattern:
    ```c
    int result = do_something();
    if (result != 0) {
        // handle error
    }
    ```
    - Follow-up: What are the drawbacks?
70. `errno` mechanism:
    ```c
    FILE *f = fopen("file.txt", "r");
    if (f == NULL) {
        perror("fopen failed");  // prints errno message
    }
    ```
    - Follow-up: Is `errno` thread-safe? (Yes in POSIX — thread-local)
71. `goto` for cleanup:
    ```c
    int foo() {
        int *a = malloc(100);
        if (!a) goto fail_a;

        int *b = malloc(200);
        if (!b) goto fail_b;

        // success path
        free(b);
        free(a);
        return 0;

    fail_b:
        free(a);
    fail_a:
        return -1;
    }
    ```
    - Follow-up: Why is this pattern used in Linux kernel?
    - Follow-up: Alternatives to `goto` for error cleanup?
72. Callback-based error handling:
    ```c
    typedef void (*error_handler_t)(int error_code, const char *msg);
    void register_error_handler(error_handler_t handler);
    ```
    - Follow-up: When is this better than return codes?
73. `setjmp` / `longjmp` — exception-like error handling:
    ```c
    jmp_buf env;
    if (setjmp(env) != 0) {
        // error recovery
    }
    // ...
    longjmp(env, 1);  // jump back
    ```
    - Follow-up: Why is this dangerous? When is it acceptable?
    - Follow-up: Does it call destructors / free memory? (No)

---

## 10. TRICK / OUTPUT QUESTIONS ON ERRORS

1. What happens here?
   ```c
   char *p = "hello";
   p[0] = 'H';
   ```
   **Answer:** Segfault or UB — writing to read-only string literal.

2. What happens here?
   ```c
   int *p = malloc(0);
   ```
   **Answer:** Implementation-defined — may return NULL or valid unique pointer that can be freed.

3. What happens here?
   ```c
   int arr[5] = {1, 2, 3, 4, 5};
   int *p = arr + 5;
   int val = *p;
   ```
   **Answer:** `arr + 5` is valid pointer (one past end), but dereferencing it is UB.

4. What happens here?
   ```c
   printf("%d\n", sizeof(printf("hello")));
   ```
   **Answer:** `printf("hello")` is NOT executed. `sizeof` evaluates type at compile time. Prints `4` (size of `int` — return type of `printf`).

5. What happens here?
   ```c
   int x;
   if (x) {
       printf("true\n");
   }
   ```
   **Answer:** UB — `x` is uninitialized local variable, value is indeterminate.

6. What happens here?
   ```c
   int a = -1;
   unsigned int b = 1;
   if (a < b) printf("less");
   else printf("greater or equal");
   ```
   **Answer:** Prints `"greater or equal"` — `a` is promoted to unsigned, becomes `UINT_MAX`.

7. What happens here?
   ```c
   int *p = NULL;
   int x = *p;
   ```
   **Answer:** Segfault — NULL pointer dereference.

8. What happens here?
   ```c
   free(NULL);
   ```
   **Answer:** Nothing — guaranteed safe by the C standard.

9. What happens here?
   ```c
   char buf[5];
   sprintf(buf, "%s", "Hello, World!");
   ```
   **Answer:** Buffer overflow — writes past `buf`, corrupts stack. Use `snprintf`.

10. What happens here?
    ```c
    int arr[3] = {1, 2, 3};
    printf("%d\n", arr[3]);
    ```
    **Answer:** UB — out-of-bounds access. May print garbage, may crash.

---

## SUMMARY TABLE — ERROR TYPES

| Error Type | When Detected | Who Detects | Example |
|------------|--------------|-------------|---------|
| Syntax Error | Compile-time | Compiler | Missing semicolon |
| Type Error | Compile-time | Compiler | Assigning string to int |
| Preprocessor Error | Preprocessing | Preprocessor | `#include` not found |
| Link Error | Link-time | Linker | Undefined reference |
| Runtime Error | Execution | OS / Hardware | Segfault, division by zero |
| Logical Error | Execution | Developer (testing) | Wrong output, off-by-one |
| Memory Error | Execution | Valgrind/ASan/OS | Leak, double free, overflow |
| Assertion Error | Execution (debug) | `assert()` | Failed precondition |
| Static Assert | Compile-time | Compiler | `_Static_assert` failure |
| Hard Fault | Execution | Hardware (ARM) | Unaligned access, invalid PC |

---

*This document covers all error categories in C — from compile-time to runtime to embedded-specific faults — with depth-probing follow-ups for senior-level interviews.*