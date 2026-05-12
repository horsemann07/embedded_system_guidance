# Microprocessor + Linux Embedded Interview Questions
> ARM Cortex-A, x86, RISC-V Processors  
> Linux Kernel, Device Drivers, Embedded Linux  
> M.Tech Graduate + 10 Years Experience  
> Basic → Intermediate → Advanced → Expert → War Stories

---

## MICROPROCESSOR FUNDAMENTALS

### Basic Level

1. What is a microprocessor vs microcontroller?
   - Follow-up: MCU: integrated memory, peripherals, limited performance.
   - Follow-up: MPU: separate memory, high performance, requires external support.
   - Follow-up: Examples: MCU = STM32, Arduino; MPU = Raspberry Pi, BeagleBone, Jetson.

2. What is instruction set architecture (ISA)?
   - Follow-up: RISC vs CISC — explain with examples.
   - Follow-up: ARM (RISC) vs x86 (CISC) — comparison.
   - Follow-up: What is RISC-V and why is it gaining attention?

3. What are the major processor architectures used in embedded?
   - ARM Cortex-A (Linux, Android) — 32-bit and 64-bit
   - ARM Cortex-R (real-time, safety-critical)
   - ARM Cortex-M (RTOS, embedded MCU)
   - x86 / x86-64 (Industrial, servers)
   - RISC-V (Emerging, open-source ISA)
   - MIPS (Legacy, IoT)
   - PowerPC (Automotive, industrial)
   - Follow-up: Which is most common in IoT?
   - Follow-up: Which is most common in automotive?

4. What is the ARM processor family tree?
   ```
   ARMv7 (32-bit)
   ├── Cortex-A7, A9, A15
   ├── Cortex-R
   └── Cortex-M0/M3/M4
   
   ARMv8 (64-bit)
   ├── Cortex-A53, A57, A72, A76, A77 (64-bit)
   ├── Cortex-A32 (32-bit)
   └── Cortex-M
   ```
   - Follow-up: What is ARMv9? What are new features?

5. What is ARM Trustzone?
   - Follow-up: Secure vs Non-Secure world.
   - Follow-up: How does Trustzone protect sensitive data?
   - Follow-up: What is TEE (Trusted Execution Environment)?

---

### ARM Cortex-A (Linux Processor)

6. What are key features of ARM Cortex-A processors?
   - Multi-core (typically 2–8 cores)
   - Cache hierarchy (L1, L2, L3)
   - MMU (Memory Management Unit) for virtual memory
   - Out-of-order execution (A7, A9, A15, A72+)
   - SIMD units (NEON on ARMv7, SVE on ARMv8)
   - Floating-point unit (optional on ARMv7, standard on ARMv8)
   - Follow-up: What is NEON? What is SVE?
   - Follow-up: Power consumption vs Cortex-M?

7. What is Cortex-A7 vs A9 vs A15 vs A72?
   - A7: In-order, simple, power-efficient, 1GHz typical
   - A9: Out-of-order, better performance, 1.5GHz typical
   - A15: High-performance, 2GHz+, server-class
   - A72: Modern, 64-bit, 2–3GHz, used in Raspberry Pi 3, 4
   - Follow-up: Why A7 (Cortex-A7) in Cortex-A17 (heterogeneous) instead of A9?

8. What is big.LITTLE (heterogeneous multicore)?
   - Follow-up: How does the kernel schedule between big and LITTLE cores?
   - Follow-up: Power vs performance tradeoff.
   - Follow-up: Examples: Exynos 5 Octa (4×A15 + 4×A7), MediaTek chips.

---

## LINUX EMBEDDED BASICS

### What is Linux?

9. What is Linux?
   - Follow-up: Kernel vs distribution (Debian, Ubuntu, Yocto, Buildroot).
   - Follow-up: GPL v2 license — what does it mean for embedded?
   - Follow-up: What distributions are used in embedded? (Yocto, OpenEmbedded, Buildroot, Alpine)

10. What is Linux kernel?
    - Follow-up: Monolithic vs modular (Linux is modular).
    - Follow-up: Core subsystems: process management, memory management, filesystem, networking, device drivers.

11. Difference between Linux, Android, and Embedded Linux?
    - Follow-up: Android is Linux-based + Java VM + custom frameworks.
    - Follow-up: Embedded Linux is minimal kernel + custom rootfs.
    - Follow-up: What is the relationship between kernel version and distro?

---

### Boot Process

