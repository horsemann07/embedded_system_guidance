# Debugging Interview Questions
> C Language + Embedded Systems + RTOS  
> M.Tech Graduate + 10 Years Experience  
> Basic → Intermediate → Advanced → Expert → War Stories

---

## BASIC LEVEL

### Fundamentals of Debugging

1. What is debugging?
   - Follow-up: Difference between debugging and testing?
   - Follow-up: Difference between debugging and profiling?
   - Follow-up: What is a bug vs defect vs fault vs failure?

2. What are the types of bugs?
   - Syntax bug
   - Logical bug
   - Runtime bug
   - Memory bug
   - Timing bug
   - Concurrency bug
   - Follow-up: Which type is hardest to debug? Why?
   - Follow-up: Which type does compiler catch? Which does it miss?

3. What is the first step when debugging an unknown crash?
   - Follow-up: Reproduce → Isolate → Identify → Fix → Verify.
   - Follow-up: What if you cannot reproduce the bug?

4. What is a breakpoint?
   - Follow-up: Hardware breakpoint vs software breakpoint?
   - Follow-up: How many hardware breakpoints does ARM Cortex-M4 support? (typically 6)
   - Follow-up: What is a watchpoint (data breakpoint)?

5. What is single stepping?
   - Follow-up: Step over vs step into vs step out?
   - Follow-up: Why can single stepping change timing-related bug behavior?

6. What is a watchdog timer?
   - Follow-up: How does it help detect system hangs?
   - Follow-up: What is window watchdog vs basic watchdog?
   - Follow-up: What should you do in watchdog reset handler?

7. What is printf debugging?
   - Follow-up: Advantages and disadvantages?
   - Follow-up: When is printf debugging NOT acceptable in embedded?
   - Follow-up: What is semihosting?
   - Follow-up: What is ITM (Instrumentation Trace Macrocell) and why is it better than UART printf?

---

### Basic Tools

8. What is GDB?
   - Follow-up: Basic GDB commands:
   ```bash
   gdb ./program          # start
   run                    # run
   break main             # breakpoint at function
   break file.c:42        # breakpoint at line
   next                   # step over
   step                   # step into
   finish                 # step out
   print x                # print variable
   info locals            # all local variables
   backtrace              # call stack
   continue               # continue execution
   watch x                # watchpoint on variable
   info breakpoints       # list breakpoints
   delete 1               # delete breakpoint 1
   quit                   # exit
   ```

9. What is a core dump?
   - Follow-up: How to enable core dump on Linux?
   ```bash
   ulimit -c unlimited
   ./program        # crashes → generates core
   gdb program core # analyze
   ```
   - Follow-up: What information is in a core dump?
   - Follow-up: How to read backtrace from core dump?

10. What is `valgrind`?
    - Follow-up: What errors does it detect?
    - Follow-up: Command: `valgrind --leak-check=full ./program`
    - Follow-up: Performance overhead of valgrind? (~10-50x slower)

---

## INTERMEDIATE LEVEL

### C Debugging

1. How do you debug a segmentation fault?
   - Step 1: Compile with `-g` flag.
   - Step 2: Run under GDB or get core dump.
   - Step 3: `backtrace` to find crash location.
   - Step 4: `print` variables to find bad pointer.
   - Follow-up: Common causes:
     - NULL pointer dereference
     - Dangling pointer
     - Buffer overflow
     - Stack overflow
     - Writing to read-only memory (string literal)

2. How do you debug a memory leak in C?
   - Follow-up: Valgrind `--leak-check=full`
   - Follow-up: AddressSanitizer (ASan)
   - Follow-up: Manual tracking: wrapper around malloc/free with logging
   - Follow-up: What is LSAN (LeakSanitizer)?

3. What is AddressSanitizer (ASan)?
   ```bash
   gcc -fsanitize=address -g program.c -o program
   ./program
   ```
   - Follow-up: What bugs does ASan detect?
     - Heap buffer overflow
     - Stack buffer overflow
     - Use after free
     - Use after return
     - Double free
     - Memory leak (with LSAN)
   - Follow-up: Performance overhead? (~2x slower)
   - Follow-up: ASan vs Valgrind — when to use which?

