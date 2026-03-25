


> #### 💡Tip
> When a query is selected as a favorite, it runs automatically each time you open the **Hunting** page.

---
# Custom Query

Custom queries allow you to define the following:

|Query parameter|Description|
|---|---|
|Name|Provide a name for the custom query.|
|Description|Provide a description of your query's functionality.|
|Custom query|Your KQL hunting query.|
|Entity mapping|Map entity types to columns from your query result to populate your query results with more actionable information. You can also map entities by using code in your KQL query.|
|Tactics & techniques|Specify the tactics that your query is designed to expose.|

>The [Microsoft Sentinel repository](https://github.com/Azure/Azure-Sentinel) contains out-of-the-box detections, exploration queries, hunting queries, workbooks, playbooks, and more to help you secure your environment and hunt for threats. Microsoft and the Microsoft Sentinel community contribute to this repo.

---
# Save key findings with bookmarks

To hunt for threats to Contoso's environment, you have to review large amounts of log data for evidence of malicious behavior. During this process, **you might find events that you want to remember, revisit, and analyze** as part of validating potential hypotheses and understanding the full story of a compromise. Bookmarked data is visible to you and your teammates for easy collaboration.

## Create or add to incidents by using bookmarks

You can use bookmarks to create a new incident or add bookmarked query results to existing incidents. The **Incident actions** button on the _Hunt_ toolbar enables you to perform either of these tasks when a bookmark is selected.

## Use the investigation graph to explore bookmarks

You can investigate bookmarks in the same way that you'd investigate incidents in Microsoft Sentinel. 
1. From the **Hunting** page, select your _Hunt_ with a _Bookmark_ from the **Hunts (Preview)** tab. 
2. In the _Hunt_ details pane select **Bookmarks** (or select any _Related incidents_), select a specific _Bookmark_ and then select the **Investigate** button to open the investigation graph for the incident. 

The investigation graph is a visual tool that **helps to identify entities involved in the attack and the relationships between those entities**. If the incident involves multiple alerts over time, you can also review the alert timeline and correlations between alerts.

![[Pasted image 20250701114400.png | 500]]

### Review entity details

You can select each entity on the graph to observe complete contextual information about it. This information includes relationships to other entities, account usage, and data flow information. For each information area, you can go to the related events in Log Analytics and add the related alert data into the graph.

---
# Observe threats over time with livestream

**You can use a livestream to:**

- Test new queries against live events.
- Generate notifications for threats.
- Launch investigations.

>Livestream queries refresh every 30 seconds and generate Azure notifications of any new results from the query.

## Create a livestream

To create a livestream from the **Hunting** page in Microsoft Sentinel, select the **Livestream** tab and then select **New livestream** from the toolbar.

>#### ⚠️ Note
> Livestream queries run continuously against your live environment, so you can't use time parameters in a livestream query.


## View a livestream

On the new **Livestream** page, specify a name for the livestream session and the query that provides results for the session. Notifications for livestream events appear in your *Azure portal notifications*.

## Manage a livestream

You can play the livestream to review results or save the livestream. Saved livestream sessions can be viewed from the **Livestream** tab on the **Hunting** page. You can also elevate events from a livestream session to an alert by selecting the events and then selecting **Elevate to alert** from the command bar.

You might use a livestream to track baseline activities for Azure resource deletion, and identify other Azure resources that should be tracked. For example, the following query **returns any Azure Activity events that recorded a deleted resource**:

```sql
AzureActivity
  | where OperationName has 'delete'
  | where ActivityStatus == 'Accepted'
  | extend AccountCustomEntity = Caller
  | extend IPCustomEntity = CallerIpAddres
```

## Use a livestream query to create an analytics rule

If the query returns significant results, you can select **Create analytics rule** from the command bar to create an analytics rule based on the query. After the rule refines the query to identify the specific resources, it can generate alerts or incidents when the resources are deleted.