12. What happens when you power on an embedded Linux system?
    - Step 1: Bootloader (U-Boot, Barebox) — initializes hardware, loads kernel.
    - Step 2: Kernel boot — mounts rootfs, starts init.
    - Step 3: Init system (systemd, OpenRC, BusyBox init) — starts services.
    - Step 4: User space — shell, applications.
    - Follow-up: What is the bootloader's job exactly?
    - Follow-up: What is the kernel command line and how is it passed?

13. What is U-Boot?
    - Follow-up: U-Boot phases: SPL (Secondary Program Loader), full U-Boot.
    - Follow-up: Why does embedded Linux need bootloader but MCU-RTOS does not?
    - Follow-up: What is environment variables in U-Boot?

14. What is the Linux kernel command line?
    - Follow-up: Example: `console=ttyS0,115200 root=/dev/mmcblk0p2 rootfstype=ext4 init=/sbin/init`
    - Follow-up: How to pass kernel command line (bootloader, device tree)?

15. What is rootfs (root filesystem)?
    - Follow-up: Mounted as `/` in Linux.
    - Follow-up: What must rootfs contain? (`/bin`, `/sbin`, `/lib`, `/etc`, `/dev`, `/proc`, `/sys`)
    - Follow-up: What is busybox? (unified binary for all basic utilities)

16. What is initramfs?
    - Follow-up: Temporary root filesystem loaded into RAM by bootloader.
    - Follow-up: Used for driver loading, filesystem mounting before switching to real rootfs.
    - Follow-up: How does initramfs change the boot process?

---

### Processes & Scheduling

17. What is a process in Linux?
    - Follow-up: Process vs thread — what is the difference in Linux?
    - Follow-up: PID (Process ID), PPID (Parent PID).
    - Follow-up: Process states: Running, Runnable, Blocked, Stopped, Zombie.

18. What is a thread in Linux?
    - Follow-up: Threads in Linux are lightweight processes (LWP).
    - Follow-up: All threads in same process share memory, file descriptors.
    - Follow-up: What is pthread library?

19. What is task switching and scheduling in Linux?
    - Follow-up: Kernel scheduler selects which process to run.
    - Follow-up: Preemptive scheduling (task can be interrupted).
    - Follow-up: Time slice / time quantum (default 10ms).

20. What is CFS (Completely Fair Scheduler)?
    - Follow-up: Modern Linux uses CFS (since 2.6.23).
    - Follow-up: How does CFS work? (virtual runtime, red-black tree)
    - Follow-up: Fair share of CPU time across all tasks.

21. What is process priority in Linux?
    - Follow-up: Nice values: -20 (highest) to +19 (lowest).
    - Follow-up: Real-time priority (SCHED_FIFO, SCHED_RR): 1–99 (highest priority).
    - Follow-up: What is SCHED_DEADLINE?

---

### Memory Management

22. What is virtual memory in Linux?
    - Follow-up: Virtual address space vs physical address space.
    - Follow-up: MMU (Memory Management Unit) translates virtual → physical.
    - Follow-up: Page table structure.

23. What is paging in Linux?
    - Follow-up: Memory divided into pages (typically 4KB on ARM).
    - Follow-up: Page fault — when page not in memory.
    - Follow-up: Demand paging — load page only when accessed.

24. What is swap in Linux?
    - Follow-up: Swap space on disk, used when physical RAM full.
    - Follow-up: Swap is slow (disk access) — avoid in embedded real-time.
    - Follow-up: How to disable swap? `swapoff -a`

25. What is memory layout of a process?
    ```
    0xFFFFFFFF  ┌─────────────────┐
                │  Kernel space   │
    0xC0000000  ├─────────────────┤
                │  Stack ↓        │
                ├─────────────────┤
                │  Heap ↑         │
                ├─────────────────┤
                │  BSS            │
                ├─────────────────┤
                │  Data           │
                ├─────────────────┤
                │  Text (Code)    │
    0x00000000  └─────────────────┘
    ```
    - Follow-up: Stack grows downward, heap upward.
    - Follow-up: What causes stack overflow in this model?

26. What is DMA (Direct Memory Access)?
    - Follow-up: Peripherals access memory without CPU.
    - Follow-up: DMA constraints in Linux: cache coherency, IOMMU.
    - Follow-up: Why is DMA buffer alignment important?

---

### Filesystem

27. What is filesystem in Linux?
    - Follow-up: ext4, btrfs, f2fs, JFFS2, UBIFS.
    - Follow-up: ext4 — most common in embedded.
    - Follow-up: JFFS2, UBIFS — designed for flash (embedded).

28. What is mount point in Linux?
    - Follow-up: Directory where filesystem is attached.
    - Follow-up: `/proc` (process info), `/sys` (kernel objects), `/dev` (devices).

