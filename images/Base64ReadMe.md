#  Task 1 – Base64 Encoding and File Transfer Using Docker

##  Module: Foundation of Computer Science  
##  Topic: Enhancing Secure Data Exchange with Encoding Formats and Protocol Integration  

---

##  Overview

This task demonstrates secure data exchange using:

- Docker containerisation  
- Docker bridge networking  
- Client–Server architecture  
- Base64 encoding  
- HTTP protocol  
- IP-based communication  

The practical shows how data is encoded before transmission and decoded after reception between two isolated containers connected through a Docker network.

---

##  System Architecture

- Docker Network: `blue`
- Server Container: Hosts encoded file using Python HTTP server
- Client Container: Downloads and decodes file
- Communication Protocol: HTTP
- Encoding Method: Base64

---

#  Step 1 – Create Docker Network

```bash
docker network create blue
docker network ls
```

---

#  Step 2 – Start Server Container

```bash
docker run -it --name server --network blue python:3.12-slim bash
```

---

#  Step 3 – Install Required Packages (Server)

```bash
apt update
apt install -y wget iputils-ping
```

---

#  Step 4 – Create Original File (Server)

```bash
echo "This is secret data for Base64 Demo" > dem.txt
ls
```

---

#  Step 5 – Encode File Using Base64 (Server)

```bash
base64 dem.txt > encoded.txt
cat encoded.txt
```

Example encoded output:

```
VGhpcyBpcyBzZWNyZXQgZGF0YSBmb3IgQmFzZTY0IERlbW8K
```

---

#  Step 6 – Check Server IP Address (Windows CMD)

```bash
docker inspect -f "{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}" server
```

Example output:

```
172.18.0.2
```

---

#  Step 7 – Start HTTP Server (Server)

```bash
python -m http.server 8082
```

---

#  Step 8 – Start Client Container

(Open a new CMD window)

```bash
docker run -it --name client --network blue python:3.12-slim bash
```

---

#  Step 9 – Install Required Packages (Client)

```bash
apt update
apt install -y wget iputils-ping
```

---

#  Step 10 – Test Network Connectivity (Client)

Using container name:

```bash
ping server
```

Using IP address:

```bash
ping 172.18.0.2
```

---

#  Step 11 – Download Encoded File (Client)

Using container name:

```bash
wget http://server:8082/encoded.txt
```

Using IP address:

```bash
wget http://172.18.0.2:8082/encoded.txt
```

---

#  Step 12 – Decode File (Client)

```bash
base64 -d encoded.txt > decoded.txt
cat decoded.txt
```

Expected output:

```
This is secret data for Base64 Demo
```

---

#  Common Errors

### Connection refused
Cause: HTTP server not running.  
Fix: Run `python -m http.server 8082` in server container.

### Temporary failure in name resolution
Cause: Containers not on same network.  
Fix: Ensure both are connected to `blue`.

### SSL error when using HTTPS
Cause: Python HTTP server does not support TLS.  
HTTP is unencrypted. HTTPS requires TLS configuration.

---

#  Learning Outcomes

- Understanding Docker networking  
- Implementing client-server communication  
- Demonstrating Base64 encoding before transmission  
- Transferring files using HTTP protocol  
- Verifying communication using container names and IP addresses  
- Understanding difference between HTTP and HTTPS  

---

#  References

- Docker Inc. (2024). Docker Networking Documentation. https://docs.docker.com/network/  
- Josefsson, S. (2006). RFC 4648 – The Base16, Base32, and Base64 Data Encodings. IETF.  
- Fielding, R. & Reschke, J. (2014). RFC 7230 – Hypertext Transfer Protocol (HTTP/1.1). IETF.  
- Python Software Foundation (2024). http.server documentation. https://docs.python.org/3/library/http.server.html  
