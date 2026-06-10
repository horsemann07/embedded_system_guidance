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
