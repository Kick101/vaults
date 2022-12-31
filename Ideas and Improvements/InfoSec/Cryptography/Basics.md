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

```