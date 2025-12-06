# 🛰️ Assisted Lab: Performing Asset Discovery  
**Course:** CompTIA CySA+  
**Environment:** Kali Linux (root) — Structureality Inc. Server Subnet  
**Tools Used:** Nmap  

---

## 📌 Overview
In this lab, I performed **asset discovery** across multiple internal subnets for Structureality Inc. using **Nmap**, one of the most widely used network scanning and security auditing tools. The goal was to identify active hosts, discover open ports, enumerate running services, and detect operating systems across server, client, and screened subnets.

This assessment helps validate inventory accuracy and detect potentially **rogue or undocumented systems** within the environment.

---

## 🎯 Objectives
This exercise aligns with these CySA+ domains:

- **2.1:** Implement vulnerability scanning methods and concepts  
- **2.2:** Analyze output from vulnerability assessment tools  

---

## 🧩 Network Subnet Targets
| Subnet Type        | IP Range        |
|-------------------|------------------|
| Server Subnet     | `10.1.16.0/24`   |
| Client Subnet     | `10.1.24.0/24`   |
| Screened Subnet   | `172.16.0.0/24`  |

---

## 🔧 Step 1 — Ping Sweeps (Host Discovery)
```bash
nmap 10.1.16.0/24 -sn -oN server_assets_pingsweep.nmap
nmap 10.1.24.0/24 -sn -oN client_assets_pingsweep.nmap
nmap 172.16.0.0/24 -sn -oN screened_assets_pingsweep.nmap
✔ Discovered hosts in each subnet
✔ Output saved to .nmap files

🔧 Step 2 — Fast Port Scan (Top 100 Ports)
Target file creation:

bash
Copy code
echo 10.1.16.0/24 > targets.txt
echo 10.1.24.0/24 >> targets.txt
echo 172.16.0.0/24 >> targets.txt
Run Nmap Fast Scan:

bash
Copy code
nmap -iL targets.txt -F -sS -oA assets_fast_port_scan
This generated:

assets_fast_port_scan.nmap

assets_fast_port_scan.gnmap

assets_fast_port_scan.xml

✔ SYN scan detected open ports on all active systems
✔ Confirmed ping sweep accuracy

🔎 Step 3 — Analyze Grepable Output
bash
Copy code
cat assets_fast_port_scan.gnmap | grep open
Shows all hosts with discovered open ports.

🔧 Step 4 — Version Scan (Service Enumeration)
bash
Copy code
nmap -iL targets.txt -F -sS -sV -oN service_versions.nmap
✔ Identified open services and their versions
✔ Banner grabbing performed on top 100 ports

Services Detected
SSH

FTP

DNS

NTP

Web (HTTP/HTTPS)

🖥️ Step 5 — OS Detection
bash
Copy code
nmap -iL targets.txt -F -sS -O -oN asset_OSes.nmap
bash
Copy code
cat asset_OSes.nmap | grep Running
Operating Systems Identified
Windows

Linux

(OS detection results depend on the accuracy of Nmap's TCP/IP fingerprinting.)

📂 Output Files Generated
File Name	Purpose
server_assets_pingsweep.nmap	Ping sweep results for server subnet
client_assets_pingsweep.nmap	Ping sweep results for client subnet
screened_assets_pingsweep.nmap	Ping sweep results for screened subnet
assets_fast_port_scan.nmap/.gnmap/.xml	SYN fast port scan results
service_versions.nmap	Service version enumeration
asset_OSes.nmap	Operating system detection

📸 Screenshots (Insert Proof of Work)
Replace these placeholders with your images

scss
Copy code
![Ping Sweep Results](images/ping_sweep_output.png)
![Fast Port Scan](images/fast_port_scan.png)
![Service Version Scan](images/service_version_scan.png)
![OS Detection](images/os_detection.png)
🧠 Key Learnings
Nmap is a powerful tool for host discovery, service enumeration, and OS fingerprinting.

Ping sweeps alone are unreliable; port scans are required to identify non-responsive hosts.

SYN scans provide accurate port status without completing a full TCP handshake.

Version scanning (-sV) enables deeper service identification for vulnerability analysis.

Grepable output allows efficient filtering of results for automation or reporting.

OS detection assists in validating inventory documentation and identifying unauthorized devices.

📌 Conclusion
This lab demonstrated real-world asset discovery techniques used by cybersecurity analysts to map networks, validate documentation, detect rogue hosts, and prepare for vulnerability assessments.

The use of Nmap across multiple scan types gives a complete visibility snapshot of Structureality Inc.’s internal and screened subnets.
