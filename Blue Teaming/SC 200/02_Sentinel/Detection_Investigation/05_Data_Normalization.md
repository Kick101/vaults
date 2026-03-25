### Advanced Security Information Model (ASIM) 

It is a *layer that is located between these diverse sources & the user*. ASIM follows the robustness principle: "*Be strict in what you send, be flexible in what you accept*". ASIM transforms Microsoft Sentinel's inconsistent, and hard to use source telemetry to user friendly data.
- ASIM aligns with the OSSEM common information model, allowing for predictable entities correlation across normalized tables.
 
### Open Source Security Events Metadata

OSSEM is a community-led project that focuses primarily on the documentation and standardization of security event logs from diverse data sources and operating systems. The project also provides a *Common Information Model* (CIM) that can be used for data engineers during data normalization procedures to allow security analysts to query and analyze data across diverse data sources.

### ASIM Components
| Component                          | Description                                                                                                                                                                                                                                                                             |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Normalized schemas                 | Cover standard sets of predictable event types that you can use when building unified capabilities. Each schema defines the fields that represent an event, a normalized column naming convention, and a standard format for the field values.                                          |
| Parsers                            | *Map existing data to the normalized schemas* using KQL functions. Many ASIM parsers are available out of the box with Microsoft Sentinel. <br>- More parsers, and versions of the built-in parsers that can be modified can be deployed from the Microsoft Sentinel GitHub repository. |
| Content for each normalized schema | Includes analytics rules, workbooks, hunting queries, and more. Content for each normalized schema works on any normalized data without the need to create source-specific content.                                                                                                     |

![[Pasted image 20250809134031.png]]

### ASIM terminology

| Term                     | Description                                                                                                                                                                                                                 |
| ------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Reporting device         | The system that sends the records to Microsoft Sentinel. This system may not be the subject system for the record that's being sent.                                                                                        |
| Record                   | A unit of data sent from the reporting device. A record is often referred to as log, event, or alert, but can also be other types of data.                                                                                  |
| Content, or Content Item | The different, customizable, or user-created artifacts than can be used with Microsoft Sentinel. Those artifacts include, for example, Analytics rules, Hunting queries and workbooks. A content item is one such artifact. |

### View ASIM Parsers

To view ASIM functions in your Microsoft Sentinel environment.

- Microsoft Sentinel workspace > Select Logs from the left navigation
- Expand the schema and filter pane on the left side (if needed use the ellipsis to reveal all the tools)
- Select Functions
- Expand Microsoft Sentinel

Users use ASIM parsers *instead of table names* in their queries to view data in a normalized format, and to include all data relevant to the schema in your query.

---

### Built-in ASIM parsers and workspace-deployed parsers

| Compare       | Built-in                                                                        | Workspace-deployed                                                               |
| ------------- | ------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| Advantages    | Exist in every Microsoft Sentinel instance. Usable with other built-in content. | New parsers are often delivered first as workspace-deployed parsers.             |
| Disadvantages | Can't be directly modified by users. Fewer parsers available.                   | Not used by built-in content.                                                    |
| When to use   | Use in most cases that you need ASIM parsers.                                   | Use when deploying new parsers, or for parsers not yet available out-of-the-box. |

It's recommended to use built-in parsers for schemas for which built-in parsers are available.

### Parsers Types

- **Unifying parser**
	- The unifying parser in turn calls source-specific parsers to perform the actual parsing and normalization, which is specific for each source.
	- The unifying parser name is *_Im_Schema* for built-in parsers
	- *imSchema* for workspace deployed parsers
- **source-specific parsers**


| Schema          | Unifying parser                        |
| --------------- | -------------------------------------- |
| Authentication  | imAuthentication                       |
| Dns             | _Im_Dns                                |
| File Event      | imFileEvent                            |
| Network Session | _Im_NetworkSession                     |
| Process Event   | imProcessCreate and imProcessTerminate |
| Registry Event  | imRegistry                             |
| Web Session     | _Im_WebSession                         |

>Every schema that supports filtering parameters supports at least the *starttime* and *enttime* parameters and using them is often critical for optimizing performance.

---
## parameterized KQL functions

![[Pasted image 20250809192454.png]]

```sql
AzureActivityByCategory("Administrative", todatetime("2021/04/05 5:40:01.032 PM")) 
```

---
## Create an ASIM Parser

### 1. Collect sample logs

- Find the vendor documentation and samples for the logs can help accelerate development
- **A representative set of logs should include:**
	- Events with different event results.
	- Events with different response actions.
	- Different formats for username, hostname and IDs, and other fields that require value normalization.

