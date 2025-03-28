**Standard Operating Procedure (SOP)**  
**Setting Up an On-Premises Domain Controller**  

**College:** Manitoba Institute of Trades and Technology (MITT)  
**Course:** Network and System Administration  
**Version:** 1.0  
**Written By:** Aaron Queskekapow, *Add your names here*  
**Date:** 03/26/2025  

## Approval

| Name              | Role     | Date Approved |
|-------------------|----------|----------------|
| Aaron Queskekapow | Approver | 03/26/2025     |

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
   - Set the Administrator password
3. Log in as Administrator.
4. Rename the machine to `DC-01`:
   - Open **Settings > System > About > Rename this PC**
   - Set to `DC-01`, restart when prompted.

### Step 2: Configure Static IP Address
1. Go to **Settings > Network & Internet > Ethernet > Change Adapter Options**
2. Right-click the active network interface > **Properties**
3. Select **Internet Protocol Version 4 (TCP/IPv4)** > **Properties**
4. Configure the following:
   - IP Address: `192.168.0.2`
   - Subnet Mask: `255.255.255.0`
   - Default Gateway: `192.168.0.1`
   - Preferred DNS Server: `127.0.0.1`
5. Click OK and close all windows.

### Step 3: Install AD DS Role
1. Open **Server Manager**
2. Click **Manage > Add Roles and Features**
3. Choose **Role-based or feature-based installation** > Select local server
4. Select **Active Directory Domain Services (AD DS)**
5. Add required features when prompted
6. Click **Next** through remaining screens, then click **Install**
7. Wait for installation to complete, but **do not restart yet**

### Step 4: Promote to Domain Controller
1. In Server Manager, click the notification flag > **Promote this server to a domain controller**
2. Select **Add a new forest**, enter `NSA.local`
3. Set Forest and Domain functional levels to **Windows Server 2025**
4. Set a Directory Services Restore Mode (DSRM) password
5. Accept defaults for DNS and NetBIOS
6. Review and click **Install**, then **restart when prompted**

### Step 5: Verify Domain and DNS
1. Log in after reboot using domain admin credentials
2. Open **Server Manager > Tools > DNS**
3. Verify that `NSA.local` zone exists under **Forward Lookup Zones**
4. Optionally, create a reverse lookup zone for `192.168.0.x`
5. Test DNS with:
   ```bash
   nslookup nsa.local
   ```

### Step 6: Install DHCP Role
1. Open **Server Manager > Manage > Add Roles and Features**
2. Add **DHCP Server** role
3. Click **Next** until **Install**, then click **Install**
4. Once installed, click **Complete DHCP Configuration**
5. Authorize the DHCP server using the local domain admin credentials

### Step 7: Configure DHCP Scope
1. Open **Server Manager > Tools > DHCP**
2. Expand IPv4 > Right-click > **New Scope**
3. Name: `NSA-DHCP`
4. IP Range:
   - Start: `192.168.0.21`
   - End: `192.168.0.254`
5. Add exclusion range: `192.168.0.1 – 192.168.0.20`
6. Default lease time: leave default or adjust as needed
7. Configure DHCP options:
   - Router: `192.168.0.1`
   - DNS Server: `192.168.0.2`
   - Domain Name: `NSA.local`
8. Activate the scope

### Step 8: Testing and Validation
- Boot a client device and connect it to the network
- Confirm it receives an IP from the DHCP range
- Join the domain `NSA.local`
- Run `ipconfig /all` to verify DNS and gateway
- Use `nslookup` to test name resolution

### Step 9: Backup & Documentation
- Save a screenshot of each step where possible
- Backup DHCP settings using PowerShell (optional)
- Commit this SOP and evidence to the group GitHub repo

## Summary and Conclusion

This SOP walked through setting up an on-premises domain controller using Windows Server 2025 with a GUI on real hardware. We configured a static IP address, installed and promoted the server to a domain controller, and set up DNS and DHCP services. Testing confirmed that the domain, name resolution, and IP address assignment worked as expected.

With this setup complete, the network is ready for additional client computers and future infrastructure to join the domain `NSA.local`. This SOP can be reused and updated for future deployments or referenced when expanding the network.
