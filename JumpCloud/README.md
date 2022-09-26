# Managing devices with JumpCloud
You can do little with the GUI. API - this is were the fun part starts

## Getting API access to JumpCloud
1. go to admin portal. Click your portret top right -> Get API Token
2. install powershell for macOS or use native one
3. run powershell (for macOS -> Terminal, "pwsh", Enter)
4. Install JC module ```Install-Module -Name JumpCloud```
5. Run it ```Import-Module -Name JumpCloud``` then ```Connect-JCOnline```
## Sort Apple silicon based systems to separate group in JC



get token: click top right corner of admin portal -> API key

```bash
pwsh
Get-JCSystem | Where-Object {$_.os -like "Mac OS X" -and $_.arch -eq "arm64"} | Add-JCSystemGroupMember -GroupName "Mac - Apple silicon"
Get-JCSystem | Where-Object {$_.os -like "Mac OS X" -and $_.arch -ne "arm64"} | Add-JCSystemGroupMember -GroupName "Mac - Intel systems"
Get-JCSystem | Where-Object {$_.os -like "Windows"} | Add-JCSystemGroupMember -GroupName "Windows"
```
```powershell
# create folders
New-JCSystemGroup -GroupName "Windows"
New-JCSystemGroup -GroupName "Windows Home"
New-JCSystemGroup -GroupName "macOS 13"
New-JCSystemGroup -GroupName "macOS 12"
New-JCSystemGroup -GroupName "macOS 11"
New-JCSystemGroup -GroupName "macOS legacy"

# sort systems
Get-JCSystem | Where-Object {$_.os -like "Mac OS X" -and $_.version -lt 11} | Add-JCSystemGroupMember -GroupName "macOS legacy"
Get-JCSystem | Where-Object {$_.os -like "Mac OS X" -and $_.version -ge 11 -and $_.version -lt 12} | Add-JCSystemGroupMember -GroupName "macOS 11"
Get-JCSystem | Where-Object {$_.os -like "Mac OS X" -and $_.version -ge 12 -and $_.version -lt 13} | Add-JCSystemGroupMember -GroupName "macOS 12"
Get-JCSystem | Where-Object {$_.os -like "Mac OS X" -and $_.version -ge 13 -and $_.version -lt 14} | Add-JCSystemGroupMember -GroupName "macOS 13"
Get-JCSystem | Where-Object {$_.os -like "Windows"} | Add-JCSystemGroupMember -GroupName "Windows"
Get-JCSystem | Where-Object {$_.version -like "*Home"} |  Add-JCSystemGroupMember -GroupName "Windows Home"
```


Save this as .ps1 file and run with scheduler

```powershell
# Edit the variables below, you shouldn't need to edit any other lines.
$jcApiKey = '<YOUR_TOKEN_HERE>'
$groupNames = @{
    IntelMacGroupName = 'Mac - Intel systems'
    AppleMacGroupName = 'Mac - Apple silicon'
    WindowsGroupName = 'Windows'
}

# Check if JumpCloud module is installed, if not, install it and connect.
try {
    if (-not (Get-Module -ListAvailable JumpCloud)) {
        Install-Module -Name JumpCloud -Scope CurrentUser -Force
    }
    Import-Module -Name JumpCloud
    Connect-JCOnline -JumpCloudApiKey $jcApiKey
} catch {
    Write-Error "Something went wrong connecting to JumpCloud! $($_.Exception.Message)" -ErrorAction Stop
}

# Create the groups if they don't exist.
try {
    Write-Host "`nCreating JumpCloud System Groups if needed..." -ForegroundColor Green
    foreach ($group in $groupNames.GetEnumerator()) {
        if (-not (Get-JCGroup -Type system -Name $group.Value -ErrorAction SilentlyContinue)) {
            New-JCSystemGroup -GroupName $group.Value | Out-Null
            Write-Host $group.Value 'System Group created in JumpCloud.'
        } else {
            Write-Host $group.Value 'System Group already exists in JumpCloud.'
        }
    }
} catch {
    Write-Error "Something went wrong when checking for groups! $($_.Exception.Message)" -ErrorAction Stop
}

# Populate the groups with the relevant systems.
try {
    Write-Host "`nPopulating JumpCloud System Groups with relevant systems..." -ForegroundColor Green
    $macSystems = Get-JCSystem -os 'Mac OS X' -returnProperties os,displayName,arch | Sort-Object arch
    $AppleMacGroupMembers = Get-JCSystemGroupMember -GroupName $groupNames.AppleMacGroupName
    $IntelMacGroupMembers = Get-JCSystemGroupMember -GroupName $groupNames.IntelMacGroupName
    $WindowsGroupMembers = Get-JCSystem -os 'Windows' -returnProperties os,displayName | Sort-Object os

    foreach ($system in $macSystems) {
        if (($system.arch -eq 'x86_64') -and ($system._id -notin $IntelMacGroupMembers.SystemID)) {
            Add-JCSystemGroupMember -SystemID $system._id -GroupName $groupNames.IntelMacGroupName
        }
        if (($system.arch -eq 'arm64') -and ($system._id -notin $AppleMacGroupMembers.SystemID)) {
            Add-JCSystemGroupMember -SystemID $system._id -GroupName $groupNames.AppleMacGroupName
        }
    }

    foreach ($system in $WindowsSystems) {
        if ($system._id -notin $WindowsGroupMembers.SystemID) {
            Add-JCSystemGroupMember -SystemID $system._id -GroupName $groupNames.WindowsGroupName
        }
    }


} catch {
    Write-Error "Something went wrong when trying to add our systems to the groups! $($_.Exception.Message)" -ErrorAction Stop
}
```

```bash
Get-JCSystem | % {Set-JCSystem -SystemID $_._id -displayName  $_.hostname}
```

# More to read
[0] https://community.jumpcloud.com/t5/community-scripts/populate-jumpcloud-system-groups-based-on-mac-chipset-via/td-p/405
[1] https://github.com/TheJumpCloud/support/wiki/
