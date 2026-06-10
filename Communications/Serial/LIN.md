# Embedded Communication Protocols Interview Questions
> UART, I2C, I3C, SPI, CAN, CANopen, Ethernet, Automotive Ethernet, 1-Wire, LIN, Modbus, etc.  
> M.Tech Graduate + 10 Years Experience  
> Basic → Intermediate → Advanced → Expert → War Stories


## LIN (Local Interconnect Network)

### Basic Level

1. What is LIN?
   - Follow-up: Single-master, multi-slave serial bus (automotive).
   - Follow-up: 1-wire bus (cheaper than CAN).
   - Follow-up: Speeds: 9.6kbps, 19.2kbps (typical).
   - Follow-up: Max 20 nodes per bus.
   - Follow-up: Used in body electronics, climate control.

2. LIN vs CAN?
   | Feature | LIN | CAN |
   |---------|-----|-----|
   | Cost | Low | Higher |
   | Speed | ~19k bps | 1M bps |
   | Wires | 1 | 2 |
   | Distance | <40m | <100m |
   | Nodes | 20 | 110+ |
   | Real-time | Poor | Good |

3. LIN frame structure?
   - Header (master): sync break, sync field, PID (protection ID).
   - Response (slave): checksum, data (0-8 bytes).

4. LIN schedule?
   - Follow-up: Master drives scheduled message transmission.
   - Follow-up: Predefined schedule: which message every N ms.

---
