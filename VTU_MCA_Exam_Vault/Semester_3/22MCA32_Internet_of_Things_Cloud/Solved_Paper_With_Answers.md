# VTU MCA 2022/2024 Scheme - IoT & Cloud (22MCA32)
## Full Solved Examination Paper with MQTT, CoAP & Cloud Gateways
Time: 3 Hours | Max Marks: 100

================================================================================
MODULE 1: IOT ARCHITECTURE & SENSOR NETWORKS
================================================================================

Q.1 (a) Explain MQTT (Message Queuing Telemetry Transport) Architecture: Publisher, Subscriber, Broker, and Quality of Service (QoS 0, 1, 2). [10 Marks]
Answer:
MQTT is a lightweight, publish-subscribe messaging protocol designed for constrained devices with low bandwidth:
1. Publisher: Sensor client that publishes telemetry data to a Topic (e.g. `factory/temp`).
2. Broker: Central server (e.g. Mosquitto, AWS IoT Core) that filters and routes messages to subscribed clients.
3. Subscriber: Dashboard or server that subscribes to topics.

QoS Levels:
- QoS 0 (At most once): Fire and forget; no guarantee of delivery.
- QoS 1 (At least once): Message delivered with PUBACK; guarantees delivery, duplicate possible.
- QoS 2 (Exactly once): Four-step handshake (PUBLISH -> PUBREC -> PUBREL -> PUBCOMP); zero loss and zero duplicates.
