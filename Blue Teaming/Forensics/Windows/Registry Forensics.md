### Structure of the Registry
1. HKEY_CURRENT_USER
2. HKEY_USERS
3. HKEY_LOCAL_MACHINE
4. HKEY_CLASSES_ROOT
5. HKEY_CURRENT_CONFIG

### Windows Registry Predefined Keys

| Folder / Predefined Key       | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| ----------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **HKEY_CURRENT_USER** (HKCU)  | Contains the root of the configuration information for the user who is currently logged on. The user's folders, screen colors, and Control Panel settings are stored here. This information is associated with the user's profile. This key is sometimes abbreviated as HKCU.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| **HKEY_USERS** (HKU)          | Contains all the actively loaded user profiles on the computer. HKEY_CURRENT_USER is a subkey of **HKEY_USERS.** HKEY_USERS is sometimes abbreviated as HKU.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| **HKEY_LOCAL_MACHINE** (HKLM) | Contains configuration information particular to the computer (for any user). This key is sometimes abbreviated as HKLM.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| **HKEY_CLASSES_ROOT** (HKCR)  | Is a subkey of `HKEY_LOCAL_MACHINE\Software` . The information that is stored here makes sure that the correct program opens when you open a file by using Windows Explorer. This key is sometimes abbreviated as HKCR.<br><br>Starting with Windows 2000, this information is stored under both the **HKEY_LOCAL_MACHINE** and **HKEY_CURRENT_USER** keys. The `HKEY_LOCAL_MACHINE\Software\Classes` key contains default settings that can apply to all users on the local computer.  The `HKEY_CURRENT_USER\Software\Classes` key has settings that override the default settings and apply only to the interactive user.<br><br>The **HKEY_CLASSES_ROOT** key provides a view of the registry that merges the information from these two sources. **HKEY_CLASSES_ROOT** also provides this merged view for programs that are designed for earlier versions of Windows. To change the settings for the interactive user, changes must be made under `HKEY_CURRENT_USER\Software\Classes` instead of under **HKEY_CLASSES_ROOT**.<br><br>To change the default settings, changes must be made under `HKEY_LOCAL_MACHINE\Software\Classes`  .If you write keys to a key under **HKEY_CLASSES_ROOT**, the system stores the information under `HKEY_LOCAL_MACHINE\Software\Classes` .<br><br>If you write values to a key under **HKEY_CLASSES_ROOT**, and the key already exists under `HKEY_CURRENT_USER\Software\Classes` , the system will store the information there instead of under `HKEY_LOCAL_MACHINE\Software\Classes` <br> |
| **HKEY_CURRENT_CONFIG**       | Contains information about the hardware profile that is used by the local computer at system startup.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |

### Registry Hives
> ##### The majority of these hives are located in the `C:\Windows\System32\Config`:

1. **DEFAULT** (mounted on `HKEY_USERS\DEFAULT`)
2. **SAM** (mounted on `HKEY_LOCAL_MACHINE\SAM`)
3. **SECURITY** (mounted on `HKEY_LOCAL_MACHINE\Security`)
4. **SOFTWARE** (mounted on `HKEY_LOCAL_MACHINE\Software`)
5. **SYSTEM** (mounted on `HKEY_LOCAL_MACHINE\System`)

### Hives containing user information
>##### For Windows 7 and above, a user’s profile directory is located in `C:\Users\<username>\`

