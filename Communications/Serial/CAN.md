# Embedded Communication Protocols Interview Questions
> UART, I2C, I3C, SPI, CAN, CANopen, Ethernet, Automotive Ethernet, 1-Wire, LIN, Modbus, etc.  
> M.Tech Graduate + 10 Years Experience  
> Basic → Intermediate → Advanced → Expert → War Stories
> 
## CAN (Controller Area Network)

### Basic Level

1. What is CAN?
   - Follow-up: Automotive serial bus, invented by Bosch.
   - Follow-up: Multi-master, multi-node (up to 127 nodes).
   - Follow-up: 2-wire differential: CAN_H and CAN_L.
   - Follow-up: Speeds: 125kHz (long distance), 500kHz (typical), 1MHz (short distance).
   - Follow-up: Used in automotive, industrial, medical devices.

2. Why is CAN differential?
   - Follow-up: Noise rejection — noise affects both lines equally, differential stage rejects common-mode.
   - Follow-up: Twisted pair for CAN_H and CAN_L.
   - Follow-up: Typical impedance: 120Ω.
   - Follow-up: Termination resistors at both ends of bus.

3. CAN frame structure (CAN 2.0B)?
   ```
   [Start Bit] [Arbitration Field] [Control Field] [Data Field] [CRC Field] [ACK Field] [End Field]
   [1 bit]     [29 bits]           [6 bits]        [0-64 bits]  [16 bits]   [2 bits]    [7 bits]
   ```
   - Follow-up: Arbitration: 11-bit identifier (standard), 29-bit (extended).
   - Follow-up: Data: 0-8 bytes.
   - Follow-up: CRC: 15-bit polynomial (error detection).
   - Follow-up: Dominant/recessive bits.

4. What is CAN identifier (ID)?
   - Follow-up: Standard CAN: 11-bit ID (0x000–0x7FF, 2048 possible).
   - Follow-up: Extended CAN: 29-bit ID (0x00000000–0x1FFFFFFF).
   - Follow-up: Lower ID = higher priority (dominates arbitration).
   - Follow-up: Example: ID = 0x123, priority = high. ID = 0x500, priority = low.

