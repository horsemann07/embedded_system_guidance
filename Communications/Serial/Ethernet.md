# Embedded Communication Protocols Interview Questions
> UART, I2C, I3C, SPI, CAN, CANopen, Ethernet, Automotive Ethernet, 1-Wire, LIN, Modbus, etc.  
> M.Tech Graduate + 10 Years Experience  
> Basic → Intermediate → Advanced → Expert → War Stories

## Ethernet

### Basic Level

1. What is Ethernet?
   - Follow-up: IEEE 802.3 standard for local area networks.
   - Follow-up: Packet-based, collision detection (CSMA/CD, mostly historical).
   - Follow-up: Speeds: 10M, 100M (Fast), 1G (Gigabit), 10G.
   - Follow-up: Not real-time (unbounded latency).

2. Ethernet frame structure?
   ```
   [Preamble 7] [SFD 1] [Dest MAC 6] [Src MAC 6] [Type 2] [Payload 46-1500] [FCS 4]
   ```

3. MAC address?
   - Follow-up: 48-bit (6 bytes), written as XX:XX:XX:XX:XX:XX (hex).
   - Follow-up: First 3 bytes: manufacturer (OUI).
   - Follow-up: Last 3 bytes: device-specific.

4. Ethernet on embedded systems?
   - Follow-up: Typically via external PHY (physical layer transceiver).
   - Follow-up: Microcontroller has MAC (media access control), PHY handles electrical.
   - Follow-up: Connection: MII, RMII, GMII (parallel interface), or SPI (serial).

---

### Intermediate Level

5. What is IP, TCP, UDP?
   - Follow-up: IP (layer 3): routing between networks, address 32-bit (IPv4).
   - Follow-up: TCP (layer 4): reliable, connection-oriented, stream.
   - Follow-up: UDP (layer 4): unreliable, connectionless, datagram.

6. How to use Ethernet on embedded Linux?
   - Follow-up: Device driver for PHY and MAC.
   - Follow-up: Network interface (eth0).
   - Follow-up: IP configuration via DHCP or static.

7. What is socket API for Ethernet?
   ```c
   int sock = socket(AF_INET, SOCK_STREAM, 0);  // TCP
   int sock = socket(AF_INET, SOCK_DGRAM, 0);   // UDP
   ```

---

### Advanced Level

8. What is TSN (Time-Sensitive Networking)?
   - Follow-up: Deterministic Ethernet for real-time applications.
   - Follow-up: IEEE 802.1Qbv (time-aware shaper), 802.1Qbu (frame preemption).
   - Follow-up: Guaranteed latency, bounded jitter.
   - Follow-up: Used in industrial (TSN profile), automotive (future).

---
