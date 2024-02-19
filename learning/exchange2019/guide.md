
### Setting Up Active Directory

Before installing Exchange Server 2019, you must have Active Directory (AD) deployed. Here’s how to install AD on a Windows Server 2019 system:

1. **Install Windows Server 2019**: Install the OS on a server that will act as your Domain Controller (DC).

2. **Configure Static IP**: Assign a static IP address to this server. This is crucial for the stability of your AD environment.

3. **Install Active Directory Domain Services (AD DS)**: Use PowerShell to install AD DS. Run PowerShell as Administrator and execute:

   ```powershell
   Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
   ```

4. **Promote Server to a Domain Controller**: After installing AD DS, you need to promote the server to a domain controller. Use the following PowerShell command, replacing placeholders with your specific information:

   ```powershell
   Install-ADDSForest -DomainName "yourdomain.com" -InstallDNS -ForestMode Win2012R2 -DomainMode Win2012R2 -DatabasePath "C:\Windows\NTDS" -SysvolPath "C:\Windows\SYSVOL" -LogPath "C:\Windows\NTDS" -NoRebootOnCompletion -Force
   ```

   **Note**: Choose your domain name, and adjust the forest and domain modes if necessary. This example uses Windows Server 2012 R2 modes for broad compatibility, but you should use the highest level that all domain controllers in the forest can support.

### Installing Prerequisites for Exchange Server 2019

Once Active Directory is set up, proceed to install the prerequisites for Exchange Server 2019. This includes Windows features and external dependencies like .NET Framework and Visual C++ Redistributable.

1. **Install Required Windows Features**: Run the following command in PowerShell to install the necessary roles and features:

   ```powershell
   Install-WindowsFeature AS-HTTP-Activation, Desktop-Experience, NET-Framework-45-Features, RPC-over-HTTP-proxy, RSAT-Clustering, RSAT-Clustering-CmdInterface, RSAT-Clustering-Mgmt, RSAT-Clustering-PowerShell, Web-Mgmt-Console, WAS-Process-Model, Web-Asp-Net45, Web-Basic-Auth, Web-Client-Auth, Web-Digest-Auth, Web-Dir-Browsing, Web-Dyn-Compression, Web-Http-Errors, Web-Http-Logging, Web-Http-Redirect, Web-Http-Tracing, Web-ISAPI-Ext, Web-ISAPI-Filter, Web-Lgcy-Mgmt-Console, Web-Metabase, Web-Mgmt-Console, Web-Mgmt-Service, Web-Net-Ext45, Web-Request-Monitor, Web-Server, Web-Stat-Compression, Web-Static-Content, Web-Windows-Auth, Web-WMI, Windows-Identity-Foundation
   ```

2. **Install .NET Framework 4.8**: Download and install the .NET Framework 4.8 from the official Microsoft website.

3. **Install Visual C++ Redistributable**: Download and install the Visual C++ Redistributable for Visual Studio 2012 from the official Microsoft website.

4. **Install Unified Communications Managed API 4.0**: This is required for certain Exchange Server functionalities. Download and install it from the Microsoft Download Center.

### Summary

This part has covered setting up Active Directory on Windows Server 2019 and installing the prerequisites necessary for Exchange Server 2019. Each step is crucial to ensure a smooth installation and operation of Exchange Server in your environment.

Next, we will delve into the installation process of Exchange Server 2019 itself, focusing on using PowerShell commands for setup without relying on the graphical user interface.
