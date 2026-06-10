# Embedded Communication Protocols Interview Questions
> UART, I2C, I3C, SPI, CAN, CANopen, Ethernet, Automotive Ethernet, 1-Wire, LIN, Modbus, etc.  
> M.Tech Graduate + 10 Years Experience  
> Basic → Intermediate → Advanced → Expert → War Stories

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

