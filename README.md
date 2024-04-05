<h1>Active Directory Home Lab</h1>

 ### [YouTube Demonstration](https://www.youtube.com/watch?v=MHsI8hJmggI)

<h2>Description</h2>
<b>In this Lab I am going to walk through how I created an Active Directory home Lab Infrastructure using Oracle Virtual Box. Configuring and running this lab definitely helped develop my understanding of how Active Directory works and windows networking in general. This Active Directory will be used alongside a Powershell script to automate 1,000 (one thousand) users into the Active Directory. This Lab emulates a mini corporate network, using the Client1 </b>

<h2>Steps</h2>

- <b>STEP 1: Download and Install OracleVM, Windows 10, and SERVER2019ISO </b> 
- <b>STEP 2: Create and setup first virtual machine(domain controller) </b>
- <b>Step 3 : Configure IP Addressing on both NICs </b> 
- <b>Step 4 : INSTALL AD DS(Active Directory Domain Services) & create a Domain </b>
- <b>Step 5 : Install RAS/NAT (Remote Access Server/Network address Translation)</b> 
- <b>Step 6 : Setup DHCP server on Domain Controller</b>
- <b>Step 6 : Create Powershell Script</b>
- <b>Step 7: Create Windows 10 VM in Virtual Box</b>

<h2>Environment Layout </h2>

- <b>This is essentially how our Infrastructure is going to be set up, I will be reffering to this diagram later on in the walkthrough </b> 

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

- After finishing the installation, I quickly ran the Oracle VM guest additions in order to get faster response time from the VM, this prompted a reboot.

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

Then I Configured the IP address on the internal network. Right Click Internal Network>Properties> Double Click IPV4, and for this lab I used the given IP ADDRESSING from the diagram. 

![ipadressing](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/4a2769a3-b5c6-4032-9b4e-b4077b4aa8df)

We input the IP Addressing, I am not going to use a Default Gateway because the Domain Controller itself will server as the Default gateway since we have 2 NICs: One on the internet and one on the inside. 
This server will also use itself as a DNS server so for preferred DNS server you can either put the ip address as the DNS or the loopback address(127.0.0.1)

![ipv4](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/cc5e53d9-3078-419e-9475-2eb5834db8d1)

For this step it will require us to reboot our system, I will be renaming this PC to "DC" for "Domain Controller" for clarification purposes. 

![rename 2](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/46ab58eb-d0d9-493e-a6f1-934adc3b9805)

<h2>Step 4 : INSTALL AD DS(Active Directory Domain Services) & create a Domain </h2>

![adds5](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/dd2e5940-2507-4990-9afa-5e4ab6b77ac7)

I will start with Add roles and features, Install server destination. check the box for Active Directory Domain Services

![serverman](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/e0d79ac5-ea0a-4b6d-8ef5-f38240d85372)

Click next until Installation 

![installadds](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/ed40e5c5-951e-4d1b-97a0-2ca6153f4645)

Next we see we get a flag notification "Post-deployment Configuration" this is bc we installed the software for ADDS(Active Directory Domain Services) 
but I havent created the Domain yet. > Promote this server to domain>Add new forest, for domain name you can put anything you want. Set your password> Then Install.

![forest](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/3347cc7d-891d-4386-be4c-b0c1ec889ef7)

![install](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/e244578a-a022-409e-b75d-d81104ae2e89)

After installation it will automatically sign out since Active Directory is installed

Now I will create my own dedicated domain admin account instead of using the built in admin account on windows 

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

<h2>Step 5 : Install RAS/NAT (Remote Access Server/Network address Translation) </h2>
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

This will give our Windows 10 Client an IP address that will get them on the Internet while still being on this private internal network
First 
From Server Manager > Add roles and features > from server roles : DHCP> Add Features > install

![dhcp](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/385229ac-3b31-4eb1-b8d3-c1a3b63cb0af)

After Installation is complete we head to Tools> DHCP> to set up our scope 

The whole purpose of DHCP is to allow Client Computers to automatically get their IP Addresses. From our Diagram we are going to use the range listed. I will be also naming the scope of what the IP Range is 

![scope](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/8d826d04-e582-4bc9-869a-b6319e9c30ee)

I filled out the information with our Range,  and set the subnet mask to 255.255.255.0, For this example I didnt use any Exclusions or Delays, Lease Duration leave at 8 days, and Yes to configure options so our clients can access the internet

![confignew](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/2a62625e-6823-4987-aeda-f7e00faf7105)

From our Network diagram we configured NAT on the domain controller, the DC also has routing configured aswell. Its job is to forwards clients traffic to the internet. Since we configured it that way, the clients will use the Internal NIC of the domain controller as their default gateway. In this example, I am using the DC IP address that has configurations. ADD> NEXT

![hdcpip](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/f7e30d74-7975-46a7-a81f-529901dae8ec)

When you install Active Directory on the DC it automatically installs DNS, so we have to use the DC or we cant join the domain. WINS SERVER NEXT> YES TO ACTIVATE SCOPE> FINISH 

![scope2](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/bde384b2-a4bd-41d8-9df4-70201c17a040)

(You may have to refresh > right click dc> authorize to get it working)

<h2>NON-LISTED STEP: Create Config to Browse Internet on DC</h2>
usually you dont want to do this in a production enviorment, but since we are in a lab it'll be ok. 