29. What is /proc filesystem?
    - Follow-up: Virtual filesystem — no disk backing.
    - Follow-up: `/proc/cpuinfo` — CPU information.
    - Follow-up: `/proc/meminfo` — memory usage.
    - Follow-up: `/proc/<pid>/` — per-process information.

30. What is /sys filesystem?
    - Follow-up: Sysfs — kernel object model.
    - Follow-up: `/sys/class/` — device classes.
    - Follow-up: `/sys/devices/` — hardware topology.

---

## INTERMEDIATE LEVEL

### Device Drivers

31. What is a device driver in Linux?
    - Follow-up: Hardware abstraction layer — uniform interface for different hardware.
    - Follow-up: Character device vs block device.
    - Follow-up: Device driver = kernel module or compiled in kernel.

32. What is a kernel module?
    - Follow-up: `.ko` file — loadable kernel code.
    - Follow-up: Compiled separately, loaded at runtime via `insmod`.
    - Follow-up: Why use modules? (flexibility, faster development, keep kernel small)

33. How to write a simple character device driver?
    - Follow-up: `file_operations` structure — open, read, write, ioctl, release.
    - Follow-up: `register_chrdev()` — register device with kernel.
    - Follow-up: `mknod` — create device file in `/dev`.
    ```c
    static struct file_operations fops = {
        .open = device_open,
        .read = device_read,
        .write = device_write,
        .release = device_release,
    };
    
    int init_module(void) {
        major_num = register_chrdev(0, DEVICE_NAME, &fops);
        return 0;
    }
    ```

34. What is udev (userspace devfs)?
    - Follow-up: Dynamic device file creation in `/dev`.
    - Follow-up: udev rules — how to customize device naming and permissions.
    - Follow-up: Why udev instead of static `/dev` files?

35. How to interact with device drivers from userspace?
    - Follow-up: `open()`, `read()`, `write()` — standard POSIX calls.
    - Follow-up: `ioctl()` — device-specific control commands.
    - Follow-up: `mmap()` — memory-map device registers.

36. What is ioctl?
    - Follow-up: Generic interface for device-specific operations.
    - Follow-up: Example: GPIO control, UART configuration.
    ```c
    ioctl(fd, GPIO_SET_OUTPUT, &pin);
    ioctl(fd, GPIO_WRITE, &value);
    ```

37. What is interrupt handling in Linux device drivers?
    - Follow-up: `request_irq()` — register ISR.
    - Follow-up: ISR runs in interrupt context — must not sleep.
    - Follow-up: Deferred work: tasklet, workqueue for non-urgent work.
    - Follow-up: Difference between tasklet and workqueue?

38. What is tasklet in Linux?
    - Follow-up: Deferred execution context — softer than ISR.
    - Follow-up: Tasklets run with interrupts enabled (but cannot sleep).
    - Follow-up: Use: finish ISR processing, timers, bottom-half processing.

39. What is workqueue in Linux?
    - Follow-up: Kernel thread context — can sleep, call blocking functions.
    - Follow-up: Use: I/O operations, memory allocations, locking.
    - Follow-up: Workqueue vs tasklet — when to use each?

---

### Device Tree

40. What is device tree?
    - Follow-up: Hardware description in text format (`.dts` file).
    - Follow-up: Compiled to binary (`.dtb`) and loaded by bootloader.
    - Follow-up: Why device tree? (single kernel image, flexible hardware config)

41. Device tree syntax?
    ```
    / {
        cpus {
            cpu@0 {
                compatible = "arm,cortex-a9";
                clock-frequency = <1000000000>;
            };
        };
        memory {
            device_type = "memory";
            reg = <0 0x80000000 0 0x40000000>;  // 1GB at 0x80000000
        };
        serial@1c28000 {
            compatible = "snps,dw-apb-uart";
            reg = <0x01c28000 0x400>;
            interrupts = <0 1 4>;
            clock-frequency = <24000000>;
        };
    };
    ```
    - Follow-up: `compatible` string — matches driver.
    - Follow-up: `reg` — memory address and size.
    - Follow-up: `interrupts` — IRQ number and flags.

42. How does Linux use device tree?
    - Follow-up: Bootloader passes `.dtb` to kernel.
    - Follow-up: Kernel parses device tree, probes drivers.
    - Follow-up: Device tree platform data — alternative to hardcoded probe.

43. What is device tree compiler?
    ```bash
    dtc -I dts -O dtb board.dts -o board.dtb
    dtc -I dtb -O dts board.dtb -o board.dts
    ```

---

### Kernel Configuration & Build

