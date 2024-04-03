<h1>Active Directory Home Lab</h1>

 ### [YouTube Demonstration](https://www.youtube.com/watch?v=MHsI8hJmggI)

<h2>Description</h2>
In this lab we're going to walk through how to create an Active Directory home lab Enviorment using Oracle Virtual Box. Configuring and running this lab will definitely help develop my understanding of how active directory and windows networking works. Our Objectives are to setup a basic home lab running active directory (Oracle VirtualBox)
<br />


<h2>Steps</h2>

- <b>STEP 1: Download and Install Windows 10, SERVER2019ISO, OracleVM </b> 
- <b>STEP 2: Create and setup first virtual machine(domain controller) </b>
- <b>Step 3 : Configure IP Addressing on both NICs </b> 
- <b>Step 4 : INSTALL AD DS(Active Directory Domain Services) & create a Domain </b>
- <b>Step 5 : Install RAS/NAT (Remote Access Server/Network address Translation)</b> 
- <b>Step 6 : Setup DHCP server on Domain Controller</b>

<h2>Environment Layout </h2>

<img width="744" alt="Screenshot 2024-03-27 at 8 12 07 AM" src="https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/8914c649-bb23-4153-a4bc-aa3671bd5075">

<h2>Step 1 : Oracle VM</h2>
Install VMBox and Extension Packs 

<img width="1244" alt="Screenshot 2024-03-27 at 8 04 57 AM" src="https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/1a959662-f630-42cf-914f-d0509e80eb69">

<h2> Server2019ISO + Windows 10 ISO</h2>

