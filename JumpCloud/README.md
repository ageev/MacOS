## Sort Apple silicon based systems to separate group in JC

https://community.jumpcloud.com/t5/community-scripts/populate-jumpcloud-system-groups-based-on-mac-chipset-via/td-p/405

Get-JCSystem | Where-Object {$_.os -like "Mac OS X" -and $_.arch -eq "arm64"} | Add-JCSystemGroupMember -GroupName "Mac - Apple silicon"

Get-JCSystem | Where-Object {$_.os -like "Mac OS X" -and $_.arch -ne "arm64"} | Add-JCSystemGroupMember -GroupName "Mac - Intel systems"