1. **NTUSER.DAT** (mounted on HKEY_CURRENT_USER when a user logs in)
	-  `C:\Users\<username>\`.
2. **USRCLASS.DAT** (mounted on HKEY_CURRENT_USER\Software\CLASSES)
	- `C:\Users\<username>\AppData\Local\Microsoft\Windows`.

> #### The Amcache Hive: Information on programs that were recently run on the system.
> - `C:\Windows\AppCompat\Programs\Amcache.hve`

### Transaction Logs
The transaction logs can be considered as the journal of the changelog of the registry hive.
- the transaction log for the SAM hive will be located in `C:\Windows\System32\Config\SAM.LOG`
- Sometimes there can be multiple transaction logs as well. 


### Registry backups
Hives are copied to the `C:\Windows\System32\Config\RegBack` directory ***every ten days***. It might be an excellent place to look if you suspect that some registry keys might have been deleted/modified recently.


---
## Analyzing Registry Data
### SYSTEM INFO

| Info                               | Directory                                                                                                                                                                                                                                                                                           |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| OS Version                         | `SOFTWARE\Microsoft\Windows NT\CurrentVersion`                                                                                                                                                                                                                                                      |
| Current control set                | `SYSTEM\ControlSet001`, `SYSTEM\ControlSet002`, `HKLM\SYSTEM\CurrentControlSet`                                                                                                                                                                                                                     |
| Computer Name                      | `SYSTEM\CurrentControlSet\Control\ComputerName\ComputerName`                                                                                                                                                                                                                                        |
| Time Zone                          | `SYSTEM\CurrentControlSet\Control\TimeZoneInformation`                                                                                                                                                                                                                                              |
| Network Interfaces & Past Networks | `SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\Interfaces`                                                                                                                                                                                                                                     |
| Autostart Programs                 | `NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\RunOnce`,<br>`NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Run,`SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce`, `SOFTWARE\Microsoft\Windows\CurrentVersion\policies\Explorer\Run`,`SOFTWARE\Microsoft\Windows\CurrentVersion\Run` |
| Services                           | `SYSTEM\CurrentControlSet\Services`                                                                                                                                                                                                                                                                 |
| SAM hive and user information      | `SAM\Domains\Account\Users`                                                                                                                                                                                                                                                                         |


> ##### In most cases (_but not always_), **ControlSet001** will point to the Control Set that the machine booted with, & **ControlSet002** will be the `last known good`  configuration.
> - Windows creates a ***volatile Control Set*** when the machine is live, called the CurrentControlSet ( `HKLM\SYSTEM\CurrentControlSet` ).
>- We can find out which Control Set is being used as the CurrentControlSet by looking at the following registry value:
`SYSTEM\Select\Current`
>
>##### If the `start` key is set to 0x02, this means that this service will start at boot.

###  Recent Files

Windows maintains a list of recently opened files for each user. This information is stored in the ***NTUSER*** hive. `NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs`

> ##### if we are looking specifically for the last used PDF files, we can look at the following registry key:
>`NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs\.pdf`

### Office Recent Files
Microsoft Office also maintains a list of recently opened documents. This list is also located in the NTUSER hive. 
`NTUSER.DAT\Software\Microsoft\Office\VERSION`

The version number for each Microsoft Office release is different. An example registry key will look like this:
`NTUSER.DAT\Software\Microsoft\Office\15.0\Word`

