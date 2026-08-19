Exercise Overview
Technique Under Test: Credential Dumping via LSASS Memory Access (MITRE ATT&CK T1003.001).

Participants: Red Team Lead (Attacker), Blue Team SOC Analyst/Detection Engineer (Defender), Purple Team Facilitator.

Goal: Verify if the Blue Team’s EDR and SIEM trigger an alert when an attacker tries to dump passwords from the lsass.exe process memory.


Phase 1: Alignment & Baseline Verification:Pre-test preparation and telemetry check.Before running any attack code, both teams verify that logging and telemetry channels are actively sending data from the test workstation to the central SIEM.Blue Team check: Verifies that Microsoft Sysmon (System Monitor) and EDR agents are healthy and recording Process Access events (Sysmon Event ID 10).Baseline check: Confirms no active alerts exist for lsass.exe on the target machine (WORKSTATION-01).

Phase 2: Execution Attempt 1 (Standard Attack Tool):Red Team executes attack using a known utility.The Red Team member opens a command prompt on WORKSTATION-01 and executes a native Windows tool (procdump.exe from Sysinternals) to create a memory dump of the LSASS process: procdump.exe -ma lsass.exe lsass_dump.dmp
