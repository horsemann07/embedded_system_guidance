# Embedded Communication Protocols Interview Questions
> UART, I2C, I3C, SPI, CAN, CANopen, Ethernet, Automotive Ethernet, 1-Wire, LIN, Modbus, etc.  
> M.Tech Graduate + 10 Years Experience  
> Basic → Intermediate → Advanced → Expert → War Stories

## CANopen

### Basic Level

1. What is CANopen?
   - Follow-up: Application layer protocol on top of CAN.
   - Follow-up: Standard: EN 50325-4, ISO 11898.
   - Follow-up: Defines message structure, object dictionary, state machine.
   - Follow-up: Used in industrial automation, robotics, renewable energy.

2. CANopen object dictionary?
   - Follow-up: Collection of all parameters and variables in device.
   - Follow-up: Index (2 bytes) + subindex (1 byte) addressing.
   - Follow-up: Example: index 0x6040 = control word, subindex 0 = entire word.
   - Follow-up: Device profile standard (DS401, DS402, DS409, etc. by device type).

3. CANopen COB-ID?
   - Follow-up: Communication Object Identifier = CAN ID + function.
   - Follow-up: Base ID + node ID determines final CAN ID.
   - Follow-up: Example: base 0x000 (NMT), base 0x080 (sync), base 0x180 (TPDO1).

4. CANopen function codes?
   - NMT (Network Management): 0x000 (broadcast).
   - SYNC: 0x080 (synchronization).
   - EMCY (Emergency): 0x080 + node_id.
   - TPDO1-4 (Transmit PDO): 0x180, 0x280, 0x380, 0x480 + node_id.
   - RPDO1-4 (Receive PDO): 0x200, 0x300, 0x400, 0x500 + node_id.
   - TSDO (Transmit SDO): 0x580 + node_id.
   - RSDO (Receive SDO): 0x600 + node_id.

5. What is PDO (Process Data Object)?
   - Follow-up: Cyclic message for real-time data.
   - Follow-up: TPDO: device sends, RPDO: device receives.
   - Follow-up: Up to 4 PDOs, configurable (which object dictionary entries map to PDO).
   - Follow-up: Triggered by SYNC, timer, or event.

6. What is SDO (Service Data Object)?
   - Follow-up: Non-cyclic, service-oriented message for configuration.
   - Follow-up: Request-response pattern (client-server).
   - Follow-up: Access object dictionary (read, write).
   - Follow-up: Slower than PDO, but reliable (with retransmission).

7. CANopen NMT state machine?
   ```
   Initialization
   ↓
   Pre-operational (parameter configuration)
   ↓
   Operational (data exchange)
   ```
   - Follow-up: NMT command: start, stop, preop.
   - Follow-up: Heartbeat monitoring — if no heartbeat, assume device failed.

8. How to implement simple CANopen device?
   ```c
   // Object dictionary entry
   typedef struct {
       uint16_t index;
       uint8_t subindex;
       uint8_t attr;  // read/write
       void *ptr;
       uint16_t len;
   } OD_Entry;

   // CANopen receive
   void canopen_rx(CAN_Message *msg) {
       uint16_t function = msg->id & 0x780;
       uint8_t node_id = msg->id & 0x7F;

       switch (function) {
           case 0x000:  // NMT
               process_nmt(msg->data[1], node_id);
               break;
           case 0x180:  // TPDO1
               process_pdo_rx(msg);
               break;
           case 0x600:  // SDO RX
               process_sdo_rx(msg);
               break;
       }
   }
   ```

---

### Intermediate Level

9. What is SYNC message?
   - Follow-up: Broadcast from master (usually), triggers PDO transmission.
   - Follow-up: Ensures all devices synchronized.
   - Follow-up: Interval configurable.

10. What is heartbeat monitoring?
    - Follow-up: Each device sends heartbeat PDO periodically.
    - Follow-up: Master monitors heartbeat timeout.
    - Follow-up: If heartbeat missed, master assumes device dead, take action (error handling).

11. What is EMCY (Emergency) message?
    - Follow-up: Device sends emergency frame on error.
    - Follow-up: Error code (2 bytes) + manufacturer-specific data (5 bytes).
    - Follow-up: Used for immediate error notification (not waiting for next cycle).

12. What is COB-ID remapping?
    - Follow-up: Change COB-ID mapping for TPDO/RPDO at runtime.
    - Follow-up: Allows flexibility without hardware change.
    - Follow-up: Configuration via SDO.

---

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
