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


## Part 2: Managing Active Directory and Creating Service Accounts with PowerShell

After setting up Active Directory (AD) and DNS, the next step involves managing your AD environment and creating service accounts for Exchange Server 2019. Managing AD and creating service accounts via PowerShell enhances automation, efficiency, and repeatability. Here's how to manage your Active Directory and create service accounts with PowerShell:

#### Managing Active Directory

**1. Install the Active Directory Module for PowerShell:**

Before you can manage AD with PowerShell, ensure the RSAT (Remote Server Administration Tools) AD module is installed. On Windows Server 2019, you can install it using the following command:

```powershell
Install-WindowsFeature RSAT-AD-PowerShell
```

**2. Create Organizational Units (OUs):**

Organizational Units help organize your AD objects, such as users, groups, and computers. To create an OU for Exchange, use:

```powershell
New-ADOrganizationalUnit -Name "Exchange Resources" -Path "DC=yourdomain,DC=com"
```

Replace `"DC=yourdomain,DC=com"` with your actual domain path.

**3. Finding AD Objects:**

To find a user in AD, use:

```powershell
Get-ADUser -Filter 'Name -like "*John Doe*"'
```

To find a computer, use:

```powershell
Get-ADComputer -Filter 'Name -like "*Server01*"'
```

#### Creating Service Accounts for Exchange

Exchange Server requires specific service accounts for operations like mailbox access and database management. Here’s how to create these accounts:

**1. Create a Service Account:**

To create a new user account in AD for Exchange services, use:

```powershell
New-ADUser -Name "ExchangeServiceAccount" -GivenName "Exchange" -Surname "ServiceAccount" -UserPrincipalName "exchangeadmin@yourdomain.com" -Path "OU=Exchange Resources,DC=yourdomain,DC=com" -AccountPassword (ConvertTo-SecureString "YourStrongPasswordHere!" -AsPlainText -Force) -Enabled $true
```

- **Name**: Specify the account's name.
- **GivenName** and **Surname**: Optional details for the account.
- **UserPrincipalName**: The account's email address.
- **Path**: The OU where you want to place this account.
- **AccountPassword**: A secure string for the account's password.
- **Enabled**: Activates the account immediately.

**2. Assign Permissions:**

Depending on the service account's role, you might need to grant it specific permissions within AD or to specific mailboxes in Exchange. For Exchange, most permission assignments are done within the Exchange admin center or via Exchange Management Shell rather than directly in AD.

**3. Set Password Never Expires (Optional):**

For service accounts, you may want to set the password to never expire:

```powershell
Set-ADUser -Identity "ExchangeServiceAccount" -PasswordNeverExpires $true
```

**Note**: While convenient, consider the security implications of this setting.

### Summary

This part covered essential Active Directory management tasks and the creation of service accounts for Exchange Server 2019 using PowerShell. Efficiently managing your AD environment and properly setting up service accounts is crucial for a smooth Exchange operation. Always ensure to follow best practices for security, especially when handling account permissions and passwords.
