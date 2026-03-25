An incident is a collection of correlated alerts that make up the story of an attack. Microsoft Defender XDR automatically aggregates malicious and suspicious events that are found in different device, user, and mailbox entities in the network. Grouping related alerts into an incident gives security defenders a comprehensive view of an attack.

- where the attack started
- what tactics were used
- how far the attack has gone into the network
- how many devices, users, and mailboxes were impacted
- how severe the impact was, and other details about affected entities.

By default, the queue in the Microsoft Defender portal displays incidents seen in the last 30 days. The most recent incident is at the top of the list so that you can see it first.

## 🔍 Incident Filters

| **Filter**                        | **Description**                                                                                        |
| --------------------------------- | ------------------------------------------------------------------------------------------------------ |
| **Status**                        | Filter incidents by their current state: Active or Resolved.                                           |
| **Severity**                      | Filter by impact level: higher severity indicates greater risk and urgency.                            |
| **Incident Assignment**           | Show incidents assigned to you or handled by automation.                                               |
| **Multiple Service Source**       | Filter incidents based on whether they involve multiple Defender sources.                              |
| **Service Sources**               | Filter by originating services: Defender for Endpoint, Identity, Office 365, Cloud Apps.               |
| **Tags**                          | Filter incidents using assigned custom tags.                                                           |
| **Multiple Category**             | View incidents mapped to multiple attack categories.                                                   |
| **Categories**                    | Focus on incidents tied to specific tactics or techniques (e.g., credential access, lateral movement). |
| **Entities**                      | Filter by specific entity names or IDs (e.g., devices, users).                                         |
| **Data Sensitivity**              | Show only incidents involving sensitive data (requires Microsoft Purview Information Protection).      |
| **Device Group**                  | Filter incidents based on assigned device groups.                                                      |
| **OS Platform**                   | Filter by the operating system (e.g., Windows, Linux, macOS).                                          |
| **Classification**                | Filter by alert classification: True positive, False positive, Not set.                                |
| **Automated Investigation State** | Filter by the status of the automated investigation process.                                           |
| **Associated Threat**             | Filter based on known threat names or indicators tied to the incident.                                 |

## Preview incidents

The portal pages provide preview information for most list-related data.

In this screenshot, the three highlighted areas are the circle, the greater than symbol, and the actual link.

[![Screen shot of Incident Preview information options.](https://learn.microsoft.com/en-us/training/wwl-sci/mitigate-incidents-microsoft-365-defender/media/preview-options-from-list.png)](https://learn.microsoft.com/en-us/training/wwl-sci/mitigate-incidents-microsoft-365-defender/media/preview-options-from-list.png#lightbox)

**Circle**

Selecting the circle will open a details window on the right side of the page with a preview of the line item with an option to open the full page of information.

**Greater than symbol**

If there are related records that can be displayed, selecting the greater than sign will display the records below the current record.

**Link**

The link will navigate you to the full page for the line item.

## 🛠️ Incident Management

| **Feature**            | **Description**                                                                                                    |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------ |
| **Edit Incident Name** | Modify the auto-generated name to match custom naming conventions or investigation clarity.                        |
| **Assign Incidents**   | Take ownership of an incident and its associated alerts by using **Assign to me**.                                 |
| **Set Status**         | Mark incidents as **Active** (in progress) or **Resolved** (remediated and closed).                                |
| **Set Classification** | Tag an incident as a **True Alert**, **False Alert**, or leave as **Not set** to aid threat intelligence learning. |
| **Move Alerts**        | Use the **Alerts tab** to move specific alerts to another incident, merging or splitting as needed.                |
| **Add Comments**       | Document investigation notes; changes and history are logged and viewable chronologically.                         |
| **Add Tags**           | Apply custom tags to group similar incidents for easier filtering                                                  |

---
# Action Center

>Payload reputation/detonation and grader analysis are not done in all tenants. Information is blocked from going outside the organization when data is not supposed to leave the tenant boundary for compliance purposes.

## Report suspicious content to Microsoft

- You need to have one of following roles:
    - Security Administrator or Security Reader in the Microsoft Defender portal.
- Admins can submit messages as old as 30 days if it's still available in the mailbox and not purged by the user or another admin.
- Admin submissions are throttled at the following rates:
    - Maximum submissions in any 15-minutes period: 150 submissions
    - Same submissions in a 24 hour period: Three submissions
    - Same submissions in a 15-minute period: One submission

>File and URL submissions are not available in the clouds that do not allow for data to leave the environment. The ability to select File or URL will be greyed out.

>If organizations are configured to send user reported messages to the custom mailbox only, reported messages will appear in User reported messages but their results will always be empty (as they would not have been rescanned).

### Undo user submissions

Once a user submits a suspicious email to the custom mailbox, the user and admin don't have an option to undo the submission. If the user would like to recover the email, it will be available for **recovery in the Deleted Items or Junk Email folders**.

### Converting user reported messages from the custom mailbox into an admin submission

If you've configured the custom mailbox to intercept user-reported messages without sending the messages to Microsoft, you can find and send specific messages to Microsoft for analysis.

On the User reported messages tab, select a message in the list, select Submit to Microsoft for analysis, and then select one of the following values from the dropdown list:

- Report clean
- Report phishing
- Report malware
- Report spam
- Trigger investigation

---
# Entra sign-in logs

To perform a sign-in investigation including **conditional access policies** evaluated, you can query the following tables with KQL:

| Location                              | Table                 |
| ------------------------------------- | --------------------- |
| Microsoft Defender XDR Threat Hunting | `AADSignInEventsBeta` |
| Microsoft Entra ID Log Analytics      | `SigninLogs`          |

The Microsoft Entra monitoring `Sign-in Logs` provide access to the same information available in the `SigninLogs` table. 
- To access the Sign-in Logs blade, select Microsoft Entra ID in the Azure portal, then Sign-in Logs in the Monitoring Group. 
- The query output will provide default columns including the Date, User, Application, Status, and Conditional Access (policy applied).

---

