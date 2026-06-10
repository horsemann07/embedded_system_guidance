# Embedded Communication Protocols Interview Questions
> UART, I2C, I3C, SPI, CAN, CANopen, Ethernet, Automotive Ethernet, 1-Wire, LIN, Modbus, etc.  
> M.Tech Graduate + 10 Years Experience  
> Basic → Intermediate → Advanced → Expert → War Stories

## 1-Wire (Dallas/Maxim)

### Basic Level

1. What is 1-Wire?
   - Follow-up: Single-wire protocol, invented by Dallas Semiconductor.
   - Follow-up: One data line, pull-up resistor (open-drain).
   - Follow-up: Low speed: ~16kbps.
   - Follow-up: Common devices: DS18B20 (temperature sensor), DS2431 (EEPROM).

2. 1-Wire protocol?
   - Follow-up: Master initiates, slave responds.
   - Follow-up: Bits sent by timing: short pulse (0), long pulse (1).
   - Follow-up: Slave detected by presence pulse after reset.

3. 1-Wire addressing?
   - Follow-up: 64-bit ROM ID (manufacturer + unique serial + CRC).
   - Follow-up: Multiple devices on one wire (parasitic addressing).
   - Follow-up: Master addresses specific device by ID.

4. How to use DS18B20 temperature sensor?
   - Follow-up: 1-Wire temperature sensor.
   - Follow-up: Command: reset, send ROM command (match ID), send function command (start conversion).
   - Follow-up: Read scratchpad: 2 bytes temperature.

---


## Summary Comparison Table

| Protocol | Type | Speed | Wires | Distance | Use Case |
|----------|------|-------|-------|----------|----------|
| UART | Async | 115.2k | 2 | ~15m | Debug, simple sensors |
| I2C | Sync | 400k | 2 | ~5m | On-board sensors, EEPROM |
| I3C | Sync | 12.5M | 2 | ~5m | High-speed sensors, new designs |
| SPI | Sync | 50M+ | 4 | ~0.5m | Flash, fast peripherals |
| CAN | Async | 1M | 2 | ~100m | Automotive, industrial |
| CANopen | App layer | (CAN) | 2 | ~100m | Industrial automation |
| LIN | Async | 19.2k | 1 | ~40m | Body electronics |
| Ethernet | Packet | 1G+ | 4 | ~100m | Networks, IP-based |
| Auto Ethernet | Packet | 1G | 1 | ~15m | Modern vehicles |
| 1-Wire | Async | 16k | 1 | ~100m | Temperature, EEPROMs |
| Modbus | Master-slave | Variable | 1–4 | Variable | SCADA, industrial |

---

## EXPERT WAR STORIES / DEEP QUESTIONS

1. "You said UART baud rate error tolerance is ±5% — but I'm seeing failures at ±3%, why?"
   - **Answer:** Crystal accuracy, clock divider rounding, OS jitter on host side (if USB converter).

2. "You said I2C pull-up is 4.7kΩ — but when I use 10kΩ, the bus hangs. Why?"
   - **Answer:** Capacitance on bus (cable, device pins) slows rise time. 10kΩ might not be strong enough. Use lower resistance.

3. "You said SPI is full-duplex — but my slave never drives MISO, so I'm always reading garbage. What am I missing?"
   - **Answer:** Slave might not be selected (CS high), or slave firmware not active. Check CS timing, slave state.

4. "You said CAN arbitration is deterministic — but my low-priority node never gets to transmit. Is it starved?"
   - **Answer:** Possibly. If high-priority nodes continuously transmitting, low-priority starves. Use time-triggered CAN (TTCAN) or limit high-priority message rate.

5. "You said CANopen is industrial standard — can I use it for robotics control with <1ms latency?"
   - **Answer:** Technically yes, but PDO overhead might push you >1ms. Consider specialized real-time CAN or EtherCAT.

6. "You said Automotive Ethernet is single pair — how does it handle noise vs 4-pair standard?"
   - **Answer:** Better differential design, equalizer in PHY, cable shielding more critical. Not necessarily noisier.

7. "You said 1-Wire is parasitic-powered — how much power can device draw?"
   - **Answer:** Limited by pull-up resistor. Typically <1mA max. Device must use strong pull-down during transmission.

8. "You said Modbus is master-slave — can slaves initiate communication?"
   - **Answer:** Not in standard Modbus. Slaves only respond to master. For reverse communication, use different polling or TCP (more flexible).

9. "You said TSN guarantees deterministic latency — but what if network is saturated?"
   - **Answer:** TSN provides bounded latency to TSN-capable devices, but non-TSN traffic can affect others. Requires TSN scheduling policy.

10. "You said CANopen NMT — how do I recover from error passive state?"
    - **Answer:** Wait for NMT "reset" command, or error counters decrement (if no more errors). Hard reset via master NMT command safest.

---

## Embedded Communication Protocols Cheat Sheet

| Protocol | Physical | Data Rate | Duplex | Distance | Typical Use |
|----------|----------|-----------|--------|----------|------------|
| UART | CMOS levels | 115.2k | Full | ~15m | Debug/sensors |
| I2C | Open-drain | 400k | Half | ~5m | On-board chips |
| I3C | OD-PP | 12.5M | Half | ~5m | Next-gen sensors |
| SPI | CMOS push-pull | 50M+ | Full | ~0.5m | Memory/fast ICs |
| CAN | Differential | 1M | Half | ~100m | Automotive |
| CANopen | (CAN layer) | (CAN) | (CAN) | (CAN) | Industrial apps |
| LIN | Single-wire | 19.2k | Half | ~40m | Low-cost body |
| Ethernet | Twisted pair | 1G+ | Full | ~100m | Networks |
| Auto Ethernet | Single pair | 100M–1G | Full | ~15m | Modern vehicles |
| 1-Wire | Single wire | 16k | Half | ~100m | Temp/ID chips |
| Modbus | Serial/Ethernet | Variable | Half | Variable | SCADA/PLC |

---

*This document covers all major embedded communication protocols from fundamentals to expert level, suitable for M.Tech graduate with 10+ years embedded systems experience.*