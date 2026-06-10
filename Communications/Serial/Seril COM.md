# Embedded Communication Protocols Interview Questions
> UART, I2C, I3C, SPI, CAN, CANopen, Ethernet, Automotive Ethernet, 1-Wire, LIN, Modbus, etc.  
> M.Tech Graduate + 10 Years Experience  
> Basic → Intermediate → Advanced → Expert → War Stories

---

## UART (Universal Asynchronous Receiver Transmitter)

### Basic Level

1. What is UART?
   - Follow-up: Asynchronous serial communication protocol.
   - Follow-up: One TX line, one RX line (full-duplex).
   - Follow-up: No separate clock line — timing based on baud rate.
   - Follow-up: Common use: debug console, telemetry, GPS, serial sensors.

2. What is baud rate?
   - Follow-up: Bits per second (bps).
   - Follow-up: Common rates: 9600, 19200, 38400, 57600, 115200, 230400.
   - Follow-up: How is baud rate generated from clock? (`UART_CLK / (16 * BAUD)`)
   - Follow-up: Why 16x oversampling?

3. UART frame structure?
   ```
   [Idle] [Start Bit] [D0] [D1] [D2] [D3] [D4] [D5] [D6] [D7] [Parity] [Stop Bit(s)] [Idle]
   0      1          0    1    1    0    1    0    1    1    1        1            0
   ```
   - Follow-up: Start bit: 0 (transition from idle).
   - Follow-up: Data bits: typically 8, LSB first.
   - Follow-up: Parity: odd, even, or none.
   - Follow-up: Stop bits: 1 or 2.
   - Follow-up: Total bits per character: 10–12 bits.

4. How does UART timing work without clock line?
   - Follow-up: Both transmitter and receiver must use same baud rate.
   - Follow-up: Receiver samples at ~1.5 bit time from start bit edge.
   - Follow-up: Each bit sampled at center (roughly).
   - Follow-up: Baud rate error tolerance: typically ±5%.

5. What is parity in UART?
   - Follow-up: Odd parity: parity bit set so total 1s is odd.
   - Follow-up: Even parity: parity bit set so total 1s is even.
   - Follow-up: Parity is simple 1-bit error detection (not correction).
   - Follow-up: When would you use parity? (noisy environments, but rare in modern embedded)

6. What is flow control in UART?
   - Follow-up: Software flow control: XON/XOFF characters.
   - Follow-up: Hardware flow control: RTS/CTS (Request To Send / Clear To Send).
   - Follow-up: RTS: transmitter ready to send, CTS: receiver ready to accept.
   - Follow-up: Why flow control? (prevent buffer overflow in receiver)

---

### Intermediate Level

7. What is USART?
   - Follow-up: Universal Synchronous/Asynchronous Receiver Transmitter.
   - Follow-up: UART + synchronous mode (with clock).
   - Follow-up: In synchronous mode: CLK line sends clock, no baud rate needed.
   - Follow-up: When to use synchronous USART? (higher speed, clock provided by transmitter)

8. How to configure UART in embedded (STM32 example)?
   ```c
   USART_InitTypeDef USART_InitStructure;
   USART_InitStructure.USART_BaudRate = 115200;
   USART_InitStructure.USART_WordLength = USART_WordLength_8b;
   USART_InitStructure.USART_StopBits = USART_StopBits_1;
   USART_InitStructure.USART_Parity = USART_Parity_None;
   USART_InitStructure.USART_Mode = USART_Mode_Rx | USART_Mode_Tx;
   USART_InitStructure.USART_HardwareFlowControl = USART_HardwareFlowControl_None;
   USART_Init(USART1, &USART_InitStructure);
   USART_Cmd(USART1, ENABLE);
   ```

9. How to transmit and receive over UART?
   ```c
   // Transmit one byte
   while (USART_GetFlagStatus(USART1, USART_FLAG_TXE) == RESET);
   USART_SendData(USART1, 'A');

   // Receive one byte
   while (USART_GetFlagStatus(USART1, USART_FLAG_RXNE) == RESET);
   uint8_t data = USART_ReceiveData(USART1);
   ```

10. How to use UART with interrupts?
    ```c
    USART_ITConfig(USART1, USART_IT_RXNE, ENABLE);
    NVIC_EnableIRQ(USART1_IRQn);

    void USART1_IRQHandler(void) {
        if (USART_GetITStatus(USART1, USART_IT_RXNE) != RESET) {
            uint8_t data = USART_ReceiveData(USART1);
            // process data
        }
    }
    ```

