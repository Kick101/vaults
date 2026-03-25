Microsoft Graph exposes `REST APIs` and `client libraries` to access data on the following Microsoft cloud services:

- `Microsoft 365 core services`: Bookings, Calendar, Delve, Excel, Microsoft Purview eDiscovery, Microsoft Search, OneDrive, OneNote, Outlook/Exchange, People (Outlook contacts), Planner, SharePoint, Teams, To Do, Viva Insights
- `Enterprise Mobility + Security services`: Advanced Threat Analytics, Advanced Threat Protection, Microsoft Entra ID, Identity Manager, and Intune
- `Windows services`: activities, devices, notifications, Universal Print
- Dynamics 365 Business Central services

## Microsoft Graph Security API

The Microsoft Graph Security API provides a **unified interface** to access data from **multiple Microsoft security solutions** programmatically.
- Requests to the `Microsoft Graph security API` are *federated* to all applicable security providers. 
- The results are aggregated and returned to the requesting application in a common schema.

![Diagram showing the Microsoft Security Graph architecture|700](https://learn.microsoft.com/en-us/training/wwl-sci/introduction-microsoft-365-threat-protection/media/graph-security-overview-diagram.png)


**Developers can use the Security Graph to build intelligent security services that:**

- Integrate and correlate security alerts from multiple sources.
- Stream alerts to SIEM solutions.
- Automatically send threat indicators to Microsoft security solutions to enable alert, block, or allow actions.
- Unlock contextual data to inform investigations.
- Discover opportunities to learn from the data and train your security solutions.
- Automate SecOps for greater efficiency.

### Microsoft Graph Security API Versions
- Microsoft Graph REST API v1.0
- Microsoft Graph REST API Beta

The `beta version provides new or enhanced APIs` that are still in preview status. APIs in preview status are subject to change, and may break existing scenarios without notice.

Microsoft Graph API versions support advanced hunting using the `runHuntingQuery` method. This method includes a query in `KQL`.

```json
POST https://graph.microsoft.com/v1.0/security/runHuntingQuery

{
    "Query": "DeviceProcessEvents | where InitiatingProcessFileName =~ \"powershell.exe\" | project Timestamp, FileName, InitiatingProcessFileName | order by Timestamp desc | limit 2"
}
```

You can use [Graph Explorer](https://developer.microsoft.com/graph/graph-explorer) to run the hunting query:

![Screenshot of the Microsoft Graph Explorer running the KQL hunting query.](https://learn.microsoft.com/en-us/training/wwl-sci/introduction-microsoft-365-threat-protection/media/graph-explorer-hunting-kql-query-2023-06-08.png)

**Additional reading** - For more information, see [The Microsoft Graph Security API](https://learn.microsoft.com/en-us/graph/security-concept-overview)