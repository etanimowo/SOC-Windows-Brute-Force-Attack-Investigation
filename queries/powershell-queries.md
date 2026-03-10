# PowerShell Queries

## Pull recent failed logons

```powershell
Get-WinEvent -FilterHashtable @{LogName='Security'; Id=4625} -MaxEvents 10 |
Select-Object TimeCreated, Id, Message
```

## Pull recent successful logons

```powershell
Get-WinEvent -FilterHashtable @{LogName='Security'; Id=4624} -MaxEvents 10 |
Select-Object TimeCreated, Id, Message
```

## Export failed logons to a text file

```powershell
Get-WinEvent -FilterHashtable @{LogName='Security'; Id=4625} -MaxEvents 20 |
Format-List TimeCreated, Id, Message |
Out-File C:\Users\Public\failed_logons.txt
```