11. How to use UART with DMA?
    - Follow-up: DMA copies data without CPU intervention.
    - Follow-up: Setup: `DMA_InitStructure` for `USART1->DR`.
    - Follow-up: DMA ISR on completion.
    - Follow-up: Advantage: CPU free to do other work.

12. What is baud rate error?
    - Follow-up: Example: desired 115200, actual 114942 (error = 0.22%).
    - Follow-up: Formula: `Error% = abs(actual - desired) / desired * 100`.
    - Follow-up: Safe limit: < ±5% for reliable operation.

13. How to debug UART communication?
    - Follow-up: USB-to-UART adapter + terminal program (PuTTY, TeraTerm, minicom).
    - Follow-up: Logic analyzer to verify waveform, timing, baud rate.
    - Follow-up: Oscilloscope to check signal integrity, noise.

---

### Advanced Level

14. What is FIFO in UART?
    - Follow-up: TX FIFO and RX FIFO (typically 16 or 32 bytes deep).
    - Follow-up: Threshold for interrupt triggering: 1/4, 1/2, 3/4, full.
    - Follow-up: Reduces interrupt frequency when using DMA/interrupts.

15. How to implement circular buffer for UART RX?
    ```c
    #define RX_BUFFER_SIZE 256
    static uint8_t rx_buffer[RX_BUFFER_SIZE];
    static volatile uint16_t rx_head = 0, rx_tail = 0;

    void USART1_IRQHandler(void) {
        if (USART_GetITStatus(USART1, USART_IT_RXNE) != RESET) {
            rx_buffer[rx_head] = USART_ReceiveData(USART1);
            rx_head = (rx_head + 1) % RX_BUFFER_SIZE;
            if (rx_head == rx_tail) {
                // buffer overflow
            }
        }
    }

    uint8_t get_byte(void) {
        while (rx_tail == rx_head);  // wait for data
        uint8_t byte = rx_buffer[rx_tail];
        rx_tail = (rx_tail + 1) % RX_BUFFER_SIZE;
        return byte;
    }
    ```

16. What is line ending issue in UART?
    - Follow-up: Different line endings: `\r` (carriage return), `\n` (line feed), `\r\n` (CRLF).
    - Follow-up: Terminal program confusion — some convert automatically.
    - Follow-up: When writing embedded code receiving UART: handle all variants.

17. How to do UART at high speed (>1Mbps)?
    - Follow-up: Crystal accuracy becomes critical (error must be <1%).
    - Follow-up: Noise becomes issue — need good PCB layout, shielding.
    - Follow-up: Alternative: use SPI for higher speeds, or use UART with very high clock.

18. What is UART echo and how to implement?
    ```c
    void echo_task(void) {
        while (1) {
            uint8_t byte = get_byte();  // wait for RX
            send_byte(byte);             // echo back TX
        }
    }
    ```
    - Follow-up: Useful for debugging — user sees what they typed.

---

### Expert Level

19. What causes UART communication failure?
    - Baud rate mismatch
    - Inverted RX/TX lines
    - No ground connection
    - Voltage level mismatch (3.3V vs 5V)
    - Noise on lines
    - Buffer overflow
    - Parity error
    - Framing error (stop bit not detected)

20. How to detect UART framing errors?
    - Follow-up: `USART_FLAG_FE` (Framing Error flag).
    - Follow-up: Indicates stop bit was 0 when expecting 1.
    - Follow-up: Causes: baud rate mismatch, noise, timing skew.

21. How to recover from UART errors?
    ```c
    if (USART_GetFlagStatus(USART1, USART_FLAG_FE) == SET) {
        USART_ClearFlag(USART1, USART_FLAG_FE);
        // resync or request retransmission
    }
    ```

---

## I2C (Inter-Integrated Circuit)

### Basic Level

1. What is I2C?
   - Follow-up: 2-wire serial bus: SDA (data), SCL (clock).
   - Follow-up: Multi-master, multi-slave capability.
   - Follow-up: Open-drain outputs (pull-up resistors required).
   - Follow-up: Speeds: 100kHz (standard), 400kHz (fast), 1MHz (fast-plus).
   - Follow-up: Common use: sensors, EEPROMs, RTC, power management ICs.

