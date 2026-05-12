# CMake and Make Interview Questions (Structured by Levels)

> Focus: Build systems for C/C++ projects, embedded-friendly, practical depth

---

## CMAKE INTERVIEW QUESTIONS

### Basic

1. What is CMake and why is it used?
   - Follow-up: Is CMake a build system or a build-system generator?

2. Difference between CMake and Make?
   - Follow-up: When would you use one directly vs together?

3. What are the main components of a `CMakeLists.txt` file?
   - Follow-up: Role of `cmake_minimum_required()`, `project()`, `add_executable()`.

4. What is out-of-source build in CMake? Why preferred?
   - Follow-up: What issues does in-source build cause?

5. How do you define targets in CMake?
   - Follow-up: Difference between executable, static lib, shared lib, interface lib.

6. How do you link libraries in CMake?
   - Follow-up: What does `target_link_libraries()` do?

7. Difference between `target_include_directories()` and `include_directories()`?
   - Follow-up: Why modern CMake recommends target-based commands?

8. What are CMake variables and how are they used?
   - Follow-up: Difference between normal variable, cache variable, environment variable.

9. What is a CMake cache variable?
   - Follow-up: How do `option()` and `set(... CACHE ...)` work?

10. How do you set compiler flags/options in CMake?
   - Follow-up: Difference between global flags and target-specific options.

---

### Intermediate

11. Explain `PUBLIC`, `PRIVATE`, `INTERFACE` in CMake.
   - Follow-up: How does usage requirement propagation work?

12. How do you handle conditional compilation in CMake?
   - Follow-up: `if()`, generator expressions, platform/compiler checks.

13. How do you add compile definitions?
   - Follow-up: `target_compile_definitions()` vs `add_definitions()`.

14. How do you integrate external libraries?
   - Follow-up: `find_package()`, `FetchContent`, `add_subdirectory()` differences.

15. What is `find_package()`? Config mode vs module mode?
   - Follow-up: Why imported targets are preferred over raw variables?

16. How do you create/install/export a CMake package?
   - Follow-up: `install(TARGETS ...)`, `install(EXPORT ...)`, package config files.

17. How do you set C/C++ standards in CMake?
   - Follow-up: `C_STANDARD`, `C_EXTENSIONS`, `C_STANDARD_REQUIRED`.

18. How do you integrate unit tests in CMake?
   - Follow-up: `enable_testing()`, `add_test()`, CTest usage.

19. How do you support multiple build types?
   - Follow-up: `Debug`, `Release`, `RelWithDebInfo`, `MinSizeRel`.

20. How do you integrate CMake with IDEs and CI pipelines?
   - Follow-up: Ninja vs Makefiles vs Visual Studio generator.

---

### Advanced

21. What are generator expressions in CMake?
   - Follow-up: Example: `$<CONFIG:Debug>`, `$<TARGET_PROPERTY:...>`.

22. How do you cross-compile with CMake?
   - Follow-up: Toolchain file essentials (`CMAKE_SYSTEM_NAME`, compilers, sysroot).

23. How do you control output directories for binaries/libs?
   - Follow-up: Multi-config vs single-config generator behavior.

24. What is an imported target?
   - Follow-up: Why imported targets reduce include/link mistakes?

25. What is an interface library in CMake?
   - Follow-up: Embedded use case for header-only/config-only target.

26. How do you avoid global state in CMake projects?
   - Follow-up: Why avoid `include_directories()` and `link_libraries()` globally?

27. How do you diagnose CMake configure/generate issues?
   - Follow-up: `--trace`, `--debug-find`, `message(STATUS ...)`, cache cleanup strategy.

28. What is the difference between configure-time and build-time logic?
   - Follow-up: When should you use `configure_file()`?

29. How do you handle platform/compiler-specific flags cleanly?
   - Follow-up: `target_compile_options()` + generator expressions.

30. How do you make CMake builds reproducible and maintainable?
   - Follow-up: Version pinning, presets, lock step toolchain in CI.

---

### Expert / Deep-Dive Counter Questions