44. What is kernel configuration (`.config`)?
    - Follow-up: Thousands of options: `CONFIG_*`.
    - Follow-up: How to configure? (`make menuconfig`, `make defconfig`)
    - Follow-up: Why is kernel configuration important in embedded?

45. How to build Linux kernel?
    ```bash
    make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- -j4
    make ARCH=arm modules
    ```
    - Follow-up: `ARCH` — target architecture.
    - Follow-up: `CROSS_COMPILE` — cross-compiler prefix.
    - Follow-up: Output: `arch/arm/boot/zImage` or `Image`.

46. What is kernel command line again (detailed)?
    - Follow-up: `console=ttyS0,115200` — where to send kernel logs.
    - Follow-up: `root=/dev/mmcblk0p2` — where rootfs is.
    - Follow-up: `rootfstype=ext4` — filesystem type.
    - Follow-up: `init=/sbin/init` — which init to start.
    - Follow-up: `debug` — enable kernel debug output.
    - Follow-up: How to pass? (bootloader environment, device tree `chosen` node)

47. What is CMDLINE_FROM_BOOTLOADER?
    - Follow-up: Some embedded Linux systems pass command line dynamically.
    - Follow-up: Alternative: hardcode in device tree `chosen` node.

---

### Networking in Embedded Linux

48. What is network stack in Linux?
    - Follow-up: ISO/OSI model — how Linux implements it.
    - Follow-up: Sockets API (BSD sockets) — userspace network programming.

49. What are network interfaces in Linux?
    - Follow-up: `eth0`, `wlan0` — network interface names.
    - Follow-up: `ifconfig`, `ip` command to configure.
    - Follow-up: IP address, netmask, gateway, DNS.

50. How to configure network in embedded Linux?
    - Follow-up: `/etc/network/interfaces` (Debian style).
    - Follow-up: `/etc/hostname`, `/etc/resolv.conf`.
    - Follow-up: DHCP vs static IP.
    - Follow-up: How to bring up network at boot?

51. What is socket in Linux?
    - Follow-up: BSD socket API — `socket()`, `bind()`, `listen()`, `accept()`, `connect()`.
    - Follow-up: TCP vs UDP — connection-oriented vs connectionless.

---

## ADVANCED LEVEL

### Kernel Internals

1. What is kernel mode vs user mode?
   - Follow-up: Hardware privilege levels — how ARM enforces separation.
   - Follow-up: Syscall — how user space calls kernel services.
   - Follow-up: System call overhead — typically 1–10 µs.

2. What is a syscall?
   - Follow-up: Examples: `read()`, `write()`, `fork()`, `exit()`, `open()`.
   - Follow-up: On ARM: SVC instruction — software interrupt.
   - Follow-up: Syscall number in register, kernel handles via dispatch table.

3. What is fork and exec?
   - Follow-up: `fork()` — creates child process, copy of parent.
   - Follow-up: `exec()` — replace process image with new program.
   - Follow-up: Why do you need both? (shell redirects, signal handlers setup)

4. What is context switch in Linux?
   - Follow-up: Same as RTOS — save/restore registers, change virtual address space.
   - Follow-up: Linux context switch is heavier (TLB flush, cache miss).
   - Follow-up: Typical overhead: 1–10 µs per context switch.

5. What is TLB (Translation Lookaside Buffer)?
   - Follow-up: Hardware cache for page table entries.
   - Follow-up: TLB miss triggers page table walk.
   - Follow-up: Why TLB flush on context switch is expensive?

6. What is cache hierarchy?
    ```
    L1 cache (per core)
    ├── L1 instruction (I-cache): 32KB
    └── L1 data (D-cache): 32KB
    
    L2 cache (per core or shared): 256KB–1MB
    
    L3 cache (shared): 4–16MB
    
    RAM
    ```
   - Follow-up: Access times: L1 = 4 cycles, L2 = 10 cycles, L3 = 40 cycles, RAM = 100+ cycles.
   - Follow-up: Cache coherency on multicore — what is it?

7. What is cache coherency problem?
   - Follow-up: CPU0 writes to address X, CPU1 reads stale value from its cache.
   - Follow-up: MESI protocol — hardware ensures coherency.
   - Follow-up: Embedded impact: false sharing, performance degradation.

8. What is page table in Linux?
   - Follow-up: Virtual address translation: page number + offset.
   - Follow-up: Multi-level page tables (ARM uses 2–3 levels).
   - Follow-up: Page table setup — kernel responsibility.

9. What is COW (Copy-on-Write)?
   - Follow-up: `fork()` doesn't immediately copy memory — copy only when written.
   - Follow-up: Optimization: `fork()` + `exec()` pattern (shell).

