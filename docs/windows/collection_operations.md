
# Windows Collection Operations

## File Share Hunting

### Decrypt VBE scripts:

```https://blog.didierstevens.com/2016/03/29/decoding-vbe/```

### look for all items in a directory with the * file format

Here some interesting examples:

```powershell
 Get-ChildItem H:\DIR-TO-SCAN -Recurse -name *.vsdx
 Get-ChildItem H:\DIR-TO-SCAN -Recurse -name *.vsd
 Get-ChildItem H:\DIR-TO-SCAN -Recurse -name *.dmg
 Get-ChildItem H:\DIR-TO-SCAN -Recurse -name *.pptx
 Get-ChildItem H:\DIR-TO-SCAN -Recurse -name *.docx
 Get-ChildItem H:\DIR-TO-SCAN -Recurse -name *.vsd
```

### Look for all XLS files that are password protected (Binary Format)

```powershell
Get-ChildItem -path C:\ -recurse | foreach {gc -encoding byte -TotalCount 3000 -ReadCount 20 ./$_ |% {"{0:x2}" -f $_} | Select-String -Pattern "13 00 02 00" |% {$_ -match '13 00 02 00 (.{5})'}; $matches[1]}
```