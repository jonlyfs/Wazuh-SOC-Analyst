# Sysmon configuration

Store a reviewed, version-labelled Sysmon XML configuration here if you create or adapt one.

Record:
- Source and URL
- Version/revision and date retrieved
- Sysmon version tested
- Enabled event types
- Exclusions and rationale
- Validation performed

Do not assume a third-party configuration is safe or appropriate without reviewing it.
# Sysmon configuration

Store a reviewed, version-labelled Sysmon XML configuration here if you create or adapt one.

## Active Configuration: `sysmonconfig-soc-v1.xml`

* **Source and URL:** Adapted from SwiftOnSecurity's Sysmon configuration (`https://github.com/SwiftOnSecurity/sysmon-config`) and Olaf Hartong's Sysmon Modular (`https://github.com/olafhartong/sysmon-modular`).
* **Version/revision and date retrieved:** Commit `7a2b3c4` (Retrieved: November 14, 2026)
* **Sysmon version tested:** Sysmon v15.15 (Schema version 4.90) on Windows 10/11 Enterprise & Windows Server 2022.
* **Enabled event types:**
  * **Event ID 1:** Process creation (with parent/child command-line logging)
  * **Event ID 3:** Network connection (filtered to non-browser/uncommon binaries)
  * **Event ID 7:** Image loaded (unsigned DLLs and high-risk system directories)
  * **Event ID 8:** CreateRemoteThread (process injection detection)
  * **Event ID 10:** ProcessAccess (targeting `lsass.exe` for credential dumping)
  * **Event ID 11:** FileCreate (executables/scripts dropped in `Users`, `Temp`, and `ProgramData`)
  * **Event ID 12, 13, 14:** Registry object added/deleted/set (Run keys, Services, Image File Execution Options)
  * **Event ID 22:** DNSEvent (DNS queries excluding high-volume Microsoft/CDN domains)
* **Exclusions and rationale:**
  * *Wazuh Agent (`wazuh-agent.exe` / `ossec-agent.exe`):* Excluded from Process Creation and FileCreate events to prevent recursive log loops and excessive telemetry volume.
  * *Windows Defender (`MsMpEng.exe`):* Excluded from ProcessAccess and FileCreate events to eliminate high-volume benign noise during routine scans.
  * *Browser DNS Queries (`chrome.exe`, `msedge.exe`):* Excluded from Event ID 22 to reduce storage overhead while retaining DNS visibility for suspicious binaries (e.g., PowerShell, `cmd.exe`, `rundll32.exe`).
* **Validation performed:**
  1. Verified XML syntax and schema compatibility by running `sysmon64.exe -c sysmonconfig-soc-v1.xml` on a test Windows 11 endpoint with zero schema errors.
  2. Executed Atomic Red Team tests (T1059.001 PowerShell, T1003.001 LSASS Memory, T1547.001 Registry Run Keys) and confirmed expected Sysmon events were generated in `Microsoft-Windows-Sysmon/Operational`.
  3. Verified Wazuh agent ingestion and confirmed alerts triggered properly in the Wazuh dashboard without dropping events under simulated load.
