# ⚙️ Rivan Home Labs Configuration
A step-by-step guide for setting up and configuring a home lab network, including IP phones, access points, security cameras, and automation scripts.

---

[![GitHub Stars](https://img.shields.io/github/stars/yyynnot/Take-Home-Lab.svg?style=social)](https://github.com/yyynnot/Take-Home-Lab/stargazers)

## 🔌 Task 1 - Device Configuration and Connection Setup
Below is a diagram showing the connection setup:

![{5802C41E-B0D1-43ED-A2B7-9B83F70A6C26}](https://github.com/user-attachments/assets/1f579574-5384-4dbb-9df6-c0973ae0af26)

This table provides a detailed overview of the devices connected to the router along with their respective ports, device types, and descriptions.
| Switch Port | Device Type     | Description        | Image                                |
|-------------|-----------------|--------------------|--------------------------------------|
| 0/1/1       | IP Phone        | Cisco IP Phone     | <img src="https://github.com/user-attachments/assets/86502828-cd0f-484c-916d-636870ab9a11" width="150" height="100"> |
| 0/1/3       | IP Camera       | Security Camera    | <img src="https://github.com/user-attachments/assets/69434466-979c-4488-8c11-e1983d3d108b" width="150" height="100"> |
| 0/1/5       | Access Point    | Wireless AP        | <img src="https://github.com/user-attachments/assets/58fbc216-ae27-401b-891d-1239e80b4a95" width="150" height="100"> |
| 0/1/7       | PC              | UDP Cable to PC    | <img src="https://github.com/user-attachments/assets/4f45d0d8-6544-4692-a042-7e89f30f1350" width="150" height="100"> |
| Console     | Console Cable   | Console Cable to PC| <img src="https://github.com/user-attachments/assets/d130f8b7-338d-462f-86c1-c9cf659e95d7" width="150" height="100"> |

## 🌐 Task 2 - Change your Windows PC Ethernet IP manually
`Win + R` to open run command and enter `ncpa.cpl` then enter <br><br>
![{86C5C063-7B66-48D0-9447-3880105168E0}](https://github.com/user-attachments/assets/f62c64ef-285d-4615-8e09-1ea5ed1fc643)

Click `Ethernet > Properties > Internet Protocol Version 4 (TCP/IPv4)` <br> <br>
>IP address: 10.28.1.10 <br>
>Subnet mask: 255.255.255.0 <br>
>Default gateway: 10.28.1.1 <br>

<br>
<img src="https://github.com/user-attachments/assets/ac2f22e3-defc-4f2e-9bc0-9df24f51b7c4">

## 🔧 Task 3: Access console using SecureCRT
Open SecureCRT and click the quick connect button 
<br><br>
<img src=https://github.com/user-attachments/assets/b9db7d7d-1623-4759-b294-cb7664901495 width="400">
<br><br>
Select `Serial` as Protocol
<br><br>
<img src=https://github.com/user-attachments/assets/64ec6f89-c5af-462a-a041-e8106efa2cfe width="400">
<br><br>
For Port select the `USB-SERIAL` and `9600` Baud rate
<br><br>
![image](https://github.com/user-attachments/assets/8813ba8d-b855-46e1-b7c8-7121ee029112)

## ⚡ Task 4: Check Power Inline Status

Use the `show power inline` command to check the power status of each port. This is important for verifying that devices like IP phones and cameras are receiving Power over Ethernet (PoE).

```
show power inline
```
**Example Output:**
```
CallCenter-28#
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
## 🌐 Task 5: VLAN Checking and Configuration
**💡 Why Do Companies Need VLANs?**
>Virtual LANs (VLANs) allow organizations to segment a physical network into multiple logical networks. <br>

This improves:
- **Security** – Isolates sensitive data traffic (e.g., VoIP or video streams)
- **Performance** – Reduces broadcast domains, improving network efficiency
- **Manageability** – Simplifies network management through logical grouping

---

Set up VLANs to segment the network. Use the 'show vlan-switch' commands to verify VLANs. Create at least 4 VLANs for different network sections.

```
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
Output: <br><br>
![{4BDFCEDC-7687-4E9D-BF20-47B11402F62A}](https://github.com/user-attachments/assets/2f87713b-3373-4746-8546-a0c390f12e6e)


## 🔄 Task 6: LAN/Ethernet Ports to VLAN Assignment
Assign Ethernet ports to specific VLANs for proper network organization. This includes both data and voice VLANs.

```
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
Output: <br><br>
![{A37D31FC-C077-43CB-B2D9-B351CDE2ADC5}](https://github.com/user-attachments/assets/23b1109c-6970-4dd5-ba00-3d11f95373df)


## 🖧 Task 7: Switch VLAN Interface (SVI)

**📖 Concept Overview**

- **VLANs** act like **folders** that logically separate different types of traffic.
- **SVIs** act like **names for those folders** to help the switch identify and route between them.
- Each VLAN should have a corresponding SVI with an IP address for network management and Layer 3 communication.

Create Switch Virtual Interfaces (SVIs) for each VLAN. These interfaces provide routing capability between VLANs.

```
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
Output: <br><br>
![{E1245459-4A41-4E80-ACD2-EFDA11F5B58D}](https://github.com/user-attachments/assets/1b09e3b5-f5e5-4d2f-9af3-55ba0a1c25ae)


## 🖥️ Task 8: Prepare the DHCP Server
**💡 What is DHCP?**

> **DHCP (Dynamic Host Configuration Protocol)** is a network protocol that automatically assigns IP addresses, subnet masks, default gateways, and other configuration information to devices on a network.

**✅ Why DHCP is Important?**

- **Automation**: Reduces manual IP configuration errors.
- **Efficiency**: Saves time in large environments.
- **Centralized Management**: All IP assignments can be managed from one DHCP server.
- **Dynamic Allocation**: IPs are reused when devices disconnect.

Set up DHCP pools for different VLANs and configure exclusions for reserved IPs.
```
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

## 🎥 Task 9: IP Camera Reserved IP
Configure reserved IP addresses for devices like security camera that require static IPs.
```
sh mac-address-table 
```
Output:<br><br>
![{AA524F57-6646-4947-AC85-D7E5AE8B2C52}](https://github.com/user-attachments/assets/ae11ed75-4eed-4bca-b508-98caa9a5b6a1)

> If `FastEthernet 0/1/3` is not showing try to ping it
```
ping 10.28.50.8
sh mac-address-table 
```
Output:<br><br>
![{D21BBD54-0AE4-4535-9195-866EA98FBFFF}](https://github.com/user-attachments/assets/89ed29db-32d0-41e8-a84f-c97223f22be5)
> `FastEthernet 0/1/3` is now showing copy the mac address

Copy the mac address of IP Camera on port `FastEthernet 0/1/3` and paste it on client identifier on this code:
```
config t
ip dhcp pool SECURITYCAMERA
 host 10.28.50.8 255.255.255.0
 client-identifier ____.____.____ !change to mac address of IP Camera (FastEthernet 0/1/3)
 default-router 10.28.50.1
end
```
Output:<br><br>
![{3C89A641-D974-47CA-B976-AF75FEE5443A}](https://github.com/user-attachments/assets/14d22e75-3780-421b-be35-f77989b10c42)

Try to ping the camera IP on your Windows CMD 
```powershell
ping 10.28.50.8
```
If your PC is able to ping the camera try to open any browser and go to `10.28.50.8`
>If you can ping the camera but cannot access the web GUI enter this on your CMD:
```
route add 10.28.0.0 mask 255.255.0.0 10.28.1.1
```
Go to `10.28.50.8` again and check if you can access the camera.

## ☎️ Task 10: Super Call Center Setup
**Configure telephony service and assign phones in the call center, ensuring each has a unique extension number.** <br><br>
First we need the MAC address of your IP Phone 
```
sh mac-address-table 
```
Output:<br><br>
![{AA524F57-6646-4947-AC85-D7E5AE8B2C52}](https://github.com/user-attachments/assets/ae11ed75-4eed-4bca-b508-98caa9a5b6a1)<br>
>The IP Phone MAC address is FastEthernet0/1/1 <br>
>Edit the config below and input your IP Phone MAC address

```
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
  Mac-address ____.____.____ !change to mac address of IP Phone (FastEthernet 0/1/1)
  type 8945
  button 1:8 2:7 3:6 4:5
  restart
end
```
> If its still not working Go to `Settings > Administrator Settings > Reset Settings > All Settings > Reset` then paste the command again <br>

**IVR (Interactive Voice Response) Commands**
```
config t
dial-peer voice 69 voip
 service rivanaa out-bound
 destination-pattern 2869 #this is the number to dial to talk to IVR
 session target ipv4:10.28.100.1
 incoming called-number 2869
 dtmf-relay h245-alphanumeric
 codec g711ulaw
 no vad
!
telephony-service
 moh "flash:/en_bacd_music_on_hold.au" !on hold music 
!
application
 service rivanaa flash:app-b-acd-aa-3.0.0.2.tcl
  paramspace english index 1        
  param number-of-hunt-grps 2
  param dial-by-extension-option 8
  param handoff-string rivanaa
  param welcome-prompt flash:en_bacd_welcome.au
  paramspace english language en
  param call-retry-timer 15
  param service-name rivanqueue
  paramspace english location flash:
  param second-greeting-time 60
  param max-time-vm-retry 2
  param voice-mail 1234
  param max-time-call-retry 700
  param aa-pilot 2869
 service rivanqueue flash:app-b-acd-3.0.0.2.tcl
  param queue-len 15
  param aa-hunt1 2800
  param aa-hunt2 2877
  param aa-hunt3 2801
  param aa-hunt4 2833
  param queue-manager-debugs 1
  param number-of-hunt-grps 4
```

After configuring IVR dial `2869` for automated response

## 📱 Task 11 - Cellphone to IP Phone Connection
Configure SIP settings to enable communication between mobile phones and IP phones.
> First we need the mac address of your mobile phone

**For Android Devices:** <br><br>

Go to **Settings > My Phone/About Phone**<br><br>
<img src=https://github.com/user-attachments/assets/a7f9c11f-e2b5-4125-bd8a-c7382bfed1f0 height=700>
<br><br>

Scroll down and click on **More Information**<br><br>
<img src=https://github.com/user-attachments/assets/fc4accae-0248-40d2-8f14-d5f727a53d86 height=700>
<br><br>

Select **Phone Information**<br><br>
<img src=https://github.com/user-attachments/assets/ebea13b0-9e1a-473b-88ac-e39e3bb2e294 width=350>
<br><br>

Scroll down and find **Wi-Fi MAC address**<br><br>
<img src=https://github.com/user-attachments/assets/60f09e52-9263-4f4c-ab40-e00f54b7af6f height=700>
---

**For Apple Devices:** <br><br>
Go to **Settings > General**<br><br>
<img src=https://github.com/user-attachments/assets/8c401a8f-4a69-4e4d-90e4-7dcfcfb8c298 height=700>

Click **About**<br><br>
<img src=https://github.com/user-attachments/assets/78c80cc8-d23c-4554-8ac2-75e9bc743007 height=700>
<br><br>

Scroll down and you will see **WIFI Address** that is your MAC address<br><br>
<img src=https://github.com/user-attachments/assets/54774a12-40c3-4f99-8aab-79127d8661bb height=700>
<br><br>
>Edit the config below and input your phone MAC address
```
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
    id mac ____.____.____ !enter your mobile phone mac address
    number 1 dn 1
    dtmf-relay sip-notify
    username 2823 password 2823
    codec g711ulaw
```
---

## 📱 Task 12 - Download Linphone
**For Android Devices:** <br><br>
[Linphone for Android](https://play.google.com/store/apps/details?id=org.linphone&hl=en)<br><br>
<img src=https://github.com/user-attachments/assets/d936482b-eeca-4a81-8acc-c89b936d89a7 height=700>

**For Apple/IOS Devices:**<br><br>
[Linphone for Apple](https://apps.apple.com/us/app/linphone/id360065638)<br><br>
<img src=https://github.com/user-attachments/assets/6565a602-1bdc-4336-8c05-ce7457b8d31f height=700>

## 📶 Task 13 - WIFI Setup using Python
Automate your wireless access point setup by using a Python script to configure SSID, password, and VLAN tagging through `netmiko`.
> Before proceeding, ensure that Python is installed on your PC. <br>
> If you don't have Python installed, follow this tutorial: <a href="https://www.geeksforgeeks.org/how-to-install-python-on-windows/" target="_blank">How to install Python on Windows</a>

We need to install the `netmiko` Python module using the following command:
```bash
python -m pip install netmiko
```
or 
```bash
py -m pip install netmiko
```

After installing Netmiko, open the terminal on your PC and try pinging your Access Point at `10.28.10.3`.
```bash
ping 10.28.10.3
```
Output: <br><br>
![{B74A5D42-459C-4342-A050-D875B6FE57B7}](https://github.com/user-attachments/assets/e52b69e4-b40f-4cbd-9792-98aadfc0f1b2)<br><br>
If the ping is successful, it means your PC is connected to the Access Point.

Next, open the Command Prompt (CMD) on your PC, then copy and paste the following command to download the Python script from the repository:
```
git clone https://github.com/magdeil/autowifi.git
```
Open Visual Studio Code, then go to the autowifi folder. Inside that folder, find and open the file named `autoAP-jsn.json`.
Once it's open, you can update the `ssid` and `wifi-pass` values with your own Wi-Fi details.
However, if the default values work for you, you can leave them as they are. <br><br>
![{FA25CC70-1A82-4F84-97B4-4D75D0EC9C15}](https://github.com/user-attachments/assets/3cae10a3-fd5a-4785-a20f-e5733bae147b)<br><br>
Next, run `autowifi-jsn.py`. <br><br>
<img src=https://github.com/user-attachments/assets/fd285666-da87-446f-b9b9-faf7b35cac4e width=1000><br><br>
The terminal should look like this if there are no errors and everything is working: <br><br>
![{19290986-AC25-47B9-BB95-AAE0CF3A7C47}](https://github.com/user-attachments/assets/cc419e22-1f70-497f-9321-c23c7099379a) <br><br>
Now, use your mobile phone to connect to the network. Look for the Wi-Fi network name (SSID) you set earlier and connect to it. <br><br>
<img src=https://github.com/user-attachments/assets/3ea17b8b-6b0b-4a54-8e4a-df8040089203 width="400"><br><br>

---


## 📱 Task 14 - Linphone Setup and Configuration
>Before proceeding, ensure that you have completed Task 10 and 11, which involves setting up your SIP account and downloading Linphone, which is required for this step.<br>

After connecting to the access point, open the Linphone app on your mobile device. In the app, select the option to use a SIP account. Once configured, you can now make calls between your mobile phone and your IP phone seamlessly. <br>

In Linphone, choose the option to use a SIP account.<br><br>
<img src=https://github.com/user-attachments/assets/d1123b3d-891d-42ad-80b6-c22ebe466c38 width=400><br><br>
Enter the following details and click **Login**<br>
>Username: `2823`<br>
>Password: `2823`<br>
>Domain: `10.28.1.1` (Your Router IP)<br>
>Transport: `UDP` 

<br>
<img src=https://github.com/user-attachments/assets/b7f342a9-2acb-4549-a240-a7c1b5a280da width=400><br>

Wait for it to show connected <br><br>
<img src=https://github.com/user-attachments/assets/83b852d6-2c37-42e4-acba-c199da8507d0 width=400><br>

Once its connected try to dial your IP Phone `2888`<br><br>
<img src=https://github.com/user-attachments/assets/6e118f1c-e0d0-41cf-a728-f5b17c51b189 width=400><br>

You can now make calls between your mobile phone and your IP phone<br><br>
<img src=https://github.com/user-attachments/assets/51668ea4-8720-4206-ab74-5b956021cd58 width=400><br>

IP Phone ringing: <br><br>
<img src=https://github.com/user-attachments/assets/591beae5-a667-4635-89e9-0bd61edbe23a width=400><br>

---
