---
layout: post
title:  "How Forward Proxies Work"
date:   2024-12-16 12:40:00 +0100
categories:
---

# Proxy

First, What even is a proxy? A proxy is like a middle person that helps you get things from the internet. Imagine you want to send a letter, but instead of sending it yourself, you ask a friend to send it for you. Your friend sends the letter, but the person receiving it thinks it came from your friend, not you.
In the same way, when you use a proxy, it helps you go online by sending requests to websites for you. The website sees the proxy, not you. This can help keep your information safe or make things faster.

**There are multiple types of proxies that are commonly used. Here are some of them:**
1. Forward Proxy
2. Reverse Proxy
3. Transparent Proxy
4. Anonymous Proxy
5. SOCKS Proxy
6. HTTP/HTTPS Proxy
7. DNS Proxy

# Forward Proxy

As the title says, our main topic is only forward proxies. I might also do the same for other types later. Anyway, What's a forward proxy? A forward proxy is like a helper that goes to the internet for you. When you want to visit a website, you send your request to the proxy, and the proxy sends it to the website on your behalf. The website only knows about the proxy, not you. It helps keep you safe, hide your information, or even block certain websites from you.

Still don't get it? A **Forward proxy** simply forwards your request to the server and forwards the server's response back to you!

# Forward Proxy VS Reverse Proxy

You might have heard the term **Reverse Proxy** Alot. So here's a simple comparison to not get you confused:

Imagine you want to watch a movie, but you don’t want the movie theater to know it's you.
A **forward proxy** is like you telling your friend to go to the movie theater and buy the ticket for you. The theater only sees your friend, not you.
A **reverse proxy** is like the movie theater having a person at the front who helps you get to the right movie. When you walk in, you talk to the helper, who then sends you to the right movie. The helper makes sure everything is in order without the theater knowing exactly what you wanted.

So in a more simple way, **Forward proxy** acts as a middle man. It sends your request to the destination and delivers its response back to you without letting the server know anything about your existence while a **Reverse proxy** handles your request and decides where you should go. For example if there are multiple backend servers, the **Reverse proxy** decides which one is more suitable for your request. So as you can see, these two types of proxy are quiet different.

# Applications

Each proxy type has its own applications. For example **Reverse proxies** are mainly used for caching contents while **Forward proxies** are mainly used to increase anonymity, bypassing restrictions, content filtering etc. Here are some main applications of **Forward proxies**:

1. Increase Anonymity
2. Content Filtering
3. Bypass Geolocation Restrictions
4. Improved Security
5. Caching for Speed
6. Monitoring and Logging
7. Load Balancing

You also might have used tools like Burp Suite, ZAProxy etc. They're implementations of a **Forward proxy** in a big scale.

# Getting More Technical

If you're like me and understand things better when doing it practically, here's a simple implementation in Python:


```python
import socket

# Configuration
HOST = '127.0.0.1'
PORT = 8888

# Create the proxy server socket
proxy_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
proxy_socket.bind((HOST, PORT))
proxy_socket.listen(5)

print(f"Forward Proxy listening on {HOST}:{PORT}...")

# Function to handle client connections
def handle_client(client_socket):
    # Receive the request from the client
    request = client_socket.recv(1024)
    if not request:
        return

    # Extract the target host from the request (first line)
    request_line = request.decode().splitlines()[0]
    target_host = request_line.split(' ')[1].split('/')[2]

    # Create a socket to connect to the target server
    target_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    target_socket.connect((target_host, 80))  # Assuming HTTP traffic on port 80

    # Forward the request to the target server
    target_socket.sendall(request)

    # Receive the response from the target server
    response = target_socket.recv(4096)

    # Send the response back to the client
    client_socket.sendall(response)

    # Close the sockets
    target_socket.close()
    client_socket.close()

# Main loop to accept client connections
while True:
    # Accept a new client connection
    client_socket, addr = proxy_socket.accept()
    print(f"Accepted connection from {addr}")

    # Handle the client connection
    handle_client(client_socket)
```


I used `socket` library to listen for connection on `HOST`:`PORT`, Extract the packet's destination, forward the packet to it, wait for its response, and finally forward the server's response to the client. It's a quiet simple example. How can this simple example be helpful? Imagine you're in a restricted location and can't access some services. You can run a simple **Forward proxy** on a server located in an unrestricted network and set that server's IP:PORT as your proxy server. Your each request will first reach the unrestricted server, then from the server to the destination, response from the destination to server and finally from the server to you. 

*Note: As we're working on basics, this code only works with HTTP requests and not HTTPS. So when testing, make sure to check a HTTP website like `neverssl.com` or `httpbin.org/get`*

Let's take some more examples like content filtering. Let's say you don't want to let your clients in your network to access `neverssl.com`(for no reason). We can tell our code to check the packet's destination. If it's `neverssl.com`, then simply don't send it. Let's add a function to check the destination:


```python
def is_neverssl(request_line):
	if 'neverssl' in request_line:
		return True
	else:
		return False
```


Now add this check to this section:


```python
# Extract the target host from the request (first line)
request_line = request.decode().splitlines()[0]
target_host = request_line.split(' ')[1].split('/')[2]

# Check if the target is neverssl
is_neverssl = is_neverssl(request_line)
if is_neverssl:
	return None

# Create a socket to connect to the target server
target_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
target_socket.connect((target_host, 80))  # Assuming HTTP traffic on port 80
```


Well, it's a nasty method but it works for simple tests and debuggings. I just wanted to show some of its applications in a practical way. 