2. I2C hardware (electrical)?
   ```
   Master                 Slave
   SDA ----+----[Rp]----+---- SDA
           |            |
   SCL ----+----[Rp]----+---- SCL
           |            |
           +---- GND ----+
   ```
   - Follow-up: Pull-up resistors: typically 4.7kΩ on 5V, 10kΩ on 3.3V.
   - Follow-up: Why open-drain? (allows multi-master, wire-OR logic)
   - Follow-up: What is clock stretching? (slave pulls SCL low to pause)

3. I2C addressing?
   - Follow-up: 7-bit address (128 possible, 112 usable) or 10-bit address.
   - Follow-up: Address format: `<7-bit address> <R/W bit>`.
   - Follow-up: R/W bit: 0 = write, 1 = read.
   - Follow-up: Special addresses: 0x00 (general call), 0x78–0x7F (reserved).

4. I2C protocol frame?
   ```
   [START] [Address + R/W] [ACK] [Data Byte] [ACK] ... [STOP]
   ```
   - START: SDA goes low while SCL high.
   - Address 7-bit: MSB first.
   - ACK: slave pulls SDA low during 9th clock.
   - NACK: slave releases SDA (master can detect missing slave).
   - STOP: SDA goes high while SCL high.

5. How to transmit over I2C (write)?
   ```c
   // Write 1 byte to address 0x50
   uint8_t addr = 0x50 << 1;  // 7-bit addr, shift for I2C format
   uint8_t data = 0xAA;
   
   i2c_start();
   i2c_send_byte(addr | 0);  // address + write bit
   if (!i2c_receive_ack()) goto error;
   i2c_send_byte(data);
   if (!i2c_receive_ack()) goto error;
   i2c_stop();
   ```

6. How to receive over I2C (read)?
   ```c
   // Read 1 byte from address 0x50
   uint8_t addr = 0x50 << 1;
   uint8_t data;
   
   i2c_start();
   i2c_send_byte(addr | 1);  // address + read bit
   if (!i2c_receive_ack()) goto error;
   data = i2c_receive_byte();
   i2c_send_nack();  // no more data
   i2c_stop();
   ```

7. I2C repeated START condition?
   - Follow-up: Master can restart without releasing bus (no STOP).
   - Follow-up: Allows write-then-read without releasing bus.
   - Follow-up: Used in: read from address register, then read data.

8. How to read register from I2C device?
   ```c
   // Read register 0x10 from device 0x50
   uint8_t data;
   
   i2c_start();
   i2c_send_byte(0x50 << 1 | 0);  // address + write
   i2c_receive_ack();
   i2c_send_byte(0x10);            // register address
   i2c_receive_ack();
   
   // Repeated START
   i2c_start();
   i2c_send_byte(0x50 << 1 | 1);  // address + read
   i2c_receive_ack();
   data = i2c_receive_byte();
   i2c_send_nack();
   i2c_stop();
   ```

---

### Intermediate Level

9. How to implement I2C bit-banging (software I2C)?
   ```c
   #define SCL_PORT  GPIOB
   #define SCL_PIN   GPIO_PIN_10
   #define SDA_PORT  GPIOB
   #define SDA_PIN   GPIO_PIN_11

   void i2c_scl_low(void) {
       GPIO_ResetBits(SCL_PORT, SCL_PIN);  // drive low
   }

   void i2c_scl_high(void) {
       GPIO_SetBits(SCL_PORT, SCL_PIN);   // release (pull-up)
   }

   int i2c_scl_read(void) {
       return GPIO_ReadInputDataBit(SCL_PORT, SCL_PIN);
   }

   // similar for SDA
   ```
   - Follow-up: Must handle clock stretching (wait if slave holds SCL low).
   - Follow-up: Timing delays between SCL/SDA changes for clock stretching.

10. What is I2C clock stretching?
    - Follow-up: Slave pulls SCL low to pause master.
    - Follow-up: Master detects SCL = 0, waits until slave releases.
    - Follow-up: Slave uses this to slow down communication if needed.
    - Follow-up: How to implement: after releasing SCL, wait for it to go high.

11. What is I2C timeout?
    - Follow-up: If slave never releases SCL, bus deadlock.
    - Follow-up: Implementation: count clock cycles, reset if timeout.
    - Follow-up: Bus recovery: master toggles clock, then STOP.