<b> [Install Server2019 ISO](https://www.microsoft.com/en-us/evalcenter/download-windows-server-2019)<br/> 

and <b> [Windows 10 ISO](https://www.microsoft.com/en-us/software-download/windows10)<br/>

<h2>Step 2 : Setup Our Virtual Machine</h2>


first we configure the settings for adapter 1 to be our NIC to the internet, and Adapter 2 to the internal network  
![ACTIVEDIRECT2](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/c445129e-a85d-4dfa-a6a4-cfda408b4a78)

- After configuring our network, we boot the VM to install Server 2019 ISO into the Virtual Machine

![ISO ADD](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/56e013b9-06a8-4188-9467-5c13901f10be)

- Go through all the steps of installation

![isoadown](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/0e3c21c5-f2c8-4640-a004-b1fef79d8a70)

![Screenshot 2024-03-27 193852](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/31a52d72-46c9-4912-8fdd-c46ed9616568)

- After finishing the installation, I quickly ran the Oracle VM guest additions in order to get faster response time from the VM, the rebooted the machine.

![files](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/10be20e5-1ad3-4e44-aa2a-e11588c9255f)

<h2>Step 3 : Configure IP Addressing on both NICs</h2>

we have 2 NICs, one dedicated to the internet, and the other used for the interal. The one on the internet will get a IP address from my local router so I dont have to do any configurations with that one,
although with the internal we have to set it up manually.

![nicpic1](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/2d525ab3-6880-4a80-b5fc-25db392848e0)

First we click our network and go on Network & Internet Settings > Ethernet > Change adapter options 

![ethernet adp](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/f1ce3b51-eadb-4097-940a-46273dfd80fe)

Then right click the first Ethernet > Status> and Identify which  Ethernet is for Internet and which one is for Internal and label them accordingly 

![rename](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/24c09f1b-ab5f-4a66-8a30-e6457de51e05)

You can tell which one is for the internet if it has an IP address of 10.0... 

![ivp43](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/6303e91e-2a0a-493f-a840-2e18865f4c65)

Then I Configured the IP address on the internal network. Right Click Internal Network>Properties> Double Click IPV4, and for this lab I used the given IP ADDRESSING from the video demenstration. 

![ipadressing](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/4a2769a3-b5c6-4032-9b4e-b4077b4aa8df)

We input the IP Addressing, I am not going to use a Default Gateway because the Domain Controller itself will server as the Default gateway since we have 2 NICs: One on the internet and one on the inside. 
This server will also use itself as a DNS server so for preferred DNS server you can either put the ip address as the DNS or the loopback address(127.0.0.1)

![ipv4](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/cc5e53d9-3078-419e-9475-2eb5834db8d1)

For this step it will require us to reboot our system, I will be renaming this PC to "DC" for "Domain Controller" for clarification purposes. 

![rename 2](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/46ab58eb-d0d9-493e-a6f1-934adc3b9805)

<h2>Step 4 : INSTALL AD DS(Active Directory Domain Services) & create a Domain </h2>

![adds5](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/dd2e5940-2507-4990-9afa-5e4ab6b77ac7)

I will start with Add roles and features, Insstall server destination. check the box for Active Directory Domain Services

![serverman](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/e0d79ac5-ea0a-4b6d-8ef5-f38240d85372)

Click next until Installation 

![installadds](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/ed40e5c5-951e-4d1b-97a0-2ca6153f4645)

Next we see we get a flag notification "Post-deployment Configuration" this is bc we installed the software for ADDS(Active Directory Domain Services) 
but I havent created the Domain yet. > Promote this server to domain>Add new forest, for domain name you can put anything you want. Set your password> Then Install.

![forest](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/3347cc7d-891d-4386-be4c-b0c1ec889ef7)

![install](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/e244578a-a022-409e-b75d-d81104ae2e89)

After installation it will automatically sign out since Active Directory is installed

Now I will create my own dedicated domain admin account instead of using the the built in admin account on windows 

START> Administrative Tools >Active Directory Users and Computers>

![users](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/d1698e19-b675-4ed4-9691-bf0e9beeb006)

From here I will Create a Organizational Unit to put our Admin account in naming it _ADMINS

![ADMINS](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/cc1b9132-ee51-4c07-a01b-f40dd7571775)

And from the Organizational Unit I will add a new User and assign it a password aswell, unchecking the box that says "User must change password at next logon" and checking "Password never expires" since we are in a live enviorment.

![orgunit ](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/0c6d3d5e-d720-4170-af7f-b1ed80fddfa1)

Now our User is Created but it isnt an admin yet. To give admin privlidges Right Click User>Properties >Member Of> Add> Naming it "Domain Admins"> Click check names> Apply> OK

![adminsset](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/b2be4d66-e7bf-4576-b42f-0d79693691e6)

I signed out of the MYDOMAIN\Adminstrator account and logged into the Domain Admins account I just created. 

![singoff](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/f2bb24be-f2fd-49ed-a778-8614905d69bf)

<h2>Step 5 : Instal RAS/NAT (Remote Access Server/Network address Translation) </h2>
The purpose of this is to allow our windows 10 client1 to be on a virtual private network but still be able to access the internet through the Domain Controller so we would need to install RAS/NAT on the domain controller to allow the Client1
to do that 

![NAT](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/37e07dca-3b81-4a56-b4f3-a1785b1f2d17)

To do this I will go to "Add roles and features" from the Server Manager, checking the "Remote Access" box from the "Server Roles" and on "Role Service" checking the "Routing" box

Next 4x> Install

![routing](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/ceb6be7d-3e68-4899-ade9-ad714c7f87ad)

After Installation, from the Server Manager Dashboard go to > TOOLS> Routing and Remote Access> Click on "DC" > Configure and Enable Routing and Remote Access check the NAT, allowing internal clients to connect to the Internet using one public IP Address 

![natty](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/d1222e25-caef-4c55-a92b-d2b70c74651b)

From there we will select our Internet NAT to connect to Internet > Next> Finish

![ineer](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/4094e891-987e-4227-84da-5fead65cd6eb)

<h2>Step 6 : Setup DHCP server on Domain Controller  </h2>
This will give our Windows 10 Clients an IP address that will get them on the Internet and allow them to browse the Internet, while still being on this private internal network



 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
