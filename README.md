# 🛡️ Windows Event Monitoring & Azure Attack Lab

This repository documents my personal cybersecurity lab focused on detecting, exporting, and analyzing attack activity against a Windows VM hosted in **Microsoft Azure**.  
The goal of this project was to simulate a real-world blue team scenario: monitoring logon activity, identifying attack sources, and mapping network activity.

---

## 🧰 Tech Stack & Tools

- 🪟 Windows Server VM (Azure)  
- ⚔️ PowerShell & Event Viewer  
- ☁️ Microsoft Azure (Resource Groups, Sentinel, Network Watcher)  
- 🌐 GeoIP Lookup for attacker location mapping  
- 🧾 NSG & Security Event Logs  
- 🕵️ Manual log analysis & visualization

---

## 📁 Project Structure

---

## 🧠 Project Summary

- Monitored **Windows Security, System, and Application** logs from an Azure-hosted VM.
- Exported relevant logs to `.evtx` and `.csv` formats for offline analysis.
- Identified failed and successful logon attempts (`Event ID 4625` and `4624`).
- Mapped attacker IP addresses through **GeoIP** lookups.
- Generated a simple attack map and network topology visualization.
- Integrated the VM with **Azure Sentinel** for security monitoring and alerting.

---

## 📊 Evidence Collected

| Category                    | Description                                                  | File(s)                                   |
|-----------------------------|--------------------------------------------------------------|-------------------------------------------|
| Security Logs               | Windows Security events (4624, 4625)                          | `security_logons_last7days.csv`          |
| System & Application Logs   | OS and app event traces                                      | `system.evtx`, `application.evtx`         |
| Azure Infrastructure        | Resource template, topology, Sentinel configuration           | `exported_template.json`, `network_topology.png` |
| GeoIP & Attack Summary      | IP geolocation and attack mapping                             | `geoip_summary.csv`, `attack_map.png`     |
| PowerShell Scripts          | Automated log exports                                        | `export_security.ps1`                     |
| Screenshots & Visuals       | Event Viewer & Azure portal screenshots                       | in `Screenshots/`                         |

---

## 🕵️ Key Observations

- Multiple failed RDP login attempts (Event ID 4625) from external IPs.  
- Attackers originated from various countries (GeoIP lookup).  
- Event Viewer logs provided timeline and user context for each attempt.  
- Network topology matched the flow of external attempts to the VM endpoint.  
- Sentinel rules successfully ingested alerts.

---

## 🌍 Attack Source GeoIP Summary

| Source (redacted) | Attempts | Country |
|-------------------|----------|---------|
| REDACTED_IP_1     | 12       | 🇺🇸 USA |
| REDACTED_IP_2     | 5        | 🇧🇷 Brazil |
| REDACTED_IP_3     | 3        | 🇩🇪 Germany |

> ⚠️ All IPs have been redacted for privacy.

---

## 🚀 How to Recreate This Lab

1. Deploy a Windows VM on Azure.  
2. Enable Network Watcher and configure security logging.  
3. Expose RDP for a limited period to attract brute force attempts.  
4. Collect Security, System, and Application logs with PowerShell:
   ```powershell
   wevtutil epl System system.evtx
   wevtutil epl Application application.evtx
   Get-WinEvent -FilterHashtable @{LogName='Security'; Id=4624,4625} |
   Export-Csv security_logons_last7days.csv -NoTypeInformation


