### Volatility Plugins

| Pulgin               | Description                                                                         |
| -------------------- | ----------------------------------------------------------------------------------- |
| Windows.cmdline      | Lists process command line arguments                                                |
| windows.drivermodule | Determines if any loaded drivers were hidden by a rootkit                           |
| Windows.filescan     | Scans for file objects present in a particular Windows memory image                 |
| Windows.getsids      | Print the SIDs owning each process                                                  |
| Windows.handles      | Lists process open handles                                                          |
| Windows.info         | Show OS & kernel details of the memory sample being analyzed                        |
| Windows.netscan      | Scans for network objects present in a particular Windows memory image              |
| Widnows.netstat      | Traverses network tracking structures present in a particular Windows memory image. |
| Windows.mftscan      | Scans for Alternate Data Stream                                                     |
| Windows.pslist       | Lists the processes present in a particular Windows memory image                    |
| Windows.pstree       | List processes in a tree based on their parent process ID                           |

---
#### Files accessed that are stored in the memory dump
`vol -f memdump.mem windows.filescan > filescan_out`

#### Detailed info: when the file was accessed or modified
>`mftscan` is used to **analyze the Master File Table (MFT)** of **NTFS file systems** . It helps recover deleted files or inspect file metadata directly from raw disk or image files.

`vol -f memdump.mem windows.mftscan.MFTScan > mftscan_out`

#### Dump the memory region of a process
`vol -f memdump.mem -o . windows.memmap --dump --pid 1612`

`strings pid.1612.dmp |less`


---
### Common Windows Process
![[Pasted image 20250514124732.png]]
---