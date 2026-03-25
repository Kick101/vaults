The Azure portal and all Microsoft Sentinel tools use a standard API to access this data store. The same API is also available for external tools such as Python and PowerShell. There are two libraries that you can use to simplify API access:

- Kqlmagic
- msticpy

### Kqlmagic

The Kqlmagic library provides an easy to implement API wrapper to run KQL queries.

### msticpy

**Microsoft Threat Intelligence Python Security Tools** is a set of Python tools intended to be used for **security investigations and hunting**. Many of the tools originated as code *Jupyter notebooks* written to solve a problem as part of a security investigation. Some of the tools are only useful in notebooks (for example, much of the nbtools subpackage), but many others can be used from the Python command line or imported into your code.

The package addresses three central needs for security investigators and hunters:

- Acquiring and enriching data
- Analyzing data
- Visualizing data

msticpy can query using KQL; the library also **provides predefined queries** for Microsoft Sentinel, Microsoft Defender XDR for Endpoint, and the Microsoft Security Graph. An example of a function is the `list_logons_by_account`, which retrieves the logon events for an account. For details about msticpy visit: [https://msticpy.readthedocs.io/](https://msticpy.readthedocs.io/)

---

>A Jupyter Notebook allows you to create and share documents that contain live code, equations, visualizations, and explanatory text. Uses include data cleaning and transformation, numerical simulation, statistical modeling, machine learning, and much more.

Notebooks have two components:

- The browser-based interface where you enter and run queries and code and where the execution results are displayed.
- The kernel is responsible for parsing and executing the code itself.

The Microsoft Sentinel notebook's kernel runs on an Azure virtual machine (VM). Several licensing options exist to use more powerful virtual machines if your notebooks include complex machine learning models.

The Microsoft Sentinel notebooks use many popular Python libraries such as pandas, matplotlib, bokeh, etc. There are a great many other Python packages for you to choose from, covering areas such as:

- Visualizations and graphics
- Data processing and analysis
- Statistics and numerical computing
- Machine learning and deep learning

The `msticpy` package is used in many of the included notebooks. 

>**Msticpy tools are explicitly designed to help with creating notebooks for hunting and investigation.**

---
# Create a notebook

To get started with Notebooks, use the _Getting Started Guide For Microsoft Sentinel ML Notebooks_ notebook.

1. In the Microsoft Sentinel navigation menu, expand the _Threat Management_ section, and select **Notebooks**
2. You need to create an Azure Machine Learning (ML) Workspace. From the menu, select **Configure Azure Machine Learning**, then **Create new Azure ML workspace**.
3. In the Subscription box, select your subscription.
4. Select **Create a new Resource group** and choose a name for your new resource group.
5. In the Workspace details section:
    
    - Give your workspace a unique name.

    - Choose your Region        
    - Keep the default Storage account, Key vault, and Application insights information.
    - The Container registry option can remain as None.

6. At the bottom of the page, select **Review + create**. Then on the next page, select **create**. It takes a moment to deploy the workspace.
    
> #### ⚠️ Note
> It takes a few minutes to deploy the Machine Learning workspace.
    
7. After _Your deployment is complete_ message appears, return to Microsoft Sentinel.
    
8. Navigate to the Threat Management section, and select **Notebooks**.
    
9. Select the **Templates** tab.
    
10. Select the **A Getting Started Guide For Microsoft Sentinel ML Notebooks** from the list.
    
11. Select **Create from template** button on the bottom of the detail pane.
    
12. Review the default options and then select **Save**.
    
13. Select the **Launch notebook** button.
    
14. Select **Close** if an informational window appears in the Microsoft Azure Machine Learning studio.
    
15. 1. In the command bar, to the right of the **Compute:** selector, select the **+** symbol to _Create Azure ML compute_ instance. **Hint:** It might be hidden inside the ellipsis icon **(...)**.
    
> #### ⚠️ Note
> You can have more screen space by hiding the Azure ML Studio left menu selections. Select the _Hamburger menu_ (3 horizontal lines on the top left), and by collapsing the Notebooks Files by selecting the **<<** icon.
    
16. Type a unique name in the _Compute name_ field. This identifies your compute instance.
    
17. Scroll down and select the first option available.
    
>#### 💡 Tip
> Workload type: Development on Notebooks (or other IDE) and lightweight testing.
    
18. Select the **Review + Create** button at the bottom of the screen, then scroll down and select **Create**. Close any feedback window that appears. This takes a few minutes. You see a notification (bell icon) when it completes and the _Compute instance_ left icon turns from blue to green.
19. Once the Compute is created and running, verify that the kernel to use is `Python 3.10` - `Pytorch` and `Tensorflow`.
    
> #### 💡 Tip
> This is shown in the right of the menu bar. If that kernel isn't selected, select the _Python 3.10 - Pytorch and Tensorflow_ option from the drop-down list. You can select the **Refresh** icon on the far right to see the kernel options.
    
20. Select the **Authenticate** button and wait for the authentication to complete.
21. Clear all the results from the notebook by selecting the **Clear all outputs** (Eraser icon) from the menu bar and follow the _Getting Started_ tutorial.
    
> #### 💡 Tip
> This can be found by selecting the ellipsis (...) from the menu bar.
    
22. Review section _1 Introduction_ in the notebook and proceed to section _2 Initializing the notebook and MSTICPy_.
    
> #### 💡 Tip
> Section 1.2 _Running code in notebooks_ lets you practice running small lines of Python code.
    
23. In section _2 Initializing the notebook and MSTICPy_, review the content on Initializing the notebook and installing the MSTICPy package.
24. Run the _Python code_ to initialize the cell by selecting the **Run cell** button (Play icon) to the left of the code.
25. It should take >30 seconds to run. Once it completes, review the output messages and _disregard any warnings about the Python kernel version_ or other error messages.
26. The code ran successfully if _msticpyconfig.yaml_ was created in the _utils_ folder in the _file explorer_ pane on the left. It can take another 30 seconds for the file to appear. If it doesn't appear, select the **Refresh** icon in the _file explorer_ pane.
    
> #### 💡 Tip
> You can clear the output messages by selecting the ellipsis (...) on the left of the code window for the _Output menu_ and selecting the _Clear output_ (square with an x*) icon.
    
27. Select the **msticpyconfig.yaml** file in the _file explorer_ pane on the left to review the contents of the file and then close it.
28. Proceed to section _3 Querying data with MSTICPy_ and review the contents. Don't run the _Multiple Microsoft Sentinel workspaces_ code cell as it fails, but the other code cells can be run successfully.

---
# Explore notebook code

The following code blocks provide a representative example of using notebooks to work with Microsoft Sentinel data.

**Code Block**

- Create a new variable `test_query` that contains the KQL query.
- Next, you run the query `qry_prov.exec_query()`. This utilizes the `msticpy` library to execute the KQL query in the Microsoft Sentinel Log Analytics related workspace. The results are stored in the `test_df` variable.
- Next, display the first five rows with the `xxx_xxxx.head()` function.

![Screenshot of a Microsoft Sentinel Notebook Sample 1 Query.](https://learn.microsoft.com/en-us/training/wwl-sci/perform-threat-hunting-sentinel-with-notebooks/media/threat-hunt-1.png)

**Code Block**

- You create a new function called lookup_res that takes a variable row.
- Next, you save the IP address stored in row to the variable [ip].
- The next line of code uses the `msticpy` function [ti.lookup_ioc()] to query the **ThreatIntelligenceIndicator** table for a row that is sourced from **VirusTotal** with a matching ip-address.
- Next, the `msticpy` function [ti.result_to_df()] will return a `DataFrame` representation of response.
- The new function returns the Severity of the IP address.
    

![Screenshot of a Microsoft Sentinel Notebook Sample 2 Query.](https://learn.microsoft.com/en-us/training/wwl-sci/perform-threat-hunting-sentinel-with-notebooks/media/threat-hunt-3.png)

**Code Block**

- Create a new variable [vis_q] that contains the KQL query.
- Next, you run the query [qry_prov.exec_query()]. This utilizes the `msticpy` library to execute the KQL query in the Microsoft Sentinel Log Analytics related workspace. The results are stored in the [vis_data] variable.
- Then, [qry_prov.exec_query()] returns a `pandas` DataFrame that provides visualization features. You then plot a bar graph with the unique IP addresses and how many times they were used in the first five entries of the Dataframe.
    

![Screenshot of a Microsoft Sentinel Notebook Sample 3 Query.](https://learn.microsoft.com/en-us/training/wwl-sci/perform-threat-hunting-sentinel-with-notebooks/media/threat-hunt-2.png)

---
