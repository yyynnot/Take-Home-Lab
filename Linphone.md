# Linphone Setup Guide

Welcome to **Linphone Setup Guide**, where we setup your router to allow your mobile phone to call cisco IP Phone.

---

## 📚 Prerequisites

- Linphone App
- Python installed on your PC

---
## 📱 Task 1 - Check your PC Ethernet IP
Check your PC IP address: <br>
`Win + R` to open run command and enter `ncpa.cpl` then enter <br><br>
![{86C5C063-7B66-48D0-9447-3880105168E0}](https://github.com/user-attachments/assets/f62c64ef-285d-4615-8e09-1ea5ed1fc643)<br><br>

Click `Ethernet > Properties > Internet Protocol Version 4 (TCP/IPv4)` <br> <br>
`IP address: 10.28.1.10` <br>
`Subnet mask: 255.255.255.0` <br>
`Default gateway: 10.28.1.1` <br> <br>
![{8C145EC3-58B3-4CC6-B4C7-BE4006522C41}](https://github.com/user-attachments/assets/ac2f22e3-defc-4f2e-9bc0-9df24f51b7c4)<br><br>

## 📱 Task 2 - Ping the devices
To check the connection between PC and Router you can use ping command <br>
`Windows IP: 10.28.1.10` <br>
`Router IP: 10.28.1.1` <br>
`Access Point IP: 10.28.10.3` <br>

Ping Devices on Router:
```bash
ping 10.28.1.10
ping 10.28.10.3
```
Output: <br><br>
![{DFE87018-AE8E-4A83-9811-5F3A3C218D36}](https://github.com/user-attachments/assets/6ed14d28-494f-436b-9641-62dce8880744)<br><br>

Ping Devices on Windows CMD:
```bash
ping 10.28.1.1
ping 10.28.10.3
```
Output: <br><br>
![{A7BCF772-48A1-427B-9253-11CE1097FA4C}](https://github.com/user-attachments/assets/0d77fe23-c500-44fc-a437-0916c7ef6c95)<br><br>

## 📱 Task 2 - WIFI Setup using Python
Automate your wireless access point setup by using a Python script to configure SSID, password, and VLAN tagging through `netmiko`.

On your PC open terminal and try to ping your Access Point (10.28.10.3)
```bash
ping 10.28.10.3
```
Output: <br><br>
![{B74A5D42-459C-4342-A050-D875B6FE57B7}](https://github.com/user-attachments/assets/e52b69e4-b40f-4cbd-9792-98aadfc0f1b2)<br><br>
If its pinging it means they have connection

Install netmiko module
```bash
python -m pip install netmiko
```
or 
```bash
py -m pip install netmiko
```
Now clone this repository 
```git
git clone https://github.com/yyynnot/Take-Home-Lab.git
```
Open Visual Studio Code and Open the `Take-Home-Labs > wifi` folder and edit `autoAP-jsn.json` Replace hostname, ssid, and wifi-pass. <br><br>
![{FA25CC70-1A82-4F84-97B4-4D75D0EC9C15}](https://github.com/user-attachments/assets/3cae10a3-fd5a-4785-a20f-e5733bae147b)<br><br>
After editing run `autowifi-jsn.py` <br><br>
<img src=https://github.com/user-attachments/assets/fd285666-da87-446f-b9b9-faf7b35cac4e width=1000><br><br>
Terminal should be like this <br><br>
![{19290986-AC25-47B9-BB95-AAE0CF3A7C47}](https://github.com/user-attachments/assets/cc419e22-1f70-497f-9321-c23c7099379a) <br><br>
Now connect to it using your mobile phone. <br><br>
<img src=https://github.com/user-attachments/assets/3ea17b8b-6b0b-4a54-8e4a-df8040089203 width="400"><br><br>

---

## 📞 Task 3 - Linphone Setup and Configuration
After connecting to the access point, open the Linphone app on your mobile device. In the app, select the option to use a SIP account. Once configured, you can now make calls between your mobile phone and your IP phone seamlessly.
>Before proceeding, make sure you have completed [Task 10 - Cellphone to IP Phone Connection](https://github.com/yyynnot/Take-Home-Lab/wiki#-task-10---cellphone-to-ip-phone-connection), as that task involves setting up your SIP account, which is required for this step.<br>

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