4. What is UndefinedBehaviorSanitizer (UBSan)?
   ```bash
   gcc -fsanitize=undefined -g program.c -o program
   ```
   - Follow-up: What does UBSan detect?
     - Signed integer overflow
     - Null pointer dereference
     - Misaligned access
     - Out-of-bounds array access
     - Invalid enum value
   - Follow-up: Can you combine ASan + UBSan?
   ```bash
   gcc -fsanitize=address,undefined -g program.c -o program
   ```

5. What is ThreadSanitizer (TSan)?
   ```bash
   gcc -fsanitize=thread -g program.c -o program
   ```
   - Follow-up: What does TSan detect?
   - Follow-up: Why can't ASan and TSan be used together?

6. How do you debug infinite loop?
   - Follow-up: Attach GDB to running process: `gdb -p <pid>`
   - Follow-up: `Ctrl+C` in GDB to pause, then `backtrace`.
   - Follow-up: How to detect infinite loop without debugger in embedded?

7. How do you debug stack corruption?
   - Follow-up: Stack canary (`-fstack-protector-strong`).
   - Follow-up: What is the canary value? How is it checked?
   - Follow-up: Manual fill pattern approach.
   - Follow-up: How does compiler detect smashed stack canary?

8. How do you debug uninitialized variable bugs?
   - Follow-up: `-Wuninitialized` compiler warning.
   - Follow-up: MemorySanitizer (MSan):
   ```bash
   clang -fsanitize=memory -g program.c -o program
   ```
   - Follow-up: Valgrind memcheck — uninitialized value use.

9. How do you debug format string bugs?
   - Follow-up: `-Wformat` compiler warning.
   - Follow-up: `-Wformat-security` for dangerous format strings.

10. How do you use `objdump` for debugging?
    ```bash
    objdump -d program          # disassemble
    objdump -S program          # interleave source and assembly
    objdump -t program          # symbol table
    objdump -x program          # all headers
    ```

11. How do you use `nm` for debugging?
    ```bash
    nm program                  # list symbols
    nm -u program               # undefined symbols (unresolved)
    nm -C program               # demangle C++ names
    ```

12. How do you use `readelf`?
    ```bash
    readelf -h program          # ELF header
    readelf -S program          # sections
    readelf -s program          # symbol table
    readelf -l program          # segments (program headers)
    readelf -d program          # dynamic section
    ```

13. How do you use `strace`?
    ```bash
    strace ./program            # trace system calls
    strace -e open,read ./prog  # filter specific syscalls
    strace -p <pid>             # attach to running process
    ```

14. How do you use `ltrace`?
    ```bash
    ltrace ./program            # trace library calls
    ```

15. What is `addr2line`?
    ```bash
    addr2line -e program 0x08001234   # convert address to file:line
    ```
    - Follow-up: How to use crash address from embedded fault handler?

---

### Embedded Debugging

16. What debugging interfaces are available in embedded systems?
    - JTAG (Joint Test Action Group)
    - SWD (Serial Wire Debug — ARM specific, 2-pin)
    - UART (printf debugging)
    - ITM / SWO (Serial Wire Output)
    - ETM (Embedded Trace Macrocell — instruction trace)
    - Follow-up: JTAG vs SWD — differences and use cases?
    - Follow-up: How many pins does JTAG use vs SWD?

17. What is OpenOCD?
    - Follow-up: OpenOCD + GDB workflow:
    ```bash
    openocd -f interface/stlink.cfg -f target/stm32f4x.cfg
    # in another terminal:
    arm-none-eabi-gdb firmware.elf
    (gdb) target remote :3333
    (gdb) monitor reset halt
    (gdb) load
    (gdb) continue
    ```

18. What is SEGGER J-Link and J-Trace?
    - Follow-up: J-Link vs ST-Link vs CMSIS-DAP?
    - Follow-up: What is J-Trace used for? (instruction trace via ETM)

19. What is ITM (Instrumentation Trace Macrocell)?
    - Follow-up: How does `ITM_SendChar()` work for printf via SWO?
    - Follow-up: Why is ITM better than UART debugging?
      - No UART peripheral needed
      - Non-blocking, timestamped
      - Can be decoded by debugger
    - Follow-up: What is SWO (Serial Wire Output) pin?