10. What is swap (detailed)?
    - Follow-up: When to use swap vs disable for real-time?
    - Follow-up: Swap thrashing — disk I/O dominates, system becomes unresponsive.
    - Follow-up: Embedded systems (limited storage) — often no swap.

11. What is memcg (memory cgroup)?
    - Follow-up: Limit memory usage per process group.
    - Follow-up: OOM killer — system out of memory, kills process.

12. What is OOM killer?
    - Follow-up: When memory exhausted, kernel kills process.
    - Follow-up: Selection based on memory usage and oom_score.
    - Follow-up: How to prevent OOM of critical processes? (mlockall, oom_score_adj)

---

### Power Management

13. What is CPU frequency scaling (DVFS)?
    - Follow-up: Dynamic Voltage and Frequency Scaling.
    - Follow-up: Reduces power consumption by lowering frequency when demand low.
    - Follow-up: Governors: `ondemand`, `powersave`, `performance`, `schedutil`.
    - Follow-up: How does scheduler interact with frequency scaling?

14. What are CPU power states (C-states)?
    - Follow-up: C0 — processor running.
    - Follow-up: C1 — shallow sleep, wakes immediately.
    - Follow-up: C2, C3, C4 — deeper sleep, longer wakeup latency.
    - Follow-up: Tradeoff: deeper sleep = more power savings, slower wakeup.

15. What is cpuidle subsystem?
    - Follow-up: Selects CPU power state based on idle time.
    - Follow-up: `drivers/cpuidle/` — platform-specific idle handlers.
    - Follow-up: For embedded real-time — use shallow sleep (C1) to minimize latency.

16. What is PM domain (power domain)?
    - Follow-up: Groups of hardware that share power control.
    - Follow-up: Can power off entire domain (e.g., GPU, ISP) when not needed.

17. How to optimize power consumption in embedded Linux?
    - Follow-up: Disable unused devices in device tree.
    - Follow-up: Use shallow CPU idle states (C1, C2) for real-time.
    - Follow-up: GPIO control for power supply to peripherals.
    - Follow-up: Monitor with `powertop` or power profiler.

---

### Real-Time Linux

18. What is real-time Linux?
    - Follow-up: Standard Linux is not real-time (no bounded latency).
    - Follow-up: PREEMPT_RT patch — adds preemptiveness to Linux kernel.
    - Follow-up: Hard real-time vs soft real-time Linux.

19. What is PREEMPT_RT patch?
    - Follow-up: Replaces spinlocks with mutexes (preemptible).
    - Follow-up: Replaces non-preemptible sections with preemptible equivalents.
    - Follow-up: Allows threads to be preempted at almost any point.
    - Follow-up: Latency reduction: typical embedded Linux ~100µs → ~10µs with PREEMPT_RT.

20. What is interrupt priority in real-time Linux?
    - Follow-up: `SCHED_FIFO`, `SCHED_RR` — real-time scheduling classes.
    - Follow-up: Priority range: 1–99 (higher = more critical).
    - Follow-up: How to set: `sched_setscheduler()`.

21. What is mlockall?
    - Follow-up: Locks process memory in RAM — prevents page fault.
    - Follow-up: Critical for real-time — page fault causes unpredictable latency.
    - Follow-up: How to use: `mlockall(MCL_CURRENT | MCL_FUTURE);`

22. What is jitter in real-time Linux?
    - Follow-up: Variation in task wake-up time.
    - Follow-up: Sources: scheduler preemption, cache miss, TLB miss.
    - Follow-up: How to measure: `cyclictest` tool.

---

### Multicore & SMP

23. What is SMP (Symmetric Multiprocessing) in Linux?
    - Follow-up: Multiple identical processors sharing memory.
    - Follow-up: Kernel runs on all cores.
    - Follow-up: Scheduler balances tasks across cores.

24. What is CPU affinity?
    - Follow-up: Bind process/thread to specific CPU core(s).
    - Follow-up: `taskset` command to set affinity.
    - Follow-up: Why use affinity? (cache locality, real-time guarantees, isolation)

25. What is IPI (Inter-Processor Interrupt)?
    - Follow-up: One CPU sends interrupt to another CPU.
    - Follow-up: Used for TLB flush, CPU hotplug, load balancing.
    - Follow-up: IPI overhead on many-core systems.

26. What is cache coherency protocol?
    - Follow-up: MESI (Modified, Exclusive, Shared, Invalid).
    - Follow-up: Ensures all cores see consistent memory.
    - Follow-up: Coherency traffic — bottleneck on many-core.

27. How does Linux scheduler work on SMP?
    - Follow-up: Per-core runqueue — each CPU has own ready list.
    - Follow-up: Load balancing — migrate tasks between cores.
    - Follow-up: When does load balancing occur?

