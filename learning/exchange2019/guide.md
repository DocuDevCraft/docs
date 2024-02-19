### Part 1: System Requirements and Preparing Active Directory

#### System Requirements

Before installing Exchange Server 2019, ensure your system meets the following requirements:
- **Operating System**: Windows Server 2019 Standard or Datacenter.
- **Processor**: 64-bit, compatible with x64 instruction set.
- **Memory**: Minimum 128 GB RAM for Mailbox servers; 32 GB for Edge Transport servers.
- **Disk Space**: At least 30 GB on the drive where you're installing Exchange. Plus, additional space for the mailbox database and log files.
- **.NET Framework**: Version 4.8 or later.
- **Other Software**: Visual C++ Redistributable Package for Visual Studio 2012, and Unified Communications Managed API 4.0.

#### Preparing Active Directory

1. **Extend the Active Directory Schema**: Exchange needs to add new attributes to the AD schema. Run the following command in PowerShell as an administrator:

   ```powershell
   Setup.exe /PrepareSchema /IAcceptExchangeServerLicenseTerms
   ```

2. **Prepare Active Directory**: This step creates the Exchange organization in AD. Execute:

   ```powershell
   Setup.exe /PrepareAD /OrganizationName:"YourOrgName" /IAcceptExchangeServerLicenseTerms
   ```

3. **Prepare Domains**: Prepares every domain in the Active Directory forest where Exchange will be installed or where mail-enabled users will be located.

   ```powershell
   Setup.exe /PrepareAllDomains /IAcceptExchangeServerLicenseTerms
   ```

   **Note**: Replace `"YourOrgName"` with your actual organization's name. These steps require appropriate administrative privileges in your Active Directory environment.

### Summary

This section covered the basic system requirements for Exchange Server 2019 and the initial steps to prepare Active Directory for Exchange installation. Ensuring your environment meets these requirements and correctly preparing AD are crucial first steps before proceeding with the Exchange Server installation.

In the next part, we'll discuss the installation process of Exchange Server 2019, focusing on PowerShell commands for a non-GUI setup.
