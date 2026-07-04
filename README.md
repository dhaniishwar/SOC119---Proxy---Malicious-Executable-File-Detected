**Platform:** LetsDefend

**Lab Name:** SOC119 - Proxy - Malicious Executable File Detected

**Difficulty:** Medium

**Date Completed:** July 3, 2026

**Type:** Proxy Alert / Malicious Executable File Download

**Target:** SusieHost (172.16.17.5), user account "Susie"

---
<h3 align =center> GOAL </h3>

<br>
<br>
&nbsp;&nbsp;&nbsp;&nbsp; Check whether the executable download flagged by the proxy is malicious or a false positive. Review the evidence, decide the correct verdict, and close the alert with your findings.
<br>
<br>

---
<h3 align =center> Walkthrough </h3>

***Alert Review:***

<br>
<br>
&nbsp;&nbsp;&nbsp;&nbsp; Opening the alert in the Monitoring tab under the Investigation Channel reveals all the critical details. 
<br>
<br>

<img width="1020" height="337" alt="1" src="https://github.com/user-attachments/assets/ef0bd9bb-aca2-43e9-b28b-f362c6bab213" />

<br>
<br>
&nbsp;&nbsp;&nbsp;&nbsp; The most important field on this page  is Divce Action: " Allowed ". because this was Allowed, I have to assume the file may have actually landed and executed on SusieHost. let's analyze this alert deeper by creating playbook.
<br>
<br>

---
***Starting the Playbook:***

<br>
<br>
&nbsp;&nbsp;&nbsp;&nbsp; Once you've reviewed the alert, head to Case Management and start the playbook. Think of the playbook as your structured checklist — it makes sure you don't skip steps under pressure.
<br>
<br>

<img width="594" height="287" alt="2" src="https://github.com/user-attachments/assets/8fe1d345-c371-47d0-9e80-ef1ea0cb8db1" />

<br>
<br>
&nbsp;&nbsp;&nbsp;&nbsp;  I already had these from the alert

- Source Address = 172.16.17.5

- Destination Address = 51.195.68.163

- User-Agent = Chrome-Windows
<br>
<br>

---
***Log Management Investigation:***

<img width="582" height="227" alt="3" src="https://github.com/user-attachments/assets/590349ea-437d-4767-84e7-f0369fd4e58f" />

<br>
<br>
&nbsp;&nbsp;&nbsp;&nbsp; Let's move to Log Management and Investigat the traffic.
<br>
<br>

<img width="862" height="287" alt="4" src="https://github.com/user-attachments/assets/e34f4390-8844-4f31-a718-53b4a4be204d" />

<br>
<br>
&nbsp;&nbsp;&nbsp;&nbsp; The parent-child process relationship here matters a lot. chrome.exe was spawned by explorer.exe. It feelslike it was a normal traffic, Nothing unusal here. Also, Now we have parent process MD5. So, let's rify explorer.exe's own integrity on VirusTotal.
<br>
<br>

- SEE ABOVE BEFORE DOING ANYTHIING







