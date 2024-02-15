![Banner](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F95fa80c9-fc09-41c7-a313-856f4155a90a%2F5a1947a3-a09e-4c51-94a1-8b6fffcf0521%2Fcomputer_fire_blog_extinguisher-01.png?table=block&id=d048762c-1984-419d-8fcd-a60cc03e9392&spaceId=95fa80c9-fc09-41c7-a313-856f4155a90a&width=2000&userId=c51186de-849a-4003-b8ae-be16e8b5d545&cache=v2)
# Incident Response with ThreatLocker

In our Incident Response Class, we'll dive into the process of analyzing logs to uncover any malicious behavior residing on a machine. This involves scrutinizing various log entries to identify and understand potential threats lurking within the system, in this case we will be going through one of our simple playbooks. Additionally, we will craft a comprehensive report detailing our findings, which will serve to inform the client about the detected malware and its potential impact on their system.

# Scenario

REKT Corp. has been breached. Their Administrator has reached out to us in hope we can conduct an investigation and find out what happened.

**Email:**

```markdown
Hello Team,
 
I need assistance. I have a compromised machine and need to find out what happened. These Unified Audit Logs are not making any sense to me.
I am looking for help from someone who can help me break things down. I also need to know what ThreatLocker can do for mitigation.
 
Here are the details that are known by us so far:
This happened between the hours of 11:20am - 1:30pm on 2/14/24.
Machine is REKT-FRONT-OFFI
The admin that was logged in at that time informed us that they were abruptly logged out during their session and didn't know why.
When the admin logged back into the machine, they noticed that the computer's performance was significantly slower than usual.
When looking in the Unified Audit, there are an abundnance of network logs that I need to make sense of.
 
Thank you for your support in addressing this critical issue. Please feel free to reach back out and give me a call at your earliest availability.
 
Regards,
Frank Pint
Technical Administrator
407-555-5555
```

# Gather Information

When collecting information, understanding the tools at your disposal is crucial for obtaining the necessary data. In this context, our **Unified Audit** proves invaluable as it enables us to access logs tailored to specific machines. These logs encompass a wide array of information, including network activity, command line parameters, file executions, registry modifications, and other pertinent details. Leveraging such a tool provides us with comprehensive insights into the system's activities, facilitating effective incident response and investigation processes.

# Playbook

A playbook acts as our blueprint or strategic guide when encountering incidents like the one described. It outlines specific steps and procedures to be followed in response to various scenarios, ensuring a systematic and organized approach to incident management. These playbooks contain detailed instructions on the actions to be taken, such as identifying the incident, containing it, mitigating its effects, and ultimately resolving the issue. They serve as a vital resource for incident responders, providing clarity and direction during high-stress situations.

**Strategy:** 

![playbook](https://curious-cloth-153.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F95fa80c9-fc09-41c7-a313-856f4155a90a%2F67bce66b-8450-4e4a-af43-54e4ad3a752b%2FUntitled.png?table=block&id=6a3c14cb-c9fd-4004-888f-05c8f0965d15&spaceId=95fa80c9-fc09-41c7-a313-856f4155a90a&width=1920&userId=&cache=v2)


# ThreatLocker Ops

**ThreatLocker Ops** is a policy-based Endpoint Detection and Response (EDR) solution. This EDR addition to the ThreatLocker Endpoint Protection Platform watches for unusual events or Indicators of Compromise (IoCs). 

**ThreatLocker Ops** facilitates our job by alerting us based on malicious behaviour, in this case we can navigate to the **Response Center** and see if any indicators of compromise(Threats) have been detected

# Report Findings

During this phase, we compile a detailed report outlining all our discoveries and any recommended actions. This report serves as a comprehensive document summarizing our investigation, findings, and suggested steps to address any identified issues or vulnerabilities.

**Report:** 

[Untitled](Incident%20Response%20with%20ThreatLocker%20d048762c1984419d8fcda60cc03e9392/Untitled.docx)
