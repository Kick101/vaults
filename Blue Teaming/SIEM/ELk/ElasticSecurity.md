### Rules Types
|**Rule Type**|**Description**|**Trigger Condition**|**Example**|
|---|---|---|---|
|**Custom Query**|Executes a specific KQL (Kibana Query Language) query and alerts if any match is found.|Match found in the query results.|Alert when a specific process name appears in logs.|
|**Threshold**|Monitors for aggregated occurrences of a condition exceeding a set threshold.|Number of matches meets or exceeds (or is below) the defined threshold.|Alert if failed logins exceed 10 attempts in 5 minutes.|
|**Event Correlation**|Detects a defined sequence of events occurring in a specific order.|Events must match the query and occur in the specified sequence.|Alert on process creation followed by network activity from the same process.|
|**Indicator Match**|Matches log fields against known threat indicators from a separate index (e.g., threat intel).|One or more fields match values in the indicator index.|Alert if a process communicates with an IP found in a threat intelligence feed.|
|**Machine Learning**|Uses ML to detect anomalies and behavioral deviations. Currently listed but disabled by default.|Statistical deviation from baseline behavior (e.g., unusual login locations).|Alert when a user logs in from an atypical IP address not seen in prior behavior.|


