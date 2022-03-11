|Data Types Categories|Types|
|-|-|
|Text types|str|
|Numeric Types|int, float, complex|
|Sequence Types|list(mutable), tuple(immutable), range|
|Mapping Type|dict|
|Set Types|set, frozenset|
|Boolean Type|bool|
|BInary Types|bytes, bytearray, memoryview|

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
<center><h3>Oops</h3><center>
	
