![Banner](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F95fa80c9-fc09-41c7-a313-856f4155a90a%2F5a1947a3-a09e-4c51-94a1-8b6fffcf0521%2Fcomputer_fire_blog_extinguisher-01.png?table=block&id=d048762c-1984-419d-8fcd-a60cc03e9392&spaceId=95fa80c9-fc09-41c7-a313-856f4155a90a&width=2000&userId=c51186de-849a-4003-b8ae-be16e8b5d545&cache=v2)

# Incident Response with ThreatLocker

In our Incident Response Class, we'll dive into the process of analyzing logs
to uncover any malicious behavior residing on a machine. This involves
scrutinizing various log entries to identify and understand potential threats
lurking within the system, in this case we will be going through one of our
simple playbooks. Additionally, we will craft a comprehensive report detailing
our findings, which will serve to inform the client about the detected malware
and its potential impact on their system.

# Scenario

> REKT Corp. has been breached. Their Administrator "Frank Pint" has reached out
> to us in hope we can conduct an investigation and find out what happened.

**Email:**

![Email](https://curious-cloth-153.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F95fa80c9-fc09-41c7-a313-856f4155a90a%2F76563e23-8e31-405c-8601-aae2feddb109%2FUntitled.png?table=block&id=a1e33980-3673-4128-81ae-9aea81b3334a&spaceId=95fa80c9-fc09-41c7-a313-856f4155a90a&width=1900&userId=&cache=v2)

# Gather Information

When collecting information, understanding the tools at your disposal is crucial
for obtaining the necessary data. In this context, our **Unified Audit** proves
invaluable as it enables us to access logs tailored to specific machines. These
logs encompass a wide array of information, including network activity, command
line parameters, file executions, registry modifications, and other pertinent
details. Leveraging such a tool provides us with comprehensive insights into the
system's activities, facilitating effective incident response and investigation
processes.

# Playbook

A playbook acts as our blueprint or strategic guide when encountering incidents
like the one described. It outlines specific steps and procedures to be followed
in response to various scenarios, ensuring a systematic and organized approach
to incident management. These playbooks contain detailed instructions on the
actions to be taken, such as identifying the incident, containing it, mitigating
its effects, and ultimately resolving the issue. They serve as a vital resource
for incident responders, providing clarity and direction during high-stress
situations.

## Phase 1 [Incident Scope]

The Scope encapsulates the area affected and time of potential breach. This helps
us focus on the assets that are truly in danger.

* Gather Time of Attack:
* Gather Organization Name:
* Gather Hostname(s):

## Phase 2 [Machine State]
Understanding the state in which the machine is in is crucial as we can better 
understand how that machine is intended to behave.
* **Learning Mode**: Automatically Catalogs Files based on users behaviour and 
* learns them(Storing File Dat and Allowing it to run Next Time)
* **Monitor Mode**: Does not take action on the End Point. This just monitors 
* the behaviour on the machine to forward logs
* **Secure Mode**: Only allows application previously trusted to run, 
* everything else is blocked by default

### Phase 3 [Mystery Phase]
This is step involves the use of our new tool we just released, we will 
cover this later in this class.

This is step involves the use of our new tool we just released, we will 
cover this later in this class.

### Phase 4 [Network Activity]
Check network logs in the Unified Audit for any unusual behaviour. 
Red Flags indicators are  external connection to common ports of 
entry(Ports that can are commonly used gain access to a device):
| PROTOCOL | PORT |
| --- | --- |
| SSH  | 22 |
| RDP | 3389 |
| FTP | 21 |
| SMB | 445 |

### Phase 5 [Proof of Execution]
Look for signs of permitted execution, this lets us know what actually ran on 
the system.
* Filter Unified Audit: (Action = Permit) (Action Type = Execute) 

### Phase 6 [Registry Changes]
Check logs for registry changes, in many occasions this can be a sign of a 
breach or persistence.
Below is a list of Registry Keys often targeted by malicious actors for 
persistence:
Run and RunOnce Keys:

1. **Run and RunOnce Keys**:
   - `HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run`
   - `HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\RunOnce`
   - `HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run`
   - `HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\RunOnce`
2. **Run Services Keys**:
   - `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services`
3. **Explorer Run Keys**:
   - `HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\StartupApproved\Run`
   - `HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\StartupApproved\Run32`
4. **AppInit_DLLs**:
   - `HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\Windows`
5. **Winlogon**:
   - `HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\Winlogon`
6. **Shell Registry Key**:
   - `HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer\Run`
7. **Userinit Key**:
   - `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon`
8. **Scheduled Tasks**:
   - `HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\Schedule\TaskCache\Tree`
9. **Service DLL**:
   - `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services`
10. **App Paths Key**:
    - `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\App Paths`
11. **Install Location**:
    - `HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Uninstall`
12. **Office Registry Keys** (for macros and add-ins):
    - `HKEY_CURRENT_USER\Software\Microsoft\Office`
    - `HKEY_CURRENT_USER\Software\Microsoft\Office\15.0\Word\Security`
13. **Browser Extensions**:
    - `HKEY_LOCAL_MACHINE\Software\Microsoft\Internet Explorer\Extensions`
    - `HKEY_CURRENT_USER\Software\Microsoft\Internet Explorer\Extensions`
14. **Lsaas and LSASS Protection**:
    - `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa`
15. **Firewall Rules**:
    - `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\SharedAccess\Parameters\FirewallPolicy\FirewallRules`
### Phase 7 [Explicit Denies]
This applies to machine in **Secure Mode**, when a machine is in secure mode 
malware mayb be attempting to run against it but gets denied by our platform 
in cases like these we still want to know about it and recrod the data so we 
can further understand where the attakc is coming from and how to clode that 
point of entry for the attacker

### Phase 8 [Record Findings]
This stage involves organizing all your discoveries and transforming them 
into a format that is easy to understand, allowing you to effectively 
communicate your findings to the customer. Additionally, it includes saving the 
organized information to later modify and generate a comprehensive report. 

Record all your findings, this can include but not be limited to

* Execution Time Stamps: (List of malicious binaries executed and time stamps)
* Explanation: (Explanation of what those executable do)
* Hashes: (Provide SHA256 or MD5 hashes)
* Malicious Network Activity: (Any Malicious Connection)
* Other Malicious Behavior: (Signs of persistence, Data Exfiltration)

# ThreatLocker Ops

**ThreatLocker Ops** is a policy-based Endpoint Detection and Response (EDR)
solution. This EDR addition to the ThreatLocker Endpoint Protection Platform
watches for unusual events or Indicators of Compromise (IoCs).

**ThreatLocker Ops** facilitates our job by alerting us based on malicious
behavior, in this case we can navigate to the **Response Center** and see if any
indicators of compromise(Threats) have been detected

### Response Center
The **Response Center** allows you to manage all your **ThreatLocker Ops** 
alerts from one place under the **Threats** tab. 

![ResponseCenter](https://curious-cloth-153.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F95fa80c9-fc09-41c7-a313-856f4155a90a%2Fc327b279-dc7c-4c99-980b-9535f5945605%2FUntitled.png?table=block&id=8a21489f-ef50-406a-aedf-5543dba9880c&spaceId=95fa80c9-fc09-41c7-a313-856f4155a90a&width=2000&userId=&cache=v2)

### Managing Alerts
Alerts in the **Response Center** are organized by machine(hostname) so if you 
click an alert you will see all the alerts relative to that machine.

![ManageAlerts1](https://curious-cloth-153.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F95fa80c9-fc09-41c7-a313-856f4155a90a%2Fce358ff6-8ea6-4356-aa50-586a5ae9557d%2FUntitled.png?table=block&id=fb998b7f-f395-4c21-9eb0-a1ed82db1c4b&spaceId=95fa80c9-fc09-41c7-a313-856f4155a90a&width=2000&userId=&cache=v2)
# Report Findings

This is the final step, we will compile a detailed report outlining all our 
discoveries and any recommended actions. This report serves as a comprehensive document
summarizing our investigation, findings, and suggested steps to address any
identified issues or vulnerabilities.

**Report:**

[Report Security Breach Incident Response Form](Report/Security_Breach_Incident_Response_Form.docx)