12. I2C on microcontroller (STM32 example)?
    ```c
    I2C_InitTypeDef I2C_InitStructure;
    I2C_InitStructure.I2C_ClockSpeed = 100000;
    I2C_InitStructure.I2C_Mode = I2C_Mode_I2C;
    I2C_InitStructure.I2C_OwnAddress1 = 0x50;
    I2C_InitStructure.I2C_Ack = I2C_Ack_Enable;
    I2C_InitStructure.I2C_AcknowledgedAddress = I2C_AcknowledgedAddress_7bit;
    I2C_Init(I2C1, &I2C_InitStructure);
    I2C_Cmd(I2C1, ENABLE);
    ```

13. How to use I2C with interrupts?
    - Follow-up: Events: `I2C_IT_EVT` (events), `I2C_IT_BUF` (buffer), `I2C_IT_ERR` (errors).
    - Follow-up: Interrupt handler decodes event type and responds.

14. I2C multi-byte transaction?
    ```c
    // Write 3 bytes to device
    i2c_start();
    i2c_send_byte(0x50 << 1);
    i2c_receive_ack();
    i2c_send_byte(0xBB);
    i2c_receive_ack();
    i2c_send_byte(0xCC);
    i2c_receive_ack();
    i2c_send_byte(0xDD);
    i2c_receive_ack();
    i2c_stop();
    ```

15. How to detect I2C bus stuck?
    - Follow-up: SDA = low, SCL = high (slave holding SDA).
    - Follow-up: Recovery: master toggles SCL up to 9 times (clocking out stuck byte), then STOP.

16. What is I2C slave mode?
    - Follow-up: Microcontroller as I2C slave (device).
    - Follow-up: Interrupts on: address match, RX data, TX request.
    - Follow-up: Slave must ACK received bytes, provide data when read.

---

### Advanced Level

17. What is I3C (Improved Inter-Integrated Circuit)?
    - Follow-up: I2C successor, higher speed, backward compatible.
    - Follow-up: Speeds: 12.5MHz (typical), up to 25MHz (5MHz I2C equivalent standard).
    - Follow-up: Open-drain push-pull (OD-PP) for higher speed.
    - Follow-up: Dynamic address assignment replaces static 7-bit address.
    - Follow-up: In-Band Interrupts (IBI) — slave can interrupt master.

18. I3C vs I2C?
    | Feature | I2C | I3C |
    |---------|-----|-----|
    | Speed | 100–1000 kHz | 2.7–25 MHz |
    | Addresses | Static 7/10-bit | Dynamic |
    | Interrupts | No | Yes (IBI) |
    | Backward compat | N/A | Yes (I2C legacy devices) |
    | Power | Low | Higher |

    - Follow-up: When to use I3C? (sensor hubs, high-speed data collection)

19. How to debug I2C issues?
    - Follow-up: Logic analyzer to verify START, STOP, ACK/NACK, data waveforms.
    - Follow-up: Oscilloscope to check signal levels, noise, rise time.
    - Follow-up: Check pull-up resistor values.
    - Follow-up: Verify clock stretching behavior.
    - Follow-up: Test with known-good master and slave.

20. What causes I2C failure?
    - Incorrect pull-up resistor values
    - No pull-up resistors
    - Bus capacitance too high (cable too long, many devices)
    - Slave not ACKing (wrong address, slave not responding)
    - Master not recognizing slave address
    - Clock stretching not implemented
    - Noise on lines

21. How to calculate I2C pull-up resistor?
    - Follow-up: Constraint: rise time < 1000ns (for 100kHz), < 300ns (for 400kHz).
    - Follow-up: Formula: `Rp < 0.8473 * (1000ns - tr) / C_total`
    - Follow-up: Typical: 4.7kΩ (5V), 10kΩ (3.3V).
    - Follow-up: Too low: more power, faster rise. Too high: slower rise (timing violation).

---

## SPI (Serial Peripheral Interface)

### Basic Level

1. What is SPI?
   - Follow-up: Synchronous serial protocol.
   - Follow-up: 4-wire: SCLK (clock), MOSI (Master Out Slave In), MISO (Master In Slave Out), CS (Chip Select).
   - Follow-up: Master-only protocol (one master, one or more slaves).
   - Follow-up: Full-duplex communication.
   - Follow-up: Speeds: up to 50MHz+ on embedded.
   - Follow-up: Common use: memory (Flash, EEPROM), sensors, display controllers.