---

### Filesystem & Storage

28. What is flash filesystem (JFFS2, UBIFS)?
    - Follow-up: JFFS2 — Journaling Flash File System 2, designed for NOR flash.
    - Follow-up: UBIFS — UBI File System, designed for NAND flash.
    - Follow-up: Why not ext4 on flash? (wear, erase before write)

29. What is UBI (Unsorted Block Images)?
    - Follow-up: Abstraction layer over raw NAND flash.
    - Follow-up: Handles wear leveling, bad block management.
    - Follow-up: UBIFS runs on top of UBI.

30. What is YAFFS2 (Yet Another Flash File System)?
    - Follow-up: Designed for NAND flash, simpler than UBIFS.
    - Follow-up: Proprietary with open-source fork.
    - Follow-up: When to use YAFFS2 vs UBIFS?

31. What is MTD (Memory Technology Devices)?
    - Follow-up: Linux abstraction for flash memory.
    - Follow-up: `/dev/mtd*` — raw flash devices.
    - Follow-up: `mtdutils` — `flash_erase`, `nandwrite`, `nandread`.

---

## EXPERT LEVEL

### Kernel Debugging

1. How to enable kernel debug symbols?
   ```bash
   make CONFIG_DEBUG_INFO=y
   make CONFIG_DEBUG_KERNEL=y
   make CONFIG_GDB_SCRIPTS=y
   ```
   - Follow-up: Debug kernel image is much larger.
   - Follow-up: Ship unstripped kernel for post-mortem analysis.

2. What is KGDB (Kernel GNU Debugger)?
   - Follow-up: Allows remote GDB debugging of kernel.
   - Follow-up: Serial connection to kernel, run GDB on host.
   - Follow-up: How to enable: `CONFIG_KGDB`, `CONFIG_KGDB_SERIAL_CONSOLE`.

3. What is netconsole?
   - Follow-up: Kernel logs sent over network instead of serial.
   - Follow-up: Useful for systems with only Ethernet (no UART).
   - Follow-up: Configuration: `netconsole=@<local_ip>/<dev>,<remote_port>@<remote_ip>/<remote_mac>`.

4. What is sysrq (SysRq)?
   - Follow-up: Magic key combo to trigger kernel actions.
   - Follow-up: Alt+SysRq+{S,U,B,H,T,E,M,etc.} for sync, remount RO, reboot, halt, etc.
   - Follow-up: Useful for debugging unresponsive system.

5. What is kdump / kexec?
   - Follow-up: kexec — load new kernel without reboot.
   - Follow-up: kdump — capture kernel crash dump to disk.
   - Follow-up: Post-mortem analysis of kernel crash.

6. What is crash utility?
   - Follow-up: Analyzes kernel crash dumps.
   - Follow-up: Inspect kernel data structures, backtraces, task lists.

7. How to use ftrace (kernel function tracer)?
   ```bash
   # Mount debugfs
   mount -t debugfs debugfs /sys/kernel/debug

   # Use ftrace
   echo function > /sys/kernel/debug/tracing/current_tracer
   cat /sys/kernel/debug/tracing/trace
   ```
   - Follow-up: Trace every function call in kernel.
   - Follow-up: Overhead — high, use only for debugging.

8. What is perf (performance profiler)?
   ```bash
   perf record ./program
   perf report
   ```
   - Follow-up: Profile CPU cache misses, branch misses, context switches.
   - Follow-up: Sample-based profiling — low overhead.

9. What is SystemTap?
   - Follow-up: Dynamic tracing tool — instrument kernel and user space.
   - Follow-up: Write tracing scripts without recompiling kernel.
   - Follow-up: More flexible than ftrace, higher overhead.

---

### Performance Analysis

10. How to analyze system performance?
    - Follow-up: Top commands: `top`, `htop`, `ps`, `vmstat`, `iostat`.
    - Follow-up: `top` — real-time process monitor.
    - Follow-up: `vmstat` — virtual memory and CPU stats.
    - Follow-up: `iostat` — I/O statistics.

11. What is context switch overhead?
    - Follow-up: Measure via `perf stat -e context-switches`.
    - Follow-up: High context switches indicate scheduler thrashing.

12. What is cache miss analysis?
    - Follow-up: `perf stat -e cache-misses,cache-references`.
    - Follow-up: L1/L2/L3 cache miss rates indicate memory access patterns.

13. What is branch prediction analysis?
    - Follow-up: `perf stat -e branch-misses,branches`.
    - Follow-up: High branch miss rate indicates unpredictable code paths.

