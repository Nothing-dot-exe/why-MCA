# Master Study Notes: Internet of Things (IoT) & Cloud Infrastructure
## Course Code: 22MCA32 | Visvesvaraya Technological University (VTU)
### Department of MCA | BKIT College, Bhalki

---

## 📑 Syllabus Architecture & Module Breakdown

* **Module 1**: IoT Definition, Physical & Logical Design, IoT Communication Models (Publish-Subscribe, Request-Response), IoT vs. M2M, 6 IoT Levels.
* **Module 2**: Sensors & Actuators, Hardware Platforms (Raspberry Pi, ESP32, Arduino), GPIO Interfacing, Microcontrollers vs. Microprocessors.
* **Module 3**: IoT Wireless Networking & Protocols, 6LoWPAN, ZigBee, LoRaWAN, Application Protocols (MQTT Architecture, Topics, QoS 0/1/2, CoAP vs. HTTP).
* **Module 4**: Cloud Platforms for IoT, AWS IoT Core, Device Shadow / Digital Twin, Rules Engine, Edge Computing vs. Fog Computing vs. Cloud.
* **Module 5**: IoT Security Vulnerabilities, Hardware Security, Industrial IoT (IIoT), Smart City, Smart Agriculture & Healthcare Telemetry Architectures.

---

# MODULE 1: IOT ARCHITECTURE & COMMUNICATION MODELS

## 1.1 The Four Communication Models
1. **Request-Response**: Client sends request; server handles and responds (Stateless, e.g., HTTP).
2. **Publish-Subscribe**: Publishers send data categorized by Topics to a central **Message Broker**. Subscribers express interest in topics and receive asynchronous updates without direct publisher connection.
3. **Push-Pull**: Publishers push data to queues; consumers pull messages from queues. Provides decoupling and load balancing.
4. **Exclusive Pair**: Bidirectional, full-duplex persistent connection over a single socket (e.g., WebSockets).

---

# MODULE 2: SENSORS, ACTUATORS & HARDWARE PLATFORMS

## 2.1 Sensors & Actuators Taxonomy
* **Sensor**: Converts physical environment phenomena (analog/digital) into electrical signals (e.g., DHT11/DHT22 for Temperature & Humidity, HC-SR04 Ultrasonic for distance, PIR for motion).
* **Actuator**: Converts electrical signals into physical motion or action (e.g., Relay modules for high-voltage switching, Servo motors for angular positioning, Solenoid valves).

---

## 2.2 Hardware Comparison: ESP32 vs. Raspberry Pi

| Feature | ESP32 Microcontroller | Raspberry Pi 4 (SBC) |
| :--- | :--- | :--- |
| **Processor** | Dual-core Tensilica Xtensa 32-bit (240 MHz) | Quad-core ARM Cortex-A72 64-bit (1.5 GHz) |
| **RAM & Storage**| 520 KB SRAM + 4 MB Flash | 2 GB to 8 GB LPDDR4 + MicroSD (up to 1 TB) |
| **Operating System**| Bare-metal / FreeRTOS (Real-Time) | Full Linux OS (Debian / Raspberry Pi OS) |
| **Connectivity** | Built-in 2.4 GHz Wi-Fi + BLE 4.2 | Gigabit Ethernet, Dual-band Wi-Fi, Bluetooth 5.0 |
| **Power Draw** | Low (Micro-amperes in Deep Sleep) | High (Requires 5V / 3A power supply) |
| **Best Used For** | Battery-powered field sensor nodes | Edge gateways, video processing, local AI inference |

---

# MODULE 3: PROTOCOLS & MQTT ARCHITECTURE

## 3.1 MQTT (Message Queuing Telemetry Transport)
Extremely lightweight publish/subscribe messaging protocol running on top of TCP/IP port 1883 (8883 for TLS).

