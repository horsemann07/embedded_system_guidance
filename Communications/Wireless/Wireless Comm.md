# Wireless Communication Protocols Interview Questions
> WiFi, BLE, Zigbee, LoRaWAN, NB-IoT, LTE-M, 5G, Thread, NRF24L01, etc.
> M.Tech Graduate + 10 Years Experience
> Basic → Intermediate → Advanced → Expert → War Stories

---

## WiFi (802.11 a/b/g/n/ac/ax)

### Basic Level

1. What is WiFi and how does it differ from wired Ethernet?

2. What are the main WiFi standards and their speeds?

3. What is the 2.4GHz vs 5GHz frequency band in WiFi?

4. What is SSID and how does WiFi discovery work?

5. What is WPA/WPA2/WPA3 and why is encryption important?

6. How does WiFi handle multiple devices on same frequency?

7. What is the WiFi frame structure?

8. How to configure WiFi on embedded systems (ESP32 example)?

---

### Intermediate Level

9. What is MIMO (Multiple Input Multiple Output) in WiFi?

10. What is channel bandwidth (20MHz, 40MHz, 80MHz, 160MHz)?

11. How does WiFi roaming work between access points?

12. What is WiFi power saving mode (sleep/wake)?

13. How to implement WiFi in IoT devices with battery constraints?

14. What is 802.11ac vs 802.11ax (WiFi 6)?

15. How does WiFi handle interference from other 2.4GHz devices?

16. What is the WiFi MAC layer and CSMA/CA?

---

### Advanced Level

17. How does WiFi beamforming improve range and speed?

18. What is MU-MIMO and how does it improve throughput?

19. How to debug WiFi connectivity issues?

20. What causes WiFi disconnection and how to recover?

21. How to implement WiFi mesh networks?

22. What is WiFi Direct (peer-to-peer)?

---

### Expert Level

23. What is 802.11w (Management Frame Protection)?

24. How does WiFi 6E utilize 6GHz band?

25. What is OFDMA and how does it improve efficiency?

26. How to optimize WiFi power consumption in embedded devices?

27. What are security vulnerabilities in WiFi and mitigation?

---

## Bluetooth / Bluetooth Low Energy (BLE)

### Basic Level

1. What is Bluetooth and what are its main versions?

2. What is BLE (Bluetooth Low Energy) and how does it differ from Classic Bluetooth?

3. What is the Bluetooth frequency band and how many channels?

4. What is Bluetooth advertising and how does device discovery work?

5. What is GATT (Generic Attribute Profile) and GAP (Generic Access Profile)?

6. What is a Bluetooth characteristic and descriptor?

7. How to configure BLE on embedded systems (nRF52 example)?

8. What is MTU (Maximum Transmission Unit) in BLE?

---

### Intermediate Level

9. What is Bluetooth connection parameters (interval, latency, timeout)?

10. How does BLE handle multi-slave connections?

11. What is Bluetooth bonding and pairing?

12. How to implement custom BLE service?

13. What is BLE advertising modes (connectable, scannable, non-connectable)?

14. How to optimize BLE power consumption?

15. What is BLE whitelist and filtering?

16. How does BLE handle interference from WiFi?

---

### Advanced Level

17. What is Bluetooth mesh networking?

18. How does BLE security work (encryption, authentication)?

19. What is Just Works, Numeric Comparison, Passkey Entry pairing?

20. How to implement BLE over multiple profiles?

21. What is BLE GATT caching and data persistence?

22. How to debug BLE communication issues?

---

### Expert Level

23. What is Bluetooth 5.0+ features (extended range, higher speed)?

24. How does BLE handle QoS (Quality of Service)?

25. What is LE Advertising Extensions?

26. How to implement BLE device coordinated for IoT?

27. What are BLE security vulnerabilities and mitigation strategies?

---

## Zigbee

### Basic Level

1. What is Zigbee and its primary use case?

2. What frequency bands does Zigbee operate on?

3. What is the Zigbee network topology (star, tree, mesh)?

4. What is Zigbee addressing (short address vs IEEE address)?

5. What is Zigbee frame structure?

6. What are Zigbee device roles (coordinator, router, end device)?

7. How does Zigbee joining and formation work?

8. What is Zigbee cluster and attribute?

---

### Intermediate Level

9. How does Zigbee routing work?

10. What is Zigbee security and how to configure it?

11. How to handle Zigbee device pairing?

12. What is Zigbee OTA (Over-The-Air) update?

13. How to implement Zigbee on embedded systems?

14. What is Zigbee endpoint and profile?

15. How does Zigbee handle network congestion?

16. What is Zigbee inter-PAN communication?

---

### Advanced Level

17. What is Green Power profile in Zigbee?

18. How to implement Zigbee gateway/bridge?

19. What causes Zigbee network instability and how to debug?

