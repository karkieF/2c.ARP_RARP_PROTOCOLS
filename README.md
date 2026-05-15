# 2c.SIMULATING ARP /RARP PROTOCOLS
## AIM
To write a python program for simulating ARP protocols using TCP.
## ALGORITHM:
## Client:
1. Start the program
2. Using socket connection is established between client and server.
3. Get the IP address to be converted into MAC address.
4. Send this IP address to server.
5. Server returns the MAC address to client.
## Server:
1. Start the program
2. Accept the socket which is created by the client.
3. Server maintains the table in which IP and corresponding MAC addresses are
stored.
4. Read the IP address which is send by the client.
5. Map the IP address with its MAC address and return the MAC address to client.
P
## PROGRAM - ARP
### arp_server.py:
```
import socket

s = socket.socket()

s.bind(('localhost', 8000))
s.listen(1)
print("ARP Server waiting on localhost:8000...")

conn, addr = s.accept()
print("Connected to", addr)

address = {
    "165.165.80.80": "6A:08:AA:C2",
    "165.165.79.1": "8A:BC:E3:FA"
}

try:
    while True:
        ip = conn.recv(1024).decode()
        if not ip:
            break

        try:
            conn.send(address[ip].encode())
        except KeyError:
            conn.send("Not Found".encode())
finally:
    conn.close()
    s.close()
```
### arp_client.py:
```
import socket

s = socket.socket()

try:
    s.connect(('localhost', 8000))
except ConnectionRefusedError:
    print("Connection refused: start arp_server.py first.")
    raise

try:
    while True:
        ip = input("Enter Logical Address (IP): ")
        if not ip:
            break

        s.send(ip.encode())
        print("MAC Address:", s.recv(1024).decode())
finally:
    s.close()
```
### rarp_client.py:
```
import socket

s = socket.socket()

try:
    s.connect(('localhost', 9000))
except ConnectionRefusedError:
    print("Connection refused: start rarp_server.py first.")
    raise

try:
    while True:
        mac = input("Enter MAC Address: ")
        if not mac:
            break

        s.sendall(mac.encode())
        print("Logical Address:", s.recv(1024).decode())
finally:
    s.close()
```
### rarp_server.py:
```
import socket

s = socket.socket()
s.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)

s.bind(('localhost', 9000))
s.listen(1)
print("RARP Server waiting on localhost:9000...")

conn, addr = s.accept()
print("Connected to", addr)

address = {
    "6A:08:AA:C2": "192.168.1.100",
    "8A:BC:E3:FA": "192.168.1.99"
}

try:
    while True:
        mac = conn.recv(1024).decode()
        if not mac:
            break

        try:
            conn.sendall(address[mac].encode())
        except KeyError:
            conn.sendall("Not Found".encode())
finally:
    conn.close()
    s.close()
```
## OUTPUT:

### arp_client.py:
<img width="393" height="40" alt="image" src="https://github.com/user-attachments/assets/1bceba5c-d9a4-4137-ae35-bf1a6623f479" />

### rarp_client.py:
<img width="330" height="43" alt="image" src="https://github.com/user-attachments/assets/7658e82e-45ec-4e75-8e9b-754a3836f073" />

## RESULT
Thus, the python program for simulating ARP protocols using TCP was successfully 
executed.
