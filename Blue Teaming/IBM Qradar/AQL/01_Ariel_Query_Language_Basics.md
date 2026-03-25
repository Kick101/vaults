### Sources

- [simarc fields](https://www.ibm.com/docs/en/qradar-on-cloud?topic=language-event-flow-simarc-fields-aql-queries)
- 

### Examples

```sql
SELECT sourceip,destinationip FROM events LIMIT 5 LAST 24 HOURS
```

```sql
SElECT * FROM events ORDER BY magnitude DESC LAST 24 HOURS
```

```SQL
SElECT * FROM events WHERE magnitude AND sourceip='169.0.245.1' >= 3 LAST 24 HOURS
```

```sql
SELECT * FROM events WHERE NOT INCIDR('169.0.245.0/24',sourceip) 
-- where sourceip is not in the CIDR we specified
```

```sql
SELECT * FROM events WHERE TEXT SEARCH 'firewall'
```

```sql
SELECT * FROM events WHERE username IS NOT NULL
```

**Functions**

```sql
SELECT LOGSOURCETYPENAME(devicetype) AS log_source_name, COUNT(log_source_name) AS event_count WHERE devicetype NOT IN (368,105) FROM events GROUP BY log_source_name ORDER BY event_count DESC LIMIT 3
```

```sql
SELECT UTF8(payload) FROM events
```

```sql
SELECT qid,QIDNAME(qid),* FROM events
```

```sql
categoryname(category) -- category is ID
```

```sql
LOGSOURCENAME(logsourceid) -- logsourceid is ID
```

```sql
SELECT * FROM events WHERE sourceip = '10.10.10.100' AND hasoffense = TRUE 
```

```sql
 INOFFENSE(171) -- events involved in offense ID
```

```sql
count(*)
```

```sql
SELECT logsourcename(logsourceid) as LogSource, sum(eventcount) / ((max(endTime) - min(startTime))/1000) AS eps FROM events GROUP BY logsourceid ORDER BY EPS ASC LAST 6 Hours
```

```sql
SELECT GEOGRAPHICLOCATION AS country, COUNT(geographiclocation) AS country_hits FROM events WHERE geographiclocation != 'other' GROUP BY geographiclocation ORDER BY country_hits DESC
```

```sql
SELECT sourceip AS source, destinationip AS 'Destination', ASSETUSER(sourceip,now()) as 'User' FROM events WHERE sourceip = '172.16.18.101'
-- username associated with source IP
```

```sql
SELECT NETWORKNAME(sourceip) AS SourceNet, NetworkName(destinationip) AS DestinationNet FROM events WHERE DestinationNet != NULL
-- 
```

```sql
SELECT username, UNIQUECOUNT(sourceip) FROM events GROUP BY username
```






---