20. How to optimize Zigbee for battery-powered devices?

21. What is Zigbee PRO vs Zigbee 3.0?

22. How to implement Zigbee security policies?

---

### Expert Level

23. What is Zigbee coordinator backup and recovery?

24. How does Zigbee handle network link key rotation?

25. What is Zigbee trust center and key establishment?

26. How to implement Zigbee for large-scale deployments?

27. What are Zigbee interoperability challenges?

---

## LoRaWAN

### Basic Level

1. What is LoRa and LoRaWAN?

2. What is LoRa modulation and spread spectrum?

3. What frequency bands does LoRaWAN use (ISM bands)?

4. What is the difference between LoRa and LoRaWAN?

5. What is LoRaWAN network architecture (gateway, network server, application)?

6. What are LoRaWAN device classes (A, B, C)?

7. How does LoRaWAN addressing work (DevAddr, AppEUI, DevEUI)?

8. What is LoRaWAN frame structure?

---

### Intermediate Level

9. How does LoRa modulation improve range and power?

10. What is LoRaWAN adaptive data rate (ADR)?

11. How does LoRaWAN security work (encryption, authentication)?

12. What is LoRaWAN join procedure (OTAA vs ABP)?

13. How to implement LoRaWAN on embedded (SX127x example)?

14. What is LoRaWAN duty cycle and how to comply?

15. How does LoRaWAN handle downlink messages?

16. What is LoRaWAN Class B scheduled receive?

---

### Advanced Level

17. What is LoRa gateway backhaul (Ethernet, cellular)?

18. How to implement LoRa mesh networks?

19. What causes LoRaWAN connection failures?

20. How to optimize LoRaWAN for maximum range?

21. What is LoRaWAN Class C continuous receive?

22. How to debug LoRaWAN communication?

---

### Expert Level

23. What is LoRa geolocation using time difference of arrival (TDOA)?

24. How does LoRaWAN handle network clock sync?

25. What is LoRa modulation parameters impact on range/power?

26. How to implement LoRaWAN for wide-area IoT deployment?

27. What are LoRaWAN scalability challenges?

---

## NB-IoT (Narrowband IoT)

### Basic Level

1. What is NB-IoT and its primary applications?

2. How does NB-IoT differ from LTE and LTE-M?

3. What is the bandwidth of NB-IoT?

4. What is NB-IoT network architecture?

5. How does NB-IoT provide extended coverage?

6. What is NB-IoT power saving mode (PSM)?

7. What is eDRX (extended Discontinuous Reception)?

8. What is NB-IoT device addressing and identification?

---

### Intermediate Level

9. How does NB-IoT repetition improve reliability?

10. What is NB-IoT packet structure and frame format?

11. How to implement NB-IoT on embedded (modem interface)?

12. What is NB-IoT CoAP vs HTTP protocols?

13. How does NB-IoT security work (encryption, authentication)?

14. What is NB-IoT registration and attachment process?

15. How to handle NB-IoT latency constraints?

16. What is NB-IoT device triggered discontinuous reception (DRX)?

---

### Advanced Level

17. How does NB-IoT handle mobility and handover?

18. What causes NB-IoT connection issues and recovery?

19. How to optimize NB-IoT for long battery life?

20. What is NB-IoT QoS (Quality of Service)?

21. How to implement NB-IoT firmware update mechanism?

22. How to debug NB-IoT connectivity issues?

---

### Expert Level

23. What is NB-IoT uplink/downlink timing and synchronization?

24. How does NB-IoT handle congestion in dense deployments?

25. What is NB-IoT random access procedure?

26. How to implement NB-IoT for critical IoT applications?

27. What are NB-IoT scalability and spectrum efficiency considerations?

---

## LTE-M (LTE Category M1)

### Basic Level

1. What is LTE-M (LTE Category M1)?

2. How does LTE-M differ from NB-IoT?

3. What is LTE-M bandwidth and data rates?

4. What is LTE-M use case vs NB-IoT?

5. What is LTE-M power saving features?

6. How does LTE-M provide extended coverage?

7. What is LTE-M network architecture?

8. What is LTE-M device categories?

---

### Intermediate Level

9. How does LTE-M handle voice (VoLTE)?

10. What is LTE-M handover and mobility?

11. How to implement LTE-M on embedded systems?

12. What is LTE-M QoS and bearer management?

13. How does LTE-M security work?

14. What is LTE-M CoAP and HTTP protocols?

15. How to optimize LTE-M for battery efficiency?

16. What is LTE-M NFVI (Network Function Virtualization)?

---

### Advanced Level

17. What causes LTE-M connection failures?

18. How to debug LTE-M connectivity issues?

19. How does LTE-M handle congestion control?

20. What is LTE-M spectrum sharing with LTE?

21. How to implement LTE-M firmware OTA update?

22. What is LTE-M backhaul architecture?

