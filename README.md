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

- Create Azure Resource Group & Virtual Machines (Linux & Windows)
- Observe ICMP Traffic
- Observe SSH Traffic
- Observe DHCP Traffic
- Observe DNS Traffic
- Observe RDP Traffic

<h2>Actions and Observations</h2>
<p>ICMP</p>
<p>
<img width="1170" alt="Screenshot 2023-09-21 at 12 07 56 PM" src="https://github.com/MariahTali/azure-network-protocols/assets/76408932/76fddc7c-f577-4d88-bf9f-2c463fbcb865"> <br>
<img width="1294" alt="Screenshot 2023-09-21 at 12 18 55 PM" src="https://github.com/MariahTali/azure-network-protocols/assets/76408932/422e4c82-e8cf-4d87-8235-90dd34bb0771"><br>
<img width="1288" alt="Screenshot 2023-09-21 at 12 23 38 PM" src="https://github.com/MariahTali/azure-network-protocols/assets/76408932/8cd764f6-1bcf-483f-b4aa-57d16a058f4d"><br>
</p>
<p>
Create and provision our resources, connect to them via Microsoft Remote Desktop, download Wireshark, send some traffic, and observe.
</p>
<br />

<p>
<img width="1075" alt="Screenshot 2023-09-21 at 12 27 00 PM" src="https://github.com/MariahTali/azure-network-protocols/assets/76408932/a064f543-2f07-467e-8839-4c33b85c1721"><br>
<img width="1060" alt="Screenshot 2023-09-21 at 12 29 36 PM" src="https://github.com/MariahTali/azure-network-protocols/assets/76408932/60d72f1e-e5df-4397-8db8-6bc295ac6041"><br>
<img width="1136" alt="Screenshot 2023-09-21 at 12 37 13 PM" src="https://github.com/MariahTali/azure-network-protocols/assets/76408932/a72422d5-f2fe-4967-b06a-3f85ccff920a">
<br>
</p>
<p>
Filter traffic for ICP (internet control messaging protocol, ping uses this), there will be no traffic until we ping the second virtual machine using its private IP address (10.0.0.5 in this instance). Ping google and observe the response of alphabet.
</p>
<br />
<p>Network Firewall</p>
<p>
<img width="1138" alt="Screenshot 2023-09-21 at 12 45 55 PM" src="https://github.com/MariahTali/azure-network-protocols/assets/76408932/e4af319b-fcd5-4513-895e-d8fe689d6db9"><br>
</p>
<p>
In Powershell ping the second virtual machine(VM2) "ping 10.0.05 -t" so that we can change the VM2 firewall to disallow icmp traffic to come through. We should observe the halting of icmp traffic.
</p>
<br />
<p>
</p><img width="1636" alt="Screenshot 2023-09-21 at 12 55 41 PM" src="https://github.com/MariahTali/azure-network-protocols/assets/76408932/bccd47bd-3e8e-4442-aa3d-0974cc1adfde"><br>
<img width="1162" alt="Screenshot 2023-09-21 at 12 57 54 PM" src="https://github.com/MariahTali/azure-network-protocols/assets/76408932/881150e8-69f6-42aa-819e-9e5b2f45e28c">
<p>
Go to Vm2's Network Security Group in Azure portal. Add a rule that denies icmp traffic. Observe "Request timed out" on VM2. You can "allow" icmp and observe the continuation of pinging to VM2 from VM1.
</p>
<br />
<p> SSH</p>
<p>
</p><img width="1135" alt="Screenshot 2023-09-21 at 1 03 49 PM" src="https://github.com/MariahTali/azure-network-protocols/assets/76408932/9b9510e4-c589-48df-9283-511af5b84b0c"><br>
<img width="1134" alt="Screenshot 2023-09-21 at 1 06 16 PM" src="https://github.com/MariahTali/azure-network-protocols/assets/76408932/b4c5da06-4ceb-422a-aa05-6b3ac58662ec">
<p>
In wireshark filter for SSH, observe no responses. SSH into VM2 from VM1 via PowerShell:<br>
"ssh (Vm username you created)@(VM2privateIPaddress)". Note: When you say "yes" to continue connecting, just type your password even though it doesn't show and hit enter. 
</p>
<br />

<p>
<img width="944" alt="Screenshot 2023-09-21 at 1 10 34 PM" src="https://github.com/MariahTali/azure-network-protocols/assets/76408932/e7f250d0-7300-4e5b-b417-b6eefc4c2e02"><br>
<img width="936" alt="Screenshot 2023-09-21 at 2 00 46 PM" src="https://github.com/MariahTali/azure-network-protocols/assets/76408932/6dcc95d1-85f4-4e0a-84cf-00f66f9e41f7">
</p>
<p>
Observe access to VM2 once logged in. Use some Linux commands to verify connection. Type Linux command 
"exit" to close ssh connection.
</p>
<br />
<p>DHCP</p>
<p>
<img width="1149" alt="Screenshot 2023-09-21 at 2 09 52 PM" src="https://github.com/MariahTali/azure-network-protocols/assets/76408932/4e8ee675-6191-4594-a298-52a5843c8454">
</p>
<p>
DHCP is used to automatically assign your IP address. Force renewal of IP address by using the command "ipconfig /renew", the connection will be momentarily lost in Remote Desktop viewing, but should reconnect. Observe IP address alteration.
</p>
<br />
<p>DNS</p>
<p>
<img width="1139" alt="Screenshot 2023-09-21 at 2 14 49 PM" src="https://github.com/MariahTali/azure-network-protocols/assets/76408932/1e394014-7bf2-4c15-bf4f-be8bbfa7f758">
</p>
<p>
Observe DNS traffic by asking the DNS server what the IP address is for any given hostname ex. google.
<br />

<p>
RDP
</p>
<p>
<img width="1137" alt="Screenshot 2023-09-21 at 2 17 30 PM" src="https://github.com/MariahTali/azure-network-protocols/assets/76408932/1700e4a2-6e1e-4c1d-9646-71d38e1f121d">
</p>
<p>There should already be RDP traffic because we are connected via Microsoft Remote Desktop. As we interact on our local computer, it interacts with our virtual machines.</p>
<br />
