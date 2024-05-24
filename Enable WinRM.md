 ## Guide for setting up WinRM on target machines
#### Open Powershell >Run as Admin 

 ```sh
# Enable WinRM
Set-NetConnectionProfile -NetworkCategory Private
winrm quickconfig -force
Enable-PSRemoting -force

# Allow non-SSL traffic (port 5985)
winrm set winrm/config/service '@{AllowUnencrypted="true"}'
winrm set winrm/config/service/auth '@{Basic="true"}'
winrm set winrm/config/client/auth '@{Basic="true"}'

# Create a cert for SSL (port 5986)
$Hostname = [System.Net.Dns]::GetHostByName($env:computerName).HostName
$Thumbprint = (New-SelfSignedCertificate -Subject "CN=$Hostname" -TextExtension '2.5.29.37={text}1.3.6.1.5.5.7.3.1').Thumbprint
$A = '@{Hostname="'+$Hostname+'"; CertificateThumbprint="'+$Thumbprint+'"}'
winrm create winrm/config/Listener?Address=*+Transport=HTTPS $A

# Firewall Rules
New-NetFirewallRule -DisplayName "WinRM" -Group "Windows Remote Management" -Program "System" `
  -Protocol TCP -LocalPort "5985" -Profile Domain,Private
New-NetFirewallRule -DisplayName "WinRM" -Group "Windows Remote Management" -Program "System" `
  -Protocol TCP -LocalPort "5985" -Profile Public
New-NetFirewallRule -DisplayName "WinRM Secure" -Group "Windows Remote Management" -Program "System" `
  -Protocol TCP -LocalPort "5986" -Profile Domain,Private
New-NetFirewallRule -DisplayName "WinRM Secure" -Group "Windows Remote Management" -Program "System" `
  -Protocol TCP -LocalPort "5986" -Profile Public

```