1. Why is `target_link_libraries(A PUBLIC B)` not same as linking only inside `A`?
2. Why can `find_package()` succeed but link still fail?
3. Why do relative paths in include dirs break install/export packages?
4. What is wrong with forcing flags via `CMAKE_C_FLAGS` globally?
5. How do you debug transitive dependency conflicts in CMake?
6. How do you structure mono-repo CMake with many modules cleanly?
7. How do you support both host tests and cross-compiled firmware in one tree?
8. Why does `add_custom_command()` sometimes not re-run when expected?
9. How do you expose compile options safely to downstream consumers?
10. How do CMake Presets improve developer workflow consistency?

---

## MAKE INTERVIEW QUESTIONS

### Basic

1. What is Make and why is it used?
2. What is a Makefile?
3. What is a target, dependency (prerequisite), and recipe?
4. What is the default target in a Makefile?
5. What is the role of tabs in recipe lines?
6. What is the difference between `=` and `:=` in Make variables?
7. What are automatic variables: `$@`, `$<`, `$^`?
8. What is `.PHONY` and why is it needed?
9. How does Make decide whether to rebuild a target?
10. Difference between static library and shared library from build perspective?

---

### Intermediate

11. What is pattern rule? Example with `%.o: %.c`.
12. What are suffix rules vs pattern rules?
13. What is implicit rule in Make?
14. How do you pass compiler/linker flags in Makefiles (`CFLAGS`, `CPPFLAGS`, `LDFLAGS`, `LDLIBS`)?
15. What is the difference between recursive and simple variable expansion?
16. What is `make -j` and how does parallel build work?
17. What issues appear in parallel builds?
18. How do order-only prerequisites (`|`) work?
19. How do you include other makefiles (`include`)?
20. How do you write conditional logic in Make (`ifeq`, `ifneq`)?

---

### Advanced

21. What is recursive Make and what are its pitfalls?
22. How do you structure a non-recursive Make build for large projects?
23. How do you generate dependencies automatically (`-MMD -MP`)?
24. How do you avoid unnecessary rebuilds?
25. What is the difference between `?=`, `+=`, `override`?
26. How do you debug Make execution (`make -n`, `make -d`, `--trace`)?
27. How do you write portable Makefiles across Linux/macOS/Windows environments?
28. How do you handle cross-compilation in Make?
29. How do you safely use shell commands/functions in Make (`$(shell ...)`)?
30. How do you package/install artifacts via Make targets (`install`, `clean`, `distclean`)?

---

### Expert / Deep-Dive Counter Questions

1. Why can timestamp-based rebuild logic fail on network filesystems?
2. Why do missing header dependencies cause random CI failures?
3. Why can recursive Make break dependency correctness?
4. How do you prevent race conditions in parallel recipes?
5. What is the difference between Make functions and shell commands in evaluation timing?
6. How do you enforce reproducible builds in Make?
7. Why is `.PHONY` critical for targets like `clean`?
8. Why does using wildcards naively cause nondeterministic builds?
9. How do you combine generated sources with proper dependency ordering?
10. How do you benchmark and optimize Make build performance?

---

## CMAKE vs MAKE (Interview Comparison Questions)

1. CMake vs Make: conceptual difference.
2. Is CMake a replacement for Make?
3. Which one is better for cross-platform projects and why?
4. Which one is better for small low-level builds and why?
5. How does dependency handling differ in CMake-generated Makefiles vs handwritten Makefiles?
6. How do both handle external dependencies?
7. Which tool scales better for large multi-module codebases?
8. Which gives finer low-level control?
9. How do debugging and traceability compare?
10. When should a team choose:
   - pure Make
   - CMake + Ninja
   - CMake + Makefiles

---

## Practical Coding / Task Questions

1. Write a minimal `CMakeLists.txt` to build:
   - one static library
   - one executable linked to it.
2. Add platform-specific compile definitions in CMake.
3. Create a Makefile compiling all `.c` into `.o` and linking final binary.
4. Add auto dependency generation in Make (`-MMD -MP`).
5. Add `Debug` and `Release` options in CMake.
6. Add `clean`, `run`, and `test` targets in Make.
7. Use `find_package()` for an external library and link imported target.
8. Write a toolchain file for ARM cross-compilation in CMake.
9. Build same project with:
   - CMake + Ninja
   - CMake + Unix Makefiles
   and compare outputs.
10. Convert a legacy Makefile project into modern target-based CMake.

---

*This set is complementary to your C interview docs and focuses only on build-system interview depth (CMake + Make).*