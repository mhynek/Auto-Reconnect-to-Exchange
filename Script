$KeyPath = 'c:\scripts'
. .\Functions-PSStoredCredentials.ps1
$credential= Get-StoredCredential -username adm@contoso.com


Function Connect2OP {
    Get-PSSession -Name "OP" -ErrorAction SilentlyContinue |Remove-PSSession
    $Exchange = "mail.contoso.com"
    $OPSession = New-PSSession -Name "OP" -ConfigurationName Microsoft.Exchange -ConnectionUri "http://$Exchange/PowerShell/" -Authentication Kerberos
    Import-PSSession $OPSession -AllowClobber -prefix OnPrem
  }

Function Connect2EXO {
    Get-PSSession -Name "EXO" -ErrorAction SilentlyContinue |Remove-PSSession
    $EXOSession = New-PSSession -ConfigurationName Microsoft.Exchange -ConnectionUri "https://outlook.office365.com/powershell-liveid/" -Credential $credential -Authentication "Basic" -AllowRedirection -Name "EXO"
    Import-PSSession $EXOSession -AllowClobber -prefix 365
  }

DO {IF((Get-PSSession -Name "EXO" -ErrorAction SilentlyContinue).State -ne "Opened") {Connect2EXO}}
UNTIL((Get-PSSession -Name "EXO").State -eq "Opened")

DO {IF((Get-PSSession -Name "OP" -ErrorAction SilentlyContinue).State -ne "Opened") {Connect2OP}}
UNTIL((Get-PSSession -Name "OP").State -eq "Opened")