5. What is dominant and recessive in CAN?
   - Follow-up: Dominant: drives CAN_H high, CAN_L low (logic 0).
   - Follow-up: Recessive: both lines float to recessive voltage (logic 1).
   - Follow-up: Dominant overrides recessive (wired-AND).
   - Follow-up: Node with lower ID wins arbitration (its dominant bits override others' recessive).

6. CAN arbitration?
   - Follow-up: Nodes transmit simultaneously.
   - Follow-up: Bit-by-bit comparison of identifiers.
   - Follow-up: If node's ID bit is recessive but bus shows dominant, it loses arbitration and backs off.
   - Follow-up: No collision detection needed (like Ethernet).

7. How to transmit CAN message?
   ```c
   CAN_TxHeaderTypeDef TxHeader;
   uint8_t TxData[8] = {0x11, 0x22, 0x33, 0x44, 0x55, 0x66, 0x77, 0x88};
   uint32_t TxMailbox;

   TxHeader.StdId = 0x123;      // 11-bit ID
   TxHeader.ExtId = 0;
   TxHeader.IDE = CAN_ID_STD;   // standard ID
   TxHeader.RTR = CAN_RTR_DATA; // data frame (not remote)
   TxHeader.DLC = 8;            // 8 bytes of data
   TxHeader.TransmitGlobalTime = DISABLE;

   if (HAL_CAN_AddTxMessage(&hcan1, &TxHeader, TxData, &TxMailbox) != HAL_OK) {
       Error_Handler();
   }
   ```

8. How to receive CAN message?
   ```c
   CAN_RxHeaderTypeDef RxHeader;
   uint8_t RxData[8];
   uint32_t RxFifo = CAN_RX_FIFO0;

   if (HAL_CAN_GetRxMessage(&hcan1, RxFifo, &RxHeader, RxData) == HAL_OK) {
       // Process message
       uint32_t id = RxHeader.StdId;
       uint8_t dlc = RxHeader.DLC;
   }
   ```

9. CAN message filtering?
   - Follow-up: Mask + ID filter.
   - Follow-up: Example: mask = 0x7F0, ID = 0x120 → accepts 0x120–0x12F.
   - Follow-up: Hardware filtering reduces CPU load (kernel discards non-matching messages).

10. How to configure CAN baud rate?
    - Follow-up: Timing parameters: prescaler, time segment 1, time segment 2, SJW (synchronization jump width).
    - Follow-up: Formula: `Baud = CAN_CLK / (Prescaler * (1 + TS1 + TS2))`.
    - Follow-up: STM32 CubeMX auto-calculates timing for desired baud rate.

---

### Intermediate Level

11. What is CAN FD (Flexible Data-rate)?
    - Follow-up: Extended CAN protocol with up to 64 bytes per frame.
    - Follow-up: Two bit rates: arbitration phase (standard speed), data phase (higher speed).
    - Follow-up: Backward compatible with CAN 2.0 (physical layer).
    - Follow-up: Throughput improvement: 8 bytes at 1Mbps → 64 bytes at 5Mbps.

12. CAN 2.0 vs CAN FD?
    | Feature | CAN 2.0 | CAN FD |
    |---------|---------|--------|
    | Max data length | 8 bytes | 64 bytes |
    | Speeds | 125k–1M bps | 125k–1M (arb), 500k–5M (data) |
    | Frame format | Standard | Extended (BRS, ESI flags) |

    - Follow-up: When to use CAN FD? (high bandwidth, less overhead)

13. What is CAN-TP (Transport Protocol)?
    - Follow-up: Layer on top of CAN for messages > 8 bytes (or > 62 bytes for FD).
    - Follow-up: First frame + consecutive frames.
    - Follow-up: Flow control: sender waits for receiver's "continue" message.
    - Follow-up: Used in automotive diagnostics (UDS, OBD-II).

14. What is remote frame in CAN?
    - Follow-up: Request for data from another node (RTR bit set).
    - Follow-up: Remote frame has no data, receiver responds with data.
    - Follow-up: Rarely used in modern systems.

15. How to debug CAN bus?
    - Follow-up: CAN bus analyzer (hardware tool) to capture and decode frames.
    - Follow-up: Logic analyzer to view waveforms.
    - Follow-up: Oscilloscope to check differential voltage, signal integrity.
    - Follow-up: Software simulation tools (CANoe, Vector).

16. What causes CAN communication failure?
    - Termination resistors missing or incorrect value
    - CAN_H and CAN_L swapped
    - Improper baud rate
    - Identifier conflicts (multiple nodes same ID)
    - Insufficient shielding (noise)
    - CAN controller not enabled
    - Receiving node filtering out message ID

17. What is CAN error frame?
    - Follow-up: Node detects error, sends error frame (6 dominant bits).
    - Follow-up: Error counter incremented (RX error counter, TX error counter).
    - Follow-up: Error passive: counter > 127, node stops transmitting (can receive).
    - Follow-up: Bus off: counter > 255, node fully disconnected (must reset).

18. CAN error handling?
    ```c
    if (HAL_CAN_GetError(&hcan1) != HAL_CAN_ERROR_NONE) {
        uint32_t error = HAL_CAN_GetError(&hcan1);
        if (error & HAL_CAN_ERROR_EWG) {
            // Error warning (counter near limit)
        }
        if (error & HAL_CAN_ERROR_EPV) {
            // Error passive
        }
        if (error & HAL_CAN_ERROR_BOF) {
            // Bus off
        }
    }
    ```

---

### Advanced Level

19. How does CAN arbitration prevent collisions?
    - Follow-up: Bit-by-bit arbitration during identifier transmission.
    - Follow-up: Non-destructive collision (winning node completes frame).
    - Follow-up: Why CAN is deterministic (CSMA/CR vs CSMA/CD in Ethernet).

20. What is time-triggered CAN (TTCAN)?
    - Follow-up: Variant with time-based scheduling.
    - Follow-up: Synchronization between nodes via reference message.
    - Follow-up: Reduced latency variation (jitter).

21. What is ISO-CAN vs non-ISO CAN?
    - Follow-up: ISO (ISO 11898) defines physical layer, protocol.
    - Follow-up: Non-ISO: manufacturer-specific variations (rare now).

22. How to do CAN gateway (bridge)?
    - Follow-up: Node receiving on CAN1, transmitting on CAN2.
    - Follow-up: Filtering, ID remapping, rate conversion.

---