2. SPI hardware connection?
   ```
   Master                  Slave1              Slave2
   SCLK  ------+-------+----SCLK             (parallel)
               |       |
   MOSI  ------+-------+----MOSI
               |       |
   MISO  ------+-------+----MISO
               |       |
   CS0   ------+-------+----CS
               
   CS1   ------+-------+----CS
   ```
   - Follow-up: Each slave has dedicated CS line (one master can control multiple slaves).
   - Follow-up: MOSI/MISO shared across all slaves.
   - Follow-up: Only slave with CS = 0 responds.

3. SPI modes (CPHA, CPOL)?
   - Follow-up: CPOL = 0: idle SCL = low, CPOL = 1: idle SCL = high.
   - Follow-up: CPHA = 0: data captured on leading edge, CPHA = 1: data captured on trailing edge.
   - Follow-up: Modes: SPI_MODE_0 (CPOL=0, CPHA=0), ..., SPI_MODE_3 (CPOL=1, CPHA=1).

   | Mode | CPOL | CPHA | Leading Edge | Trailing Edge |
   |------|------|------|--------------|---------------|
   | 0 | 0 | 0 | Capture | Setup |
   | 1 | 0 | 1 | Setup | Capture |
   | 2 | 1 | 0 | Capture | Setup |
   | 3 | 1 | 1 | Setup | Capture |

4. How does SPI transfer work?
   - Follow-up: Shift register — bits shifted in on MOSI, out on MISO, synchronized to SCLK.
   - Follow-up: Full-duplex: master sends byte while slave sends byte back.
   - Follow-up: CS must be held low during transfer, released after.

5. How to transmit/receive over SPI (bit-bang)?
   ```c
   void spi_cs_low(void) {
       GPIO_ResetBits(CS_PORT, CS_PIN);
   }

   void spi_cs_high(void) {
       GPIO_SetBits(CS_PORT, CS_PIN);
   }

   uint8_t spi_transfer(uint8_t byte_out) {
       uint8_t byte_in = 0;
       for (int i = 7; i >= 0; i--) {
           // Setup MOSI
           if (byte_out & (1 << i)) {
               GPIO_SetBits(MOSI_PORT, MOSI_PIN);
           } else {
               GPIO_ResetBits(MOSI_PORT, MOSI_PIN);
           }
           
           // Clock pulse
           GPIO_SetBits(SCLK_PORT, SCLK_PIN);
           delay_ns(10);
           
           // Read MISO
           if (GPIO_ReadInputDataBit(MISO_PORT, MISO_PIN)) {
               byte_in |= (1 << i);
           }
           
           GPIO_ResetBits(SCLK_PORT, SCLK_PIN);
           delay_ns(10);
       }
       return byte_in;
   }

   // Usage
   spi_cs_low();
   uint8_t response = spi_transfer(0xAA);
   spi_cs_high();
   ```

6. How to use SPI peripheral on microcontroller?
   ```c
   SPI_InitTypeDef SPI_InitStructure;
   SPI_InitStructure.SPI_Direction = SPI_Direction_2Lines_FullDuplex;
   SPI_InitStructure.SPI_Mode = SPI_Mode_Master;
   SPI_InitStructure.SPI_DataSize = SPI_DataSize_8b;
   SPI_InitStructure.SPI_CPOL = SPI_CPOL_Low;
   SPI_InitStructure.SPI_CPHA = SPI_CPHA_1Edge;
   SPI_InitStructure.SPI_NSS = SPI_NSS_Soft;  // software CS
   SPI_InitStructure.SPI_BaudRatePrescaler = SPI_BaudRatePrescaler_16;
   SPI_Init(SPI1, &SPI_InitStructure);
   SPI_Cmd(SPI1, ENABLE);
   ```

7. How to transfer over SPI peripheral?
   ```c
   // Transmit
   while (SPI_GetFlagStatus(SPI1, SPI_FLAG_TXE) == RESET);
   SPI_SendData(SPI1, 0xAA);

   // Receive
   while (SPI_GetFlagStatus(SPI1, SPI_FLAG_RXNE) == RESET);
   uint8_t data = SPI_ReceiveData(SPI1);

   // Full-duplex transfer
   while (SPI_GetFlagStatus(SPI1, SPI_FLAG_TXE) == RESET);
   SPI_SendData(SPI1, 0xAA);
   while (SPI_GetFlagStatus(SPI1, SPI_FLAG_RXNE) == RESET);
   uint8_t response = SPI_ReceiveData(SPI1);
   ```

