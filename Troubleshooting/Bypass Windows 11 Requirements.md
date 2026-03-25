### Windows Installation
__Bypass windows 11 PC Requirements__
- shift + f10 -> CMD
- regedit -> Local Machine -> System -> Setup 
- Create new key by right clicking: LabConfig
- In lab config right click to create new DWORDs in it: `BypassTPMCheck, BypassSecureBoot, BypassRAMCheck`
- Set all values to `1`.

