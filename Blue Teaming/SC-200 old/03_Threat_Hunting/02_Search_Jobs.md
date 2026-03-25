search jobs are ideally suited to search archived logs. If you need to do a full investigation on archived data, you can *restore that data into the hot cache* to run high performing queries and analytics.

`Search` in Microsoft Sentinel is built on top of search jobs. Search jobs are **asynchronous** queries that fetch records. The results are returned to a search table that's created in your Log Analytics workspace after you start the search job. The search job uses **parallel processing** to run the search across long time spans, in large datasets. So search jobs don't impact the workspace's performance or availability.

Search results remain in a search results table that has a `*_SRCH` suffix.

### ✅ Supported log types

- Analytics logs
- Basic logs

### ⛔ Limitations of a search job

- One table per job
- Up to **1-year** date range
- **24-hour** timeout
- Max **1 million records**
- Max **5 concurrent** jobs per workspace
- Max **100 search jobs/day**, **100 result tables/workspace**

### 🔍 Start a search job

Go to Search in Microsoft Sentinel to enter your search criteria.

1. In the Azure portal, go to Microsoft Sentinel and select the appropriate workspace.
2. Under General, select Search.
3. In the Search box, enter the search term.
4. Select the appropriate Time range.
5. Select the Table that you want to search.
6. When you're ready to start the search job, select Search.
	- When the search job starts, a notification and the job status show on the search page.
7. Wait for your search job to complete. Depending on your dataset and search criteria, the search job may take a few minutes or up to 24 hours to complete. If your search job takes longer than 24 hours, it times out. If that happens, refine your search criteria and try again.

### 🔎📂 View search job results

1. In your Microsoft Sentinel workspace, select Search > Saved Searches.
2. On the search card, select View search results.
3. By default, you see all the results that match your original search criteria. In the search query, notice the time columns referenced.
    - `TimeGenerated` is the date and time the data was ingested into the search table.
    - `_OriginalTimeGenerated` is the date and time the record was created.
4. To refine the list of results returned from the search table, edit the KQL query.
5. As you're reviewing your search job results, bookmark rows that contain information you find interesting so you can attach them to an incident or refer to them later.

---
## 🧱Restore historical data

When you **restore archived log data** in Microsoft Sentinel, the restored table will have a suffix: `_RST`

### ⛔ Limitations of log restore

- Minimum **2-day** restore window
- Data must be **older than 14 days**
- Max **60 TB** restored
- **1 active restore** per table
- Max **4 tables/week**, **2 concurrent restores/workspace**

### 🔁 Restore archived log data

To restore archived log data in Microsoft Sentinel, specify the table and time range for the data you want to restore. Within a few minutes, the log data is available within the Log Analytics workspace. Then you can use the data in high-performance queries that support full KQL.

You can restore archived data directly from the Search page or from a saved search.

1. In the Azure portal, go to Microsoft Sentinel and select the appropriate workspace.
2. Under General, select Search.
3. Restore log data in one of two ways:
    - At the top of Search page, select Restore, or
    - Select the Saved Searches tab and Restore on the appropriate search.
4. Select the table you want to restore.
5. Select the time range of the data that you want restore.
6. Select Restore.
7. Wait for the log data to be restored. View the status of your restoration job by selecting on the Restoration tab.

### 🔎📂 View restored log data

View the status and results of the log data restore by going to the Restoration tab. You can view the restored data when the status of the restore job shows Data Available.

1. In your Microsoft Sentinel workspace, select Search > Restoration.
2. When your restore job is complete, select the table name.
3. Review the results.

The Logs query pane shows the name of table containing the restored data. The Time range is set to a custom time range that uses the start and end times of the restored data.

### 🧹 Delete restored data tables

To save costs, we recommend you delete the restored table when you no longer need it. When you delete a restored table, Azure doesn't delete the underlying source data.

1. In your Microsoft Sentinel workspace, select Search > Restoration.
2. Identify the table you want to delete.
3. Select Delete for that table row.

