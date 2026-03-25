Snort is an open-source tool that functions as an IDS, IPS, packet logger, and sniffer. It inspects network traffic to detect and log activity, especially at the application layer. Snort uses rule sets to define what threats or patterns to identify.

## Snort Operation Modes

- Inline IDS/IPS
- Passive IDS
- Network-based IDS
- Host-based IDS (however, Snort is not ideally a host-based IDS. We would recommend opting for more specialized tools for this.)

## Snort Architecture
In order for Snort to transition from a ***simple packet sniffer to a robust IDS***, several key
components were added.


|**Component**|**Function/Description**|**Configuration Location**|
|---|---|---|
|**Packet Sniffer & Decoder**|Extracts raw network traffic, recognizes packet structure, forwards packets to Preprocessors.|N/A|
|**Preprocessors**|Identify the type or behavior of forwarded packets (e.g., HTTP plugin for HTTP packets, port_scan plugin for port scanning attempts).|`snort.lua` configuration file|
|**Detection Engine**|Compares each packet against predefined Snort rules; forwards matches to Logging and Alerting System.|N/A|
|**Logging and Alerting System**|Records or triggers alerts based on rule actions; logs stored in syslog, unified2, or databases.|`snort.lua` configuration file|
|**Output Modules**|Manage how logs and alerts are output/stored.|`snort.lua` configuration file|

## Snort Configuration & Validating Snort's Configuration

**The snort.lua file serves as the principal configuration file for Snort. This file contains the following sections:**
- Network variables
- Decoder configuration
- Base detection engine configuration
- Dynamic library configuration
- Preprocessor configuration
- Output plugin configuration
- Rule set customization
- Preprocessor and decoder rule set customization
- Shared object rule set customization

---
#### Parameters

| Parameter | Description                                                                                                              |
| --------- | ------------------------------------------------------------------------------------------------------------------------ |
| -v        | Verbose. Display the TCP/IP output in the console.                                                                       |
| -d        | Display the packet data (payload).                                                                                       |
| -e        | Display the link-layer (TCP/IP/UDP/ICMP) headers.                                                                        |
| -X        | Display the full packet details in HEX.                                                                                  |
| -i        | Define a specific network interface to listen/sniff. Useful with multiple interfaces.                                    |
| -c        | Specify the Snort configuration file to use.                                                                             |
| -l        | Logger mode, target **log and alert** output directory. <br>Default action: dump as tcpdump format in **/var/log/snort** |
| -A        | Set the alert mode (e.g., fast, full, none, unsock).                                                                     |
| -K        | Set the logging mode (e.g., ascii, pcap).                                                                                |
| -N        | Disable packet logging.                                                                                                  |
| -s        | Log packets in a more human-readable format (short logging).                                                             |
| -T        | Test and validate the Snort configuration file without running Snort.                                                    |
| -r        | Reading option, read the dumped logs in Snort.                                                                           |
| -n        | Specify the number of packets that will process/read. Snort will stop after reading the specified number of packets.     |

#### Alert/IDS Modes
| Mode    | Description                                                                                       |
| ------- | ------------------------------------------------------------------------------------------------- |
| console | Provides fast style alerts on the console screen.                                                 |
| cmg     | Provides basic header details with payload in hex and text format.                                |
| full    | Full alert mode, providing all possible information about the alert.                              |
| fast    | Fast mode, shows the alert message, timestamp, source and destination IP along with port numbers. |
| none    | Disabling alerting.                                                                               |

__Test Config__
```shell
 sudo snort -c /etc/snort/snortv2.conf  -T
```
---
### Components of Snort

- **Packet Decoder -** Packet collector component of Snort. It collects and prepares the packets for pre-processing. 
- **Pre-processors -** A component that arranges and modifies the packets for the detection engine.
- **Detection Engine -** The primary component that process, dissect and analyse the packets by applying the rules. 
- **Logging and Alerting** - Log and alert generation component.
- **Outputs and Plugins** - Output integration modules (i.e. alerts to syslog/mysql) and additional plugin (rule management detection plugins) support is done with this component.

