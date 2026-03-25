# Extract data from unstructured string fields

Security log data is often contained in unstructured string fields and requires parsing to extract data. There are multiple ways of pulling information from string fields in KQL. The two primary operators used are: **extract and parse**.

## Extract

Extract gets a match for a regular expression from a text string. You may optionally convert the extracted substring to the indicated type.

```sql
extract("x=([0-9.]+)", 1, "hello x=45.6|wo")
```

- Looks for a pattern `x=` followed by one or more digits or dots (`[0-9.]+`).
- The parentheses create **capture group 1**, which extracts just the number (`45.6`).

**Arguments**

| **Argument**   | **Type**         | **Required?** | **Description**                                                                         |
| -------------- | ---------------- | ------------- | --------------------------------------------------------------------------------------- |
| `regex`        | `string`         | ✅ Yes         | Regular expression pattern used to match content in the input string.                   |
| `captureGroup` | `int` (positive) | ✅ Yes         | Capture group to extract: `0` = full match, `1` = first group, etc.                     |
| `text`         | `string`         | ✅ Yes         | Input string to search against using the regular expression.                            |
| `typeLiteral`  | `type literal`   | ❌ Optional    | Target data type (`typeof(long)`, `typeof(datetime)`, etc.) to convert the result into. |

```sql
SecurityEvent
| where EventID == 4672 and AccountType == 'User'
| extend Account_Name = extract(@"^(.*\\)?([^@]*)(@.*)?$", 2, tolower(Account))
| summarize LoginCount = count() by Account_Name
| where Account_Name != ""
| where LoginCount < 10
```

## Parse

Parse evaluates a string expression and parses its value into one or more calculated columns. The computed columns have nulls for unsuccessfully parsed strings.

**Syntax**

`T | parse [kind=regex [flags=regex_flags] |simple|relaxed] Expression with * (StringConstant ColumnName [: ColumnType]) *`

|**Argument**|**Type**|**Required?**|**Description**|
|---|---|---|---|
|`T`|Table|✅ Yes|The input table (rows over which parsing is done).|
|`kind`|Keyword|❌ Optional|Parsing mode:• `simple` – default, exact string match• `regex` – treat delimiters as regex• `relaxed` – allows partial type matches• `flags` – apply RE2 regex flags like `i`, `m`, `s`, `U`|
|`Expression`|`string`|✅ Yes|Column or expression to extract data from (usually a raw log string).|
|`ColumnName`|`string`|✅ Yes|Name of the column to create from the parsed field.|
|`ColumnType`|`scalar type`|❌ Optional|Optional type for conversion (`string`, `long`, `datetime`, etc.). Defaults to `string`.|

```sql
let Traces = datatable(EventText:string)
[
"Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=23, lockTime=02/17/2016 08:40:01, releaseTime=02/17/2016 08:40:01, previousLockTime=02/17/2016 08:39:01)",
"Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=15, lockTime=02/17/2016 08:40:00, releaseTime=02/17/2016 08:40:00, previousLockTime=02/17/2016 08:39:00)",
"Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=20, lockTime=02/17/2016 08:40:01, releaseTime=02/17/2016 08:40:01, previousLockTime=02/17/2016 08:39:01)",
"Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=22, lockTime=02/17/2016 08:41:01, releaseTime=02/17/2016 08:41:00, previousLockTime=02/17/2016 08:40:01)",
"Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=16, lockTime=02/17/2016 08:41:00, releaseTime=02/17/2016 08:41:00, previousLockTime=02/17/2016 08:40:00)"
];
Traces  
| parse EventText with * "resourceName=" resourceName ", totalSlices=" totalSlices:long * "sliceNumber=" sliceNumber:long * "lockTime=" lockTime ", releaseTime=" releaseTime:date "," * "previousLockTime=" previousLockTime:date ")" *  
| project resourceName, totalSlices, sliceNumber, lockTime, releaseTime, previousLockTime
```

---
# Extract data from structured string data

Strings fields may also contain structured data like JSON or Key-Value pairs. KQL provides easy access to these values for further analysis.

## Dynamic Fields (Dot Notation)

To access the strings within a Dynamic field, use the **dot notation**. The `DeviceDetail` field from the `SigninLogs` table is of type dynamic. In this example, you could access the Operating System with the `DeviceDetail.operatingSystem` field name.

```sql
SigninLogs 
| extend OS = DeviceDetail.operatingSystem
```

The query example below shows the use of Dynamic fields with the `SigninLogs` table.

```sql
// Example query for SigninLogs showing how to break out packed fields.

SigninLogs 
| extend OS = DeviceDetail.operatingSystem, Browser = DeviceDetail.browser 
| extend StatusCode = tostring(Status.errorCode), StatusDetails = tostring(Status.additionalDetails) 
| extend Date = startofday(TimeGenerated) 
| summarize count() by Date, Identity, UserDisplayName, UserPrincipalName, IPAddress, ResultType, ResultDescription, StatusCode, StatusDetails 
| sort by Date
```