20. How do you debug without a debugger in embedded?
    - GPIO toggle + oscilloscope/logic analyzer
    - LED blink pattern (error codes)
    - UART printf
    - Error code stored in non-volatile memory / backup registers
    - Watchdog reset counter in RTC backup register
    - Follow-up: What is the limitation of UART printf debugging in ISR?

21. What is a logic analyzer? When do you use it?
    - Follow-up: Logic analyzer vs oscilloscope — when to use each?
    - Follow-up: Protocol decoding: SPI, I2C, UART, CAN on logic analyzer.

22. What is a boundary scan (JTAG IEEE 1149.1)?
    - Follow-up: How is it used for PCB-level debugging?

23. What is ELF file and why do you need it for debugging?
    - Follow-up: `.elf` vs `.bin` vs `.hex` — what is in each?
    - Follow-up: Why is debug info (`-g`) stripped in production firmware?

24. How do you debug a hard fault on ARM Cortex-M?
    - Follow-up: Hard fault registers:
    ```c
    SCB->HFSR   // Hard Fault Status Register
    SCB->CFSR   // Configurable Fault Status Register
    SCB->MMFAR  // MemManage Fault Address Register
    SCB->BFAR   // Bus Fault Address Register
    ```
    - Follow-up: Decode stacked PC from fault handler:
    ```c
    void HardFault_Handler(void) {
        uint32_t *sp = (uint32_t*)__get_MSP();
        uint32_t pc  = sp[6];  // stacked PC
        uint32_t lr  = sp[5];  // stacked LR
        // log and reset
    }
    ```
    - Follow-up: What is BFAR and MMFAR? When are they valid?

25. What are the types of faults on ARM Cortex-M?
    - Hard Fault (escalated or direct)
    - MemManage Fault (MPU violation, null pointer)
    - Bus Fault (invalid memory access, unaligned)
    - Usage Fault (undefined instruction, divide by zero if enabled)
    - Follow-up: How to enable divide-by-zero and unaligned access trapping?
    ```c
    SCB->CCR |= SCB_CCR_DIV_0_TRP_Msk;
    SCB->CCR |= SCB_CCR_UNALIGN_TRP_Msk;
    ```

26. How do you debug stack overflow in embedded?
    - Follow-up: Fill stack with known pattern (0xDEADBEEF) at startup.
    - Follow-up: Check pattern integrity periodically or in fault handler.
    - Follow-up: MPU stack guard region — generates MemManage fault on overflow.
    - Follow-up: Linker symbol approach to check stack boundaries.

27. How do you debug a watchdog reset?
    - Follow-up: Save reset cause in backup register before reset.
    - Follow-up: Log last known good state to EEPROM/Flash.
    - Follow-up: Add task profiling to identify which task caused hang.

28. What is semi-hosting in ARM embedded?
    - Follow-up: How does it work? (SVC call intercepted by debugger)
    - Follow-up: Why is it not suitable for production?
    - Follow-up: Alternatives: ITM, UART, RTT.

29. What is SEGGER RTT (Real-Time Transfer)?
    - Follow-up: How does RTT work? (shared ring buffer in RAM, debugger reads via JTAG)
    - Follow-up: Why is RTT better than semihosting and UART?
      - No UART peripheral needed
      - Non-blocking (no CPU stall)
      - Works without debug connection for post-mortem
    - Follow-up: RTT Viewer vs SystemView.

---

## ADVANCED LEVEL

### Advanced C Debugging

1. How do you debug optimized code (`-O2`, `-O3`)?
   - Follow-up: Variables may be in registers, not on stack.
   - Follow-up: Inlined functions — no symbol in backtrace.
   - Follow-up: Reordered instructions — source line mapping unreliable.
   - Follow-up: How to force no optimization for specific function:
   ```c
   __attribute__((optimize("O0"))) void critical_debug_func(void) { }
   ```

2. How do you debug intermittent / non-reproducible bugs?
   - Follow-up: Enable all sanitizers in debug build.
   - Follow-up: Add assertions at boundaries.
   - Follow-up: Use circular log buffer — always-on lightweight logging.
   - Follow-up: Stress test: run millions of iterations.
   - Follow-up: What is the Heisenbug? (bug disappears when observed)

