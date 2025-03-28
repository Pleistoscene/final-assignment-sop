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
   - ![Add Roles and Features](images/6.png)
3. (Before You Begin) Click **Next**
4. (Installation Type) Select **Role-based or feature-based installation** and click **Next**
5. (Server Selection) Choose the local server(DC-01) and click **Next**
6. (Server Roles) Check **Active Directory Domain Services** and click **Next**
   - ![AD DS](images/7.png)
7. (Features) click **Next**
8. (AD DS) click **Next**
9. (Confirmation) click **Install**
    - ![AD DS Install](images/8.png)
10. Wait for the install to complete and **Close**

### Step 4: Promote to Domain Controller
1. In Server Manager, click the yellow flag > **Promote this server to a domain controller**
   - ![Promote DC](images/9.png)
2. (Deployment Configuration) Choose **Add a new forest**, enter `NSA.local` and click **Next**
   - ![Add Forest](images/10.png)
3. (Domain Controller Options) Enter DSRM (Directory Services Restore Mode) password and click **Next**
   - ![Domain Password](images/11.png)
4. (Deployment Configuration - DNS Options) Click **Next**
5. (Additional Options) Leave the default NetBIOS name or customize if needed
6. Continue through the wizard and click **Install** after prerequisites check
    - ![Domain Password](images/12.png)
7. After promotion completes let it restart

### Step 5: Verify Domain and DNS
1. Log in as `NSA\Administrator`
   - ![Login](images/13.png)
2. Open **Server Manager > Tools > DNS**
3. Verify **Forward Lookup Zones** contains `NSA.local`
   - ![Forward Lookup Zones](images/14.png)
4. Create a **Reverse Lookup Zone** for `192.168.0.x`
   - **Right-Click** Reverse Lookup Zone > New Zone
   - ![Reverse Lookup Click](images/15.png)
   - **Choose** Primary Zone, Check "Store the zone in..." and Click **Next**
   - Click **Next**
   - IPv4 Reverse Lookup Zone and Click **Next**
   - Network ID: Enter '192.168.0.2.x' and Click **Next**
   - **Choose** Allow only secure dynamic updates(recomended for Active Directory) and click **Next**
     ![Reverse Lookup](images/16.png)
   - Click **Finish**
5. Add a PTR(Pointer Record) in Reverse Lookup Zone for '192,168.0.x'
   - **Right-Click** the Reverse Lookup Zone > New Pointer(PTR)
   - ![Add PTR](images/17.png)
   - Enter **'192.168.0.2'** for Host IP Address and click **OK**
   - ![Host IP Address](images/18.png)
6. (Optional) Verifiy DNS
   - Start > type 'cmd', press Enter, then type 'nslookup' and hit Enter
   - ![Host IP Address](images/19.png)

### Step 6: Install DHCP Role
1. Click **Start > Server Manager**
2. Click **Manage > Add Roles and Features**
   - ![Add Roles and Features](images/6.png)
3. (Before You Begin) Click **Next**
4. (Installation Type) Select **Role-based or feature-based installation** and click **Next**
5. (Server Selection) Choose the local server(DC-01) and click **Next**
6. (Server Roles) Select **DHCP Server** and click **Next**
   - ![Add Roles and Features](images/20.png)
8. (Features) Click **Next**
9. (DHCP Server) Click **Next**
10. (Confirmation) Click **Install**, Close once comeplete
    - ![Add Roles and Features](images/21.png)

### Step 7: Authorize Server
1. In Server Manager, click the yellow flag > **Complete DHCP Configuration**
   - ![DHCP Configuration](images/22.png)
3. (Description) Click **Next**
4. (Authorization) Leave default and click **Commit**
   - ![DHCP Commit](images/23.png)
6. (Summary) Click **Close**

### Step 8: Configure DHCP Scope
1. Go to **Server Manager > Tools > DHCP**
2. Expand **Your Domain** > **IPv4** > right-click > **New Scope**
   ![DHCP Scope](images/24.png)
4. **Next** > Name: `NSA-DHCP` click **Next**
5. Set range:
   - Start IP: `192.168.0.21`
   - End IP: `192.168.0.254`
6. Lease duration: default or adjusted as needed
7. DHCP options:
   - Default Router Gateway: `192.168.0.1`
   - DNS Server: `192.168.0.2`
   - Domain Name: `NSA.local`
8. Skip WINS Server
9. Yes, activate the scope > **Finish**

### Completion



## Summary and Conclusion

This SOP walked through setting up an on-premises domain controller using Windows Server 2025 with a GUI on real hardware. We configured a static IP address, installed and promoted the server to a domain controller, and set up DNS and DHCP services. Testing confirmed that the domain, name resolution, and IP address assignment worked as expected.

With this setup complete, the network is ready for additional client computers and future infrastructure to join the domain `NSA.local`. This SOP can be reused and updated for future deployments or referenced when expanding the network.
