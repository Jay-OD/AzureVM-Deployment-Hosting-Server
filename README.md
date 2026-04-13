# Palworld Dedicated Server on Microsoft Azure

## Overview

This project documents the end-to-end deployment, configuration, and management of a dedicated server for Palworld hosted on Microsoft Azure.

The initial goal was to host a game server after encountering platform limitations on a Linux-based NAS, as Palworld currently requires a Windows environment for dedicated server hosting. This evolved into a broader exploration of Azure cloud services, including infrastructure deployment, networking, identity management, automation, and cost control.

This repository is intended as a portfolio project demonstrating practical, hands-on experience with Azure in a real-world scenario.

## Objectives

- Deploy a publicly accessible dedicated server using Azure Virtual Machines
- Configure secure and functional networking for external access
- Enable multi-user access with controlled permissions
- Implement automation for server lifecycle management
- Monitor and control cloud spending
- Troubleshoot and resolve real-world infrastructure issues

## Architecture

The solution consists of the following components:

- Azure Virtual Machine (Windows Server)
- Public IP address for external connectivity
- Network Security Group (NSG)
- Windows Defender Firewall (VM-level security)
- Azure Cost Management and Budget alerts
- Azure Mobile App for remote control
- Microsoft Entra ID for identity and access management

## Virtual Machine Deployment

A Windows-based Azure Virtual Machine was provisioned to meet the requirements of hosting a Palworld dedicated server.

Key steps included:
- Creating and configuring the VM within Azure
- Enabling Remote Desktop Protocol (RDP) access
- Installing and launching the dedicated server binaries
- Managing runtime processes through Command Prompt

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/a1cb9a47-9191-461b-8e8e-7032d6f30013" />

## Networking Configuration

To allow external players to connect, both Azure-level and OS-level networking rules were configured.

### Network Security Group (NSG)
Inbound rules were created to allow required traffic:

- UDP 8211 (game traffic)
- UDP 27015 (server query and listing)

### Windows Defender Firewall
Matching inbound rules were configured within the VM to ensure traffic was not blocked at the OS level.

Key learning:
Both the Azure NSG and the VM firewall must be configured correctly. Allowing traffic in only one layer is insufficient.

## Troubleshooting and Issue Resolution

A significant portion of the project involved diagnosing and resolving connectivity and server visibility issues.

### Issue: Server not appearing in community list
Root cause:
- Query port (27015) was not accessible externally

Resolution:
- Verified NSG and firewall rules
- Ensured correct port configuration in server settings

### Issue: Port binding errors
CreateBoundSocket: couldn't find an open port between 27015 and 27015

Root cause:
- Multiple instances of the server running simultaneously
- Port 27015 already in use

Resolution:
- Identified active processes using `netstat` and `tasklist`
- Terminated conflicting processes using `taskkill`
- Ensured only a single server instance was running

### Issue: Conflicting or incorrect configuration guidance
During troubleshooting, incorrect port suggestions and configuration changes were introduced.

Resolution:
- Cross-referenced configurations with expected defaults
- Simplified setup to known working parameters
- Reintroduced changes incrementally

Key takeaway:
Troubleshooting requires validation and controlled testing rather than applying multiple changes at once.

## Identity and Access Management

Access was configured using Microsoft Entra ID.

- An external guest user was invited
- Permissions were restricted to a single virtual machine
- This enabled shared management without exposing broader subscription access

This demonstrates the use of least-privilege access principles in a real-world scenario.

<img width="500" height="200" alt="image" src="https://github.com/user-attachments/assets/1bae7246-7bee-4a59-b1fe-c76d0b393488" />

## Remote Management

The Azure mobile application was configured on multiple mobile devices to allow:

- Starting and stopping the virtual machine
- Monitoring VM status remotely

This improved accessibility and usability, particularly for users without direct desktop access and limited technical abilty.

## Automation

### Auto Shutdown
An automatic shutdown schedule was configured for 3:00 AM daily to reduce unnecessary compute costs. This allows us to only be charged for our usage saving over 70% of potential costs.

<img width="550" height="250" alt="image" src="https://github.com/user-attachments/assets/450baa28-3ab4-47a9-8f92-76e5eab848d0" />

<img width="300" height="400" alt="image" src="https://github.com/user-attachments/assets/d0df6648-6675-415d-8480-40ba89810305" />

### Server Startup Automation
NSSM (Non-Sucking Service Manager) was used to configure the server as a Windows service:

- Automatically starts when the VM boots
- Removes the need for manual command execution
- Improves reliability and usability

## Cost Management

A monthly budget of $50 was configured using Azure Cost Management.

Alert thresholds were set at:
- 50%
- 75%
- 90%
- 100%

Email notifications are triggered when thresholds are reached.

This ensures visibility and control over cloud spending.

<img width="450" height="200" alt="image" src="https://github.com/user-attachments/assets/14361af5-e334-4c69-a962-19d222a7d703" />

## Skills Demonstrated

- Azure Virtual Machine deployment and management
- Network configuration (NSG, firewall rules, port management)
- Windows Server administration
- Identity and access management using Entra ID
- Process management and troubleshooting
- Automation using system services (NSSM)
- Cost monitoring and budget enforcement

## Key Learnings

- Cloud networking requires understanding multiple layers of access control
- Background processes can cause hidden conflicts in server environments
- Incremental troubleshooting is more effective than applying multiple changes simultaneously
- Cloud cost management is a critical operational responsibility
- Practical projects provide deeper learning than theoretical study alone

## Future Improvements

- Infrastructure as Code (IaC) using Bicep or Terraform
- Automated deployment pipelines
- Integration with Azure Monitor and Log Analytics
- Backup and restore strategy for server data
- Multi-region deployment for improved latency and availability

## Conclusion

This project demonstrates the practical application of Azure services to solve a real-world problem.

What began as a simple requirement to host a game server developed into a comprehensive exercise in cloud infrastructure, networking, security, and cost management.

It highlights the ability to design, deploy, troubleshoot, and maintain cloud-based solutions using industry-relevant tools and practices.
