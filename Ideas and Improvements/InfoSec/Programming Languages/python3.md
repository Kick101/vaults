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
|requests.get()|Returns requests.Response() object

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

```python
import socket
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

print(args.xyz)

```

---
<center><h3>Oops</h3></center>

```python
def main():
	print("test")

if __name__ == __main__:
	main()
```	