---

### Expert Level

23. What is LTE-M vs LTE Cat-4 trade-offs?

24. How does LTE-M handle network slicing?

25. What is LTE-M resource allocation and scheduling?

26. How to implement LTE-M for mission-critical IoT?

27. What are LTE-M future evolution considerations?

---

## 5G (NR - New Radio)

### Basic Level

1. What is 5G NR and how does it differ from LTE?

2. What are 5G frequency bands (sub-6GHz, mmWave)?

3. What is 5G network slicing?

4. What is 5G ultra-reliable low-latency communication (URLLC)?

5. What is 5G enhanced mobile broadband (eMBB)?

6. What is 5G massive machine type communication (mMTC)?

7. What is 5G network architecture (gNB, 5GC)?

8. What is 5G device categories?

---

### Intermediate Level

9. How does 5G massive MIMO work?

10. What is 5G beam management and beam reporting?

11. How does 5G handle inter-band and intra-band carrier aggregation?

12. What is 5G power saving (RRC connected/idle mode)?

13. How does 5G security work (encryption, authentication)?

14. What is 5G QoS flow management?

15. How to implement 5G on embedded systems?

16. What is 5G sidelink (device-to-device) communication?

---

### Advanced Level

17. What causes 5G connection failures?

18. How to debug 5G NR connectivity issues?

19. What is 5G spectrum sharing with LTE?

20. How does 5G handle mobility and handover?

21. How to optimize 5G power consumption?

22. What is 5G network exposure function (NEF)?

---

### Expert Level

23. What is 5G RAN slicing and network slicing orchestration?

24. How does 5G mmWave handle blockage and reflection?

25. What is 5G positioning and location services?

26. How to implement 5G for mission-critical applications?

27. What are 5G advanced features (predictive analytics, AI integration)?

---

## Thread

### Basic Level

1. What is Thread and its primary use case?

2. What is Thread network topology (mesh)?

3. What frequency band does Thread use?

4. What is Thread addressing (extended vs short)?

5. What is Thread device roles (leader, router, child)?

6. How does Thread joining work?

7. What is Thread frame structure?

8. What are Thread clusters?

---

### Intermediate Level

9. How does Thread routing work?

10. What is Thread border router (IPv6 gateway)?

11. What is Thread security and credential establishment?

12. How to implement Thread on embedded (OpenThread)?

13. What is Thread CoAP for application layer?

14. How does Thread handle network partition?

15. What is Thread commissioner role?

16. How to optimize Thread for battery devices?

---

### Advanced Level

17. What is Thread network formation and recovery?

18. How to debug Thread connectivity issues?

19. What is Thread energy-efficient multicast forwarding (EEMF)?

20. How does Thread integrate with WiFi (Thread + WiFi)?

21. How to implement Thread gateway functionality?

22. What is Thread interoperability with other protocols?

---

### Expert Level

23. What is Thread FTD vs MTD architecture?

24. How does Thread handle large-scale networks?

25. What is Thread commissioning at scale?

26. How to implement Thread for complex smart home?

27. What are Thread security vulnerabilities and mitigation?

---

## NRF24L01 (2.4GHz RF Module)

### Basic Level

1. What is NRF24L01 and its applications?

2. What is NRF24L01 frequency band and data rates?

3. What is NRF24L01 basic hardware connection?

4. What is NRF24L01 SPI interface requirements?

5. What is NRF24L01 addressing (pipes and addresses)?

6. What is NRF24L01 TX and RX modes?

7. What is NRF24L01 payload structure?

8. How to configure NRF24L01 via registers?

---

### Intermediate Level

9. What is NRF24L01 power amplification modes?

10. How does NRF24L01 handle multiple transmitters?

11. What is NRF24L01 automatic retransmission (ARD)?

12. How to implement NRF24L01 in embedded systems?

13. What is NRF24L01 interrupt handling?

14. How does NRF24L01 FIFO buffering work?

15. What is NRF24L01 enhanced shockburst (ESB)?

16. How to optimize NRF24L01 power consumption?

---

### Advanced Level

17. What causes NRF24L01 communication failures?

18. How to debug NRF24L01 connectivity?

19. What is NRF24L01 antenna design impact?

20. How to implement NRF24L01 mesh networking?

21. What is NRF24L01 range extension techniques?

22. How to handle NRF24L01 interference and coexistence?

---

### Expert Level

23. What is NRF24L01 dynamic payload and dynamic ACK?

24. How does NRF24L01 CRC error detection work?

25. What is NRF24L01 carrier detection and collision avoidance?

26. How to implement reliable NRF24L01 protocol layer?

27. What are NRF24L01 limitations in modern IoT?

---

## ISM Band Wireless Basics

### Basic Level

1. What are ISM bands and which frequencies?

2. What is open spectrum vs licensed spectrum?

