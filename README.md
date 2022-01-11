## Week 7 Homework: A Day in the Life of a Windows Sysadmin

### Overview  

This activity will go through some useful tactics that might be taken by a Windows Sysadmin in order to harden the system. We will create domain-hardening GPOs and revisit some PowerShell fundamentals.

- This activity uses two different Windows VMs. The main [Windows 10 Server] will be used to administer all group policies while a [Windows 10] VM will be used to mimic a user on the server's domain. 

### Task 1: Create a GPO: Disable Local Link Multicast Name Resolution (LLMNR)

**Local Link Multicast Name Resolution (LLMNR)** is a vulnerability, so we will be disabling it on our Windows 10 machine. 

A few notes about LLMNR:

- LLMNR is a protocol used as a backup (not an alternative) for DNS in Windows. 

- When Windows cannot find a local address (e.g. the location of a file server), it uses LLMNR to send out a broadcast across the network asking if any device knows the address. 

- LLMNR’s vulnerability is that it accepts any response as authentic, allowing attackers to poison or spoof LLMNR responses, forcing devices to authenticate to them. 

- An LLMNR-enabled Windows machine may automatically trust responses from anyone in the network.

Turning off LLMNR for the the Organizational Unit will prevent our Windows machine from trusting location responses from potential attackers.

[View Deliverable 1 Here](https://github.com/cdnet01/Windows_Sysadmin/blob/main/Deliverables/Deliverable%20for%20Task%201.png)

---

### Task 2: Create a GPO: Account Lockout

For security and compliance reasons, we to need implement an account lockout policy on our Windows workstation. An account lockout disables access to an account for a set period of time after a specific number of failed login attempts. This policy defends against brute-force attacks, in which attackers can enter a million passwords in just a few minutes.

It will be important to keep in mind that an overly restrictive account lockout policy (such as locking an account for 10 hours after 2 failed attempts), can potentially keep an account locked forever if an attacker repeatedly attempts to access it in an automated way.

[View Deliverable 2 Here](https://github.com/cdnet01/Windows_Sysadmin/blob/main/Deliverables/Deliverable%20for%20Task%202.png)


---

### Task 3: Create a GPO: Enabling Verbose PowerShell Logging and Transcription

Since powershell can be both a useful tool as well as a possible attack vector, best practices for enabling or disabling PowerShell are debated. This often leads to the solution of allowing only certain applications to run.

In this activity we are going to use a PowerShell practice that is recommended regardless of whether PowerShell is enabled or disabled: enabling enhanced PowerShell logging and visibility through verbosity. This type of policy is important for tools like SIEM and for forensics operations, as it helps combat obfuscated PowerShell payloads.

Note that the in order to put these administrative policies into effect, we must run `gpupdate` on our Windows 10 machine. Now, when we launch a new PowerShell window and run a script, we see verbose PowerShell logs created in the Windows 10 machine directory for the user that ran the script: `C:\Users\<user>\Documents`.

[View Deliverable 3 Here](https://github.com/cdnet01/Windows_Sysadmin/blob/main/Deliverables/Deliverable%20for%20Task%203.png)

---

### Task 4: Create a Script: Enumerate Access Control Lists

In Windows, access to files and directories are managed by Access Control Lists (ACLs). These identify which entities (known as security principals), such as users and groups, can access which resources. ACLs use security identifers to manage which principals can access which resources.

In this activity we will be writing a powershell script to show us the ACL output of each file or subdirectory where you ran the script from.

Once we have our script, we can test it by moving to any directory (`cd C:\Windows`), and running `C:\Users\sysadmin\Documents\enum_acls.ps1` (enter the full path and file name). 

[View Deliverable 4 Here](https://github.com/cdnet01/Windows_Sysadmin/blob/main/Deliverables/enum_acls.ps1)

---

### Bonus Task 5: Verify Your PowerShell Logging GPO

For this task we'll want to test and verify that our PowerShell logging GPO is working properly.

Essentially, we will run our script on the Windows 10 machine, then verify that our logging GPO is working properly by looking at the logs for whichever user ran the script. 

Our deliverable is the logfile. 

[View Deliverable 5 Here](https://github.com/cdnet01/Windows_Sysadmin/blob/main/Deliverables/(COPY)PowerShell_transcript.DESKTOP-SITPOTH.+Mvt8_yD.20220107034942)


---
### Note

This README file was originally modified from the full homework file

© 2020 Trilogy Education Services, a 2U, Inc. brand. All Rights Reserved.
