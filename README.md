# active-directory-homelab
A homelab environment built in VirtualBox to simulate an Active Directory domain with Windows Server 2022 and Windows 10 clients.

# resources needed
VirtualBox 2.7.0 https://www.virtualbox.org/wiki/Downloads
Windows Server 2022 ISO https://www.microsoft.com/en-us/evalcenter/download-windows-server-2022
Windows 10 ISO https://www.microsoft.com/en-us/software-download/windows10 

# Create Virtual Machine for Domain Controller
1. Open Virtual Box
2. Click Machine at the top 
3. Click New
4. Set VM Name to "DC"
5. Select Windows Server 2022 ISO Image
6. Uncheck "Proceed with Unattended Installation" 
7. Set Memory to 4gb, CPU Cores to 2, Disk Space to 50gb
8. Confirm
9. Go to VM Settings
10. Set Shared Clipboard and Drag and Drop to Bidirectional
11. Set up Network Adapters: Adapter 1 attached to NAT, Adapter 2 attached to Internal Network

# Set Up Domain Controller in VM
1. Open "DC" VM
2. Select Language 
3. Click Next 
4. Install Now
5. Select one of the Desktop Experience Server 2022 versions
6. Accept License Agreements
7. Custom Install
8. Select Drive 
9. Click Next
10. Setup Admin Password
11. Restart

# Add Tools to Improve VM Performance
1. Open "DC" VM
2. Select Devices at the top and click "Insert Guest Additions CD Image"
3. Open Guest Additions CD Image in File Explorer and run the AMD script
4. Restart

# Configure Network Adapters
1. Open Network Settings
2. Configure Internal Network Adapter IP to 172.16.0.1
3. Configure Internal Network Subnet mask to 255.255.0.0
4. Configure Internal Network DNS to 127.0.0.1
5. Do not worry about NAT Internal Adapter because it uses DHCP

# Rename PC
1. Right Click Windows in bottom left and select "System"
2. Click Rename This PC, I set it to "DC"
3. Restart

# Set Up Active Directory
1. In Server Manager, click "Add Roles and Features"
2. Click Next until "Server Roles"
3. Check "Active Directory Domain Services"
4. Click Next until Install

# Set Up Domain
1. Click yellow flag and click promote this server to a domain controller
2. Click Add a New Forest
3. Name Domain Name (I set it to mydomain.com)
4. Click Next until Install