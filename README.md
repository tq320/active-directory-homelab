# active-directory-homelab
A homelab environment built in VirtualBox to simulate an Active Directory domain with Windows Server 2022 and Windows 10 clients.

# resources needed
VirtualBox 2.7.0 https://www.virtualbox.org/wiki/Downloads
Windows Server 2022 ISO https://www.microsoft.com/en-us/evalcenter/download-windows-server-2022
Windows 10 ISO https://www.microsoft.com/en-us/software-download/windows10 

# Step 1 (Create Domain Controller VM)
Open Virtual Box -> Machine -> New 
-> Set VM Name to "DC" -> Select Windows Server 2022 ISO Image
-> Uncheck "Proceed with Unattended Installation"
-> Set Memory to 4gb, CPU Cores to 2, Disk Space to 50gb
-> Confirm

Go to Settings of DC 
-> Set Shared Clipboard and Drag and Drop to Bidirectional
-> Set up Network Adapters
    --> Adapter 1 attached to NAT
    --> Adapter 2 attached to Internal Network

Open VM
-> Setup
    --> Select Language --> Next --> Install Now --> Select one of the Desktop Experience Server 2022 versions
    --> Accept License Agreements
    --> Custom Install -> Select Drive -> Next -> Setup Admin Password