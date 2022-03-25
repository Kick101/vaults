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
|`$0 `| The name of the Bash script
|`$1 - $9` |The first 9 arguments to the Bash script
| `$#` | Number of arguments passed to the Bash script
| `$@` | All arguments passed to the Bash script
| `$?`| The exit status of the most recently run process
| `$$` | The process ID of the current script
| `$USER` | The username of the user running the script
| `$HOSTNAME` | The hostname of the machine
| `$RANDOM` | A random number
| `$LINENO` | The current line number in the script
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

------------
### case statement
```sh
case $1 in
        1) echo "odd";;
        2|4) echo "even";;
       *) echo "Whatever!"
esac
```


____________
### Bash Snippets
|snippet|Description|
|--|--|
|`((i++))` \| `((i=$x+$y))`|Computes Arithmetic Expression|
|`$((n=5*4))`|Computes Arithmetic Expression and can be used with echo|
|`$(cmd)`|Command execution|
|`local var=69`|Local variable, by default variables are global|
|`echo -n`|Strips trailing newline character from the output|
|`for i in $(seq 1 10); do echo $i; done;`|Online for loop|
|`i=1;while [ $i -lt 10 ]; do echo $i;((i++)); done;`|Online while loop|
|`for (( c=1; c<=5; c++ ));do echo $c; done;`| C like for loop|
|`tput setaf 1;echo h3ll0`| 1-red, 42-green, 7-white|

----
### Finding files
|Command|Description|
|--|--|
|`# which`|Finds files specified in the $PATH variable
|`# locate`|Locates files and folders indexed in locate.db `# sudo updatedb`|
|`# find`|Finds files and folders with advanced options like: permissions, owner, time-stamp, age, size, filetype etc.|

-----
### Striping strings
|Command|Description|
|--|--|
|`# cut -d ":" -f 2`|Returns specified field with respect to deliminator|
|`# tr -d "-"`|Deletes chars|
|`# awk -F "something" {'print $2'}`|cut but more chars as deliminator|

-----
### Starting local servers
|Server|Command|
|--|--|
|Python3|`# python -m http.server <port>`|
|Python2|`# python -m SimpleHTTPServer <port>`|
|PHP|`# php -S <addr>:<port>`|
|Apache2|`# systemctl start apache2.service`|

---

### Sorting files
```sh
sort -u <file-0>...<file-n> | tee <new-file>
```

---
<center><h3>Script Flags</h3></center>

[linuxconfig.org - bash script flags](https://linuxconfig.org/bash-script-flags-usage-with-arguments-examples)
[mkssoftware.com - getopts](https://www.mkssoftware.com/docs/man1/getopts.1.asp)

### `getopts`
>obtains single character options and their arguments from a list of parameters that follows the standard POSIX.2 option syntax 

><span style="color:red">NOTE:</span> when `getopts` ran, it takes only one argument among provided list of options and stores the option name in followed by variable-name and the value is stored in `$OPTARG` only if the option is followed by a `:`

__Syntax__
```sh
getopts <single letter options> <variable to store options> 
```
__Example__
In the below instance, `getopts` will take first argument it gets among `t, f, s` and stores what argument it got in `$FLAG` and it's value in `$OPTARG`
- Among `t,f,s` only `t, f` takes argument value

```sh
## flags.sh
getopts t:f:s FLAG
echo $FLAG
echo $OPTARG
```
```
$ flags.sh -t xyz.com
```

flags.sh
```sh
while getopts a:b:c FLAG; do
	case $FLAG in
		a)
			echo $FLAG $OPTARG;;
		b)
			echo $FLAG $OPTARG;;
		c)
			echo $FLAG;;
		*)
			echo "Invalid args"; exit 1;
	esac
done
```
```
$ flags.sh -a abc -b asd -c
```
When above line ran, the execution follows similar to this:
- getopts takes `-a abc` as value and run the case statement
- Then again it takes `-b asd` and does the same

---
### Handling files

```sh

```