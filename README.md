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
| **7. IP Camera Integration** | Basic IP camera setup, network configuration |
| **4. IP Telephony** | IP Phone registration, CUCM basics, call flow |
| **4. IVR & Voice Features** | Setting up basic IVR menus, auto-attendants, call handling logic |
| **6. Wireless Networking** | Setting up AP using python script |
| **5. Linphone Integration** | SIP softphone setup, connecting Linphone with CUCM or Asterisk, testing SIP endpoints |

---

## 📚 Prerequisites

- Basic networking knowledge (OSI model, TCP/IP, subnetting)
- Familiarity with Cisco Packet Tracer or GNS3 is helpful, but not required

---

## 🔧 Lab Environment Setup

1. **Connect the hardware**: Refer to the printed setup guide included in your kit or view the digital [Setup Guide PDF](Enter.pdf).
2. **Access router CLI** via console cable using:
   - PuTTY(Free) or SecureCRT(License)
3. **Power on all devices**, verify LED indicators, and begin configuration.

---

## 💡 Tips for Success

- Keep your devices labeled and workspace organized.
- Practice configurations multiple times.
- Take screenshots or notes as you work – they’ll help during troubleshooting.

---

## 📞 Support & Contact

If you encounter hardware issues or need assistance:

- Email: teamrivan@rvci.org
- Office Hours: Mon–Fri, 9:00 AM - 4:30 PM
- Website: [Rivan IT](https://rivanit.com/)

---

## 🧠 CCNA Topics Covered

✅ Networking Fundamentals  
✅ IP Connectivity & Services  
✅ Security Fundamentals 
✅ SIP Configurations
✅ Infrastructure Services (VoIP, Wireless, Cameras)

---

## 🏁 Ready to Start?

Let’s get building. Power on your lab and begin with **Task 1 – Setup & Cable Management** 🚀  
Stay consistent, stay curious. Rivan Home Labs is here to turn theory into real-world skills.

---

## 🛠️ Task 1 – Setup & Cable Management

Before configuring your devices, ensure all hardware is connected correctly. Refer to the Cabling Instructions PDF for step-by-step guidance on connecting IP Phones, Cameras, and Wireless APs to the appropriate ports. Once everything is set up, power on the devices and proceed with the configuration.

[Setup Guide PDF](Enter.pdf)

## ⚡ Task 2: Check Power Inline Status

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
## 🌐 Task 3: VLAN Checking and Configuration

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
## 🔄 Task 3: LAN/Ethernet Ports to VLAN Assignment
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
