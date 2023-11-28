|Data Types Categories|Types|
|-|-|
|Text types|str|
|Numeric Types|int, float, complex|
|Sequence Types|list(mutable)[], tuple(immutable)(), range|
|Mapping Type|dict {"key":"value"}|
|Set Types|set, frozenset {}|
|Boolean Type|bool True/False|
|Binary Types|bytes, bytearray, memoryview|

>Data type of any object can be obtained by using the `type()` function

---
<center><h3>Handling Web Requests</h3></center>

__Packages__
- requests


|Functions|Description|
|-|-|
|`requests.get()`|Returns requests.Response() object|
|`requests.Response()`| Object contains the server's response to the HTTP request

- [requests.response() - w3schools](https://www.w3schools.com/python/ref_requests_response.asp)


__Snippet__
```python
import requests
res = requests.get("https://www.google.com/")
content = res.content
print(content)
```
---
<center><h3>Sockets</h3></center> 

> Sockets are used to create endpoints in a network. A socket is a combination of IP and Port number.

__Packages__
- sockets

__server.py__
```python
import socket

# AF - AddressFamily | AF_INET - IPv4 | SOCK_STREAM - TCP
sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Bind the socket to address, port with a tuple
sock.bind(("localhost",1234))

# Enable a server to accept connections
# no.of unaccepted connections that the system will allow before refusing new connections.
sock.listen(2)

# Listen for connections forever
# More elobaration:
	# Listen for connections, when got one, send data, recieve data
	# and automatically closes connection for the client and waits for upcoming
while True:
	print("Waiting for connections...")

	# Accept client connection
	c_sock, addr = sock.accept()

	# Send data to client
	c_sock.send(bytes("H3110 Fr13nd\n",'utf-8'))

	# Recieve Data
	data = c_sock.recv(1024)
	print(data.decode('utf-8'))

# Close socket
sock.close()
```
__Client.py__
```python
import socket

sock = socket.socket(socket.AF_INET,socket.SOCK_STREAM)

# Connect to server
sock.connect(("localhost", 1234))

data = ''

while True:
	d = sock.recv(8)
	data += d.decode('utf-8')
	if d < 1:
		break
		
# Send data to server
sock.send(bytes("bye!",'utf-8'))

# Print data recieved from server
print(data)
```

---
<center><h3>Command line Arguments</h3></center>

```python
import argparse

# Initialize the parser
parser = argparse.ArgumentParser()

# Add optional parameters
parser.add_argument("-x","--xyz", help="abc", type="int",default="qwerty")

# Parse arguments
args = parser.parse_args()

# How to access arguments
print(args.xyz)

```

---
<center><h3>Encoding/Decoding</h3></center>

__utf-8__
`bytes()` and `encode()` are two ways to encode
```python
# utf-8 encode
utf_8 = bytes("hello", 'utf-8')
# defaults to utf-8 when encoding not provided
utf_8 = "hello".encode()

# decode | defaults to utf-8 when encoding not provided
data = utf_8.decode('utf-8')
```
__Base64, base16, base32, base85__
- _base16:_ base64.b16<encode/decode>()
- _base32:_ base64.b32<encode/decode>()
- _base85:_ base64.b85<encode/decode>()
```python
import base64

string = "hello"
# convert to bytes
string_bytes = string.encode()
# base64 encode
b64_encoded = base64.b64encode(string_bytes)
# Decode base64
b64_decoded = base64.b64decode(b64_encoded)
# Convert back to string from bytes
string_decoded = b64_decoded.decode()
```
__ascii value__
```python
ord('a') # 97
```
__Convert Binary to ascii__
```python
chr(int(binary, 2))
```
__Convert ascii to binary__
```python
"{0:08b}".format(ord(c))
```
---
<center><h3>Oops</h3></center>

```python
def main():
	print("test")

if __name__ == __main__:
	main()
```

---
### Errors & Fixes
__Install pip2__
```bash
sudo apt update
curl https://bootstrap.pypa.io/pip/2.7/get-pip.py --output get-pip.py
sudo python2 get-pip.py
```

__NoSQLMap Fix__
```bash
sudo pip2 uninstall certifi
pip2 install certifi==2018.10.15
```