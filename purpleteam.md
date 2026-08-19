Exercise Overview
Technique Under Test: Credential Dumping via LSASS Memory Access (MITRE ATT&CK T1003.001).

Participants: Red Team Lead (Attacker), Blue Team SOC Analyst/Detection Engineer (Defender), Purple Team Facilitator.

Goal: Verify if the Blue Team’s EDR and SIEM trigger an alert when an attacker tries to dump passwords from the lsass.exe process memory.


Phase 1: Alignment & Baseline Verification:Pre-test preparation and telemetry check.Before running any attack code, both teams verify that logging and telemetry channels are actively sending data from the test workstation to the central SIEM.Blue Team check: Verifies that Microsoft Sysmon (System Monitor) and EDR agents are healthy and recording Process Access events (Sysmon Event ID 10).Baseline check: Confirms no active alerts exist for lsass.exe on the target machine (WORKSTATION-01).

Phase 2: Execution Attempt 1 (Standard Attack Tool):Red Team executes attack using a known utility.The Red Team member opens a command prompt on WORKSTATION-01 and executes a native Windows tool (procdump.exe from Sysinternals) to create a memory dump of the LSASS process: procdump.exe -ma lsass.exe lsass_dump.dmp , Immediate Outcome: The host EDR detects procdump.exe accessing lsass.exe, terminates the process immediately, and generates a High-Severity Alert in the EDR console.
Result: Pass (EDR Level). The EDR successfully blocked the out-of-the-box tool execution.

Phase 3: Execution Attempt 2 (Evasion Technique):Red Team uses API call masking to bypass basic signatures.To test the boundaries of their detection, the Red Team uses a custom script that calls the native Windows API function MiniDumpWriteDump directly without using procdump.exe or Mimikatz.
[ Custom Script ] ---> [ Direct API: MiniDumpWriteDump() ] ---> [ Read lsass.exe Memory ]
Outcome: The custom script runs successfully and writes lsass_memory.dmp to disk. No block or high-priority EDR alert is triggered.

Phase 4: Telemetry Analysis & Gap Identification:Joint analysis to figure out why the evasion succeeded.Both teams sit together to analyze the logs generated during Attempt 2:Observation: The EDR logged the process creation event, but failed to flag the specific memory access rights requested.Log Gap: The SIEM was not receiving Sysmon Event ID 10 (Process Access) for lsass.exe because the local Sysmon configuration file filtered out generic memory read requests to prevent log bloating.Root Cause Identified: The detection logic relied on known tool names (procdump, mimikatz) rather than behavioral API calls (PROCESS_VM_READ access rights on lsass.exe).

Phase 5: Rule Tuning & Detection Engineering:Blue Team writes a behavioral detection rule.The Blue Team updates the detection logic on the spot:Sysmon Config Update: Updated to capture all process access requests targeting lsass.exe where GrantedAccess includes 0x0010 (PROCESS_VM_READ) or 0x1400 (PROCESS_QUERY_INFORMATION + PROCESS_VM_READ).SIEM Correlation Rule: Created a custom Sigma/SIEM alert rule:Condition: Any process (excluding authorized Windows binaries like csrss.exe) requesting read access to lsass.exe.

Phase 6: Re-Testing & Validation:Verifying the new detection rule works in real time.The Red Team executes the exact same custom API script from Phase 3 again.Result: Within 5 seconds, the SIEM triggers a Critical Severity Alert:"Unusual Process Access to LSASS Memory Detected on WORKSTATION-01".Confirmation: The Purple Team logs the exercise as Successfully Remediated.
