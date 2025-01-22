<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure) 
  <p>
    Part 1: Preparing Active Directory Infrastructure in Azure
  </p></h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1: Create a Domain Controller using Windows Server.
- Step 2: Create a virtual machine that will act as a client.
- Step 3: Set the client to join the domain

<h2>Deployment and Configuration Steps</h2>

<p>
<img width="1221" alt="1" src="https://github.com/user-attachments/assets/52f740e3-10c4-4026-aaa1-aab27cf4869b" />
</p>
<p>
We will first start by making a Resource Group in Azure.  
</p>
<br />

<p>
<img width="1496" alt="2" src="https://github.com/user-attachments/assets/3a5768e7-e8c6-4e79-95dd-8d6ee7f8db1d" />

</p>
<p>
Then, we will create a Virtual Network that will live in the Resource Group that we created. 
</p>
<br />

<p>
<img width="1861" alt="3" src="https://github.com/user-attachments/assets/530ac8d4-c12c-47ae-acf7-5b1b6f2e350a" />
</p>
<p>
Now, we will create the Domain Controller by creating a Windows Server Virtual Machine.  
</p>
<br />

<p>
<img width="1861" alt="4" src="https://github.com/user-attachments/assets/4423a023-8958-4276-8f28-876ad18f4d9a" />
</p>
<p>
Then, we will create another virtual machine but a Windows 10 Virtual Machine that will act like a client by joining the domain later on. 
</p>
<br />

<p>
<img width="1861" alt="5" src="https://github.com/user-attachments/assets/fdfe7245-364f-4888-bc02-ab7c1003707e" />

</p>
<p>
After Virtual Machines are created, we will set Domain Controller’s NIC Private IP address to be static. 
  </p>
  <p>
  This will ensure that the private IP address of the Domain Controller doesn’t change no matter how many times the Virtual Machine restarts. 
    </p>
    <p>
      To do this follow these steps: DC-1(my Windows Server VM’s name) > Networking > Network Settings > Click on the Network Interface [dc - 1575 z3(primary) / ipconfig1 (primary)]. 
    </p>

<br />

<p>
<img width="1861" alt="6" src="https://github.com/user-attachments/assets/2287f5a1-9375-430d-acb8-61e46940d458" />
</p>

<p>
Then, click the name of the IPv4 (ipconfig 1) and change the allocation under Privarte IP Address Settings to 'Static'. 
</p>
<br />

<p>
<img width="1282" alt="7" src="https://github.com/user-attachments/assets/6cdfa09d-6e0d-400e-865a-d55b507d7559" />
</p>
<p>
Now, we will connect to the Domain Controller through the Windows App. 
</p>
<br />

<p>
<img width="559" alt="8" src="https://github.com/user-attachments/assets/d2ac382a-1e9b-46b7-8ebe-5814092cdd8b" />

</p>
<p>
We connected to the Domain Controller to disable the Windows Firewall which will make this lab easier. 
</p>
<p>
  To disable the Windows Firewall: Right Click the Windows button > Run > Type ‘wf.msc’ > OK.  
</p>
<br />

<p>
<img width="1350" alt="9" src="https://github.com/user-attachments/assets/f0def0c8-7cc3-4f56-b700-82e79bb51516" />

</p>
<p>
When you see this window, click 'Windows Defender Firewall Properties' > Turn off Firewall state.  
</p>
<br />

<p>
<img width="1349" alt="10" src="https://github.com/user-attachments/assets/f2c693ad-1a00-4d46-9e58-09efac475858" />

</p>
<p>
What we just did was turn off the Firewall only for the Domain Profile.
</p>
<p>
  We need to do these steps to the Domain Profile, Private Profile, and the Public Profile by clicking on the tabs. 
</p>
<p>
   If done correctly, the Windows Defender Firewall with Advanced Security page should look like this. 
</p>
<br />
<p>
  -
</p>

<p>
Now, we need to set Client-1’s (name of Windows 10 VM) DNS settings to DC-1’s Private IP address. 
</p>
<p>
  This is very important because when a virtual machine is created, it will automatically set the DNS to the DNS server of Azure, but we want the client’s computer to connect to the DNS server of our Domain Controller.  
</p>
<p>
<img width="1861" alt="11" src="https://github.com/user-attachments/assets/ee795fef-4f5e-41e9-93f4-ece2a866b7fc" />

</p>
<p>
  Find DC-1's private IP address by clicking on the name. The private IP address is 10.0.0.4 for me. 
</p>
<br />

<p>
<img width="1861" alt="12" src="https://github.com/user-attachments/assets/61481dc3-ad20-4791-99d0-8ef07bf1c2a9" />
</p>

<p>
Then, go to Client-1 > Networking > Network Settings > Click the name of the Network interface [client-1106 z3 (primary) / ipconfig1 (primary)]. 
</p>
<br />

<p>
<img width="1861" alt="13" src="https://github.com/user-attachments/assets/e2704d25-580f-4111-8b86-a935570dd354" />
</p>
<p>
Under Settings > DNS Servers > Custom > write DC-1's private IP address > Save. This will allow the Client-1 computer to join the domain later.  
</p>
<br />

<p>
<img width="1861" alt="14" src="https://github.com/user-attachments/assets/83e7812b-2d83-4df6-b841-ad5ab27b9ed9" />
</p>
<p>
Restart Client-1 from the Azure Portal. 
</p>
<br />

<p>
<img width="1282" alt="15" src="https://github.com/user-attachments/assets/39f8db49-647c-4159-be22-85c8bb375973" />
</p>
<p>
We will now connect to Client-1 Virtual Machine through the Windows App to make sure the settings are correctly done.  
</p>
<br />

<p>
<img width="1121" alt="16" src="https://github.com/user-attachments/assets/c9892736-774b-47bf-8c28-704dca83ac47" />

</p>
<p>
From the Client-1 Virtual Machine, open PowerShell and ping the DC-1 VM by using the command ‘ping <DC-1's Private IP Address>’ Make sure the ping succeeds.  
</p>
<br />

<p>
<img width="857" alt="17" src="https://github.com/user-attachments/assets/5563e201-181d-4bb6-b445-5f407ae9e6a6" />

</p>
<p>
Also using PowerShell, we will check if the DNS server of Client-1 is Dc-1's private IP address. 
</p>
<p>
To do this use the command ‘ipconfig /all’ and check the DNS server. 
</p>
<p>
Make sure that the IP address matches DC-1's private IP address.
</p>
<br />

If everything looks correct, we will continue on in Part 2!