- </b>From Server Manager> 1.Configure this local server>Diasable IE Enchanced Security Configurations>Turn Off for both admin and users.  </b>

I am do this because if I attempt to browse the internet while its on, spam of popups will occur.

![Screenshot 2024-04-04 233611](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/4c8f9b9f-96f0-4ac7-adb9-e9d31c8454c0)

<h2>Step 6: Implement Powershell script </h2>

- For this lab I am using this source code for my Powershell Script

[AD_PS-master (1).zip](https://github.com/MustafaCybertests/ActiveDirectoryLab/files/14881361/AD_PS-master.1.zip)

 From the file> Names> and I added my name to the list

![extract rar](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/4b40ecdf-07ef-43e2-b03f-7bf58af9335a)

- START> WINDOWS POWERSHELL > PoweShell ISE normal> rightclick > Run as administrator

 ![ps1](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/deaddc41-a7a5-435b-bd94-b7b5616f90cc)

 - Open script> Select the AD_PS Script Downloaded earlier

   ![scrop1](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/07e9099e-956c-435b-a017-debb2e4fc3ec)

   -If I try to run the script it will give out a security feature, but to get around this I set the Execution Policy to Unrestricted> Yes to all(This is safe because I am in a live virtual enviorment)

   The script essentially sets the passwords to all the Random names I generated, The 2nd part of the script takes the generic password "Password1" and converts it into secure password, the last part is a loop to automate the configurations.

![ls cd](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/14490000-d5cb-4b06-ad6d-6cf795c95cda)

-Before I run the script, I have to go to the actually directory for the script in order for it to work.
From powershell >cd C:\users\a-Alhilal\Desktop\AD_PS-master > ls to confirm names.txt> then I ran the script which created my 1,000 users

![createw](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/d56dc889-1baa-464d-9dc2-57e8aeeb662d)

-While the users are generating we can go to Active Directory> refresh domain> and check the Users our script generated

![users](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/5ccb9b42-46b5-4445-9cc0-d38349602acc)

-Right click Domain in AD> find> find now. We can see our 1,000 users have been sucessfully generated. 

![userss](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/6843ed6f-3274-42f9-a8a2-0af6a9f14486)

<h2>Step 7: Create Windows 10 VM on VirtualBox </h2>

-This will use the Internal NIC, while getting its IP address from the DHCP server I configured, after the fact I will verify.
-1.Minimize the Domain> Go onto Virtual Box> New>Naming it "Client1"


![vIRTUAL](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/044f5a18-6f80-4821-b766-8428de273ee1)

- I gave this VM 2GB of Ram, Settings> Advanced> Turn on Bi-directional Clipboard if I ever need it.
- Then System >Processors:4 >Network:Internal Network
- Put Windows ISO in the bootup> and ran my "Client1" VM 
-When ISO installed>Next> "I dont have windows key"> Install"Windows 10 pro"> Custom Harddrive>Next

![iso install](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/b045c5ce-8647-4e5c-9f71-1001b8b3a667)

-Once installed go through the steps> When it asks to connect to an internet, put I Dont have Internet> set up personal use> Offline accout> Limited access
-This will be our local username, I named it "User" and you dont need a password

![Screenshot 2024-04-05 012732](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/2ee9c751-13f1-4f29-ae8a-8a15cd121dda)

- Choose Privacy settings> Turn all off> Finish Windows10ISO VM installation
I ran the command prompt >ipconfig to make sure our VM is connected to the internet 
![Screenshot 2022-02-15 s](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/755b7c0e-f49a-4762-86cb-8b64a0712773)

- I pinged www.google.com, since it resolved that means our DNS server is working. Which means the whole infrastructure in the diagram is correctly working.
![proper](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/c0967e08-d704-42db-a9ee-365fedfc89c6)
-To join this VM to the domain, first I need to change the PC's name and add it to the domain. 
Right click start> Systems>Rename this PC(advanced) > Change> "Client1" in order to change I had to login to the Admin account I created Earlier> Restart

![dom](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/b329a232-ebfa-42b5-beb3-260b39e90777)

TO CONFIRM WE HAVE ADDED IT TO THE DOMAIN 
-Go back to the DC we can see "Client1" Computer. Tools> DHCP>DC>Scope> Address Leasing 
-It reached out to DHCP server requesting an IP address, and the DHCP automatically assigned it an address


![connecte](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/80e434d4-8318-484b-a09e-cdcdd5c2fb85)

Start> Admin Tools> Active Directory Users and Computers > Computers> I saw the Client Computer has joined the domain. Now we can use any one of the 1,000 generated accounts to login to the Client Computer
- I logged into my account, ran whoami in cmd, and this confirmed I was a member of the domain.

![Screenshot 2024-04-05 015405](https://github.com/MustafaCybertests/ActiveDirectoryLab/assets/155025144/832ef15d-9603-46a0-a529-473827ebf135)

FROM OUR NETWORK DIAGRAM, I CREATED A MINI CORPORATE NETWORK. This Client1 VM is like my corporate laptop, and automatically logs in to it from my corporate credentials since it is already on the domain 

<h2> HOME LAB HAS BEEN COMPLETED SUCCESSFULLY </h2>