---

### Intermediate Level

8. How to use SPI with DMA?
   - Follow-up: DMA copies data to TX FIFO, copies RX FIFO to memory.
   - Follow-up: Interrupt when transfer complete.
   - Follow-up: Much faster than polled or interrupt-driven approach.

9. SPI transaction protocol (multiple bytes)?
   ```c
   // Read register 0x10 from SPI device
   uint8_t cmd = 0x03;        // read command
   uint8_t addr = 0x10;       // register address
   uint8_t data_out[2] = {0}; // don't care during read

   spi_cs_low();
   spi_transfer(cmd);   // send command
   spi_transfer(addr);  // send address
   data_out[0] = spi_transfer(0xFF);  // receive high byte
   data_out[1] = spi_transfer(0xFF);  // receive low byte
   spi_cs_high();
   ```

10. What is quad SPI (QSPI)?
    - Follow-up: 4-line data transfer instead of 1 (MOSI + MISO).
    - Follow-up: 4x faster than standard SPI (if all 4 lines used).
    - Follow-up: Typically used for Flash memory.
    - Follow-up: Backward compatible: can start in SPI mode, switch to QSPI.

11. What is dual SPI (DSPI)?
    - Follow-up: 2-line data transfer.
    - Follow-up: 2x faster than standard SPI.

12. SPI slave mode?
    - Follow-up: Microcontroller as SPI slave.
    - Follow-up: Must respond to CS and SCLK from master.
    - Follow-up: Interrupt when CS = 0, data received, or data requested.

13. How to debug SPI?
    - Follow-up: Logic analyzer to verify CS, SCLK, MOSI, MISO waveforms.
    - Follow-up: Oscilloscope for signal integrity, noise, timing.
    - Follow-up: Check clock polarity and phase match device datasheet.

14. What causes SPI failure?
    - Wrong clock mode (CPOL/CPHA)
    - Clock speed too high
    - CS toggling during transfer
    - Data setup/hold time violation
    - Improper termination (floating lines)
    - Capacitive coupling, noise

---

### Advanced Level

15. How to handle variable-length SPI transactions?
    - Follow-up: Some devices require different TX/RX lengths.
    - Follow-up: Send dummy bytes if need to clock out data without sending.
    - Follow-up: Use DMA with different lengths for TX and RX.

16. What is SPI bus contention?
    - Follow-up: Multiple masters driving same line.
    - Follow-up: SPI is not multi-master safe (unlike I2C).
    - Follow-up: Careful CS management required.

17. How to implement multi-slave SPI?
    - Follow-up: Each slave has dedicated CS line.
    - Follow-up: Pull CS low for target slave, high for others.
    - Follow-up: Daisy-chain: slaves connected in series, data shifts through.
    ```
    Master MOSI -----> Slave1 MOSI (output shift register)
                            |
                       Slave2 MOSI (input to shift register)
                            |
                       Slave3 MOSI ...
    ```

18. SPI timing constraints?
    - Follow-up: Setup time: data valid before clock edge.
    - Follow-up: Hold time: data valid after clock edge.
    - Follow-up: Clock to output delay: how long after clock before slave drives MISO.
    - Follow-up: All constraints in device datasheet.

---

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

## Modbus (RTU, ASCII, TCP)

### Basic Level

1. What is Modbus?
   - Follow-up: Industrial protocol for SCADA/PLC systems.
   - Follow-up: Master-slave model.
   - Follow-up: Variants: RTU (binary, UART), ASCII (text, UART), TCP (Ethernet).

2. Modbus RTU frame?
   - Follow-up: Slave address (1 byte), function code (1 byte), data (N bytes), CRC (2 bytes).
   - Follow-up: CRC-16 for error detection.

3. Common Modbus function codes?
   - 01: Read coils (discrete outputs).
   - 02: Read discrete inputs.
   - 03: Read holding registers.
   - 04: Read input registers.
   - 05: Write single coil.
   - 06: Write single register.

4. How to implement Modbus master/slave?
   - Follow-up: Libraries available: libmodbus (open-source).
   - Follow-up: Or implement custom protocol stack.

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