3. What are regulations (FCC, ETSI, ISED) for ISM bands?

4. What is frequency hopping for interference avoidance?

5. What is spread spectrum and why use it?

6. What is modulation schemes (FSK, GFSK, PSK)?

7. What is transmit power limits in ISM bands?

8. What is coexistence between WiFi, BLE, Zigbee on 2.4GHz?

---

### Intermediate Level

9. How to implement frequency hopping in wireless devices?

10. What is channel assessment and interference detection?

11. How to implement LBT (Listen Before Talk)?

12. What is time-domain multiplexing for coexistence?

13. How to measure RSSI (Received Signal Strength)?

14. What is link margin and fade margin calculation?

15. How does antenna design affect range?

16. What is propagation models (free space, log-distance)?

---

### Advanced Level

17. What is advanced interference mitigation techniques?

18. How to implement adaptive transmission power?

19. What is spectrum sensing and cognitive radio?

20. How to implement channel bonding for higher bandwidth?

21. What is beamforming for interference suppression?

22. How to optimize transmission efficiency in crowded ISM bands?

---

### Expert Level

23. What is machine learning for interference prediction?

24. How to implement dynamic spectrum sharing?

25. What is ultra-wideband (UWB) for precise location?

26. How to implement software-defined radio (SDR) techniques?

27. What are future ISM band evolution and regulations?

---

## Wireless System Design & Integration

### Basic Level

1. What are key wireless system design considerations?

2. What is link budget analysis and calculation?

3. What is antenna types and their characteristics?

4. What is PCB layout for wireless devices?

5. What is impedance matching and transmission lines?

6. What is RF shielding and grounding?

7. What is testing and certification requirements?

8. What is wireless coexistence planning?

---

### Intermediate Level

9. How to implement multi-protocol wireless devices?

10. How to handle power management in wireless systems?

11. What is clock distribution and synchronization?

12. How to implement firmware update over wireless?

13. What is wireless security architecture?

14. How to implement mesh networking protocols?

15. What is gateway and bridge design for protocols?

16. How to debug wireless system integration issues?

---

### Advanced Level

17. How to implement low-power wireless architecture?

18. What is scalability considerations for IoT networks?

19. How to handle wireless network partitioning?

20. What is reliability and redundancy in wireless?

21. How to implement wireless network monitoring?

22. What is edge computing in wireless IoT?

---

### Expert Level

23. How to design for millions of connected devices?

24. What is autonomous wireless network management?

25. How to implement quantum-resistant wireless security?

26. What is advanced analytics for wireless networks?

27. What are emerging wireless technologies (6G, satellite IoT)?

---

## War Stories & Tricky Situations

1. You configured WiFi but device won't connect, though config is correct?

2. BLE connection drops intermittently, only during specific hours?

3. Zigbee network unstable after adding 10th device, why?

4. LoRaWAN uplink success but no downlink, troubleshooting?

5. NB-IoT modem stuck in seeking state, how to recover?

6. 5G device shows signal but no data, what's missing?

7. Thread mesh disconnects after power outage, recovery strategy?

8. NRF24L01 works in lab but fails in production environment?

9. Wireless device works at 1 meter but not at 10 meters?

10. Multiple wireless protocols interfere with each other, how to mitigate?

11. Wireless OTA firmware update failed mid-way, recovery?

12. Wireless security credential corrupted, remediation?

13. Gateway receives some but not all messages from devices?

14. Wireless network consumes more power than expected?

15. Wireless device range inconsistent (varies 10x between units)?

---

## Summary Comparison Table

| Protocol | Frequency | Speed | Range | Power | Use Case | Topology |
|----------|-----------|-------|-------|-------|----------|----------|
| WiFi | 2.4/5/6GHz | 1Gbps+ | ~100m | Medium | Internet, data | Star/Mesh |
| BLE | 2.4GHz | 2Mbps | ~100m | Low | Wearables, IoT | Star/Mesh |
| Zigbee | 2.4GHz/915MHz | 250kbps | ~100m | Low | Home, Industrial | Mesh |
| LoRaWAN | 868/915MHz | 50kbps | ~10km | Low | Wide-area IoT | Star |
| NB-IoT | Licensed LTE | 250kbps | ~40km | Low | Cellular IoT | Star |
| LTE-M | Licensed LTE | 1Mbps | ~40km | Medium | Cellular IoT | Star |
| 5G NR | Sub-6/mmWave | 1Gbps+ | ~1km | Medium | Broadband, URLLC | Star |
| Thread | 2.4GHz | 250kbps | ~100m | Low | Smart home | Mesh |
| NRF24 | 2.4GHz | 2Mbps | ~100m | Low | Generic RF | Star/Mesh |

---

*This document covers all major wireless communication protocols from fundamentals to expert level, suitable for M.Tech graduate with 10+ years embedded systems experience preparing for big company interviews.*