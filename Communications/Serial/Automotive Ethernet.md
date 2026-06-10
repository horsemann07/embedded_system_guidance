# Embedded Communication Protocols Interview Questions
> UART, I2C, I3C, SPI, CAN, CANopen, Ethernet, Automotive Ethernet, 1-Wire, LIN, Modbus, etc.  
> M.Tech Graduate + 10 Years Experience  
> Basic → Intermediate → Advanced → Expert → War Stories

## Automotive Ethernet

### Basic Level

1. What is Automotive Ethernet?
   - Follow-up: Ethernet adapted for automotive (vehicles).
   - Follow-up: 100BASE-T1: 100Mbps, single twisted pair (vs 4 pairs for standard Ethernet).
   - Follow-up: 1000BASE-T1: 1Gbps, single pair.
   - Follow-up: 10BASE-T1S: 10Mbps, for low-speed devices.

2. Automotive Ethernet vs Standard Ethernet?
   | Feature | Automotive | Standard |
   |---------|-----------|----------|
   | Speed | 100Mbps–1Gbps | 10Mbps–10Gbps |
   | Wiring | Single pair | 4 pairs |
   | Cost | Lower (less wiring) | Higher |
   | EMI | Better (single pair) | More EMI |
   | Distance | ~15m | ~100m |

3. Automotive Ethernet layers?
   - Follow-up: Physical: 100BASE-T1, 1000BASE-T1.
   - Follow-up: MAC: standard Ethernet MAC.
   - Follow-up: Application: IP, TCP/UDP, custom protocols.

---

### Intermediate Level

4. What is SOME/IP (Scalable service-Oriented MiddlE)?
   - Follow-up: Middleware for automotive RPC (remote procedure call).
   - Follow-up: Used in AUTOSAR Adaptive Platform.
   - Follow-up: Defines message format for service discovery, method invocation, event notification.

5. What is SOMEIP protocol?
   - Follow-up: Runs over UDP or TCP.
   - Follow-up: Service ID + method ID + version + message type.

6. What is DoIP (Diagnostics over IP)?
   - Follow-up: OBD-II diagnostics via Ethernet instead of UART.
   - Follow-up: Higher bandwidth for firmware updates.

---
