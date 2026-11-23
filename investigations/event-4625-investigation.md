# Windows Failed Logon Investigation – Event ID 4625

## 1. Objective

The objective of this investigation was to simulate failed Windows login attempts, collect Windows Security Event ID 4625 logs using Wazuh SIEM, and analyze the events to identify relevant authentication details.

## 2. Lab Environment

| Component | Details |
|---|---|
| SIEM | Wazuh |
| Target System | Windows 11 Virtual Machine |
| Agent Name | Windows-11-VM-BruteForce-Lab |
| Agent ID | 002 |
| Event Source | Windows Security |
| Event ID | 4625 – Failed Logon |
| Virtualization | VirtualBox |

## 3. Scenario

Multiple incorrect login attempts were performed on the Windows 11 virtual machine to generate failed authentication events.

The Windows Security logs were collected by the Wazuh agent and analyzed through the Wazuh dashboard.

## 4. Detection Details

| Field | Value |
|---|---|
| Event ID | 4625 |
| Event Description | An account failed to log on |
| Logon Type | 2 – Interactive |
| Source IP Address | 127.0.0.1 |
| Process Name | C:\Windows\System32\svchost.exe |
| Subject Username | MYPC$ |
| Subject Domain | WORKGROUP |
| Target Username | Not populated |
| Target Domain | Not populated |
| Status | 0xc000006d |
| Substatus | 0xc0000380 |
| Wazuh Rule ID | 60122 |
| Wazuh Rule Level | 5 |
| Alert Description | Logon Failure – Unknown user or bad password |

## 5. MITRE ATT&CK Mapping

| Field | Mapping |
|---|---|
| Tactic | Credential Access |
| Tactic ID | TA0006 |
| Technique | Brute Force |
| Technique ID | T1110 |
| Sub-technique | Password Guessing |
| Sub-technique ID | T1110.001 |

### Description

Password guessing involves attempting to authenticate to an account by repeatedly guessing passwords without prior knowledge of the correct credentials.

### Relevance to This Lab

Multiple incorrect login attempts were intentionally generated in the Windows 11 VM to simulate failed authentication activity. These events were detected by Wazuh using Windows Security Event ID 4625.

This activity is mapped to MITRE ATT&CK **T1110.001 – Password Guessing** as a simulated scenario. The collected logs demonstrate failed authentication activity, but do not independently confirm a real brute-force attack.

**MITRE ATT&CK Reference:**  
https://attack.mitre.org/techniques/T1110/001/


## 6. Investigation

The failed logon events were detected in Wazuh Threat Hunting after the Windows 11 VM agent successfully connected to the Wazuh manager.

The investigation identified the following:

- Windows Security Event ID 4625 confirmed failed authentication attempts.
- The events were associated with Logon Type 2, indicating interactive logon.
- The source IP address was 127.0.0.1, indicating local activity.
- The process associated with the event was svchost.exe.
- The Subject account was MYPC$ under the WORKGROUP domain.
- Target username and target domain fields were not populated.
- Wazuh generated alerts under Rule ID 60122 with severity Level 5.
- The events were also verified using Windows Event Viewer.

## 7. Findings

- Failed authentication activity was successfully generated and detected.
- Windows Security logs were successfully collected by Wazuh.
- Wazuh identified the failed logon events and generated alerts.
- The activity originated locally from the Windows system.
- The target account could not be identified from the available event fields.
- The observed events alone do not establish that a real brute-force attack occurred.

## 8. Conclusion

This lab successfully demonstrated the detection and investigation of Windows failed logon events using Wazuh SIEM.

Event ID 4625 was collected and analyzed, and the associated authentication details were reviewed in the Wazuh dashboard and Windows Event Viewer.

Although repeated failed login attempts can be associated with brute-force activity, the available evidence in this investigation was insufficient to classify the activity as a confirmed malicious brute-force attack. Additional event correlation and investigation would be required.

## 9. Evidence

### Screenshots

- `../screenshots/01-alert-overview.png`
- `../screenshots/02-event-details.png`
- `../screenshots/03-account-fields.png`
- `../screenshots/04-event-viewer.png`

## 10. Tools Used

- Wazuh SIEM
- Windows 11
- Windows Event Viewer
- VirtualBox
