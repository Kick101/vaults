Kroll Artifact Parser and Extractor (KAPE) parses and extracts Windows forensics artifacts. It is a tool that can ***significantly reduce the time needed to respond to an incident by providing forensic artifacts from a live system or a storage device much earlier than the imaging process completes***.

| **Purpose**                | **Component** | **Role**                                  | **Description**                                                                                                                     |
| -------------------------- | ------------- | ----------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| 1. Collect files           | ***Targets*** | Define what to collect                    | Forensic artifacts (e.g., event logs, registry hives) identified for collection.                                                    |
|                            |               | Two-Pass Copying Mechanism                | - **First Pass:** Copies files accessible by the OS. <br>-**Second Pass:** Uses raw disk reads to bypass OS locks for locked files. |
|                            |               | File Integrity                            | Files are saved with original timestamps and metadata, in a directory structure that mirrors the source.                            |
| 2. Process collected files | ***Modules*** | Define how to process collected artifacts | Programs/scripts (e.g., log parsers, registry viewers) that extract useful information from the artifacts collected.                |


>##### 🗂️ What is a **Prefetch** File?
>**Prefetch files** are system-generated files created by **Windows OS** to **speed up application startup**. When you run a program, Windows creates a `.pf` (Prefetch) file that logs information about the executable and the files it accessed during startup.

![[Pasted image 20250517114804.png]]

##### Prefetch files Processing

| **Stage**  | **Tool**                | **Action**                                    | **Output**            |
| ---------- | ----------------------- | --------------------------------------------- | --------------------- |
| Collection | Target (e.g., Prefetch) | Collects `.pf` files and copies them          | Raw Prefetch files    |
| Processing | Module (e.g., PECmd)    | Parses the Prefetch files for data extraction | Structured CSV report |

>KAPE does not need to be installed. It is portable and can be used from network locations or USB drives.

---
### Targets

__Example .tkape contents__
This TKAPE file tells KAPE to collect files with the file mask `*.pf` from the path `C:\Windows\prefetch` and `C:\Windows.old\prefetch`.

![[Pasted image 20250517121112.png]]

### Compound Targets

`Compound Targets` help us collect multiple targets by giving a single command. Examples of `Compound Targets` include `!BasicCollection`, `!SANS_triage` and `KAPEtriage`. We can view the `Compound Targets` on the path `KAPE\Targets\Compound`.

The *below* `Compound Target` will collect evidence of execution from *Prefetch, RecentFileCache, AmCache, and Syscache* `Targets`.

![[Pasted image 20250517121335.png]]

#### **!Disabled**

This directory contains `Targets` that you want to keep in the KAPE instance, but you don't want them to appear in the active Targets list.

#### **!Local**

If you have created some `Targets` that you don't want to sync with the KAPE Github repository, you can place them in this directory. These can be `Targets` that are specific to your environment. Similarly, anything not present in the Github repository when we update KAPE will be moved to the `!Local` directory.

---
### Modules
Runs some command and store the output. Generally, the output is in the form of CSV or TXT files.

The following image shows the Windows_IPConfig MKAPE file.

![[Pasted image 20250517122129.png]]

---
### gKAPE

![[Pasted image 20250517122602.png]]


>`Flush` checkbox will delete all the contents of the Target destination, so we have to be careful when using that.

>`Add %d` will **append date info to the directory name**. Similarly, `Add %m` will **append machine info** to the Target destination directory. We can select our desired Target from the list shown above. The Search bar helps us search for the names of the desired Targets quickly.

---
### KAPE CLI

__Target__
KAPE will create a new directory if it doesn't already exist
```shell
kape.exe --tsource C: --target KapeTriage --tdest C:\Users\thm-4n6\Desktop\target
```

__Module__

```shell
kape.exe --tsource C: --target KapeTriage --tdest C:\Users\thm-4n6\Desktop\Target --mdest C:\Users\thm-4n6\Desktop\module --module !EZParser
```

>Run this command in an elevated shell (with Administrator privileges) for KAPE to collect the data.

#### Batch Mode

We can provide a list of commands for KAPE to run in a file named `_kape.cli`. Then we keep this file in the directory containing the KAPE binary. When `kape.exe` is executed as an administrator, it checks if there is `_kape.cli` file present in the directory. If so, it executes the commands mentioned in the cli file. This mode can be used if you need someone to run KAPE for you, you will keep all the commands in a single line, and all you need is for the person to right-click and run `kape.exe` as administrator.