---

### Security

14. What are attack vectors in embedded Linux?
    - Follow-up: Buffer overflow, format string, SQL injection (if applicable).
    - Follow-up: Privilege escalation, kernel exploits.
    - Follow-up: Side-channel attacks (timing, cache, power).

15. What is SELinux (Security-Enhanced Linux)?
    - Follow-up: Mandatory access control (MAC) framework.
    - Follow-up: Beyond traditional DAC (discretionary access control).
    - Follow-up: Too complex for most embedded systems.

16. What is AppArmor?
    - Follow-up: Simpler MAC alternative to SELinux.
    - Follow-up: Profile-based — define what app can do.

17. How to secure embedded Linux system?
    - Follow-up: Disable unnecessary services.
    - Follow-up: Use read-only rootfs where possible.
    - Follow-up: Secure boot, signed kernel, device tree.
    - Follow-up: Minimize attack surface.

---

### Embedded Linux Optimization

18. How to minimize rootfs size?
    - Follow-up: Use busybox instead of full GNU userutils.
    - Follow-up: Strip debug symbols: `arm-linux-strip`.
    - Follow-up: Use alternative libc: musl (smaller than glibc).

19. What is musl libc?
    - Follow-up: Lightweight C library, 1/10 size of glibc.
    - Follow-up: More POSIX-compliant than glibc.
    - Follow-up: Good for embedded, resource-constrained systems.

20. How to reduce boot time?
    - Follow-up: Parallel boot — systemd can parallelize service startup.
    - Follow-up: Reduce initramfs size.
    - Follow-up: Pre-populate filesystem cache.
    - Follow-up: Use faster storage (SSD vs HDD).

21. What is Buildroot?
    - Follow-up: System to build custom Linux distribution.
    - Follow-up: Generates kernel, bootloader, rootfs, cross-compiler.
    - Follow-up: Simpler than Yocto, good for small embedded projects.

22. What is Yocto Project?
    - Follow-up: Industrial-grade build system for embedded Linux.
    - Follow-up: Supports complex builds, multiple architectures.
    - Follow-up: More flexible but steeper learning curve than Buildroot.

23. How to cross-compile for embedded Linux?
    - Follow-up: Cross-compiler: `arm-linux-gnueabihf-gcc`.
    - Follow-up: Sysroot — staging directory with target libraries and headers.
    - Follow-up: PKG_CONFIG_PATH, LD_LIBRARY_PATH for build.

24. What is reproducible builds?
    - Follow-up: Same input → deterministic binary output.
    - Follow-up: Important for security — verify no tampering.
    - Follow-up: Challenges: timestamps, locale, ordering.

---

## EXPERT WAR STORIES / DEEP QUESTIONS

1. "You said PREEMPT_RT reduces latency to ~10µs — what if you need <1µs?"
   - **Answer:** Consider bare-metal (no OS) or hard real-time OS. Linux + PREEMPT_RT fundamentally has limits.

2. "You said TLB miss is expensive — can you completely avoid TLB misses?"
   - **Answer:** Huge pages reduce TLB pressure, but not eliminate misses. Transparent Huge Pages (THP) automatic on some systems.

3. "You said COW (Copy-on-Write) is optimization for fork() — what if forked process immediately writes all memory?"
   - **Answer:** Then COW overhead is bad (many page faults). Consider thread-based concurrency instead.

4. "You said context switch overhead is 1–10µs — but I measured 50µs sometimes, why?"
   - **Answer:** Cache misses, TLB flushes, lock contention, or other processes preempting the measurement itself.

5. "You said disable swap for real-time — but what if system runs out of RAM?"
   - **Answer:** Design system so no swap needed. Use memory cgroups to enforce limits and kill low-priority processes.

6. "You said CPU affinity for cache locality — what about load imbalance?"
   - **Answer:** Balance data locality vs load balance tradeoff. Usually pin critical tasks, balance others.

7. "You said JFFS2 for NOR flash — but our NOR is 200MB, how long does JFFS2 take to mount?"
   - **Answer:** JFFS2 scans entire flash on mount (slow). Consider UBI+UBIFS or pre-built filesystem images.

8. "You said secure boot — but bootloader itself is not secure, how?"
   - **Answer:** Hardware root of trust (fuses, TPM). Bootloader ROM (read-only, burned in). Trustzone firmware verification.

9. "You said systemd can parallelize boot — but some services require specific order, how to enforce?"
   - **Answer:** Systemd `Before=`, `After=`, `Wants=`, `Requires=` directives in unit files.

