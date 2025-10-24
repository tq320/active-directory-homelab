# active-directory-homelab
A homelab environment built in VirtualBox to simulate an Active Directory domain with Windows Server 2022 and Windows 10 clients.

# resources needed
1. VirtualBox 2.7.0 https://www.virtualbox.org/wiki/Downloads
2. Windows Server 2022 ISO https://www.microsoft.com/en-us/evalcenter/download-windows-server-2022
3. Windows 10 ISO https://www.microsoft.com/en-us/software-download/windows10 

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

# Create Domain Admin Account
1. Open Active Directory Users and Computers
2. Right Click Domain Name
3. Click New
4. Click Organizational Unit
5. Put Name as _ADMINS
6. Right Click _ADMINS
7. Click New
8. Click User
9. Fill in details
10. Right Click Admin Name
11. Click Properties
12. Go Member Of and Add Domain Admins
13. Sign Out
14. Sign Into Domain Admin Account

# Set up NAT
1. Go to Server Manager
2. Click Roles and Features
3. Click Next Until Server Roles
4. CLick Remote Access
5. Click Next until Role Services
6. Click Routing
7. Click Next until Install
8. Go to Tools in top right and click Routing and Remote Access
9. Right Click DC and click "Configure and Enable Routing and Remote Access"
10. Click Next and Select NAT and click Next
11. Select Internet

# Set up DHCP on Domain Controller
1. Go to Server Manager
2. Click Roles and Features
3. Click Next Until Server Roles
4. Click DHCP Server
5. Click Next until Install
6. Go to Tools in top right and click DHCP
7. Expand DC.mydomain.com
8. Right Click iPv4 and click New Scope
9. Set Name to 172.16.0.100-200
10. Set Start to 172.16.0.100 and End to 172.16.0.200
11. Set Subnet mask to 255.255.255.0
12. Skip Exclusions
13. Set Lease Duration to use case or leave at 8 days
14. Put Domain Controller IP (172.16.0.1) for Router
15. Click Next Until Finish
16. Authorize dc.mydomain.com then refresh

# Create Link to Internet from Domain Controller (don't do this in a production environment, it's okay for lab)
1. In Server Manager, Click "Configure this local server"
2. Disable "IE ENhanced Security Configuration" for admin and users

# Powershell Script to create accounts
1. Open Internet Explorer
2. Put in this link "https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1"
3. Save file to Desktop
4. Extract file
5. In names.txt, add your name to the top
6. Right click Start (bottom left), and expand Windows Powershell Folder
7. open Windows PowerShell ISE as Admin
8. Click Open Script top left and select "1_CREATE_USERS" from the folder we extracted
9. In the console, type "Set-ExecutionPolicy unrestricted", don't do this in an actual production environment
10. Press Enter and click "Yes to All"
11. Change directory to folder containing script (cd filepath)
12. Run Script (F5 or click green arrow at top)
13. In ADUC, refresh domain, _USERS should pop up
14. There should be users now and you can right click domain to find specific users

# Create Windows 10 VM
1. Open Virtual Box
2. Click Machine at the top 
3. Click New
4. Set VM Name to "Client1"
7. Set Memory to 4gb, CPU Cores to 2, Disk Space to 50gb
8. Confirm
9. Go to VM Settings
10. Set Shared Clipboard and Drag and Drop to Bidirectional
11. Set up Network Adapter: Adapter 1 attached to Internal Network
12. Open VM and mount Windows10 ISO that was downloaded
13. Install and select Windows 10 Pro
14. Accept License Agreements and Select Custom Install
15. Continue Until Installation

# Setup Windows 10 VM
1. Click next until the end
2. Password is not necessary for this lab
3. If network is asked, put dont have internet
4. Uncheck bloatware