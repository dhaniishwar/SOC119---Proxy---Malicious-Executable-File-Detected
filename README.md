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
&nbsp;&nbsp;&nbsp;&nbsp; The parent-child process relationship here matters a lot. chrome.exe was spawned by explorer.exe. It feelslike it was a normal traffic, Nothing unusal here. Also, Now we have parent process MD5. So, let's vrify explorer.exe's own integrity on VirusTotal.
<br>
<br>

<img width="1264" height="691" alt="8" src="https://github.com/user-attachments/assets/81ec8b0c-0f1e-4479-92cf-d0e8bc807271" />

<br>
<br>
&nbsp;&nbsp;&nbsp;&nbsp; This confims the parent process on SusieHost is legitimacy. 
<br>
<br>

---
***Malware Analyze:***

<img width="584" height="287" alt="5" src="https://github.com/user-attachments/assets/4748dc06-fad9-4e57-ab26-61ee4964f236" />

<br>
<br>
&nbsp;&nbsp;&nbsp;&nbsp; Let's get the URL for the file from the alert by right click the Download button and copy the link. Run the link on AnyRUN to analyze.
<br>
<br>

<img width="1280" height="696" alt="6" src="https://github.com/user-attachments/assets/ece1d556-47ae-490c-978c-01706d0b4e1c" />

<br>
<br>
&nbsp;&nbsp;&nbsp;&nbsp; AnyRUN flagged two warnings. Ignoring wrnings just because the overall score is low is a big mistake. The first warning was T1105 - Ingress Tool Transfer. A program on the host reache out and downloads another file. Attackers use this after they've broken in, to pull down malwares. It's worth flagging but here, the file being downloaded was just part of the WinRAR. The second warning was a Sigma rule hit: "Drops 7-zip archiver for unpacking." Sigma rules are just pre-written detection patterns, and this one triggers whenever a program drops a 7-zip tool to unpack files. Installers do this constantly to unpack their own setup files, so this is expected, not suspicious. So, the Answer is "Non-malicious".
<br>
<br>

---
***Documentation:***

<br>
<br>
&nbsp;&nbsp;&nbsp;&nbsp; This was the area, where we log our IOCs so they can be used for future detection and threat hunting across the environment. We add two artifacts, Malicious hash of the Source IP, Destination IP and URL.
<br>
<br>

<img width="584" height="363" alt="10" src="https://github.com/user-attachments/assets/4acd091d-09d4-4f4b-93e2-1e0b4fc039c2" />

<br>
<br>
&nbsp;&nbsp;&nbsp;&nbsp; The Analyst Note is where I write the full story in plain language, as if explaining it to someone who wasn't there for any of the investigation.
<br>
<br>

<img width="585" height="343" alt="11" src="https://github.com/user-attachments/assets/27cca836-3b11-4c98-a2b4-8cec9ade9930" />


---
***Close Alert:***

<img width="438" height="315" alt="12" src="https://github.com/user-attachments/assets/bdd91e09-b272-4563-b5cc-4386eff86c1a" />

<br>
<br>
&nbsp;&nbsp;&nbsp;&nbsp; With all evidence gathered, I closed EventID 83 with the verdict set to False Positive.
<br>
<br>

---
<h3 align =center> Summary </h3>

<br>
<br>
&nbsp;&nbsp;&nbsp;&nbsp; This alert closed as a False Positive. What looked like a suspicious executable download turned out to be completely legitimate once every piece of evidence was checked — the parent process was verified as genuine, the URL came back clean, and the sandbox detonation showed nothing more than a normal software installation running exactly as expected. 
<br>
<br>


