### Encodings
#### ASCII
- 7-bit encoding.
- Range: 0 - 127
```python
# Get ascii code from char
ascii_code = ord('a')

# Get char from ascii code
char = chr(97) # a
```

#### Hex
- 4-bit encoding
- Range: 0 - F
```python
# Convert hex to bytes
bytes_code = bytes.fromhex(hex_code)

# Convert bytes to hex
bytes_code.hex()
```

#### Base64
- 6-bit encoding
- 64 characters.
- 4 characters of Base64 encode three 8-bit bytes.
```python
# base64 encode bytes string
base64.b64encode(bytes_code)
```
#### Convert binary to ascii
```python
binary_data = "0110100001101001"
ascii_data = bytes.fromhex(hex(int(binary_data, 2))[2:]).decode()
print(ascii_data)
```
```python
import binascii
binary_data = "0110100001101001"
ascii_data = binascii.unhexlify('%x' % int(binary_data, 2))
print(ascii_data)
```
#### XOR
```python
from pwn import *
xor(b'a',100)
```
__Properties__
```txt
Commutative: A ⊕ B = B ⊕ A  
Associative: A ⊕ (B ⊕ C) = (A ⊕ B) ⊕ C  
Identity: A ⊕ 0 = A  
Self-Inverse: A ⊕ A = 0
```
__Partial output known__
```python
partial_flag = b"crypto{"
key = xor(partial_flag, hash[:len(partial_flag)])
flag = xor(key, bytes_code)
```