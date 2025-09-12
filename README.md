# ELEVATE_LABS_TASK1
Task : Scan Your Local Network for Open Ports. </br> 
Objective : Learn to discover open ports on devices in your local network to understand network exposure. </br>
Tools: Nmap (free), Wireshark (optional).
#  Scan Your Local Network for Open Ports

## 🎯 Objective

Learn to discover open ports on devices in your local network to understand network exposure.

## 🛠️ Tools

- Nmap (free)
- Wireshark (optional)

---

## 📌 Steps

### 1. Install Nmap

Nmap is usually pre-installed on Kali Linux. To verify or install:

    nmap --version
    nmap --help 
- version To check the installed version of nmap  and help is nmap commands helpbook.
- And to ensure it's up to date, you can install or reinstall it by using the following commands:

      sudo apt update                         
      sudo apt install nmap

- apt or aptget for package handling tool so you can install/update/remove nmap by using sudo for root privileges.
---
### 2.Find your local IP range (e.g., 192.168.1.0/24). 
  - IP discovery using command:
    
        ip addr show 
---
### 3.Run:
- To perform TCP SYN scan.
 
      nmap -sS 192.168.1.0/24 
 -  Its a half open scan to avoid detection.
---
### 4.Note down IP addresses and open ports found.
 - Commands used:
 
       nmap -Pn 192.168.1.0/24
       nmap -sn 192.168.1.0/24
---
### 5.Optionally analyze packet capture with Wireshark. 
 - Analyze packets using filters
 - tcp, tcp.port, tcp.port == 80, dns, http, http2, http3, bttps,ssh, udp.
---
### 6.Research common services running on those ports. 
  - Most common ports and services are:
  - port 21 ftp, 22 ssh, 23 telnet, 53 udp, 80 http, 443 https, 123 udp.
---
### 7.Identify potential security risks from open ports.
 - Open vulnerable ports leads to exploitation. 
---
THANK YOU
---
END

 

