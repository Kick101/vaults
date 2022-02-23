### Linux File system
| __Directory__ | __Description__ |
|-----------------| -------------|
| /bin | Basic programs like: ls, cd, cat|
| /sbin/ [root] | System programs like: fdisk, mkfs, systemctl|
| /etc | Configuration files|
| /tmp | Temporary files|
| /usr/bin | Application like: nc, nmap|
| /usr/share | Application support and data files |

----
### Special Bash Variables

| __Variable Name__ | __Description__ |
|-----------------| -------------|
|$0 | The name of the Bash script
|$1 - $9 |The first 9 arguments to the Bash script
| \$\# | Number of arguments passed to the Bash script
| $@ | All arguments passed to the Bash script
| $? | The exit status of the most recently run process
| $\$ | The process ID of the current script
| $USER | The username of the user running the script
| $HOSTNAME | The hostname of the machine
| $RANDOM | A random number
| $LINENO | The current line number in the script
____________
### Operators

| __Operator__ | __Description__ |
|-----------------| -------------|
|!EXPRESSION | The EXPRESSION is false.
|-n STRING | STRING length is greater than zero
|-z STRING | The length of STRING is zero (empty)
|STRING1 != STRING2 | STRING1 is not equal to STRING2
|STRING1 = STRING2 | STRING1 is equal to STRING2
|INTEGER1 -eq INTEGER2 | INTEGER1 is equal to INTEGER2
|INTEGER1 -ne INTEGER2 | INTEGER1 is not equal to INTEGER2
|INTEGER1 -gt INTEGER2 | INTEGER1 is greater than INTEGER2
|INTEGER1 -lt INTEGER2 | INTEGER1 is less than INTEGER2
|INTEGER1 -ge INTEGER2 | INTEGER1 is greater than or equal to INTEGER2
|INTEGER1 -le INTEGER2 | INTEGER1 is less than or equal to INTEGER2
|-d FILE | FILE exists and is a directory
|-e FILE | FILE exists
|-r FILE | FILE exists and has read permission
|-s FILE | FILE exists and it is not empty
|-w FILE | FILE exists and has write permission
|-x FILE | FILE exists and has execute permission

____________
### Snippets
`((i++)` -> Math operation
`$(cmd)` -> Command execution
`local var=69` -> Local variable, by default variables are global
`echo -n` -> Strips trailing newline character from the output
`for i in $(seq 1 10); do echo $i; done;` -> Online for loop
`i=1;while [ $i -lt 10 ]; do echo $i;((i++)); done;` -> Online while loop

----
### Finding files
- __which__
	- Finds files specified in the $PATH variable
- __locate__
	-  locates files and folders indexed in locate.db
	- `# sudo updatedb`
- __find__
	- Finds files and folders with advanced options like: permissions, owner, time-stamp, age, size, filetype etc.
	- `# sudo find / -name sbd*`
-----
### Striping strings
- cut | Returns specified field with respect to delimitator
	- `# cut -d ":" -f 2`
- tr | Deletes chars
	- `# tr -d "-"`
- awk | cut but more chars as delimitator
	- `# awk -F "something" {'print $2'}`
-----
### Starting local servers
- Python3 -> `# python -m http.server <port>`
- Python2 ->  `# python -m SimpleHttpServer <port>`
- PHP -> `# php -S <addr>:<port>`

---


