# Rivan Home Labs – Hands-On CCNA Training Environment

Welcome to **Rivan Home Labs**, a hands-on learning environment designed to elevate your Cisco networking skills. This course provides each student with real physical equipment, including routers, IP phones, wireless access points, and cameras — the ultimate toolkit for mastering the **Cisco Certified Network Associate (CCNA)**.

---

## 📦 Equipment Provided

Each student receives a home lab kit containing:

- **Cisco Unified Communications Manager** (Cisco 1800 Series Router)
- **Cisco IP Phones** (8945 models)
- **IP Camera**
- **Wireless Access Point** (Cisco Airnet 2700)
- All required **Ethernet cables, power adapters**, and **console cables**

---

## 🎯 Course Goals

By completing the Rivan Home Labs course, students will:

- Build and configure real Cisco networks at home
- Practice CLI commands on physical routers and switches
- Implement VoIP using Cisco IP phones
- Set up and secure wireless networks
- Integrate and manage IP cameras for security use cases
- Prepare for the **CCNA certification exam** with confidence and real experience

---

## 🛠️ What You'll Learn

| Module | Topics Covered |
|--------|----------------|
| **1. Lab Setup & Cable Management** | Unboxing, safety, cable labeling, device overview |
| **2. Router Configuration (Cisco 1800)** | Initial setup, IOS basics, interface config, routing protocols |
| **3. Switching & VLANs** | Switch basics, VLAN configuration, trunking |
| **4. IP Telephony** | IP Phone registration, CUCM basics, call flow |
| **5. IVR & Voice Features** | Setting up basic IVR menus, auto-attendants, call handling logic |
| **6. Linphone Integration** | SIP softphone setup, connecting Linphone with CUCM or Asterisk, testing SIP endpoints |
| **7. IP Camera Integration** | Basic IP camera setup, network configuration |
| **8. Wireless Networking** | Setting up AP using Python script |

---

## 📚 Prerequisites

- Basic networking knowledge (OSI model, TCP/IP, subnetting).
- Familiarity with Cisco Packet Tracer or GNS3 is helpful, but not required.
- Git and Python installed on your pc.

---

## 🔧 Lab Environment Setup

1. **Connect the hardware**: Refer to the printed setup guide included in your kit or view the digital [Setup Guide PDF](Enter.pdf).
2. **Access router CLI** via console cable using:
   - PuTTY(Free) or SecureCRT(License)
3. **Power on all devices**, verify LED indicators, and begin configuration.