### 2. Mapping

- Map all mandatory fields and preferably also recommended fields.
- Try to map any information available from the source to normalized fields. If not available as part of the selected schema, consider mapping to fields available in other schemas.
- Map values for fields at the source to the normalized values allowed by ASIM. The original value is stored in a separate field, such as *EventOriginalResultDetails*.

### 3. Developing Parsers

- Develop both a filtering (parameter) and a parameter-less parser for each relevant schema.
- A custom parser is a *KQL query* developed in the Sentinel Logs page. The parser query has three parts: *Filter > Parse > Prepare fields*

### 4. Filtering the relevant records

In many cases, a table in Microsoft Sentinel includes multiple types of events. For example:

- The Syslog table has data from multiple sources.
- Custom tables may include information from a single source that provides more than one event type and can fit various schemas.

Therefore, a parser should first filter only the records relevant to the target schema.

Filtering in KQL is done using the **where** operator. For example, **Sysmon event 1** reports process creation, and is therefore normalized to the **ProcessEvent** schema. The **Sysmon event 1** event is part of the **Event** table, so you would use the following filter:

```sql
Event | where Source == "Microsoft-Windows-Sysmon" and EventID == 1
```

### 5.  Filtering by source type using a Watchlist

- Used when events *lack distinct **source** identifiers* (e.g., Infoblox DNS via Syslog).
- Parser checks sources against the `ASimSourceType` watchlist.

**Implementation:**

1. **Add watchlist lookup** at the start of the parser:

```sql
let Sources_by_SourceType=(sourcetype:string){_GetWatchlist('ASimSourceType') | where SearchKey == tostring(sourcetype) | extend Source=column_ifexists('Source','') | where isnotempty(Source)| distinct Source };
```

2. **Apply filter** in parser’s filtering section:

```sql
| where Computer in (Sources_by_SourceType('InfobloxNIOS'))
```

**To use this sample in your parser:**

- Replace `Computer` → field holding the source info (keep `Computer` for Syslog-based parsers).
- Replace `InfobloxNIOS` → unique identifier for your parser’s source type.
- Parser users must update the `ASimSourceType` watchlist with this identifier and the relevant source list.

### 6. Filtering based on parser parameters

- Use filtering parameters defined in the relevant schema’s documentation.
- Start from an existing parser to ensure correct function signature and similar filtering logic.
- Filter _before_ parsing using physical fields > if results lack accuracy, filter again _after_ parsing for fine-tuning.
- Skip filtering if the parameter has its default value.

```sql
srcipaddr=='*' or ClientIP==srcipaddr
array_length(domain_has_any) == 0 or Name has_any (domain_has_any)
```

### 7. Filtering optimization

- **Filter on built-in fields** (raw event fields) > faster than filtering on parsed fields.
- Use **fast operators**: `==`, `has`, `startswith`.
- Avoid **slow operators**: `contains`, `matches regex`.

**Accuracy vs. Performance:**

- Built-in field filtering may be less accurate than parsed field filtering.
    1. **Pre-filter** on a built-in field with a performance-optimized operator.
    2. **Post-filter** after parsing with more accurate conditions.

### 8. Parsing

Needed when multiple event fields are stored in a single text field.
    

**Parsing Operators (fastest > slowest):**

1. `split` – Delimited values.
2. `parse_csv` – CSV-formatted values.
3. `parse` – Multiple values from a string (simple pattern faster than regex).
4. `extract_all` – Multiple values via regex (similar speed to regex-based `parse`).
5. `extract` – Single value via regex (faster than `parse` if used once, slower if repeated).
6. `parse_json` – JSON strings (prefer `parse`/`extract` if few fields needed).
7. `parse_xml` – XML strings (same optimization logic as JSON).


**Additional Processing After Parsing:**

- **Formatting / Type Conversion** > e.g., `todatetime()`, `tohex()`.
- **Value Lookup / Mapping** > Convert extracted values to schema values:
    - Few values: `iff`, `case`
```sql
extend EventResult = iff(EventId==257 and ResponseCode==0, 'Success', 'Failure')
```

- Many values: `datatable` + `lookup`

```sql
let RCodeTable = datatable(ResponseCode:int, ResponseCodeName:string) [
    0, 'NOERROR', 1, 'FORMERR'
];
...
| lookup RCodeTable on ResponseCode
| extend EventResultDetails = case(
    isnotempty(ResponseCodeName), ResponseCodeName,
    ResponseCode between (3841 .. 4095), 'Reserved for Private Use',
    'Unassigned'
)
```

