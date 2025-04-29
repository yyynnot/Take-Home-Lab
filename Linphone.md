# Linphone Setup Guide

Welcome to **Linphone Setup Guide**, where we setup your router to allow your mobile phone to call cisco IP Phone.

---

## 📚 Prerequisites

- Linphone App
- Python installed on your PC

---

## 📱 Task 1 - WIFI Setup using Python
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
