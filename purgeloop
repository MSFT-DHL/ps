##Scripted by: Dany Harding Lamarche (DHL)
##Email: HL.D@microsoft.com
#Auto Purge email folder V2.0
Connect-ExchangeOnline
Connect-IPPSSession
cls
$UserUPN = Read-Host "Enter the user's UPN"
$HardDel = read-host "Enter the name of your Content Search"
$Scope = "DiscoveryHold"
$reset = $HardDel + "_Purge"
$data = Get-MailboxFolderStatistics $UserUPN -FolderScope "recoverableitems" | where {$_.Name -eq $scope}
$TimeStart = Get-Date
Write-Host "Start Time: $TimeStart"
Do { 
$result = $data.VisibleItemsInFolder
 if ($result -eq 0) {
  Write-host "The $Scope is empty!" -ForegroundColor "Green"
  $data | select Name, FolderP*, VisbileItemsInFolder, FolderS*, Storage* | Out-GridView
 }elseif ($stats -eq 0) {
  Write-host "The $Scope is empty!" -ForegroundColor "Green"
  $data | select Name, FolderP*, VisibleItemsInFolder, FolderS*, Storage* | Out-GridView
 }else {
  $Info = Get-MailboxFolderStatistics $UserUPN -FolderScope "recoverableitems" | where {$_.Name -eq $scope}
  $stats = $Info.VisibleItemsInFolder
  New-ComplianceSearchAction -SearchName $HardDel -Purge -PurgeType HardDelete -Confirm:$False -ErrorAction Stop
  Write-Host "There is $stats item(s) left in the $Scope folder." -ForegroundColor "cyan"
  Write-Host "Purging..." -ForegroundColor "Yellow"
  Start-Sleep -Seconds 10
  Remove-ComplianceSearchAction $reset -Confirm:$False
 }
 Start-Sleep -Seconds 0
}
Until ($stats -eq "0")
Write-host "The $Scope has been purged!" -ForegroundColor "Green"
Get-MailboxFolderStatistics $UserUPN -FolderScope "recoverableitems" | where {$_.Name -eq $scope} | select Name, FolderP*, VisibleItemsInFolder, FolderS*, Storage* | Out-GridView