## JSON

KQL provides functions to manipulate **JSON** stored in *string fields*. Many logs submit data in JSON format, which requires you to know how to transform JSON data to query-able fields.

| **Function**                    | **Description**                                                                                                                                                                                                                              |
| ------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `parse-json()` or `todynamic()` | Interprets a string as a JSON value and returns the value as dynamic. Use either of these functions to refer to a field: `JsonField.Key` or `JsonField["Key"]`                                                                               |
| `mv-expand`                     | is applied on a dynamic-typed array or property bag column so that **each value in the collection gets a separate row**. All the other columns in an expanded row are duplicated. `mv_expand` is the **easiest way to process JSON arrays**. |
| `mv-apply`                      | Applies a subquery to each record and returns the union of the results of all subqueries. Apply a query to each value in an array.                                                                                                           |

> In `mv-expand`, the `mv` stands for **multi-value**.
> `mv-expand` -- Doesn't flatten nested arrays (use `mv-apply` or further parsing)

```sql
SigninLogs 
| extend AuthDetails =  parse_json(AuthenticationDetails) 
| extend AuthMethod =  AuthDetails[0].authenticationMethod 
| extend AuthResult = AuthDetails[0].["authenticationStepResultDetail"] 
| project AuthMethod, AuthResult, AuthDetails 

SigninLogs 
| mv-expand AuthDetails = parse_json(AuthenticationDetails) 
| project AuthDetails

SigninLogs 
| mv-apply AuthDetails = parse_json(AuthenticationDetails) on
(where AuthDetails.authenticationMethod == "Password")
```

---
# Integrate external data

The `externaldata` operator returns a table whose schema is defined in the query itself. And whose data is read from an external storage artifact, such as a blob in Azure Blob Storage or an Azure Data Lake Storage file.

It allows you to query external data **without ingesting** it into a *Log Analytics workspace*.

**Syntax**

```
externaldata(Column1:Type1, Column2:Type2, ...)
[
    "https://<storageaccount>.blob.core.windows.net/<container>/<file>"
]
with (
    format = '<format>',          -- csv, json, parquet
    has_header_row = <bool>,      -- true/false (for csv)
    field_terminator = '<char>',  -- optional (e.g., ',')
    encoding = '<encoding>'       -- optional (e.g., 'UTF8')
)
```

|**Argument**|**Type**|**Required?**|**Description**|
|---|---|---|---|
|`ColumnName`|`string`|✅ Yes|Name of a column in the table schema.|
|`ColumnType`|`scalar type`|✅ Yes|Data type of the column (`string`, `long`, `datetime`, etc.).|
|`StorageConnectionString`|`string`|✅ Yes|Path/connection string to the external storage (e.g., Azure Blob, ADLS).|
|`PropertyName`|`string`|❌ Optional|Additional property describing the ingestion (e.g., `format`, `encoding`, etc.)|
|`PropertyValue`|`scalar`|❌ Optional|Value associated with the above property.|

**Currently, supported properties are:**

|Property|Type|Description|
|---|---|---|
|format|string|Data format. If not specified, an attempt is made to detect the data format from file extension (defaults to CSV). Any of the ingestion data formats are supported.|
|ignoreFirstRecord|bool|If set to true, indicates that the first record in every file is ignored. This property is useful when querying CSV files with headers.|
|ingestionMapping|string|A string value that indicates how to map data from the source file to the actual columns in the operator result set. See data mappings.|

```sql
externaldata(name:string, age:int, city:string)
[
  "https://mystorage.blob.core.windows.net/logs/users.csv"
]
with (
  format = 'csv',
  has_header_row = true
)
```

You want to **match entries in the `Users` table** against a list of user IDs **stored in an external file** (`users.txt`) on Azure Blob Storage, **without ingesting** that file permanently.

```sql
Users
| where UserID in ((externaldata (UserID:string) [
    @"https://storageaccount.blob.core.windows.net/storagecontainer/users.txt" 
      h@"?...SAS..." // Secret token needed to access the blob
    ]))
| ...
```

|Part|Explanation|
|---|---|
|`externaldata (UserID:string)`|Declares the schema (one column: `UserID` of type `string`)|
|`[...]`|URL to the external blob file, can be local or Azure-hosted|
|`h@"..."`|Indicates a **SAS token** is required (h = header), and should be kept confidential|
|`in (...)`|Filters the `Users` table where `UserID` matches any entry from the external file|

---
# Create parsers with functions

Parsers are functions that **define a virtual table** with already parsed unstructured strings fields such as Syslog data.

In the Logs window, you create a query, select the Save button, enter the Name, and select Save As Function from the drop-down. In this case, if we name the function "PrivLogins", I can then access the table using the name PrivLogins.

```sql
SecurityEvent
| where EventID == 4672 and AccountType == 'User'
```

```sql
PrivLogins
```

