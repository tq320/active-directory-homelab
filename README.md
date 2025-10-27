# 🖥️ Active Directory Homelab
A homelab environment built in VirtualBox to simulate an Active Directory domain using Windows Server 2022, Windows 10, DHCP, NAT, and automated user creation via PowerShell.

---

## 📌 Requirements
1. VirtualBox 2.7.0  
   https://www.virtualbox.org/wiki/Downloads  
2. Windows Server 2022 ISO  
   https://www.microsoft.com/en-us/evalcenter/download-windows-server-2022  
3. Windows 10 ISO  
   https://www.microsoft.com/en-us/software-download/windows10

---

## 🏗️ Create the Domain Controller VM (DC)
1. Open VirtualBox.
2. Click **Machine → New**.
3. Name the VM `DC`.
4. Select the Windows Server 2022 ISO.
5. Uncheck **Proceed with unattended installation**.
6. Hardware recommendations:
   - RAM: **4 GB**
   - CPU Cores: **2**
   - Disk: **50 GB**
7. Confirm the VM creation.
8. Open **Settings**:
   - **General → Advanced**: Shared Clipboard + Drag & Drop = **Bidirectional**
   - **Network**:
     - Adapter 1: **NAT**
     - Adapter 2: **Internal Network**

---

## 🔧 Install Windows Server 2022
1. Start the `DC` VM.
2. Select language → **Next** → **Install Now**.
3. Choose **Windows Server 2022 (Desktop Experience)**.
4. Accept license → Custom install.
5. Select the disk → **Next**.
6. Set Administrator password when prompted.
7. Restart.

---

## 🚀 Install VirtualBox Guest Additions
1. In the VM, click **Devices → Insert Guest Additions CD Image**.
2. Open the mounted CD.
3. Run the installer.
4. Restart.

---

## 🌐 Configure Internal Network Adapter
Open **Network & Internet Settings → Change adapter options**

Set IPv4:
- IP Address: `172.16.0.1`
- Subnet Mask: `255.255.255.0`
- DNS: `127.0.0.1`

*(Leave NAT adapter default.)*

---

## 🏷️ Rename the Server
1. Right-click Start → **System**.
2. Click **Rename this PC**.
3. Set name to `DC`.
4. Restart.

---

## 📁 Install Active Directory Domain Services (AD DS)
1. Open **Server Manager**.
2. Click **Add Roles and Features**.
3. Continue to **Server Roles**.
4. Check **Active Directory Domain Services**.
5. Install.

---

## 🌲 Promote to Domain Controller
1. Click the yellow notification flag.
2. Select **Promote this server to a domain controller**.
3. Choose **Add a new forest**.
4. Domain Name example: `mydomain.com`
5. Continue through setup and install.
6. Restart.

---

## 👤 Create Domain Admin Account
1. Open **Active Directory Users and Computers (ADUC)**.
2. Right-click the domain → **New → Organizational Unit**.
3. Name it `_ADMINS`.
4. Right-click `_ADMINS` → **New → User**.
5. Fill in details.
6. Open the user’s **Properties → Member Of**.
7. Add group: `Domain Admins`.
8. Sign out and sign in using the new admin account.

---

## 🌐 Configure NAT (Internet for Clients)
> ⚠️ Lab only — do NOT do this in production.

1. Open **Add Roles and Features** → **Remote Access**.
2. At **Role Services**, check **Routing**.
3. Install.
4. Open **Tools → Routing and Remote Access**.
5. Right-click `DC` → **Configure and Enable…**
6. Choose **NAT**.
7. Select the NAT adapter.

---

## 📡 Install & Configure DHCP
1. Open **Add Roles and Features** → **DHCP Server**.
2. Install and open **DHCP** from Tools.
3. Expand your server name → Right-click **IPv4** → **New Scope**.
4. Configure:
   - Name: `172.16.0.100–200`
   - Start: `172.16.0.100`
   - End: `172.16.0.200`
   - Subnet Mask: `255.255.255.0`
5. Skip exclusions.
6. Lease duration optional.
7. Set Router (Gateway): `172.16.0.1`
8. Finish and **Authorize** the server.

---

## 🌍 Allow Browsing (Optional)
> ⚠️ Never do this in production.

1. In **Server Manager**, click **Configure this local server**.
2. Disable **IE Enhanced Security Configuration**.

---

## ⚙️ Bulk User Creation via PowerShell
1. Open Internet Explorer.
2. Navigate to:  
   `https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1`
3. Download/extract to Desktop.
4. In `names.txt`, add your name (optional).
5. Open **Windows PowerShell ISE** as Administrator.
6. Open `1_CREATE_USERS.ps1`.
7. Run: Set-ExecutionPolicy Unrestricted
*(Lab only)*
8. `cd` into the script folder.
9. Run the script (F5).
10. In ADUC, refresh — `_USERS` OU appears.

---

## 💻 Create Windows 10 Client VM
1. **New** VM → Name it `Client1`.
2. Specs:
- RAM: **4 GB**
- CPU Cores: **2**
- Disk: **50 GB**
3. Shared Clipboard + Drag & Drop = **Bidirectional**.
4. Network:
- Adapter 1 → **Internal Network**
5. Boot from Windows 10 ISO.
6. Choose **Windows 10 Pro**.
7. Custom install → continue.

---

## 🔑 Windows 10 Initial Setup
1. Continue setup prompts.
2. No password required for lab.
3. Choose **I don’t have internet** if asked.
4. Uncheck bloatware.
5. Once on desktop:
- Run `ipconfig` — verify DHCP lease.
- Test domain:
  ```
  ping mydomain.com
  ```
6. Right-click Start → **System → Rename this PC (Advanced)**.
7. Name: `Client1`.
8. Join `mydomain.com` with domain admin credentials.
9. Restart.

---

## ✅ Verify Client Registration
On the Domain Controller:

### DHCP
- Open **DHCP**.
- Check **Address Leases** — `Client1` should appear.

### ADUC
- Check domain computers — `Client1` should be listed.

---

## 🎉 Homelab Complete!

You now have:
✅ Active Directory Forest  
✅ DHCP Scope  
✅ NAT Routing  
✅ Domain Admin Account  
✅ Bulk User Accounts  
✅ Domain-Joined Client  

