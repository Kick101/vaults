### Components
__Indexer__
- Processes, normalises into field-value pairs, stores them as events.

__Search Head__
- Search the indexed logs
- Splunk search Processing Language (SPL)
- SPL sent to indexer & relevant events are returned in the form of field value pairs.
- Provides the ability to **transform the results into presentable tables**, *visualisation* like pie-chart, bar-chart column chart.

__Forwarder__
- Web traffic
- Windows Event Logs, Powershell, sysmon data
- Database connection requests, responses, errors
- Linux hosts