3. What is a Heisenbug?
   - Follow-up: Common causes in embedded:
     - Timing changed by debug output
     - Optimizer removes "unnecessary" code with side effects
     - Debugger changes memory layout (padding)
   - Follow-up: How to debug Heisenbug without disturbing it?

4. How do you debug strict aliasing violations?
   ```bash
   gcc -fno-strict-aliasing  # disable for debugging
   gcc -Wstrict-aliasing=2   # warn about violations
   ```
   - Follow-up: Why does UB from aliasing only manifest at `-O2`?

5. How do you debug linker issues?
   ```bash
   ld -Map output.map        # generate map file
   arm-none-eabi-size firmware.elf  # section sizes
   ```
   - Follow-up: How to find what is consuming flash/RAM?
   - Follow-up: How to use map file to find symbol location?
   - Follow-up: Linker garbage collection: `--gc-sections` + `-ffunction-sections -fdata-sections`.

6. How do you debug startup code issues?
   - Follow-up: Issues: `.data` copy from flash to RAM not done.
   - Follow-up: `.bss` not zeroed.
   - Follow-up: Static constructors not called.
   - Follow-up: How to verify startup with memory view in debugger?

7. What is post-mortem debugging?
   - Follow-up: Crash log saved to non-volatile storage.
   - Follow-up: Coredump saved to external Flash/EEPROM.
   - Follow-up: How to reconstruct backtrace from saved stack?

8. How do you use GDB scripting for automated debugging?
   ```python
   # GDB Python script
   import gdb
   gdb.execute("break main")
   gdb.execute("run")
   frame = gdb.selected_frame()
   print(frame.read_var("my_variable"))
   ```

---

### Embedded Advanced Debugging

9. What is instruction trace? What is ETM?
   - Follow-up: ETM records every instruction executed.
   - Follow-up: How does J-Trace capture ETM trace?
   - Follow-up: Use case: find exact instruction that caused fault.
   - Follow-up: Difference between ETM and MTB (Micro Trace Buffer).

10. What is MTB (Micro Trace Buffer)?
    - Follow-up: Available on Cortex-M0+, M23, M33.
    - Follow-up: Small on-chip SRAM used as circular trace buffer.
    - Follow-up: How to read MTB buffer after crash.

11. How do you debug DMA issues?
    - Follow-up: Cache coherency — data written by DMA not visible to CPU.
    - Follow-up: Cache invalidation before DMA read, cache clean before DMA write.
    - Follow-up: Buffer alignment requirements.
    - Follow-up: DMA transfer complete but data corrupt — what to check?

12. How do you debug clock/timing issues?
    - Follow-up: Oscilloscope on GPIO toggle to measure task period.
    - Follow-up: Logic analyzer to verify SPI/I2C timing.
    - Follow-up: How to verify interrupt latency?

13. How do you debug flash write/erase issues?
    - Follow-up: Verify write enable, erase before write, verify after write.
    - Follow-up: What is write protection issue?
    - Follow-up: How to debug CRC mismatch after firmware update?

14. How do you debug power issues?
    - Follow-up: Current measurement with bench power supply ammeter.
    - Follow-up: Power profiler kit (Nordic PPK2, Otii Arc).
    - Follow-up: How to find unexpected current draw in sleep mode?

15. How do you debug communication protocol issues?
    - I2C: Check ACK/NACK, clock stretching, address conflicts.
    - SPI: Check CPOL/CPHA, CS timing, bit order.
    - UART: Check baud rate, parity, stop bits.
    - CAN: Check bit timing, termination, error frames.
    - Follow-up: How to use logic analyzer protocol decoder?

---

### RTOS Debugging

16. What are common RTOS bugs?
    - Stack overflow
    - Priority inversion
    - Deadlock
    - Race condition (ISR vs task shared data)
    - Missing `FromISR` API
    - Calling blocking API from ISR
    - Timer callback too long
    - Memory leak in heap (deleted tasks not cleaned up)
    - Follow-up: Which is hardest to debug?

