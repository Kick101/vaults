## SNORT 101

### Global Commands

| Mode              | Command Example |
|-------------------|-----------------|
| Display version   | `snort -V` or `snort -version` |
| Quiet mode        | `snort -q` |
| Specify interface | `snort -i eth0` |
| Verbose mode      | `snort -v` |
| Show link-layer headers | `snort -e` |
| Show payload      | `snort -d` |
| Show full HEX     | `snort -X` |
| Show all details  | `snort -eX` |
| Capture N packets | `snort -v -n 10` |

### Using Config Files

| Description                    | Command |
|--------------------------------|---------|
| Use config file                | `snort -c /etc/snort/snort.conf` |
| Test config                    | `snort -c /etc/snort/snort.conf -T` |
| Disable logging                | `snort -c /etc/snort/snort.conf -N` |
| Run in background              | `snort -c /etc/snort/snort.conf -D` |

### Alert Modes

| Mode                | Command |
|---------------------|---------|
| No output           | `snort -c /etc/snort/snort.conf -v -A none` |
| Console output 1    | `snort -c /etc/snort/snort.conf -v -A console` |
| Console output 2    | `snort -c /etc/snort/snort.conf -v -A cmg` |
| File output (fast)  | `snort -c /etc/snort/snort.conf -v -A fast` |
| File output (full)  | `snort -c /etc/snort/snort.conf -v -A full` |

### Other Logging Options

- **Use specific rules file**:  
  `snort -c /etc/snort/rules/local.rules -v -A console`  
- **Default log path**: `/var/log/snort`  
- **Custom log path**: `snort -v -l /home/username/Desktop`  
- **Log in ASCII format**: `snort -v -K ASCII`  

### Reading Logs and PCAPs

| Task                          | Command |
|-------------------------------|---------|
| Read snort log                | `snort -v -r snort.log` |
| Read N packets from log       | `snort -v -r snort.log -n 10` |
| Filter BPF (e.g., TCP)        | `snort -v -r snort.log tcp` |
| Filter UDP DNS traffic        | `snort -v -r snort.log 'udp and port 53'` |

### PCAP Processing

| Task                         | Command |
|------------------------------|---------|
| Process single PCAP          | `snort -c /etc/snort/snort.conf -q -r file.pcap -A console` |
| Multiple PCAPs               | `snort -c /etc/snort/snort.conf -q --pcap-list="file1.pcap file2.pcap" -A console` |
| PCAPs from folder            | `snort -c /etc/snort/snort.conf -q --pcap-dir=/home/pcap-folder -A console` |
| Show PCAP names              | Add `--pcap-show` to the above |

---

## 🛡️ Snort Rule Breakdown

A Snort rule has **two parts**:
- **Header**: action, protocol, source/destination IP and port, direction
- **Options**: details like message, content match, references

### Example Rule
```snort
alert tcp $EXTERNAL_NET any -> $HOME_NET $HTTP_PORTS (
    msg:"Directory Traversal Attempt!";
    flow:established;
    nocase;
    content:"HTTP";
    fast_pattern;
    content:"|2E 2E 2F|";
    content:"/..";
    session:all;
    reference:CVE,XXX;
    sid:100001;
    rev:1;
)
```

### Header Breakdown
|Field|Description|
|---|---|
|`alert`|Action|
|`tcp`|Protocol|
|`$EXTERNAL_NET`|Source IP|
|`any`|Source Port|
|`->`|Direction|
|`$HOME_NET`|Destination IP|
|`$HTTP_PORTS`|Destination Port|

#### 🔹 General Rule Options
Used to define matching criteria for packets.

```snort
msg:"<message>";               # Message to display when rule matches
sid:<id>;                      # Unique Snort rule ID
rev:<revision>;                # Revision number of the rule
flow:<criteria>;               # TCP session direction/state
nocase;                        # Case-insensitive content matching
content:"<string>";            # Payload content to match
fast_pattern;                  # Optimize content matching
reference:<type,id>;           # Reference to CVE or other identifiers
```

#### Non-Payload Detection Rule Options
```snort
fragoffset:<bytes>;            # Offset for IP fragments
ttl:<value>;                   # Match specific Time-To-Live
tos:<value>;                   # Match Type of Service
id:<value>;                    # Match packet ID
ipopts:<options>;              # Match IP options
flags:<tcp_flags>;             # Match TCP flags (e.g., S, A, F)
seq:<number>;                  # Match TCP sequence number
ack:<number>;                  # Match TCP acknowledgment number
window:<size>;                 # Match TCP window size
```

#### 🔻 Post-Detection Rule Options

```snort
classtype:<type>;              # Classify the alert (e.g., trojan-activity)
priority:<level>;              # Alert priority level
resp:<action>;                 # Response (e.g., reset TCP connection)
react:<action>;                # HTML-based response (for web content)
sid:<id>;                      # Rule identifier
rev:<revision>;                # Rule version
tag:<tracking>;                # Track packets after match
```
