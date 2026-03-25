# History

**COM** - Binary Standard.
Windows’ API always used both the language and structure of the C. The C was ideal due to its access to low-level system resources as well as its widespread adoption among software developers at the time. However, as Windows grew in size and complexity, it became difficult to manage everything in a C-style language. To offset the complications, Microsoft released the Component Object Model (COM).

**DCOM**
As Windows-based computers became networked, the COM was upgraded resulting in the *Distributed Component Object Model* (DCOM). This addressed new issues between COM objects including *memory issues, and formatting issues* when data is passed between objects running on two different networked machines. ***In simple terms, DCOM allows us to open a Word document on a network share as if it were stored on our machine. DCOM also enables us to do the same with executables stored on a file server.***

**ActiveX** 
This framework provided code wrappers for COM that would enable the execution of
code to run within the browser. Rather than requiring another application in Windows to view multimedia,ban ActiveX control could play a sound or view a short video. The ActiveX framework evolved into .NET as well as .NET Core, which aimed to address shortcomings of ActiveX while enhancing reliability and suitability for applications in an increasingly-connected enterprise.

**.NET and .NET Core**
Microsoft began a significant development platform modernization in the early 2000s with the .NET Framework. .NET introduced the new programming languages C# and Visual Basic.NET, which provide wrappers for the Windows API as well as COM objects within the operating system. The framework also incorporated secure coding mechanisms, and can differentiate between locally-developed code and Internet-downloaded code. It can also determine whether code execution requires elevated privileges and verify if these privileges have been granted to the application. These features are a part of the Common Language Runtime (CLR). The CLR is a virtual machine incorporated within Windows that executes code in a different way than
traditionally-compiled languages.

>**.NET**, which serves as a wrapper around COM, adding more security controls and platform compatibility.

---
### 🔐 COM in Windows Internals

- COM objects are **registered in the Windows Registry**.
    
- Registry keys like:
    
    - `HKEY_CLASSES_ROOT\CLSID\{GUID}`
        
    - Define the **location and permissions** of COM components.
        
- Windows relies heavily on COM for:
    
    - Shell (Explorer)
        
    - Office automation
        
    - WMI (Windows Management Instrumentation)
        
    - Scripting and automation (e.g., via `Excel.Application` in PowerShell)

#### 💥 Attack Surface

Attackers abuse COM to:

- **Execute malicious payloads via trusted interfaces** (LOLBin-style).
    
- **Bypass application whitelisting** (e.g., via Excel COM objects).
    
- **Achieve persistence** by registering malicious COM components in the registry.
    

#### 📌 Example Attack:

```powershell
$excel = New-Object -ComObject Excel.Application
$excel.Visible = $false
$workbook = $excel.Workbooks.Open("C:\malicious.xls")
```

- This PowerShell snippet uses COM to automate Excel.
    
- If the file contains **malicious macros**, it may execute silently.

### 📁 Detection Implications

- Monitor use of `New-Object -ComObject` in scripts.
    
- Look for **suspicious COM CLSIDs** in the Registry.
    
- Use **Sysmon Event ID 1 or 7** to detect unusual COM DLL loading.