17. How do you debug stack overflow in FreeRTOS?
    - Follow-up: `configCHECK_FOR_STACK_OVERFLOW 2` — fill pattern method.
    - Follow-up: `vApplicationStackOverflowHook()` callback.
    - Follow-up: `uxTaskGetStackHighWaterMark()` for each task.
    - Follow-up: Systematic approach: double all task stacks, reduce until stable.

18. How do you debug deadlock in RTOS?
    - Follow-up: All tasks in Blocked state — nothing in Ready.
    - Follow-up: `vTaskList()` to see all task states.
    - Follow-up: Identify which resource each task is waiting for.
    - Follow-up: Draw resource allocation graph — find cycle.

19. How do you debug priority inversion in RTOS?
    - Follow-up: High priority task permanently blocked.
    - Follow-up: Use RTOS trace to see task switches.
    - Follow-up: SystemView timeline visualization.

20. How do you debug race condition between ISR and task?
    - Follow-up: Missing `volatile` on shared variable.
    - Follow-up: Non-atomic read-modify-write on shared variable.
    - Follow-up: Missing critical section.
    - Follow-up: How to reproduce: inject artificial delay in critical section.

21. What is SEGGER SystemView and how does it help?
    - Follow-up: Records: task switches, ISR entry/exit, semaphore take/give, queue send/receive.
    - Follow-up: Visualizes timeline of all RTOS events.
    - Follow-up: How does it timestamp? (DWT cycle counter)
    - Follow-up: Overhead of SystemView? (~0.5µs per event)

22. What is Percepio Tracealyzer?
    - Follow-up: Snapshot mode vs streaming mode.
    - Follow-up: What additional analysis does it provide vs SystemView?

23. How do you debug heap fragmentation in FreeRTOS?
    - Follow-up: `xPortGetFreeHeapSize()` vs `xPortGetMinimumEverFreeHeapSize()`.
    - Follow-up: Add malloc failed hook:
    ```c
    void vApplicationMallocFailedHook(void) {
        // log free heap size
        // log task that caused failure
    }
    ```

24. How do you debug incorrect task timing in RTOS?
    - Follow-up: Use `vTaskDelayUntil()` instead of `vTaskDelay()`.
    - Follow-up: GPIO toggle + oscilloscope to measure actual period.
    - Follow-up: DWT cycle counter for microsecond timing measurement.

25. How do you add non-intrusive logging to RTOS?
    - Follow-up: Circular buffer in RAM — log to buffer, dump via UART when idle.
    - Follow-up: SEGGER RTT — zero-copy ring buffer read by debugger.
    - Follow-up: ITM trace channel output.

---

## EXPERT LEVEL

### Expert C Debugging

1. How do you debug miscompilation?
   - Follow-up: Suspect compiler bug when bug only appears at `-O2`/`-O3`.
   - Follow-up: Bisect compiler versions.
   - Follow-up: Inspect generated assembly.
   - Follow-up: Report to compiler team with minimal reproducer.

2. How do you debug ABI mismatch issues?
   - Follow-up: Mixing compiled objects with different ABI settings.
   - Follow-up: Soft-float vs hard-float ABI on ARM.
   - Follow-up: How to check ABI of an object file?
   ```bash
   arm-none-eabi-readelf -A firmware.elf
   ```

3. How do you find and debug subtle undefined behavior?
   - Follow-up: UBSan catches many at runtime.
   - Follow-up: Static analysis tools: Coverity, PC-lint, cppcheck, Clang static analyzer.
   - Follow-up: `__builtin_expect` and how it interacts with UB elimination.

4. What is delta debugging?
   - Follow-up: Systematically reducing test case to minimal reproducer.
   - Follow-up: Binary search over commits to find regression (git bisect).

5. What is `git bisect`?
   ```bash
   git bisect start
   git bisect bad HEAD
   git bisect good v1.0
   git bisect run ./test.sh  # automated
   ```
   - Follow-up: How to bisect a flaky/intermittent bug?

6. How do you debug position independent code issues?
   - Follow-up: GOT/PLT corruption.
   - Follow-up: Relocations in embedded firmware.

7. How do you debug flash memory corruption?
   - Follow-up: CRC check over flash regions periodically.
   - Follow-up: Write protection bits.
   - Follow-up: Flash endurance — unexpected erase cycle wear.

---

### Expert Embedded Debugging