---
### Snort Rule Types

- **Community Rules** - Free ruleset under the GPLv2. Publicly accessible, no need for registration.
- **Registered Rules** - Free ruleset (requires registration). This ruleset contains subscriber rules with 30 days delay.
- **Subscriber Rules (Paid)** - Paid ruleset (requires subscription). This ruleset is the main ruleset and is updated twice a week (Tuesdays and Thursdays).

You can download and read more on the rules [here](https://www.snort.org/downloads).

> #### Once you install Snort2, it automatically creates the required directories and files. However, if you want to use the community or the paid rules, you need to indicate each rule in the snort.conf file.

---
## Snort Operating Modes

 **Sniffer Mode**  
  Reads IP packets and displays them in the console. Useful for real-time monitoring and diagnostics.

 **Packet Logger Mode**  
  Logs all inbound and outbound IP packets that traverse the network. Suitable for packet capture and analysis.

**NIDS & NIPS Modes**  
  Analyzes network traffic based on user-defined rules.  
  - **NIDS**: Detects and logs packets identified as malicious.  
  - **NIPS**: Detects and actively drops or blocks malicious packets.

---
## Snort Rules

> #### Remember, once you create a rule, it is a local rule and should be in your "/etc/snort/rules/local.rules"
### Logical Operators

| Symbol | Meaning                 | Example                              |
| ------ | ----------------------- | ------------------------------------ |
| `[]`   | OR list                 | `[21,23]` → port 21 **or** 23        |
| `!`    | NOT                     | `![80,443]` → not 80 **and not** 443 |
| `:`    | Range                   | `1024:65535` → 1024 **to** 65535     |
| `<>`   | Bidirectional matching  | `<>` means source ↔ destination      |
| `->`   | Unidirectional matching | Source → Destination                 |
### Snort Rule Options
- **General Rule Options**
- **Payload Rule Options**
- **Non-Payload Rule Options**
#### General Rule Options

| Option    | Description |
|-----------|-------------|
| **msg**   | A basic message displayed when the rule is triggered. It acts as a summary or identifier. |
| **sid**   | Unique Snort Rule ID. <br> - `<100`: Reserved <br> - `100-999999`: Built-in rules <br> - `>=1000000`: User-created rules. |
| **reference** | Used to attach external info like CVE ID or threat documentation. Helps during investigation. |
| **rev**   | Revision number of the rule. Helps track rule changes and improvements. |

__Example__
```snort
alert icmp any any <> any any (msg: "ICMP Packet Found"; sid:100001; reference:cve,CVE-XXXX; rev:1;)
```

#### Payload Detection Rule Options
| Option           | Description                                                                                                            |
| ---------------- | ---------------------------------------------------------------------------------------------------------------------- |
| **content**      | Searches payload for ASCII or HEX patterns. Can be used multiple times.                                                |
| **nocase**       | Disables case sensitivity in `content`.                                                                                |
| **fast_pattern** | Optimizes performance by marking a specific `content` for fast matching. Used when there are multiple content matches. |

**Examples**
__ASCII__
```shell
alert tcp any any <> any 80 (msg: "GET Request Found"; content:"GET"; sid:100001; rev:1;)
```

__Hex__
```shell
alert tcp any any <> any 80 (msg: "GET Request Found"; content:"|47 45 54|"; sid:100001; rev:1;)
```

**Case-insensitive search**
```shell
alert tcp any any <> any 80 (msg: "GET Request Found"; content:"GET"; nocase; sid:100001; rev:1;)
```

**Fast pattern w/ multiple contents**
```shell
alert tcp any any <> any 80 (msg: "GET Request Found"; content:"GET"; fast_pattern; content:"www"; sid:100001; rev:1;)
```

#### Non-Payload Detection Rule Options

|Option|Description|
|---|---|
|**id**|Filters specific IP ID field.|
|**flags**|Filters TCP flags (F=FIN, S=SYN, R=RST, P=PSH, A=ACK, U=URG).|
|**dsize**|Filters based on payload size (e.g., `dsize:>100`, `dsize:<100`, `dsize:100<>300`).|
|**sameip**|Triggers if source and destination IPs are the same.|

**Examples**
**ID Filtering**
```shell
alert tcp any any <> any any (msg: "ID TEST"; id:123456; sid:100001; rev:1;)
```

**TCP flag filtering**
```shell
alert tcp any any <> any any (msg: "FLAG TEST"; flags:S; sid:100001; rev:1;)
```

**Payload size**
```shell
alert ip any any <> any any (msg: "SEQ TEST"; dsize:100<>300; sid:100001; rev:1;)
```

**Same source and destination IP**
```shell
alert ip any any <> any any (msg: "SAME-IP TEST"; sameip; sid:100001; rev:1;)
```

---
## Snort Configuration Steps

#### Step #1: Set the Network Variables

This section defines the **scope** of detection and rule paths.

| **TAG NAME**        | **INFO**                                                        | **EXAMPLE**                |
|---------------------|------------------------------------------------------------------|----------------------------|
| `HOME_NET`          | Network you want to protect.                                     | `'any'` or `'192.168.1.1/24'` |
| `EXTERNAL_NET`      | Traffic from outside your network. Usually set as `any` or not `$HOME_NET`. | `'any'` or `'!$HOME_NET'` |
| `RULE_PATH`         | Hardcoded path to Snort rules.                                   | `/etc/snort/rules`         |
| `SO_RULE_PATH`      | Path for Subscriber/Registered Shared Object rules.             | `$RULE_PATH/so_rules`      |
| `PREPROC_RULE_PATH` | Path for Subscriber/Registered Preprocessor rules.              | `$RULE_PATH/plugin_rules`  |

#### Step #2: Configure the Decoder

This section sets the **IPS mode**. For a single-node Snort IPS setup, `afpacket` mode is commonly used.

| **TAG NAME**        | **INFO**                              | **EXAMPLE**           |
|---------------------|----------------------------------------|-----------------------|
| `#config daq:`       | Specifies the DAQ (Data Acquisition) method for IPS. | `afpacket`            |
| `#config daq_mode:`  | Sets DAQ mode to inline (IPS mode).   | `inline`              |
| `#config logdir:`    | Default path to store logs.           | `/var/logs/snort`     |

#### DAQ
Data Acquisition Modules (DAQ) are specific libraries used for packet I/O, bringing flexibility to process packets. It is possible to select DAQ type and mode for different purposes.
- **Pcap:** Default mode, known as Sniffer mode.
- **Afpacket:** Inline mode, known as IPS mode.
- **Ipq:** Inline mode on Linux by using Netfilter. It replaces the snort_inline patch.  
- **Nfq:** Inline mode on Linux.
- **Ipfw:** Inline on OpenBSD and FreeBSD by using divert sockets, with the pf and ipfw firewalls.  
- **Dump:** Testing mode of inline and normalisation.

> ##### The most popular modes are the default (pcap) and inline/IPS (Afpacket).

#### Step #6: Configure output plugins section.

This section manages the outputs of the IDS/IPS actions, such as logging and alerting format details. The default action prompts everything in the console application, so configuring this part will help you use the Snort more efficiently. 

#### Step #7: Customise your ruleset section.
This section manages which rule files Snort loads.

| **TAG NAME**         | **INFO**                                           | **EXAMPLE**                            |
|----------------------|----------------------------------------------------|----------------------------------------|
| `# site specific rules` | Path to local, custom, or user-generated rules.     | `include $RULE_PATH/local.rules`       |
| `#include $RULE_PATH/` | Path to default or downloaded rule files.           | `include $RULE_PATH/rulename`          |
