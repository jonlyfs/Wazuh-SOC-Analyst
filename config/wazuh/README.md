# Wazuh configuration

Store sanitized configuration snippets or exports here. Include the Wazuh version, file path, purpose, change rationale, and validation steps. Never commit secrets, passwords, certificates, or production configuration.


# Wazuh configuration

Store sanitized configuration snippets or exports here. Include the Wazuh version, file path, purpose, change rationale, and validation steps. Never commit secrets, passwords, certificates, or production configuration.

## Configuration Snippet: `ossec-soc-snippet.conf`

* **Wazuh Version:** Wazuh v4.9.2 (Manager & Windows Agent)
* **File Path:** 
  * Manager: `/var/ossec/etc/ossec.conf`
  * Windows Agent: `C:\Program Files (x86)\ossec-agent\ossec.conf`
* **Purpose:** Configure Windows endpoints to forward Sysmon operational logs to the Wazuh Manager, enable real-time File Integrity Monitoring (FIM) on high-value directories, and activate automated host isolation/blocking for brute-force attempts.
* **Change Rationale:**
  * **Sysmon Ingestion (`localfile`):** Windows Security logs lack granular command-line and process injection visibility. Forwarding `Microsoft-Windows-Sysmon/Operational` events enables custom SOC detection rules for living-off-the-land binaries (LOLBins) and credential dumping.
  * **Real-Time FIM (`syscheck`):** Standard periodic scans miss rapid malware staging. Adding `realtime="yes"` to `Startup` and `System32` folders provides immediate alerting on unauthorized persistence or binary tampering.
  * **Vulnerability Detection & Active Response:** Enables continuous CVE matching against installed packages and automated firewall drops for repeated authentication failures (Rule 5712 / 5763).
* **Validation Steps:**
  1. Validated XML syntax on the Wazuh Manager using `/var/ossec/bin/wazuh-analysisd -t` (returned `0` errors).
  2. Restarted the Wazuh Manager (`systemctl restart wazuh-manager`) and Windows Agent (`Restart-Service WazuhSvc`).
  3. Verified Sysmon event ingestion by querying `archives.json` / Wazuh Dashboard for `data.win.system.channel: "Microsoft-Windows-Sysmon/Operational"`.
  4. Dropped a test file into `C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup` and confirmed an immediate FIM alert (Rule 554) appeared in the dashboard.