8. How do you recover from a completely unresponsive embedded target?
   - Follow-up: JTAG connect and halt: `monitor reset halt`.
   - Follow-up: Read PC and registers to find where it's stuck.
   - Follow-up: If JTAG also fails — check power, clock, reset line with oscilloscope.

9. How do you debug a boot failure (blank screen, no UART output)?
   - Follow-up: Systematic checklist:
     1. Power supply voltage correct?
     2. Clock source stable? (crystal, PLL lock)
     3. Correct boot mode pins?
     4. Startup code running? (toggle LED in very first line)
     5. `.data` initialized, `.bss` zeroed?
     6. Stack pointer correct?
     7. Interrupt vector table at correct address?

10. How do you debug firmware update (OTA/DFU) failures?
    - Follow-up: CRC mismatch — verify transfer integrity.
    - Follow-up: Flash write error — verify erase before write.
    - Follow-up: Wrong jump address after update.
    - Follow-up: Bootloader stack and application stack conflict.

11. How do you debug EMI/EMC related embedded issues?
    - Follow-up: Intermittent resets caused by external interference.
    - Follow-up: Power supply decoupling.
    - Follow-up: GPIO input filtering.
    - Follow-up: How to distinguish EMI bug from software bug?

12. How do you do remote debugging in production embedded systems?
    - Follow-up: UART command shell for runtime inspection.
    - Follow-up: Diagnostic mode triggered by GPIO or special command.
    - Follow-up: Crash log uplink over communication interface.
    - Follow-up: What is a "black box" approach in embedded?

---

### Expert RTOS Debugging

13. How do you debug a task that intermittently misses its deadline?
    - Follow-up: Trace with SystemView to see what preempts it.
    - Follow-up: Measure worst-case execution time.
    - Follow-up: Check for shared resource contention (mutex wait time).

14. How do you debug ISR that causes system instability?
    - Follow-up: ISR too long — measure with GPIO toggle + oscilloscope.
    - Follow-up: ISR calling non-ISR-safe RTOS API.
    - Follow-up: ISR stack overflow.
    - Follow-up: Interrupt storm — peripheral generating too many interrupts.

15. How do you debug memory leak in long-running RTOS system?
    - Follow-up: Log `xPortGetFreeHeapSize()` periodically.
    - Follow-up: Log heap size before and after each allocation.
    - Follow-up: Custom malloc/free wrappers with caller tracking.

16. How do you debug multicore RTOS issues?
    - Follow-up: Cache coherency — CPU0 writes, CPU1 reads stale cache line.
    - Follow-up: Missing memory barrier between cores.
    - Follow-up: Spinlock fairness — one core never acquires lock.

---

## DEBUGGING STRATEGY FRAMEWORK

### Systematic Debugging Process

```
1. REPRODUCE
   └─ Find minimal, consistent reproduction steps
   └─ If intermittent: add logging, stress test

2. ISOLATE
   └─ Binary search: which subsystem?
   └─ Simplify: remove code until bug disappears
   └─ git bisect: which commit introduced it?

3. IDENTIFY ROOT CAUSE
   └─ Use tools: GDB, sanitizers, static analysis
   └─ Read all relevant code, not just crash site
   └─ Question all assumptions

4. FIX
   └─ Fix root cause, not symptom
   └─ Understand why fix works

5. VERIFY
   └─ Reproduce original scenario → fixed
   └─ Run full regression
   └─ Check for similar bugs elsewhere

6. PREVENT RECURRENCE
   └─ Add assertion / test for this case
   └─ Update documentation / coding guidelines
```

---

## TOOLS QUICK REFERENCE TABLE