Here, the 15.0 refers to Office 2013. A list of different Office releases and their version numbers can be found on [this link](https://docs.microsoft.com/en-us/deployoffice/install-different-office-visio-and-project-versions-on-the-same-computer#office-releases-and-their-version-number) .

Starting from Office 365, Microsoft now ties the location to the user's [live ID](https://www.microsoft.com/security/blog/2008/05/07/what-is-a-windows-live-id/) . In such a scenario, the recent files can be found at the following location. `NTUSER.DAT\Software\Microsoft\Office\VERSION\UserMRU\LiveID_####\FileMRU`
 This location also saves the complete path of the most recently used files.

### ShellBags
When any user opens a folder, it opens in a specific layout. Users can change this layout according to their preferences. These layouts can be different for different folders. This information about the Windows _'shell'_  is stored and can identify the Most Recently Used files and folders. Since this setting is different for each user, it is located in the user hives. We can find this information on the following locations:

`USRCLASS.DAT\Local Settings\Software\Microsoft\Windows\Shell\Bags`

`USRCLASS.DAT\Local Settings\Software\Microsoft\Windows\Shell\BagMRU`

`NTUSER.DAT\Software\Microsoft\Windows\Shell\BagMRU`

`NTUSER.DAT\Software\Microsoft\Windows\Shell\Bags`

### Open/Save and LastVisited Dialog MRUs
When we open or save a file, a dialog box appears asking us where to save or open that file from. It might be noticed that once we open/save a file at a specific location, Windows remembers that location. This implies that we can find out recently used files if we get our hands on this information. We can do so by examining the following registry keys

`NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32\OpenSavePIDlMRU`

`NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32\LastVisitedPidlMRU`

### Windows Explorer Address/Search Bars

Another way to identify a user's recent activity is by looking at the ***paths typed in the Windows Explorer address bar*** or searches performed using the following registry keys, respectively.

`NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\TypedPaths`

`NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\WordWheelQuery`

---
## Evidence of Execution

### UserAssist

Windows keeps track of applications launched by the user using Windows Explorer for statistical purposes in the User Assist registry keys. These keys contain information about the programs launched, the **time of their launch, and the number of times they were executed**. ***However, programs that were run using the command line can't be found in the User Assist keys.*** The User Assist key is present in the NTUSER hive, mapped to each user's GUID.  `NTUSER.DAT\Software\Microsoft\Windows\Currentversion\Explorer\UserAssist\{GUID}\Count`

### ShimCache/AppCompatCache

ShimCache is a *mechanism* used to keep track of application compatibility with the OS and tracks all applications launched on the machine. Its main purpose is to ensure backward compatibility of applications.
`SYSTEM\CurrentControlSet\Control\Session Manager\AppCompatCache`

ShimCache stores file name, file size, and last modified time of the executables.

`AppCompatCacheParser.exe --csv <path to save output> -f <path to SYSTEM hive for data parsing> -c <control set to parse>`

### AmCache

The AmCache hive is an artifact related to ShimCache. This performs a similar function to ShimCache, and stores additional data related to program executions. This data includes ***execution path, installation, execution and deletion times, and SHA1 hashes of the executed programs***. 
`C:\Windows\appcompat\Programs\Amcache.hve`

Information about the last executed programs can be found at the following location in the hive: `Amcache.hve\Root\File\{Volume GUID}\`

### BAM/DAM

Background Activity Monitor or BAM keeps a tab on the activity of background applications. Similar Desktop Activity Moderator or DAM is a part of Microsoft Windows that optimizes the power consumption of the device. Both of these are a part of the Modern Standby system in Microsoft Windows.This location contains information about ***last run programs, their full paths, and last execution time.***

`SYSTEM\CurrentControlSet\Services\bam\UserSettings\{SID}`

`SYSTEM\CurrentControlSet\Services\dam\UserSettings\{SID}`

---
## External Devices/USB Device

### Device identification
The following locations keep track of USB keys plugged into a system. These locations store the ***vendor id, product id, and version of the USB device*** plugged in and can be used to identify unique devices. These locations also store the ***time the devices were plugged*** into the system.

`SYSTEM\CurrentControlSet\Enum\USBSTOR`

`SYSTEM\CurrentControlSet\Enum\USB`

### First/Last Times

The following registry key tracks the ***first time the device was connected***, the ***last time it was connected*** and the ***last time the device was removed*** from the system.

`SYSTEM\CurrentControlSet\Enum\USBSTOR\Ven_Prod_Version\USBSerial#\Properties\{83da6326-97a6-4088-9453-a19231573b29}\####`

In the above key, the `####` sign can be replaced by the following digits to get the required information:

| **Value** | **Information**       |
| --------- | --------------------- |
| 0064      | First Connection time |
| 0066      | Last Connection time  |
| 0067      | Last removal time     |

Although we can check this value manually, as we have seen above, Registry Explorer already parses this data and shows us if we select the USBSTOR key.

### USB device Volume Name

The ***device name of the connected drive*** can be found at the following location:

`SOFTWARE\Microsoft\Windows Portable Devices\Devices`

We can compare the GUID we see here in this registry key and compare it with the Disk ID we see on keys mentioned in device identification to correlate the names with unique devices.

Combining all of this information, we can create a fair picture of any USB devices that were connected to the machine we're investigating.

---