### 9. Mapping values

- Normalize extracted values to match target schema format.
- Example: Convert hyphen-separated MAC > colon-separated MAC.

**Main Tools:**

- `extend` + KQL string/number/date functions.
- `lookup` with `datatable` → direct value mapping.
- `iff` / `case` → conditional mapping.

```sql
let NetworkProtocolLookup = datatable(Proto:real, NetworkProtocol:string) [
    6, 'TCP',
    17, 'UDP'
];
let DnsResponseCodeLookup = datatable(DnsResponseCode:int, DnsResponseCodeName:string) [
    0, 'NOERROR',
    1, 'FORMERR',
    2, 'SERVFAIL',
    3, 'NXDOMAIN'
];
...
| lookup DnsResponseCodeLookup on DnsResponseCode
| lookup NetworkProtocolLookup on Proto
```
### 10. Prepare fields in the result set

Ensure **normalized fields** are used in parser output.

|Operator|Purpose|Parser Usage|
|---|---|---|
|`project-rename`|Rename fields while keeping built-in field performance.|Use when an existing field only needs a name change.|
|`project-away`|Remove specific fields.|Use to drop temporary or confusing fields. Avoid removing original (non-normalized) fields unless necessary for clarity or performance.|
|`project`|Select only specified fields.|**Avoid** in parsers—removes all non-selected fields. Use `project-away` instead for selective removal.|
|`extend`|Add aliases or calculated fields.|Use to create alternate names for normalized fields or derived values.|


### 11. Handle parsing variants

- **Conditional logic** > Use `iff` or `case` for variant-specific parsing in one flow.
- **Union method** > Create a separate parsing branch for each variant, then combine results.

To use union to handle multiple variants, create a separate function for each variant, and use the union statement to combine the results:

```sql
let AzureFirewallNetworkRuleLogs = AzureDiagnostics
    | where Category == "AzureFirewallNetworkRule"
    | where isnotempty(msg_s);

// Variant 1
let parseLogs = AzureFirewallNetworkRuleLogs
    | where msg_s has_any("TCP", "UDP")
    | parse-where msg_s with
        networkProtocol:string " request from " srcIpAddr:string ":" srcPortNumber:int
    | project-away msg_s;

// Variant 2
let parseLogsWithUrls = AzureFirewallNetworkRuleLogs
    | where msg_s has_all("Url:", "ThreatIntel:")
    | parse-where msg_s with
        networkProtocol:string " request from " srcIpAddr:string " to " dstIpAddr:string
    | project-away msg_s;

union parseLogs, parseLogsWithUrls

```

**Best Practices:**

- **Filter early** → At the start of each branch, filter only relevant events using native fields.
- **Clean up fields** → Use `project-away` inside each branch to remove temporary fields before `union`.
- Avoid unnecessary overlap to reduce duplicates and processing cost.

### Deploy parsers

**Manual (for testing):**

- Copy parser query → **Azure Monitor Logs** page.
- Save as a **function**.

**Bulk Deployment (recommended for many parsers):**

1. Create **YAML file** > use the correct schema/template (filtering or parameter-less).
    
2. Convert YAML to **ARM template** using **ASIM YAML to ARM converter**.
    
3. If updating: delete old function versions (portal or PowerShell tool).
    
4. Deploy ARM template (portal or PowerShell).
    
5. Use **linked templates** to *deploy multiple parsers at once*.


---
## Configure Azure Monitor Data Collection Rules

- *Another way to Normalize*/transform logs **at ingestion time** > parsed and ready for Sentinel.
- Provides **ETL-like** processing: filter, transform, and route data before storage.

**Types of DCRs in Azure Monitor**

1. **Standard DCR** > For Azure Monitor Agent & custom logs.
2. **Workspace Transformation DCR** > Applies ingestion-time transformations *for **sources** without native DCR support*. Applied directly to a **Log Analytics workspace**.

**Transformations:**

- Written in **KQL**, applied per entry.
- Must understand source format & output target table schema.
- Common operations:
    
    - **Filter**: `where`
        
    - **Add fields**: `extend`
        
    - **Format output**: `project`

```sql
source
| where severity == "Critical"
| extend Properties = parse_json(properties)
| project
    TimeGenerated = todatetime(["time"]),
    Category = category,
    StatusDescription = StatusDescription,
    EventName = name,
    EventId = tostring(Properties.EventId)
```

>When creating a Workspace transformation DCR. What is the name of the virtual table to query? *source*

