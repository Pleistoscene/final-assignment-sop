**Standard Operating Procedure (SOP)**  
**Setting Up an On-Premises Domain Controller**  

**College:** Manitoba Institute of Trades and Technology (MITT)  
**Course:** Network and System Administration  
**Version:** 1.0  
**Written By:** Aaron Queskekapow, *Add your names here*  
**Date:** 03/27/2025  

## Approval

| Name              | Role     | Date Approved |
|-------------------|----------|----------------|
| Aaron Queskekapow | Approver | 03/27/2025     |

## Reversion

This SOP has a revision history to keep track of any changes made over time. If anything needs to be updated later, it'll be added to the table below. If a change causes problems or doesn’t work out, we can go back to an earlier version with approval.

### Revision History

| Version | Date       | Description                | Author              |
|---------|------------|----------------------------|---------------------|
| 1.0     | 03/27/2025 | Initial release of the SOP | Aaron Queskekapow   |

## Purpose

This SOP is part of our final assignment and demonstrates how to install and configure the Desktop Experience version of Windows Server 2025 (GUI) as a domain controller with DNS and DHCP services.

## Application

This applies to IT administrators or technicians responsible for deploying Windows Server 2025 in an on-premises environment using NAT networking. The steps ensure the system is prepared to support domain-joined client machines.

## Scope

This SOP covers the complete process of installing and configuring Windows Server 2025 with Desktop Experience on a physical machine. It includes setting a static IP, installing roles (Active Directory Domain Services, DNS, and DHCP), promoting the server to a domain controller, and verifying services.

## Responsibilities

- **Group Members**: Collaboratively handle server installation, network configuration, documentation, testing, and submission. Roles may be shared or rotated.

## Network Configuration

| Component          | IP Address Range            | Description                          |
|--------------------|-----------------------------|--------------------------------------|
| **Domain Name**    | NSA.local                   | Active Directory domain name         |
| **Network Address**| 192.168.0.1/24              | NAT-based local network              |
| **DC-01**          | 192.168.0.2/24              | Domain Controller and DNS server     |
| **DNS Server**     | 192.168.0.2                 | Self-assigned DNS                    |
| **Reserved Range** | 192.168.0.3 – 192.168.0.20  | Reserved for future static servers   |
| **DHCP Pool**      | 192.168.0.21 – 192.168.0.254| Dynamic IP range for client devices  |

## Step-by-Step Procedure

### Step 1: Initial Server Setup
1. Power on the physical server with Windows Server 2025 installation media.
2. Complete initial setup steps:
   - Choose "Windows Server 2025 Standard (Desktop Experience)"
     ![Initial Setup](images/1.png)
   - Set the Administrator password
     ![Initial Setup](images/2.png)
3. Log in as Administrator.
4. Rename the machine to `DC-01`:
   - Open **Server Manager**
   - Click on **Local Server** in the left-hand menu. 
   - Next to **Computer Name**, click the current name (e.g., `WIN-6OG0DUBK8TD`)
   - ![Server Manager](images/3.png)
   - Click the **Change** button
   - ![Change Name](images/4.png)
   - Enter the hostname "DC-01" and click **OK**
   - Restart the server when prompted to apply the change

### Step 2: Configure Static IP Address
1. Click **Start > Settings > Network & Internet > Ethernet**
2. Select the connected network, scroll down to **IP assignment**, and click **Edit**
   ![IP Edit](images/5.png)
4. Change to **Manual**, enable **IPv4**, and enter:
   - IP Address: `192.168.0.2`
   - Subnet Mask: `255.255.255.0`
   - Gateway: `192.168.0.1`
   - Preferred DNS: `192.168.0.2`
5. Click **Save** and verify connectivity

### Step 3: Install AD DS Role
1. Click **Start > Server Manager**
2. Click **Manage > Add Roles and Features**
   ![Add Roles and Features](images/6.png)
4. (Before You Begin) Click **Next**
5. (Installation Type) Select **Role-based or feature-based installation** and click **Next**
6. (Server Selection) Choose the local server(DC-01) and click **Next**
7. (Server Roles) Check **Active Directory Domain Services** and click **Next**
   ![AD DS](images/7.png)
8. (Features) click **Next**
9. (AD DS) click **Next**
10. (Confirmation) click **Install**
    ![AD DS Install](images/8.png)
12. Wait for the install to complete and **Close**

### Step 4: Promote to Domain Controller
1. In Server Manager, click the yellow flag > **Promote this server to a domain controller**
2. Choose **Add a new forest** and enter `NSA.local`
3. Leave the default NetBIOS name or customize if needed
4. Set DSRM (Directory Services Restore Mode) password
5. Continue through the wizard and click **Install**
6. Restart the system after promotion completes

### Step 5: Verify Domain and DNS
1. Log in as `NSA\Administrator`
2. Open **Server Manager > Tools > DNS**
3. Verify **Forward Lookup Zones** contains `NSA.local`
4. Optionally create a **Reverse Lookup Zone** for `192.168.0.x`
5. Test name resolution with:
   ```powershell
   nslookup nsa.local
   ```

### Step 6: Install DHCP Role
1. In **Server Manager**, click **Manage > Add Roles and Features**
2. Select **DHCP Server** and install
3. Click **Complete DHCP Configuration** after install finishes
4. Authorize the server when prompted using domain credentials

### Step 7: Configure DHCP Scope
1. Go to **Server Manager > Tools > DHCP**
2. Expand **IPv4** > right-click > **New Scope**
3. Name: `NSA-DHCP`
4. Set range:
   - Start: `192.168.0.21`
   - End: `192.168.0.254`
5. Add exclusion: `192.168.0.1 – 192.168.0.20`
6. Lease duration: default or adjusted as needed
7. DHCP options:
   - Router: `192.168.0.1`
   - DNS Server: `192.168.0.2`
   - Domain Name: `NSA.local`
8. Activate the scope

### Step 8: Testing and Validation
- Connect a client device to the network
- Confirm it receives an IP from the DHCP pool
- Attempt to join the domain: `NSA.local`
- Use `ipconfig /all` to verify IP and DNS settings
- Run:
   ```powershell
   nslookup nsa.local
   ```
   to confirm name resolution

### Step 9: Backup & Documentation
- Save a screenshot of each step where possible
- Backup DHCP settings using PowerShell (optional)
- Commit this SOP and evidence to the group GitHub repo

## Summary and Conclusion

This SOP walked through setting up an on-premises domain controller using Windows Server 2025 with a GUI on real hardware. We configured a static IP address, installed and promoted the server to a domain controller, and set up DNS and DHCP services. Testing confirmed that the domain, name resolution, and IP address assignment worked as expected.

With this setup complete, the network is ready for additional client computers and future infrastructure to join the domain `NSA.local`. This SOP can be reused and updated for future deployments or referenced when expanding the network.
