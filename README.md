<h1>Active Directory Home Lab</h1>

 ### [YouTube Demonstration](https://www.youtube.com/watch?v=MHsI8hJmggI)

<h2>Description</h2>
In this lab we're going to walk through how to create an Active Directory home lab Enviorment using Oracle Virtual Box. Configuring and running this lab will definitely help develop my understanding of how active directory and windows networking works. Our Objectives are to setup a basic home lab running active directory (Oracle VirtualBox)
<br />


<h2>Steps</h2>
 1.Download and install Oracle Box 2.Download Windows 10 IS 3.Create first virtual machine(domain controller) 4.install server 2019 and sign IP addressing for internal network 5.Name the server, Install active directory and create domain. 5. Setup DHCP server 6. Create powershell script 

- <b>PowerShell</b> 
- <b>Diskpart</b>

<h2>Environment Layout </h2>

<img width="744" alt="Screenshot 2024-03-27 at 8 12 07 AM" src="https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/8914c649-bb23-4153-a4bc-aa3671bd5075">

<h2>Step 1 : Oracle VM</h2>
Install VMBox and Extension Packs 

<img width="1244" alt="Screenshot 2024-03-27 at 8 04 57 AM" src="https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/1a959662-f630-42cf-914f-d0509e80eb69">

<h2>Step 2 : Server2019ISO + Windows 10 ISO</h2>

<b> [Install Server2019 ISO](https://www.microsoft.com/en-us/evalcenter/download-windows-server-2019)<br/> 

and <b> [Windows 10 ISO](https://www.microsoft.com/en-us/software-download/windows10)<br/>

<h2>Step 3 : Setup Our Virtual Machine</h2>


first we configure the settings for adapter 1 to be our NIC to the internet, and Adapter 2 to the internal network  
![ACTIVEDIRECT2](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/c445129e-a85d-4dfa-a6a4-cfda408b4a78)

- After configuring our network, we boot the VM to install Server 2019 ISO into the Virtual Machine

![ISO ADD](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/56e013b9-06a8-4188-9467-5c13901f10be)

- Go through all the steps of installation

![isoadown](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/0e3c21c5-f2c8-4640-a004-b1fef79d8a70)

![Screenshot 2024-03-27 193852](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/31a52d72-46c9-4912-8fdd-c46ed9616568)

- After finishing the installation, I quickly ran the Oracle VM guest additions in order to get faster response time from the VM!

![files](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/10be20e5-1ad3-4e44-aa2a-e11588c9255f)






 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