10. "You said ftrace for kernel debugging — but our critical ISR completes in 100ns, any impact?"
    - **Answer:** ftrace overhead >> 100ns. Use hardware trace (ETM) or event recording in ISR, analyze post-mortem.

11. "You said perf for profiling — but it's not sampling data we care about, can we instrument specific function?"
    - **Answer:** Use `uprobes` for user space, `kprobes` for kernel. More targeted but higher overhead.

12. "You said SELinux is too complex for embedded — but what about container security?"
    - **Answer:** Docker/containerization adds security isolation, but not required for single-application embedded systems.

---

## LINUX COMMAND CHEAT SHEET (Embedded Focus)

| Command | Purpose |
|---------|---------|
| `uname -a` | Kernel version, architecture |
| `cat /proc/cpuinfo` | CPU info, number of cores |
| `free -h` | Memory usage |
| `ps aux` | All running processes |
| `top` | Real-time process monitor |
| `htop` | Interactive process monitor |
| `vmstat 1` | Virtual memory stats every 1s |
| `iostat 1` | I/O statistics every 1s |
| `lsof` | Open files/sockets per process |
| `netstat -an` | Network connections |
| `ip addr show` | Network interface config |
| `ifconfig` | Network interface (legacy) |
| `route -n` | Routing table |
| `lsmod` | Loaded kernel modules |
| `insmod` | Load kernel module |
| `rmmod` | Unload kernel module |
| `modprobe` | Load module with dependencies |
| `ls -lR /dev` | Device files |
| `mount` | Mounted filesystems |
| `df -h` | Disk space usage |
| `du -sh <dir>` | Directory size |
| `dmesg` | Kernel log messages |
| `journalctl` | Systemd journal |
| `systemctl status` | System status |
| `systemctl restart <svc>` | Restart service |
| `journalctl -u <unit>` | Unit logs |
| `cat /proc/<pid>/maps` | Process memory layout |
| `cat /proc/<pid>/status` | Process status |
| `strace -f <prog>` | Trace syscalls |
| `ltrace <prog>` | Trace library calls |
| `gdb <prog>` | GNU debugger |
| `perf record <prog>` | CPU profiler |
| `perf stat <prog>` | Performance stats |
| `taskset -c 0 <prog>` | Pin to CPU 0 |
| `chrt -f 10 <prog>` | Set FIFO priority 10 |
| `nice -n -10 <prog>` | Set nice value (legacy) |
| `ulimit -a` | Resource limits |
| `ulimit -c unlimited` | Enable core dumps |
| `mlock -l 100 <prog>` | Lock memory (100 pages) |

---

## SUMMARY TABLE

| Topic | Basic | Intermediate | Advanced | Expert |
|-------|-------|-------------|----------|--------|
| Microprocessor basics | ★★★★★ | ★★★★☆ | ★★★☆☆ | ★★☆☆☆ |
| ARM Cortex-A | ★★★☆☆ | ★★★★★ | ★★★★★ | ★★★★☆ |
| Linux boot process | ★★★★☆ | ★★★★★ | ★★★★☆ | ★★★☆☆ |
| Process & scheduling | ★★★★☆ | ★★★★★ | ★★★★★ | ★★★★☆ |
| Memory management | ★★★★☆ | ★★★★★ | ★★★★★ | ★★★★★ |
| Device drivers | ★★★☆☆ | ★★★★★ | ★★★★★ | ★★★★☆ |
| Device tree | ★★★☆☆ | ★★★★★ | ★★★★☆ | ★★★☆☆ |
| Filesystem | ★★★☆☆ | ★★★★☆ | ★★★★★ | ★★★★☆ |
| Real-time Linux | ★★☆☆☆ | ★★★☆☆ | ★★★★★ | ★★★★★ |
| Multicore & SMP | ★★☆☆☆ | ★★★★☆ | ★★★★★ | ★★★★★ |
| Power management | ★★☆☆☆ | ★★★★☆ | ★★★★★ | ★★★★★ |
| Kernel debugging | ★★☆☆☆ | ★★★☆☆ | ★★★★☆ | ★★★★★ |
| Performance analysis | ★★☆☆☆ | ★★★☆☆ | ★★★★★ | ★★★★★ |
| Security | ★★☆☆☆ | ★★★☆☆ | ★★★★☆ | ★★★★★ |
| Cross-compilation | ★★★☆☆ | ★★★★☆ | ★★★★★ | ★★★★☆ |
| Buildroot / Yocto | ★★☆☆☆ | ★★★★☆ | ★★★★★ | ★★★★★ |

---

*This document covers embedded Linux development from microprocessor fundamentals to expert kernel hacking — suitable for M.Tech graduate with 10+ years experience in embedded systems.*