```
   [ Sensor Node (ESP32) ]
              |
              | Publish: "bkit/lab1/temperature" (Value: 28.5)
              v
     +-----------------+
     |   MQTT Broker   | (e.g. Eclipse Mosquitto / AWS IoT Core)
     +-----------------+
              |
              | Forward to all matching subscribers
              v
   [ Mobile Dashboard / Cloud Database ]
```

### The Three Quality of Service (QoS) Levels:
* **QoS 0 (At Most Once)**: Fire and forget. No acknowledgment. Delivery is not guaranteed; lowest bandwidth overhead.
* **QoS 1 (At Least Once)**: Message sent; broker sends `PUBACK`. Retransmitted until acknowledged; duplicate messages may occur.
* **QoS 2 (Exactly Once)**: Four-step handshake (`PUBLISH` $\to$ `PUBREC` $\to$ `PUBREL` $\to$ `PUBCOMP`). Guarantees no duplicates and guaranteed delivery; highest overhead.

---

## 3.2 CoAP vs. MQTT Comparison

| Parameter | MQTT | CoAP (Constrained Application Protocol) |
| :--- | :--- | :--- |
| **Transport Layer** | TCP (Connection-oriented, reliable) | UDP (Connectionless, low overhead) |
| **Paradigm** | Publish / Subscribe via central Broker | Request / Response (RESTful: GET, POST, PUT, DELETE) |
| **Header Size** | 2 Bytes minimum | 4 Bytes fixed |
| **Security** | TLS / SSL | DTLS (Datagram Transport Layer Security) |
| **Best Suited For** | Telemetry streaming, push notifications | Constrained networks where TCP handshake is too expensive |

---

# MODULE 4: CLOUD IOT & EDGE / FOG COMPUTING

## 4.1 Device Shadows (Digital Twins)
A JSON document maintained in the cloud (e.g., AWS IoT Device Shadow) storing the current **reported state** and desired **target state** of a physical device:
* Even if the physical device goes offline, client apps can read the latest reported state and set the desired state.
* When the device reconnects to the network, it synchronizes differences automatically.

---

## 4.2 Edge vs. Fog vs. Cloud Computing

| Layer | Location | Latency | Primary Function |
| :--- | :--- | :--- | :--- |
| **Edge Layer** | On the sensor node / gateway (ESP32, RPi) | Sub-millisecond | Real-time filtering, emergency shutoff, local actuation. |
| **Fog Layer** | Local network servers / campus LAN | 5–20 ms | Local aggregation, regional analytics, temporary caching. |
| **Cloud Layer** | Hyperscale data centers (AWS, Azure) | 100–300 ms | Massive historical storage, deep learning training, Big Data. |

---

# 🎯 Model Exam Questions with Step-by-Step Solutions

### Q1. [Module 3 - 10 Marks]
**Explain the complete MQTT Architecture. Detail the three Quality of Service (QoS) levels with message packet sequence diagrams.**

**Solution:**
1. **Core Components**:
   * **Publisher**: Client device that collects sensor telemetry and transmits formatted messages under specific topic strings (e.g., `college/block_a/power`).
   * **Broker**: Central server that receives messages, authenticates clients, manages topic subscriptions, and filters/dispatches payloads to interested endpoints.
   * **Subscriber**: Device or cloud dashboard registered to receive live feeds on specific topics or wildcards (e.g., `college/+/power`).

2. **QoS 0 Handshake (At Most Once)**:
   ```
   Client ----------------- PUBLISH ----------------> Broker
   ```

3. **QoS 1 Handshake (At Least Once)**:
   ```
   Client ----------------- PUBLISH ----------------> Broker
   Client <---------------- PUBACK ----------------- Broker
   ```

4. **QoS 2 Handshake (Exactly Once - 4 Steps)**:
   ```
   Client ----------------- PUBLISH ----------------> Broker
   Client <---------------- PUBREC (Received) ------ Broker
   Client ----------------- PUBREL (Release) -------> Broker
   Client <---------------- PUBCOMP (Complete) ----- Broker
   ```
