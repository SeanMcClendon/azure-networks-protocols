<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />




<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Create 2 Virtual Machines
- Manipulate Firewalls
- Observe traffic over different ports

<h2>Actions and Observations</h2>

<p>
<h3>Part 1(Creating our Resources)</h3>

1. Go to Azure Portal (https://portal.azure.com/)
2. Create a Resource Group: Name it RG-Lab-02, select region of your choice
3. Create a Windows 10 Virtual Machine (VM):
While creating the VM, select the previously created Resource Group,as well as region. Select Windows 10 for Image, For size choose 2 cpu's. Pick username and Password of your choice. Click Licensing box, then next Review and create, create. 
While creating the VM, allow it to create a new Virtual Network (Vnet) and Subnet
4. Create a Linux (Ubuntu) VM: While create the VM, select the previously created Resource Group, Name it VM-2, instead of SSH key create your own username and password. Next to networking, ensure it is using the existingg vnet. Review and create. 
5. Observe 2 of everything being created inside your Resource Group (2 vm, 2 NIC, 2 networksecurity groups etc.)

<h3>Part 2 (Observe ICMP Traffic)</h3>

6. Use Remote Desktop to connect to your Windows 10 Virtual Machine using public IP address, signing in with previously made username. password. 
7. Once connected, type in "download Wireshark" in your web browser, select Windows Installer

![image](https://github.com/SeanMcClendon/azure-networks-protocols/assets/142221948/f5ee1683-ee8b-4ada-b304-ce48d8cdfea4)

8. open file, Install choosing defaults.
9. Open Wireshark
10. Select ethernet, then click on the blue shark fin to start capturing packets

![image](https://github.com/SeanMcClendon/azure-networks-protocols/assets/142221948/60b62f30-a6fb-4b81-99b8-b866183dd75e)

11. filter traffic by typing in the search bar "ICMP" nad press enter

![image](https://github.com/SeanMcClendon/azure-networks-protocols/assets/142221948/3926183e-df18-49d7-ba53-2c1657be87da)

12. Minimize VMM-1, find Private IP Address for VM-2.
13. Open back VM-1 and Ping VM-2 from VM-1 using Power Shell

![image](https://github.com/SeanMcClendon/azure-networks-protocols/assets/142221948/9321fcb2-9c4f-449d-ad3e-f7eb78a2d642)


14. Notice WireShark Capturing data over ICMP (ping)

![image](https://github.com/SeanMcClendon/azure-networks-protocols/assets/142221948/bc3b403d-dec1-474d-a051-0165763efd21)

15. FOr further observation of utility, from PowerShell, Ping "www.google.com -4". (the -4 indicates IPV4)

![image](https://github.com/SeanMcClendon/azure-networks-protocols/assets/142221948/e488396b-5756-4e46-b7d4-a33486663239)

16. Below you can see VM-1 request data from Google's IP Address, then Google responding.

![image](https://github.com/SeanMcClendon/azure-networks-protocols/assets/142221948/aeb02195-5fc2-42e6-9da9-31ed7507b901)

17. Refresh Wireshark by hitting the greeen fin, Continue without saving to clear data.

![image](https://github.com/SeanMcClendon/azure-networks-protocols/assets/142221948/e7845f86-c998-44c1-8d95-de1adaa5c9c2)


18. Perpetually ping VMV-2

![image](https://github.com/SeanMcClendon/azure-networks-protocols/assets/142221948/4d931411-7012-4817-a77a-55516aab9b5f)

19. Go back to VM-2 Network Security Group

![image](https://github.com/SeanMcClendon/azure-networks-protocols/assets/142221948/c26061c4-6724-461d-af69-8001c6ead0d5)

20. Block pings from VM-1 Inside VM-2 NSG: choose Inbound security rules->Add->ICMP->Deny->200->Name Deny ICMP from anwhere->add

![image](https://github.com/SeanMcClendon/azure-networks-protocols/assets/142221948/f7d77b49-679e-435b-904f-2c78caa8f9e7)


21. Observe Powerhell Timed out an WireShark is no longer receiving replies.

![image](https://github.com/SeanMcClendon/azure-networks-protocols/assets/142221948/d6d141a2-e8c2-4a0f-86c7-e0e8580f8bcc)

22. Go back to VM-2 NSG and Allow ICMP traffic from the rule we just created, ultimately undoing what we jjust accomplished. 
23. Replies have begun again

![image](https://github.com/SeanMcClendon/azure-networks-protocols/assets/142221948/d122cb11-3849-444e-9975-d4344e3c5408)
25. inPowerShell press cntrl+C to stop Pings

<h3>Part 2 (Observe SSH Traffic)</h3>

25. Back in Wireshark, filter for SSH traffic only
26. SSH into VM-2 from VM-1: In SSH type "SSH (username)@(VM-2 Private IP Address)". Type "yes"  enter password (you wont be able to see the characters)

![image](https://github.com/SeanMcClendon/azure-networks-protocols/assets/142221948/d9b464da-d153-41e6-8cc8-1cf2f93462f3)

27. Connection into VM-2 successful as seen here:

![image](https://github.com/SeanMcClendon/azure-networks-protocols/assets/142221948/0c0a2d24-66e4-4827-bb78-98872e2e6c3e)

28. Observe SSH traffic

![image](https://github.com/SeanMcClendon/azure-networks-protocols/assets/142221948/7444520b-c5de-4475-bfd3-5c47952307a4)

29. Exit the SSH connection by typing ‘exit’ and pressing [Enter]


<h3>Part 2 (Observe DHCP Traffic)</h3>

30. Back in Wireshark, filter for DHCP traffic only
31. From your Windows 10 VM, attempt to issue your VM a new IP address from the command line (ipconfig /renew)
32. Observe the DHCP traffic appearing in WireShark

![image](https://github.com/SeanMcClendon/azure-networks-protocols/assets/142221948/30625ba9-43cf-419f-b062-f331d5091789)

<h3>Part 2 (Observe DNS Traffic)</h3>

33. Back in Wireshark, filter for DNS traffic only
34. From your Windows 10 VM within a command line, use nslookup to see what google.com and disney.com’s IP addresses are
35. Observe the DNS traffic being show in WireShark

![image](https://github.com/SeanMcClendon/azure-networks-protocols/assets/142221948/f2fc6bfa-7a53-4283-9cf1-d69662f65897)

<h3>Part 2 (Observe RDP Traffic)</h3>

36. Back in Wireshark, filter for RDP traffic only (tcp.port == 3389)
37. Oserve the immediate non-stop spam of traffic:

<h2>Why do you think it’s non-stop spamming vs only showing traffic when you do an activity?<h2>
Answer: because the RDP (protocol 3389) is constantly showing you a live stream from one computer to another, therefor traffic is always being transmitted

</p>
<br />

<p>

</p>
<br />
