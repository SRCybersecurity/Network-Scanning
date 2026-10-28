# 🔍 Local Network Port Scanning and Exposure Analysis
A hands-on network security assessment project focused on discovering active hosts, identifying open ports, enumerating visible network services, and evaluating potential network exposure using Nmap and Wireshark.

This project demonstrates fundamental network reconnaissance and defensive security practices to help understand how devices communicate across a local network and how unnecessary or insecure services can increase the attack surface.

**⚠️ Security Notice:** Perform scans only on networks and devices you own or have explicit authorization to assess. Network scanning can trigger security alerts and may affect fragile devices. Keep all activities within the approved scope.

### 🎯 Objectives

- Discover active devices within an authorized local network.

- Identify open TCP ports and commonly exposed services.

- Understand the difference between host discovery, port scanning, and service detection.

- Analyze network traffic using Wireshark.

- Identify potentially unnecessary or insecure network services.

- Recommend measures to reduce network exposure and improve security.

### 🛠️ Tools and Technologies

| Tool | Purpose |
|---|---|
| [Nmap](https://nmap.org/) | Discover hosts, scan ports, identify services, and support network security assessments. |
| [Wireshark](https://www.wireshark.org/) | Capture and analyze network packets to understand communication patterns and scan activity. |
| Linux Terminal | Execute network discovery commands and inspect local network configuration. |

### 📋 Assessment Overview

| Attribute | Details |
|---|---|
| Project Name | Local Network Port Scanning and Exposure Analysis |
| Assessment Type | Local network discovery and port exposure assessment |
| Primary Tool | Nmap |
| Optional Tool | Wireshark |
| Target Scope | Authorized local network |
| Assessment Approach | Host discovery, TCP port scanning, service identification, and network traffic analysis |
| Primary Objective | Identify exposed services and evaluate potential network security risks |
| Expected Deliverable | Documented scan results, identified security risks, and actionable remediation recommendations |


### 🔬 Assessment Methodology

| Phase | Activity | Description |
|---|---|---|
| 1 | Tool Installation | Verify the Nmap installation and install it if necessary. |
| 2 | Network Identification | Determine the local IP address, subnet mask, and authorized network range. |
| 3 | Host Discovery | Identify devices that respond to network discovery probes. |
| 4 | Port Scanning | Identify accessible TCP ports on authorized hosts. |
| 5 | Service Enumeration | Identify running services and, where appropriate, their versions. |
| 6 | Traffic Analysis | Optionally capture and inspect scan-related network traffic using Wireshark. |
| 7 | Risk Assessment | Evaluate exposed services to determine whether they are necessary, appropriately configured, and securely accessible. |
| 8 | Remediation Planning | Recommend service hardening, access restrictions, attack-surface reduction, and follow-up validation. |


**1. 📦 Install Nmap**
   
Nmap is a free, open-source utility used for network discovery and security auditing. It is available on Linux, Windows, and macOS.

Verify the Installation

``` bash
nmap --version
Display available command-line options:
```

```bash
nmap --help
Install or Update Nmap on Debian-Based Linux
```
```bash
sudo apt update
sudo apt install nmap
```

Command explanation:

| Command | Purpose |
|---|---|
| `nmap --version` | Displays the installed Nmap version. |
| `nmap --help` | Displays usage information and available command-line options. |
| `sudo apt update` | Refreshes the local package index. |
| `sudo apt install nmap` | Installs Nmap or updates it according to the configured package repositories. |

**Note:** `sudo` executes a command with elevated privileges when permitted. Root privileges are commonly required for TCP SYN scanning on Linux.

**2. 🌐 Identify Your Local Network Range**

Before scanning, identify your device's IP address and network interface.

Display Network Interfaces

```bash
ip addr show
```
To inspect routing information:

```bash
ip route
These commands help identify local IP addresses, interface details, and connected network routes.
```
**Example Network Range**

| Parameter | Example |
|---|---|
| Local IP Address | `192.168.1.25` |
| Subnet | `192.168.1.0/24` |
| IPv4 Range Represented by `/24` | `192.168.1.0` – `192.168.1.255` |
| Usable Host Address Range | `192.168.1.1` – `192.168.1.254` |

**Note:** These values are examples only. Your actual network may use a different IP address range or subnet size. Verify your network configuration before scanning.

**Important:** Confirm the correct subnet from your network configuration before scanning. Do not assume every local network uses 192.168.1.0/24.

**3. 🖥️ Discover Active Hosts**

Host discovery identifies devices that appear reachable on the network without performing a full port scan.

Nmap Host Discovery

```bash
nmap -sn 192.168.1.0/24
The -sn option disables port scanning and performs host discovery.
```
**What to Record**

| Observation | Description |
|---|---|
| IP Address | IP address of a discovered host on the network. |
| Hostname | Hostname associated with the device, when available. |
| Discovery Status | Indicates whether the host responded to the discovery probes used. |
| MAC Address | Hardware address, when discoverable on the local Ethernet segment. |
| Device Identification | Verified information about the device's role, type, or owner. |

**Note:** A host that does not respond to discovery probes is not necessarily offline. Firewalls, network isolation, and device configurations can affect host discovery results.

**4. 🔎 Scan for Open TCP Ports**

A TCP port scan identifies ports that appear accessible on the target hosts.

TCP SYN Scan

```bash
sudo nmap -sS 192.168.1.0/24
The -sS option performs a TCP SYN scan when supported by the operating system and privileges.
```
A SYN scan sends connection-establishment probes and evaluates the responses without completing a full TCP connection in the usual way. It is sometimes called a half-open scan.

Security clarification: A SYN scan is not guaranteed to avoid detection. Firewalls, intrusion detection systems, and network monitoring tools may detect it.

TCP Connection Scan

An alternative is a TCP connect scan:

```bash
nmap -sT 192.168.1.0/24
The -sT option uses the operating system's TCP connect mechanism. It is useful when raw-packet scanning is unavailable.
```
Scan Selected Ports on a Specific Host

For a more focused assessment, scan common ports on a single authorized host:

```bash
nmap --top-ports 100 192.168.1.25
Replace the example address with a host within your approved scope.
```
**Interpret Port States**

| Port State | Meaning |
|---|---|
| `open` | A service is accepting connections on the port. |
| `closed` | The host is reachable, but no service is listening on that port at the time of scanning. |
| `filtered` | Nmap cannot determine whether the port is open because packet filtering or another network condition prevents a definitive result. |

**Security Note:** An open port does not automatically indicate a vulnerability. The associated service, configuration, network exposure, authentication controls, and business requirements determine the actual security risk.

**5. 🧭 Understand the Nmap Commands**

| Command | Purpose |
|---|---|
| `nmap --version` | Checks the installed Nmap version. |
| `nmap --help` | Displays Nmap usage information and available options. |
| `ip addr show` | Displays local IP addresses and network interfaces. |
| `ip route` | Displays routing information and network routes. |
| `nmap -sn 192.168.1.0/24` | Performs host discovery without a port scan. |
| `sudo nmap -sS 192.168.1.0/24` | Performs a TCP SYN scan across the specified network. |
| `nmap -sT 192.168.1.0/24` | Performs a TCP connect scan across the specified network. |
| `nmap --top-ports 100 192.168.1.25` | Scans the 100 most common ports on the specified host. |
| `nmap -sV 192.168.1.25` | Attempts to identify running services and their versions on the specified host. |
| `nmap -oN scan-results.txt 192.168.1.25` | Saves the scan's normal output to `scan-results.txt`. |

**Security Notice:** Replace the example IP addresses and network ranges with verified targets within your authorized assessment scope. Only scan systems you own or have explicit permission to assess.

**6. 📡 Analyze Network Traffic with Wireshark (Optional)**

Wireshark is a network protocol analyzer that helps inspect packets exchanged between devices.

### 📡 Basic Wireshark Workflow

1. Open Wireshark.
2. Select the network interface associated with your authorized test network.
3. Start a packet capture before performing the test scan.
4. Run the approved Nmap command against the authorized target.
5. Stop the capture after the scan completes.
6. Apply display filters to inspect relevant network traffic.
7. Save the capture only when necessary and protect it as potentially sensitive security evidence.

### 🔎 Useful Wireshark Display Filters

| Display Filter | Purpose |
|---|---|
| `tcp` | Displays TCP packets. |
| `udp` | Displays UDP packets. |
| `tcp.port == 80` | Displays TCP traffic where the source or destination port is 80. |
| `tcp.port == 443` | Displays TCP traffic where the source or destination port is 443. |
| `dns` | Displays DNS protocol traffic recognized by Wireshark. |
| `http` | Displays recognized HTTP traffic. |
| `http2` | Displays recognized HTTP/2 traffic. |
| `tls` | Displays recognized TLS protocol traffic. |
| `ssh` | Displays recognized SSH protocol traffic. |

**Note:** Wireshark display filters differ from capture filters. Protocol recognition depends on the traffic, dissector support, and encryption. HTTPS application content is generally unreadable without the necessary decryption information.

**7. 📚 Common Ports and Services**

The following table provides a reference for interpreting common port numbers. These are conventional assignments, not proof that a particular service is running on a port.

**📚 Common Ports and Services**

| Port | Protocol | Common Service | Security Consideration |
|---|---|---|---|
| `21` | TCP | FTP | Traditional FTP may transmit credentials and data without encryption. Prefer secure alternatives where appropriate. |
| `22` | TCP | SSH | Restrict access and enforce secure authentication and configuration. |
| `23` | TCP | Telnet | Generally avoid because it does not provide transport encryption. |
| `25` | TCP | SMTP | Restrict relay permissions and configure mail security appropriately. |
| `53` | TCP/UDP | DNS | Restrict administrative access and ensure DNS services are configured correctly. |
| `80` | TCP | HTTP | Use HTTPS where appropriate and redirect HTTP traffic when required. |
| `123` | UDP | NTP | Restrict unnecessary exposure and use a trusted time synchronization configuration. |
| `443` | TCP | HTTPS | Maintain valid certificates, secure TLS settings, and application security controls. |
| `445` | TCP | SMB | Restrict access to trusted networks and disable unnecessary file-sharing services. |
| `3389` | TCP | RDP | Restrict remote access and enforce strong authentication and appropriate access controls. |

**Note:** These are conventional port assignments, not proof of the service running on a particular port. Confirm service identification using Nmap results and additional validation before assessing security risks.

**8. 🚨 Identify Potential Security Risks**

An exposed service can increase the attack surface if it is unnecessary, outdated, misconfigured, or accessible from untrusted networks.

**🚨 Network Exposure Assessment**

| Security Concern | Potential Risk | Recommended Action |
|---|---|---|
| Unnecessary Open Ports | Unneeded services may increase the network attack surface. | Disable unnecessary services and restrict access to unused ports. |
| Insecure Legacy Protocols | Protocols such as Telnet or traditional FTP may expose credentials or data in transit. | Replace insecure protocols with encrypted alternatives where practical. |
| Outdated Services | Unsupported or unpatched software may contain known vulnerabilities. | Verify software versions and apply supported security updates. |
| Unrestricted Remote Access | Management services may be accessible to unauthorized systems. | Apply firewall rules, network segmentation, and strong authentication. |
| Excessive Service Exposure | Services may be accessible from networks that do not require them. | Restrict access to approved source IP addresses and trusted network zones. |
| Information Disclosure | Service banners may reveal software details that assist attackers in identifying potential targets. | Minimize unnecessary service banners and information disclosure where practical. |
| Weak Service Configuration | Default settings or weak authentication may introduce avoidable security risks. | Apply secure configuration baselines and review authentication and access controls. |

**🛡️ Risk Assessment Principles**

| Principle | Assessment Question |
|---|---|
| **Exposure** | Is the service reachable from an untrusted network? |
| **Necessity** | Does the device require the service for its intended purpose? |
| **Configuration** | Is the service securely configured according to established security baselines? |
| **Patch Status** | Is the software supported, up to date, and free from known unpatched vulnerabilities? |
| **Access Control** | Is access restricted to authorized users, devices, and network segments? |
| **Impact** | What could happen if the service were compromised? |

**Security Note:** An open port does not automatically indicate a vulnerability. Validate the associated service, configuration, exposure, and potential business impact before assigning a risk severity.

**9. 🛡️ Recommended Remediation**

| Action | Recommendation | Security Benefit |
|---|---|---|
| **Close Unused Ports** | Disable unnecessary services and restrict access to unused ports. | Reduces unnecessary network exposure and the attack surface. |
| **Secure Required Services** | Patch software and apply secure configuration baselines. | Reduces the risk of exploitation. |
| **Restrict Access** | Apply firewall rules and network segmentation. | Limits access to authorized users, devices, and network sources. |
| **Review Service Versions** | Verify software versions, support status, and available security updates. | Helps identify outdated or unsupported components. |
| **Minimize Information Disclosure** | Disable unnecessary service banners and information disclosure where practical. | Reduces avoidable exposure of infrastructure details. |
| **Reassess Periodically** | Repeat authorized scans after configuration changes and security updates. | Detects newly exposed services and configuration drift. |

### ✅ Remediation Validation

After implementing approved configuration changes, perform the following validation steps:

1. Repeat the relevant scan against the same authorized targets.
2. Confirm that unnecessary services are no longer exposed.
3. Verify that required services remain operational.
4. Review firewall rules and access-control configurations.
5. Compare the original and updated scan results.
6. Document remediation status, validation evidence, and any remaining risks.

**📊 Assessment Results**

Use the following table to document the actual results of your authorized network assessment. Populate each field using verified scan evidence rather than assumed or illustrative findings.

| Assessment Item | Result |
|---|---|
| Assessment Date | Record the actual assessment date and time. |
| Network Range | Specify the verified, authorized subnet. |
| Hosts Discovered | Record the number of hosts reported by Nmap. |
| Hosts Scanned | Record the number of hosts actually scanned. |
| Open Ports | Document observed ports and their associated hosts. |
| Services Identified | Record identified service names and versions, where available. |
| Potential Risks | Document validated security concerns and their potential impact. |
| Remediation Status | Record each finding as Open, In Progress, Resolved, or Risk Accepted. |
| Retest Status | Document verification results and supporting evidence after remediation. |

**Security Note:** Assessment results represent observations at a specific point in time. Validate findings before assigning severity, preserve supporting evidence, and document unresolved risks for follow-up.

### ⚠️ Limitations

Host discovery may miss devices that block probes or are temporarily unavailable.

Port scans show observations at a particular point in time.

Service identification may be incomplete or inaccurate.

An open port does not establish that a vulnerability exists.

Scanning results may be affected by firewalls, routing, network segmentation, and device configuration.

Wireshark visibility depends on the capture interface, network topology, and encryption.

This exercise is a basic network exposure assessment, not a comprehensive penetration test.

### 🏁 Conclusion

This project demonstrates the fundamentals of local network reconnaissance and exposure analysis using Nmap, with optional packet-level inspection using Wireshark.

By identifying active hosts, reviewing open ports, examining visible services, and validating their necessity, network administrators and security practitioners can develop a clearer understanding of their local network's attack surface.

The most important outcome is not simply finding open ports, but determining whether exposed services are necessary, appropriately secured, and accessible only to authorized systems.

### 🔑 Key Takeaways

- **Discover:** Identify active hosts within the authorized network.
- **Enumerate:** Examine open ports and identify visible network services.
- **Analyze:** Evaluate network exposure, service configurations, and potential security risks.
- **Harden:** Disable unnecessary services and restrict network access to authorized systems.
- **Validate:** Repeat scans after remediation and document the verified results.

**Discover the exposure. Validate the risk. Reduce the attack surface. Verify the remediation.**

### 🔒 Responsible Use

This project is intended for educational purposes, defensive security research, and authorized network administration.

Only scan networks and devices that you own or have explicit permission to assess. Follow the approved scope, respect organizational security policies, and protect scan outputs and packet captures because they may contain sensitive infrastructure information.
