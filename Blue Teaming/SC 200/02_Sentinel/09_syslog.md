**Syslog** is an event logging protocol that in Linux. When the agent is installed on your VM or appliance, the *installation routine configures the Syslog daemon* to forward messages to the agent on TCP port *25224*.


>If Microsoft Defender for Cloud Auto-provisioning is enabled, the Azure Monitor Linux Agent is installed by default as an extension using Azure Policy assignment.

### Syslog Data Sources: DCR

The Data Collection Rule (DCR) only collects events with the facilities and severities that are specified in its Data sources configurations.

>The *default* is "*LOG_DEBUG*" for each Facility, and changes are automatically pushed to all configured resources.

### Parsing

A Parser is a KQL Function that is a *query saved as a function*. The reference to the function name is like accessing any other table. By creating parses, you only need to write the *SyslogMessage* parsing once.

```sql
Syslog
| where ProcessName contains "squid"
| extend URL = extract("(([A-Z]+ [a-z]{4,5}:\\/\\/)|[A-Z]+ )([^ :]*)",3,SyslogMessage), 
         SourceIP = extract("([0-9]+ )(([0-9]{1,3})\\.([0-9]{1,3})\\.([0-9]{1,3})\\.([0-9]{1,3}))",2,SyslogMessage), 
         Status = extract("(TCP_(([A-Z]+)(_[A-Z]+)*)|UDP_(([A-Z]+)(_[A-Z]+)*))",1,SyslogMessage), 
         HTTP_Status_Code = extract("(TCP_(([A-Z]+)(_[A-Z]+)*)|UDP_(([A-Z]+)(_[A-Z]+)*))/([0-9]{3})",8,SyslogMessage),
         User = extract("(CONNECT |GET )([^ ]* )([^ ]+)",3,SyslogMessage),
         RemotePort = extract("(CONNECT |GET )([^ ]*)(:)([0-9]*)",4,SyslogMessage),
         Domain = extract("(([A-Z]+ [a-z]{4,5}:\\/\\/)|[A-Z]+ )([^ :\\/]*)",3,SyslogMessage)
| extend TLD = extract("\\.[a-z]*$",0,Domain)
```

