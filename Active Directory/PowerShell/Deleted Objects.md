```powershell

Get-ADObject -Filter 'objectClass -eq "user" -and isDeleted -eq $true' -IncludeDeletedObjects

# List all deleted objects
Get-ADObject -Filter * -IncludeDeletedObjects

# List only deleted users
Get-ADObject -Filter 'objectClass -eq "user"' -IncludeDeletedObjects

# List only deleted computers
Get-ADObject -Filter 'objectClass -eq "computer"' -IncludeDeletedObjects

# Show some useful attributes
Get-ADObject -Filter 'objectClass -eq "user"' -IncludeDeletedObjects | 
    Select-Object Name, SamAccountName, DistinguishedName, LastKnownParent
```