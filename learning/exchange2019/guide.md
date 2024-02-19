## Part 1: Setting Up Active Directory and DNS for Exchange Server 2019

### Step 1: Rename Conputer
Open PowerShell as Administrator and use the Rename-Computer cmdlet to rename the DC. For example, to rename the DC to "NewDCName", you would use:
```powershell
Rename-Computer -NewName "NewDCName" -Restart
```
The `-Restart` parameter will automatically restart the computer to apply the name change.

### Step 2: Install DNS Server Role

Before promoting your server to a domain controller, ensure the DNS Server role is installed. You can install the DNS Server role alongside the Active Directory Domain Services (AD DS) role using PowerShell with the following command:

```powershell
Install-WindowsFeature DNS -IncludeManagementTools
```

### Step 3: Verify DNS Installation

After installing the DNS Server role, verify the installation by running:

```powershell
Get-WindowsFeature DNS
```

This command will show you if the DNS server role is installed successfully. Look for `[X]` next to DNS, indicating it's installed.

### Step 4: Configure DNS Client Settings

Ensure your server's DNS client settings are correctly configured to use a valid DNS server. Ideally, your server should point to itself as the primary DNS server once it's a domain controller. However, during setup, if there's an existing DNS server in your environment, point your server to that DNS server. You can set the server's DNS client settings with the following PowerShell command, replacing `x.x.x.x` with the IP address of your DNS server:

```powershell
Set-DnsClientServerAddress -InterfaceIndex (Get-NetAdapter).InterfaceIndex -ServerAddresses ("x.x.x.x")
```

### Step 5: Retry Domain Controller Promotion

After ensuring the DNS services are correctly set up and configured, promote your server to a domain controller using the `Install-ADDSForest` command. Make sure to include the `-InstallDNS` parameter if your server will be the first domain controller and you're setting up a new domain:

```powershell
Install-ADDSForest -DomainName "yourdomain.com" -InstallDNS -ForestMode Win2012R2 -DomainMode Win2012R2 -DatabasePath "C:\Windows\NTDS" -SysvolPath "C:\Windows\SYSVOL" -LogPath "C:\Windows\NTDS" -NoRebootOnCompletion -Force
```

### Step 6: Configure Forward Lookup Zone (Optional)

After successfully promoting the server and installing DNS, you may need to configure your DNS forward lookup zone to ensure proper resolution of domain names. This step is typically handled automatically during the AD DS installation but may require manual configuration in complex environments.

### Summary

By following these steps, you should be able to resolve the DNS services requirement issue and proceed with promoting your server to a domain controller. Installing the DNS server role, verifying the installation, configuring DNS client settings, and ensuring proper DNS setup are crucial steps in preparing your environment for Exchange Server 2019.