---
## 📑 Table of Contents
- [🛠️ Task 1 – Setup & Cable Management](#️-task-1--setup--cable-management)  
- [🔧 Task 2: Access console using SecureCRT](#-task-2--access-console-using-securecrt)  
- [⚡ Task 3 – Check Power Inline Status](#-task-2-check-power-inline-status)  
- [🌐 Task 4 – VLAN Checking and Configuration](#-task-3-vlan-checking-and-configuration)  
- [🔄 Task 5 – LAN/Ethernet Ports to VLAN Assignment](#-task-4-lanethernet-ports-to-vlan-assignment)  
- [🖧 Task 6 – Switch VLAN Interface (SVI)](#-task-5-switch-vlan-interface-svi)  
- [🖥️ Task 7 – Prepare the DHCP Server](#️-task-6-prepare-the-dhcp-server)  
- [🎥 Task 8 – IP Camera Reserved IP](#-task-7-ip-camera-reserved-ip)  
- [☎️ Task 9 – Super Call Center Setup](#-task-8-super-call-center-setup)  
- [📱 Task 10 – WiFi Setup using Python](#-task-9---wifi-setup-using-python)  
- [📞 Task 11 – Cellphone to IP Phone Connection](#-task-10---cellphone-to-ip-phone-connection)

---

## 🏁 Ready to Start?

Let’s get building. Power on your lab and begin with **Task 1 – Setup & Cable Management** 🚀  
Stay consistent, stay curious. Rivan Home Labs is here to turn theory into real-world skills.

## 🛠️ Task 1 – Setup & Cable Management

Before configuring your devices, ensure all hardware is connected correctly. Refer to the Cabling Instructions PDF for step-by-step guidance on connecting IP Phones, Cameras, and Wireless APs to the appropriate ports. Once everything is set up, power on the devices and proceed with the configuration.

[Basic Setup Guide PDF](Enter.pdf)

## 🔧 Task 2: Access console using SecureCRT
Open SecureCRT and click the quick connect button 
<br><br>
![{1E3424F6-F144-4CF8-AE66-551B83A34FBE}](https://github.com/user-attachments/assets/b9db7d7d-1623-4759-b294-cb7664901495)
<br><br>
Select `Serial` as Protocol
<br><br>
![image](https://github.com/user-attachments/assets/64ec6f89-c5af-462a-a041-e8106efa2cfe)
<br><br>
For Port select the `USB-SERIAL` and `9600` Baud rate
<br><br>
![image](https://github.com/user-attachments/assets/8813ba8d-b855-46e1-b7c8-7121ee029112)

## ⚡ Task 3: Check Power Inline Status

Use the `show power inline` command to check the power status of each port. This is important for verifying that devices like IP phones and cameras are receiving Power over Ethernet (PoE).


### Example Output:

```bash
CallCenter-28#show power inline
PowerSupply   SlotNum.   Maximum   Allocated       Status
-----------   --------   -------   ---------       ------
INT-PS           0        88.000    28.800          PS GOOD

Interface   Config   Device   Powered    PowerAllocated
---------   ------   ------   -------    --------------
Fa0/1/0     auto     Unknown  Off        0.000 Watts  
Fa0/1/1     auto     IEEE-2   On         7.000 Watts  
Fa0/1/2     auto     Unknown  Off        0.000 Watts  
Fa0/1/3     auto     IEEE-2   On         6.400 Watts  
Fa0/1/4     auto     Unknown  Off        0.000 Watts  
Fa0/1/5     auto     IEEE-4   On         15.400 Watts  
Fa0/1/6     auto     Unknown  Off        0.000 Watts  
Fa0/1/7     auto     Unknown  Off        0.000 Watts
```
## 🌐 Task 4: VLAN Checking and Configuration

Set up VLANs to segment the network. Use the 'show vlan-switch' commands to verify VLANs. Create at least 4 VLANs for different network sections.

```bash
config t
vlan 1
name default
vlan 10
name RIVANWIFI
vlan 50
name RIVANVIDEO
vlan 100
name RIVANVOIP
end
show vlan-switch
```
## 🔄 Task 5: LAN/Ethernet Ports to VLAN Assignment
Assign Ethernet ports to specific VLANs for proper network organization. This includes both data and voice VLANs.

```bash
config t
interface FastEthernet0/1/1 
 switchport mode access
 switchport voice vlan 100
interface FastEthernet0/1/3 
 switchport mode access
 switchport access vlan 50
interface FastEthernet0/1/5 
 switchport mode access
 switchport access vlan 10
interface FastEthernet0/1/7 
 switchport mode access
 switchport access vlan 1
end
show vlan-switch
```

## 🖧 Task 6: Switch VLAN Interface (SVI)
Create Switch Virtual Interfaces (SVIs) for each VLAN. These interfaces provide routing capability between VLANs.
```bash
config t
Int Vlan 1
 no shutdown
 ip add 10.28.1.1 255.255.255.0
 description DEFAULT
Int Vlan 10
 no shutdown
 ip add 10.28.10.1 255.255.255.0
 description RIVANWIRELESS
Int Vlan 50
 no shutdown
 ip add 10.28.50.1 255.255.255.0
 description RIVANCAMS
Int Vlan 100
 no shutdown
 ip add 10.28.100.1 255.255.255.0
 description RIVANVOIP
end
show ip interface brief
```

## 🖥️ Task 7: Prepare the DHCP Server
Set up DHCP pools for different VLANs and configure exclusions for reserved IPs.
```bash
config t
ip dhcp Excluded-address 10.28.1.1 10.28.1.100
ip dhcp Excluded-address 10.28.10.1 10.28.10.100
ip dhcp Excluded-address 10.28.50.1 10.28.50.100
ip dhcp Excluded-address 10.28.100.1 10.28.100.100
ip dhcp pool DEFAULT
   network 10.28.1.0 255.255.255.0
   default-router 10.28.1.1
   domain-name DEFAULT.COM
   dns-server 10.28.1.10
ip dhcp pool RIVANWIFI
   network 10.28.10.0 255.255.255.0
   default-router 10.28.10.1
   domain-name RIVANWIFI.COM
   dns-server 10.28.1.10
ip dhcp pool RIVANCAMS
   network 10.28.50.0 255.255.255.0
   default-router 10.28.50.1
   domain-name RIVANCAMS.COM
   dns-server 10.28.1.10
ip dhcp pool RIVANVOIP
   network 10.28.100.0 255.255.255.0
   default-router 10.28.100.1
   domain-name RIVANVOIP.COM
   dns-server 10.28.1.10
   option 150 ip 10.28.100.1
   end
```

## 🎥 Task 8: IP Camera Reserved IP
Configure reserved IP addresses for devices like security camera that require static IPs.
```bash
sh mac-address-table 
```
> If `FA 0/1/3` not showing try to ping it
```bash
ping 10.28.50.8
sh mac-address-table 
```
Copy the mac address of IP Camera on port `FastEthernet 0/1/3` and paste it on client identifier on this code:
```bash
config t
ip dhcp pool SECURITYCAMERA
 host 10.28.50.8 255.255.255.0
 client-identifier ____.____.____ #change to mac address of IP Camera (FastEthernet 0/1/3)
 default-router 10.28.50.1
end
```
## ☎️ Task 9: Super Call Center Setup
Configure telephony service and assign phones in the call center, ensuring each has a unique extension number.
```bash
config t   
no telephony-service
telephony-service
   no auto assign
   no auto-reg-ephone
   max-ephones 5
   max-dn 20
   ip source-address 10.28.100.1 port 2000
   create cnf-files
ephone-dn 1
  number 2811
ephone-dn 2
  number 2822
ephone-dn 3
  number 2833
ephone-dn 4
  number 2844
ephone-dn 5
  number 2855
ephone-dn 6
  number 2866
ephone-dn 7
  number 2877
ephone-dn 8
  number 2888
 ephone-dn 9
   number 2899
ephone-dn 10
 number 2898
Ephone 1
  Mac-address ____.____.____ #change to mac address of IP Phone (FastEthernet 0/1/1)
  type 8945
  button 1:8 2:7 3:6 4:5
  restart
end
```
> If telephone does not recieve number paste it again

## 📱 Task 10 - WIFI Setup using Python
Automate your wireless access point setup by using a Python script to configure SSID, password, and VLAN tagging through `netmiko`.

On your PC open terminal and try to ping your Access Point (10.28.10.3)
```bash
ping 10.28.10.3
```
Install netmiko library 
```bash
python -m pip install netmiko
```
If its connected clone my wifi script using this command 
```bash
git clone https://github.com/yyynnot/Take-Home-Lab.git
```
Open it using VS Code and edit `autoAP-jsn.json` Replace hostname, ssid, and wifi-pass.
After editing run `autowifi-jsn.py` and you can now connect to it using your mobile phone.

## 📱 Task 11 - Cellphone to IP Phone Connection
Configure SIP settings to enable communication between mobile phones and IP phones.
> Follow this SIP Guide first
> [SIP Guide PDF](Enter.pdf)
```bash
conf t
 voice service voip
  allow-connections h323 to sip
  allow-connections sip to h323
  allow-connections sip to sip
  supplementary-service h450.12
 sip
   bind control source-interface fa0/0
   bind media source-interface fa0/0
   registrar server expires max 600 min 60
!
 voice register global
  mode cme
  source-address 10.28.100.1 port 5060
  max-dn 12
  max-pool 12
  authenticate register
  create profile sync
 voice register dn 1
   number 2823
   allow watch
   name 2823
 voice register dn 2
   number 2824
   allow watch
   name 2824
!
  voice register pool 1
    id mac ____.____.____ #enter your mobile phone mac address
    number 1 dn 1
    dtmf-relay sip-notify
    username 2823 password 2823
    codec g711ulaw
```

---

## 💡 Tips for Success

- Keep your devices labeled and workspace organized.
- Practice configurations multiple times.
- Take screenshots or notes as you work – they’ll help during troubleshooting.

---

## 📞 Support & Contact

If you encounter hardware issues or need assistance:

- Email: [`teamrivan@rvci.org`](mailto:teamrivan@rvci.org)
- Office Hours: Mon–Fri, 9:00 AM – 4:30 PM
- Website: <a href="https://rivanit.com/" target="_blank">Rivan IT</a>

---
