# On-Premises Domain Controller Setup

This repository contains the final assignment SOP and resources for setting up a Windows Server 2025 Domain Controller in an on-premises environment.

## Overview
This project demonstrates how to:
- Install Windows Server 2025 with Desktop Experience (GUI)
- Configure static IP and DNS
- Install and promote the server to a Domain Controller
- Set up DNS and DHCP services

## Prerequisites
Before proceeding with the steps outlined in this SOP, ensure the following prerequisites are met:

- You have access to a physical machine where **Windows Server 2025 with Desktop Experience (GUI)** will be installed.
- You possess basic knowledge of installing an operating system, including booting from USB, selecting installation options, and completing setup for Windows Server.
- You have access to the **Windows Server 2025 ISO image**.
- Your machine meets the **minimum system requirements** for Windows Server 2025 with GUI:
  - **Processor**: 1.4 GHz 64-bit processor (x64 architecture, supports NX and DEP, CMPXCHG16b, LAHF/SAHF, and PrefetchW)
  - **RAM**: 4 GB minimum
  - **Storage**: 64 GB minimum
  - **Network Adapter**: Ethernet adapter capable of at least 1 Gbps
  - **UEFI firmware** with Secure Boot capability
  - **Display**: Super VGA (1024 x 768) or higher-resolution monitor

## Domain Details
| Setting                                 | Value                            |
|-----------------------------------------|----------------------------------|
| **Domain Name**                         | NSA.local                         |
| **Network**                             | 192.168.100.0/24                  |
| **Default Gateway**                     | 192.168.100.1                     |
| **Domain Controller (DC-01)**           | 192.168.100.2                     |
| **DNS Server**                          | 192.168.100.2                     |
| **Reserved Static Range (Servers)**     | 192.168.100.3 – 192.168.100.20    |
| **DHCP Range (Clients)**                | 192.168.100.21 – 192.168.100.200  |
| **Optional Reserved Range (Future IPs)**| 192.168.100.201 – 192.168.100.254 |

## Repository Contents
- `domain_controller_setup.md`: Full SOP document
- `images/`: Visual documentation
- `README.md`: This file

## How to Use
1. Follow the steps outlined in `domain_controller_setup.md`
2. Use screenshots and logs as evidence for your setup
3. The domain name `NSA.local` is used as a default example in the guide.  
   **Feel free to replace it with a domain name that suits your environment**, such as `yourcompany.local` or `mittlab.local`.

## Authors
- Aaron Queskekapow  
- Ravneet Kaur
- Jaskirat Kaur Brar
- Harpreet Singh
