```python
n = p × q
φ(n) = (p - 1)(q - 1)
e = some number that 1 < e < φ(n) and gcd(e,φ(n)) == 1
e*d mod φ(n) = 1 (or) d = e^(-1) mod φ(n)
# Encryption & Decryption
c = m^e (mod n) (or) m^e = tn + c
m = c^d (mod n) (or) m = (k * n + c)^(1/e)
```
__Tools__
- [rsaCTFtool](https://github.com/RsaCtfTool/RsaCtfTool)
	
---
__Small N__
```prob 
c: 964354128913912393938480857590969826308054462950561875638492039363373779803642185  
n: 1584586296183412107468474423529992275940096154074798537916936609523894209759157543  
e: 65537
```
- To find _d_ value (private key), we can factor give _n_ value since it's relatively small.
	- [FactorDB](http://factordb.com) - Contains already factorised numbers into their primes
---
__Small exponent__
m = c^d (mod n) (or) m = (k * n + c)^(1/e)
[medium post](https://billatnapier.medium.com/cracking-rsa-that-have-a-low-exponents-23fbb148292)

- If `m^e < n` -> `m = c^(1/e)`

```python
def find_invpow(x,n):
	high = 1
	while high ** n < x:
		high *= 2
	low = high//2
	while low < high:
		mid = (low + high) // 2
		if low < mid and mid**n < x:
			low = mid
		elif high > mid and mid**n > x:
			high = mid
		else:
			return mid
	return mid + 1
k = 0 # loop through k
long_to_bytes(find_invpow(k * n + c, e))
```
---
__Small d or Wiener Attack__
![[Pasted image 20230101090656.png | 500]]
```txt
d = e^(-1) mod φ(n)
```
