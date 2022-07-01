## Sort Apple silicon based systems to separate group in JC

https://community.jumpcloud.com/t5/community-scripts/populate-jumpcloud-system-groups-based-on-mac-chipset-via/td-p/405

get token: click top right corner of admin portal -> API key

```bash
pwsh
Get-JCSystem | Where-Object {$_.os -like "Mac OS X" -and $_.arch -eq "arm64"} | Add-JCSystemGroupMember -GroupName "Mac - Apple silicon"
Get-JCSystem | Where-Object {$_.os -like "Mac OS X" -and $_.arch -ne "arm64"} | Add-JCSystemGroupMember -GroupName "Mac - Intel systems"
Get-JCSystem | Where-Object {$_.os -like "Windows"} | Add-JCSystemGroupMember -GroupName "Windows"
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
