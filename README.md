# Virtual Machine Brute Force Detection
<img width="458" height="223" alt="image" src="https://github.com/user-attachments/assets/95a2007e-eb35-4e9a-a87d-ef45a41ebed3" />

## Platforms and Languages Leveraged
- Windows 10 Virtual Machines (Microsoft Azure)
- EDR Platform: Microsoft Defender for Endpoint
- Kusto Query Language (KQL)

## Scenario
In this lab, I simulate and investigate a brute force attack scenario using Microsoft Defender for Endpoint and Microsoft Sentinel. The goal is to detect when the same remote IP address fails to log in to the same virtual machine 10 or more times within a 5-hour window.

---
## Create Alert Rule (Brute Force Attempt Detection)
A Sentinel Scheduled Query Rule was designed within Log Analytics to discover when the same remote IP address has failed to log in to the same local host (Azure VM) 10 times or more within the last 5 hours.

<img width="1383" height="662" alt="brave_tiVsiAFEe4" src="https://github.com/user-attachments/assets/af78593b-ebe1-4692-900f-14fc9c885bd3" />

<img width="959" height="887" alt="brave_pC3QFaDFkM" src="https://github.com/user-attachments/assets/b008c08c-08de-4c31-8fc8-18b7d5320142" />

<img width="959" height="887" alt="brave_Btm2rmtkFB" src="https://github.com/user-attachments/assets/bf6c41f3-916e-4f5c-9ca5-b5135abd3f9f" />

---
## Look for any alerts
After creating the alert, the next step was to check for any alerts that were triggered and resulted in an incident. In the cyber range environment, an incident was generated immediately following the creation of the alert, indicating that the detection rule functioned as expected and successfully identified suspicious activity.

<img width="972" height="428" alt="brave_yOB7PJammk" src="https://github.com/user-attachments/assets/482dc3d2-7dca-446c-af82-aabe621eb349" />

<img width="1339" height="661" alt="brave_JKJS8VIJmQ" src="https://github.com/user-attachments/assets/3e156c98-f497-4a37-90d1-7f3f1ef5a6be" />

---
## Analysis
Checked if any of the IP addresses attempting to brute force successfully logged in with the following query, but none were successful:

```kql
DeviceLogonEvents
| where RemoteIP in ("178.22.24.78", "195.123.166.153", "103.13.215.107", "116.196.103.24")
| where ActionType  != "LogonFailed"
```

<img width="999" height="415" alt="brave_3ixqxCKqtU" src="https://github.com/user-attachments/assets/0199bc6b-dd6f-403c-91a4-f2d18b4f8938" />

---
## Work Incident
Worked the incident in accordance to nist 800-61. 

Containment:
Isolated Device in MDE on all four devices.
Ran anti-malware scan on all four devices.
NSG was locked down to prevent RDP attempts from the public internet.

<img width="1560" height="672" alt="brave_gMgfeWaKs8" src="https://github.com/user-attachments/assets/90b46821-ba54-4f0b-82fe-cca9911f396a" />

<img width="1550" height="619" alt="brave_xLUkLMYCrU" src="https://github.com/user-attachments/assets/fed7f0c3-71c3-4888-8f16-dae8b285f2e6" />


Eradication:
Brute force was not successful, no threats related to this incident. 

Post-Incident:
Notes were recorded within the incident and a corporate policy was proposed to require this for all VMs going forward. (this can be done with Azure Policy). The incident was then closed out. 


<img width="372" height="369" alt="brave_z4GzfUct4H" src="https://github.com/user-attachments/assets/c18a4cfb-b9ae-44b1-a905-7f5fa835a82b" />

<img width="484" height="279" alt="brave_7q35FMeWUI" src="https://github.com/user-attachments/assets/5be3a107-2316-4083-b89f-be2b4840f5fb" />

