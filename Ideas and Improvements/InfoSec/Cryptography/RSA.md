```python
n = p × q
φ(n) = (p - 1)(q - 1)
e = some number that 1 < e < φ(n) and gcd(e,φ(n)) == 1
e*d mod φ(n) = 1 (or) d = e^(-1) mod φ(n)
# Encryption & Decryption
c = m^e (mod n)
m = c^d (mod n)
```

__Small N__
```prob 
c: 964354128913912393938480857590969826308054462950561875638492039363373779803642185  
n: 1584586296183412107468474423529992275940096154074798537916936609523894209759157543  
e: 65537
```
- To find _d_ value (private key), we can factor give _n_ value since it's relatively small.
	- [FactorDB](http://factordb.com) - Contains already factorised numbers into their primes
	- 
- Tools:
	- [rsaCTFtool](https://github.com/RsaCtfTool/RsaCtfTool)