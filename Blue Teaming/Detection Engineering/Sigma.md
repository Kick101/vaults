>The "Sigma" format exists as a way to share detections of malicious or dangerous behaviour among security professionals.


---
## Components 

- **[Detection](https://sigmahq.io/docs/basics/rules.html#detection)**  
    _What malicious behaviour the rule searching for._
- **[Logsource](https://sigmahq.io/docs/basics/rules.html#logsources)**  
    _What types of logs this detection should search over._
- **[Metadata](https://sigmahq.io/docs/basics/rules.html#metadata)**  
    _Other information about the detection._



---
## Detection

>*Each Sigma detection* is categorised & split up into groups called "`selections`". Each "`selection`" contains the definition for the detection itself.

__Detection Methods__
- [by Keyword](https://sigmahq.io/docs/basics/rules.html#detection-keyword)
- [by Field Value](https://sigmahq.io/docs/basics/rules.html#detection-and)
- [by Multiple Field Values (List)](https://sigmahq.io/docs/basics/rules.html#detection-or)

### keywords
Each item in the "`keywords`" list is effectively separated by a logical **"OR"** operator.

```yml
logsource:
    product: linux
detection:// [!code focus:8]
    keywords:
        - 'rm *bash_history'
        - 'echo "" > *bash_history'
        - 'truncate -s0 *bash_history'
        - 'history -c'
        - 'history -w'
    condition: keywords
falsepositives:
    - Unknown
```

>### A note on efficiency
>Keep in mind that while keyword-based searches are easy to write, most SIEMs will usually perform vastly better when using [field-based searches](https://sigmahq.io/docs/basics/rules.html#detection-and).

### Field
[`EventID: 6416`](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventid=6416) A new external device was recognised by the system _and_ where the class of drive was a "DiskDrive"

```yml
detection:
  selection:
	EventID: 6416  # and where
    ClassName: 'DiskDrive'
  condition: selection
```

```splunkQL
source="WinEventLog:Security" EventCode=6416 ClassName="DiskDrive"
```

### Field List
The "by Field List" detection method is similar to the ["by Field"](https://sigmahq.io/docs/basics/rules.html#detection-and) method. It is useful when you have to search for multiple values of a field.

For example, you might want to search `Windows\Security` logs and detect when:
- [`EventID: 4728`](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventid=4728) A user is added to a Security Group (eg. Administrators) _or_
- [`EventID: 4729`](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventid=4729) A user is removed from a Security Group _or_
- [`EventID: 4730`](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventid=4730) A Security Group was deleted

```yml
detection: 
    selection:
        EventID:
            - 4728  # or where EventID: 
            - 4729  # or where EventID:
            - 4730
    condition: selection
```

---
## Logsource

It splits up each defined logsource into three distinct fields - `category`, `product`, and `service`.

>#### Standard Logsources Combination
>https://sigmahq.io/docs/basics/log-sources.html#standard-logsources

- **Category**  
    This describes a category of products.  
    (Eg. `webserver`, `firewall` or `edr`)  
- **Product**  
    This describes a specific product.  
    (Eg. `windows`, `linux`, `cisco`)  
- **Service**  
    This describes a service running within a given product  
    (Eg. `kerberos`, `defender` etc).

```yml
logsource: 
  product: windows
  category: ps_script
  definition: Script Block Logging must be enabled
```

---
## Metadata
 
Everything else that you can see around the [logsource](https://sigmahq.io/docs/basics/rules.html#logsources) and [detection](https://sigmahq.io/docs/basics/rules.html#detection) sections is what Sigma calls "Metadata".

Below is a list of standard Sigma metadata fields:
- [Title](https://sigmahq.io/docs/basics/rules.html#metadata-title)
- [ID](https://sigmahq.io/docs/basics/rules.html#metadata-id)
- [Status](https://sigmahq.io/docs/basics/rules.html#metadata-status)
- [Description](https://sigmahq.io/docs/basics/rules.html#metadata-description)
- [License](https://sigmahq.io/docs/basics/rules.html#metadata-license)
- [Author](https://sigmahq.io/docs/basics/rules.html#metadata-author)
- [Date / Modified](https://sigmahq.io/docs/basics/rules.html#metadata-date-modified)
- [References](https://sigmahq.io/docs/basics/rules.html#metadata-references)
- [Tags](https://sigmahq.io/docs/basics/rules.html#metadata-tags)
- [False Positives](https://sigmahq.io/docs/basics/rules.html#metadata-false-positives)
- [Level](https://sigmahq.io/docs/basics/rules.html#metadata-level)

### Title*

The `title` field is used to give a very short summary of what the rule is trying to achieve.

```
title: Okta User Account Locked Out
```

>Hint on writing good Alert titles
>Try to keep your alert titles as short as possible, and avoid prefixes like "Detects when ...".

### ID

The `id` field should be generated whenever you create a Sigma rule, and globally identifies the Sigma rules against all others. For this reason, Sigma recommends using randomly generated UUIDs (version 4).

You can generate your own UUIDv4 by [following the link here](https://www.uuidgenerator.net/version4).

```
id: 12345678-bef0-4204-a928-ef5e620d6fcc
```

>**Rule ID changes**
Rule identifiers should only change for the following reasons:
>- Major changes in the rule. (E.g. a different rule logic.)
> - Derivation of a new rule from an existing or refinement of a rule in a way that both are kept active.
> - Merge of rules.

---
## Modifiers

Below is a list of available field modifiers.

- [`all`](https://sigmahq.io/docs/basics/modifiers.html#all)
- [`base64` / `base64offset`](https://sigmahq.io/docs/basics/modifiers.html#base64-base64offset)
- [`cased`](https://sigmahq.io/docs/basics/modifiers.html#cased)
- [`cidr`](https://sigmahq.io/docs/basics/modifiers.html#cidr)
- [`contains`](https://sigmahq.io/docs/basics/modifiers.html#contains)
- [`endswith`](https://sigmahq.io/docs/basics/modifiers.html#endswith)
- [`exists`](https://sigmahq.io/docs/basics/modifiers.html#exists)
- [`expand`](https://sigmahq.io/docs/basics/modifiers.html#expand)
- [`fieldref`](https://sigmahq.io/docs/basics/modifiers.html#fieldref)
- [`gt`](https://sigmahq.io/docs/basics/modifiers.html#gt)
- [`gte`](https://sigmahq.io/docs/basics/modifiers.html#gte)
- [`lt`](https://sigmahq.io/docs/basics/modifiers.html#lt)
- [`lte`](https://sigmahq.io/docs/basics/modifiers.html#lte)
- [`re`](https://sigmahq.io/docs/basics/modifiers.html#re)
- [`startswith`](https://sigmahq.io/docs/basics/modifiers.html#startswith)
- [`utf16` / `utf16le` / `utf16be` / `wide`](https://sigmahq.io/docs/basics/modifiers.html#wide)
- [`windash`](https://sigmahq.io/docs/basics/modifiers.html#windash)

>Tip:
The `base64offset` modifier is usually preferred over the `base64` modifier, because an ASCII value encoded into `base64` can have 3 different offsets (or shifts) that can occur when completing the encoding process.

### windash

```yml
detection:
  selection:
    fieldname|windash|contains:
      - " -param-name "
      - " -f "
```

The windash modifier will convert any provided command-line arguments or flags to use `-`, as well as `/`, `–` (En Dash), `—` (Em Dash), and `―` (Horizontal Bar).

This is incredibly useful in the the Windows ecosystem, where Windows has [two standards for passing arguments to commands](https://learn.microsoft.com/en-us/powershell/scripting/learn/shell/running-commands?view=powershell-7.3#passing-arguments-to-native-commands), usually `-` for PowerShell (e.g. `-a`), and `/` for `cmd.exe` (e.g. `/a`), but a large number of commands will commonly accept both. Many tools, including PowerShell, will not only accept a normal hyphen, but other similar looking dashes like `–` (En Dash), `—` (Em Dash), and `―` (Horizontal Bar)

### re

```yml
detection:
  selection:
    fieldname|re: .*needle$
```

The `re` modifier will provide a search where the value of `fieldname` matches the provided regex.

There are re sub-modifiers `re|?`:

- `i`: (insensitive) to enable case-insensitive matching.
- `m`: (multi line) to match across multiple lines. `^`/`$` match the start/end of line.
- `s`: (single line) to enable that dot (.) matches all characters, including the newline character.

### fieldref

```yml
detection:
  selection:
    fieldname|fieldref: fieldasString
  condition: selection
```

The `fieldref` mofidier will convert a plain string into a field reference. `fieldname` and `fieldasString` must have the same value. A field reference can be used to compare fields of matched events directly at query/matching time.

### expand

```yml
name: value_placeholder_pipeline
vars:
  administrator_name: Administrator
transformations:
  - type: value_placeholders
```

The `expand` modifier can be used with Sigma Pipelines in order to replace placeholder values with another value common across that processing pipeline.

---
## Conditions

| **Condition**             | **Description**                                                                        | **Example Usage**                                   | **Notes**                                                                     |
| ------------------------- | -------------------------------------------------------------------------------------- | --------------------------------------------------- | ----------------------------------------------------------------------------- |
| `not`                     | Performs an inverse search; excludes events matching the selection.                    | `condition: not selection`                          | Useful for filtering out known false positives.                               |
| `and`                     | Combines two selections, requiring both to be true.                                    | `condition: selection_1 and selection_2`            |                                                                               |
| `or`                      | Combines two selections, requiring at least one to be true.                            | `condition: selection1 or selection2`               |                                                                               |
| `brackets`                | Groups operations to define precedence and remove ambiguity.                           | `condition: selection and not (filter1 or filter2)` | Translates to brackets in the detection environment.                          |
| `1 of (search pattern)`   | Combines all selections matching the regex `(search pattern)` with an `OR` statement.  | `condition: 1 of selection*`                        | `search pattern` is a regex describing the selection group name.              |
| `all of (search pattern)` | Combines all selections matching the regex `(search pattern)` with an `AND` statement. | `condition: all of selection*`                      | `search pattern` is a regex describing the selection group name.              |
| `1 of them`               | Combines _all_ defined selections within the rule with an `OR` statement.              | `condition: 1 of them`                              | **Warning:** Not generally accepted for sharing rules with SigmaHQ/community. |
| `all of them`             | Combines _all_ defined selections within the rule with an `AND` statement.             | `condition: all of them`                            | **Warning:** Not generally accepted for sharing rules with SigmaHQ/community. |

>Filters
>
You can effectively use the `and` and `not` selection conditions to filter our unwanted or known false-positives in your Sigma rule.

```yml
detection:
  selection:
    Image|endswith: "/bin/bash"
  filter:
    DestinationIp:
      - "127.0.0.1"
      - "0.0.0.0"
  condition: selection and not filter
```

---
## Meta Rules

Sigma Meta rules are an **extension** of the Sigma detection format. They allow for the definition of more advanced detection techniques, such as [**correlations**](https://sigmahq.io/docs/meta/correlations.html) and [**filters**](https://sigmahq.io/docs/meta/filters.html).

### Referencing in Meta Rules

Both [Sigma Correlations](https://sigmahq.io/docs/meta/correlations.html) and [Sigma Filters](https://sigmahq.io/docs/meta/filters.html) require you to reference an existing regular Sigma Rule. This is done by using the `rules` keyword underneath the `correlation` or `filter` section.

This pattern allows you to reference an existing rule either by using it's `name` or `id`.

```yml
filter:
  rules:
    - failed_logon # Referencing by name
    - df0841c0-9846-4e9f-ad8a-7df91571771b # Referencing by ID
```

>#### Global References
When converting Sigma rules, it's important to remember that Sigma Correlations and Sigma Filters can reference any Sigma rule – not just those within the same file.
However, it is important to supply the rule at conversion time to ensure that the Sigma Correlation or Sigma Filter can be applied correctly.

---
## Correlations
The `correlation` section has the following structure:
- `type`: The type of correlation to be used ([see types below](https://sigmahq.io/docs/meta/correlations.html#types-of-correlations)).
- `rules`: A list of Sigma rules that are used for the correlation (either by name or ID).
- `group-by`: _(Optional)_ A list of fields to group the events by.
- `timespan`: The time frame in which the events are aggregated.
- `condition`: The condition that has to be met for the correlation to match.

```yml
title: Multiple failed logons for a single user (possible brute force attack)
status: test
correlation: 
    type: event_count
    rules:
        - failed_logon
    group-by:
        - TargetUserName
        - TargetDomainName
    timespan: 5m
    condition:
        gte: 10
tags:
    - brute_force
    - attack.t1110
```

Because this correlation rule references another Sigma rule called `failed_logon`, a rule with the field `name: failed_logon` needs to be supplied alongside this rule when we're converting the correlation rule for our SIEM.

Therefore, it's common to place this "base" Sigma rule in the same file as the correlation rule, using the `---` separator to separate the two rules.
```yml
title: Windows Failed Logon Event
name: failed_logon # Rule Reference
description: Detects failed logon events on Windows systems.
logsource:
    product: windows
    service: security
detection:
    selection:
        EventID: 4625
    condition: selection
---
title: Multiple failed logons for a single user (possible brute force attack)
correlation:
    type: event_count
    rules:
        - failed_logon # Referenced here
    group-by:
        - TargetUserName
        - TargetDomainName
    timespan: 5m
    condition:
        gte: 10
```

There are a few things to note when working with Sigma Correlations:

- Correlation Rules omits the `logsource` section, as they rely on referencing other Sigma rules to correlate events.
- Correlation rules will also inhibit the original "base" rule in the output query, as the correlation rule is the one that will be used to generate the query.

