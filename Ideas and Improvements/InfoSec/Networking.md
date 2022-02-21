<center><h2>IPv4 & IPv6</h2></center>

### IPv4

|128|64|32|16|8|4|2|1|-> 255 (decimal)|
|--|--|--|--|--|--|--|--|--|
|1|1|1|1|1|1|1|1|-> 8 (bits)|

###### Possible IPv4 Addresses: 2^32 = 4,29,49,67,296
- 429 cr and 49 lakhs and 67,296 | 4 billion and 294 million and 967 thousand and 269 
1 billion = 100cr / 1 million = 10 lakhs


| class | Range | Hosts & Networks | use case |
| ------ | -----| -------------------- | --------- |
|A|1.0.0.1 - 126.255.255.254|16 million hosts, 127 networks| Private|
|B|128.1.0.1 - 191.255.255.254|65000 hosts, 16000 networks| Public|
|C|192.0.1.1 - 223.255.254.254|254 hosts, 2 million networks| Public|
|D|224.0.0.0 - 239.255.255.255| | multicast groups|
|E|240.0.0.0 - 254.255.255.255|| R&D|

|Special IP range | Use case |
| ---------------- | ---------- |
|127.0.0.1 - 127.255.255.255|localhost 
|172.16.0.0 - 172.32.255.255|Private IP
|192.168.0.0 - 192.168.255.255|Private IP
|10.0.0.0 - 10.255.255.255|Private IP

|128|64|32|16|8|4|2|1|-> 192| |128|64|32|16|8|4|2|1|-> 172|
|--|--|--|--|--|--|--|--|--|--|--|--|--|--|--|--|--|--|--|
|1|1|0|0|0|0|0|0| ||1|0|1|0|1|1|0|0| |

#### Subnetting
|11111111|.|11111111|.|11111111|.|11111111|
|--|--|--|--|--|--|--|

--------
### IPv6

- 8 groups of 4 hex digits -> 16 bytes -> 128 bits
	- Each group -> 4 hex digits -> 2 bytes -> 16 bits
	- 1 hex digit -> 1 nibble (4 bits) -> half byte
	- Each hex digit can represent 16 values [0-F] so 4 bits are required (2^4->16)

###### Binary to Hexadecimal
|8|4|2|1|-> 16||8|4|2|1|-> 10|
|--|--|--|--|--|--|--|--|--|--|--|
|1|1|1|1|-> F||1|0|1|0|-> A|
###### Base 16
|65536|4096|256|16|1|
|--|--|--|--|--|
|16^4|16^3|16^2|16^1|16^0|
###### Hexadecimal to Decimal 
|F|E|D|C|B|A|9|8|7|6|5|4|3|2|1|
|--|--|--|--|--|--|--|--|--|--|--|--|--|--|--|
|15|14|13|12|11|10|9|8|7|6|5|4|3|2|1|


--------
<center><h2>Todo</h2></center>