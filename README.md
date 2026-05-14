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
#server.py:
```
import socket
s = socket.socket()
s.bind(('localhost', 8000))
s.listen(1)
print("Waiting for connection...")
conn, addr = s.accept()
print("Connected to", addr)
while True:
 data = conn.recv(1024).decode()
 if not data:
  break
 print("Frames received:", data)
 ack = "ACK for " + data
 conn.send(ack.encode())
conn.close()
```
#client.py:
```
import socket
s = socket.socket()
s.connect(('localhost', 8000))
n = int(input("Enter number of frames: "))
w = int(input("Enter window size: "))
frames = list(range(1, n+1))
i = 0
while i < n:
 send_frames = frames[i:i+w]
 msg = " ".join(map(str, send_frames))
 print("Sending frames:", msg)
 s.send(msg.encode())
 ack = s.recv(1024).decode()
 print("Received:", ack)
 i += w
s.close()
```
## OUTPUT:
#server.py:
<img width="345" height="108" alt="image" src="https://github.com/user-attachments/assets/e1789625-461a-45e3-bcec-8a9b06234ec2" />
#client.py:
<img width="261" height="172" alt="image" src="https://github.com/user-attachments/assets/f903ec8f-4d2c-4d1f-804e-6667065e9063" />

## RESULT
Thus, the python program for simulating ARP protocols using TCP was successfully 
executed.