| Tool | Purpose | Platform |
|------|---------|----------|
| GDB | Debugger | Linux/Embedded |
| OpenOCD | JTAG/SWD server | Embedded |
| J-Link GDB Server | JTAG/SWD server | Embedded |
| Valgrind | Memory errors, leak | Linux |
| ASan | Memory errors | Linux/Embedded |
| UBSan | Undefined behavior | Linux |
| TSan | Race conditions | Linux |
| MSan | Uninitialized reads | Linux |
| SEGGER SystemView | RTOS trace | Embedded RTOS |
| Percepio Tracealyzer | RTOS trace | Embedded RTOS |
| SEGGER RTT | Non-intrusive logging | Embedded |
| ITM/SWO | Printf trace | ARM Cortex-M |
| J-Trace | Instruction trace (ETM) | ARM |
| Coverity | Static analysis | C/C++ |
| PC-lint | Static analysis | C/C++ |
| Cppcheck | Static analysis | C/C++ |
| Clang-tidy | Linting | C/C++ |
| objdump | Binary analysis | All |
| nm | Symbol inspection | All |
| readelf | ELF inspection | Linux/Embedded |
| addr2line | Address → source | All |
| strace | Syscall trace | Linux |
| ltrace | Library trace | Linux |
| Logic analyzer | Signal debug | Embedded HW |
| Oscilloscope | Timing/signal | Embedded HW |

---

## TRICK / DEEP COUNTER QUESTIONS

1. "You said ASan catches buffer overflow — can it catch overflow of a global array?"
   - **Answer:** Yes — ASan instruments global, stack, and heap buffers.

2. "You said printf debugging is bad in ISR — why exactly?"
   - **Answer:** printf uses locks (not ISR-safe), may block, changes timing.

3. "You said hardware breakpoints don't affect timing — is this always true?"
   - **Answer:** Not always — some peripherals sensitive to halted CPU (watchdog, communication timeouts).

4. "You said `volatile` fixes race condition — is that true?"
   - **Answer:** No — `volatile` only prevents compiler optimization; does not provide atomicity or memory ordering.

5. "You said watchdog detects hangs — what if the watchdog kick itself is inside the hang?"
   - **Answer:** Task-based watchdog monitoring — each task has its own token that it must refresh.

6. "You said stack overflow detection method 2 (fill pattern) is reliable — what can fool it?"
   - **Answer:** A single large stack frame that jumps over the sentinel region.

7. "How do you debug a bug that only happens on customer hardware, not your dev board?"
   - **Answer:** Hardware differences (clock speed, memory, PCB noise), enable logging, ship debug build.

8. "You said `addr2line` converts crash address to source line — what if binary is stripped?"
   - **Answer:** Keep unstripped ELF with debug symbols separate; strip only production binary.

9. "You said RTT is non-blocking — what happens when RTT buffer is full?"
   - **Answer:** Configurable: block, skip, or overwrite oldest data.

10. "You said SystemView overhead is ~0.5µs — is this acceptable for a 10µs ISR?"
    - **Answer:** No — 5% overhead, may mask timing bugs. Disable trace for tight ISRs.

---

## SUMMARY TABLE

| Debugging Area | Basic | Intermediate | Advanced | Expert |
|----------------|-------|-------------|----------|--------|
| GDB usage | ★★★★★ | ★★★★★ | ★★★★☆ | ★★★☆☆ |
| Sanitizers (ASan/UBSan) | ★★★☆☆ | ★★★★★ | ★★★★★ | ★★★★☆ |
| Core dump analysis | ★★★☆☆ | ★★★★☆ | ★★★★★ | ★★★★★ |
| Embedded JTAG/SWD | ★★★☆☆ | ★★★★★ | ★★★★★ | ★★★★★ |
| Hard fault analysis | ★★☆☆☆ | ★★★★☆ | ★★★★★ | ★★★★★ |
| RTOS trace (SystemView) | ★★☆☆☆ | ★★★★☆ | ★★★★★ | ★★★★★ |
| Instruction trace (ETM) | ★☆☆☆☆ | ★★☆☆☆ | ★★★★☆ | ★★★★★ |
| Static analysis | ★★★☆☆ | ★★★★☆ | ★★★★★ | ★★★★★ |
| Post-mortem debug | ★★☆☆☆ | ★★★☆☆ | ★★★★★ | ★★★★★ |
| Optimized code debug | ★★☆☆☆ | ★★★☆☆ | ★★★★★ | ★★★★★ |
| Multicore debug | ★☆☆☆☆ | ★★☆☆☆ | ★★★★☆ | ★★★★★ |
| Intermittent bug debug | ★★☆☆☆ | ★★★☆☆ | ★★★★★ | ★★★★★ |

---

*This document covers debugging from fundamentals to expert level across C, Embedded Systems, and RTOS.*