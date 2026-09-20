# VTU MCA 2022/2024 Scheme - Computer Networks (22MCA14)
## Full 100-Mark Solved Examination Paper with Step-by-Step Solutions
Time: 3 Hours | Max Marks: 100

================================================================================
MODULE 1: NETWORK ARCHITECTURE & PHYSICAL/DATA LINK LAYER
================================================================================

Q.1 (a) Compare OSI 7-Layer Model with TCP/IP 4-Layer Protocol Suite. [10 Marks]
Answer:
Comparison Table:
OSI Model Layers               | TCP/IP Model Layers         | Primary Protocols / Devices
7. Application                 |                             | HTTP, DNS, SMTP, FTP, SSH
6. Presentation                | Application Layer           | TLS/SSL, JPEG, ASCII
5. Session                     |                             | NetBIOS, RPC, Sockets
4. Transport Layer             | Transport Layer             | TCP (reliable), UDP (fast)
3. Network Layer               | Internet Layer              | IPv4, IPv6, ICMP, OSPF, BGP
2. Data Link Layer             | Network Interface           | Ethernet (802.3), Wi-Fi (802.11), ARP
1. Physical Layer              | (Link Layer)                | Copper Cable, Fiber Optics, Hubs

Key Differences:
1. OSI strictly separates service, interface, and protocol; TCP/IP was built around working protocols.
2. Transport layer in OSI is connection-oriented; TCP/IP supports both connection-oriented (TCP) and connectionless (UDP).
3. Network layer in OSI supports both connectionless and connection-oriented; TCP/IP Internet layer is strictly connectionless (IP).

--------------------------------------------------------------------------------
Q.1 (b) Explain CRC (Cyclic Redundancy Check) error detection with a numerical example. [10 Marks]
Answer:
Given:
Data bit sequence D = 1010000 (7 bits)
Generator polynomial G(x) = x^3 + x + 1 => Divisor = 1011 (k = 4 bits, degree r = 3).

Step 1: Append r = 3 zeros to data bits:
Dividend = 1010000000 (10 bits)

Step 2: Perform modulo-2 binary division (XOR subtraction):
  1010000000 | 1011
^ 1011
------
  0001000000
    1011
    ------
    00110000
      1011
      ------
      011100
       1011
       -----
       01010
        1011
        ----
        0001 (Remainder = 011)

Step 3: Transmitted Codeword = Data + Remainder = 1010000011.
Receiver checks by dividing codeword by 1011: Remainder is 0 => Transmission is ERROR FREE!

================================================================================
MODULE 2: NETWORK LAYER & IP ADDRESSING (SUBNETTING)
================================================================================

Q.3 (a) An organization is granted the IPv4 block 192.168.10.0/24. Divide this network into 4 equal subnets. For each subnet, determine:
(i) Subnet Mask (ii) Network ID (iii) Usable Host IP Range (iv) Directed Broadcast IP. [10 Marks]
Answer:
Given: Network 192.168.10.0/24.
To create 4 subnets, we need 2 bits (2^2 = 4).
New Prefix Length = 24 + 2 = /26.
New Subnet Mask = 255.255.255.192 (/26, because 128 + 64 = 192).
Block size = 256 - 192 = 64 IP addresses per subnet. Total usable hosts = 64 - 2 = 62 hosts.

Subnet 1:
- Network ID: 192.168.10.0/26
- Usable Host Range: 192.168.10.1 to 192.168.10.62
- Broadcast ID: 192.168.10.63

Subnet 2:
- Network ID: 192.168.10.64/26
- Usable Host Range: 192.168.10.65 to 192.168.10.126
- Broadcast ID: 192.168.10.127

Subnet 3:
- Network ID: 192.168.10.128/26
- Usable Host Range: 192.168.10.129 to 192.168.10.190
- Broadcast ID: 192.168.10.191

Subnet 4:
- Network ID: 192.168.10.192/26
- Usable Host Range: 192.168.10.193 to 192.168.10.254
- Broadcast ID: 192.168.10.255

================================================================================
MODULE 3: TRANSPORT LAYER (TCP vs UDP) & SOCKET PROGRAMMING
================================================================================

Q.5 (a) Explain TCP 3-Way Handshake Connection Establishment and 4-Way Termination. [10 Marks]
Answer:
3-Way Handshake (Connection Setup):
1. Client -> Server: SYN packet with initial sequence number seq = x. (Client enters SYN_SENT).
2. Server -> Client: SYN-ACK packet with seq = y, ack = x + 1. (Server enters SYN_RCVD).
3. Client -> Server: ACK packet with seq = x + 1, ack = y + 1. (Both enter ESTABLISHED).

4-Way Termination (Connection Teardown):
1. Client -> Server: FIN packet. (Client enters FIN_WAIT_1).
2. Server -> Client: ACK packet. (Client enters FIN_WAIT_2, Server enters CLOSE_WAIT).
3. Server -> Client: FIN packet when server finishes sending data. (Server enters LAST_ACK).
4. Client -> Server: ACK packet and enters TIME_WAIT (2*MSL duration). Both enter CLOSED.

--------------------------------------------------------------------------------
Q.5 (b) Write a runnable Python Socket program for a TCP Echo Server. [10 Marks]
Answer:
```python
import socket

def run_tcp_server():
    server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    server_socket.bind(('127.0.0.1', 8080))
    server_socket.listen(5)
    print("TCP Server listening on port 8080...")

    while True:
        client_sock, client_addr = server_socket.accept()
        print(f"Connected to client: {client_addr}")
        data = client_sock.recv(1024)
        if data:
            print(f"Received: {data.decode('utf-8')}")
            client_sock.sendall(b"ECHO: " + data)
        client_sock.close()

if __name__ == '__main__':
    run_tcp_server()
```
