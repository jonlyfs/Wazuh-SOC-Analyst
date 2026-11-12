# 04 — Connect agents and Sysmon (Part 2)

## Windows endpoint inventory
- Windows edition/build: TODO
- Wazuh agent version: TODO
- Sysmon version: TODO
- Sysmon configuration source/revision: TODO

## Agent enrollment
Document the enrollment method and the exact steps for your Wazuh version. Never commit enrollment passwords, authentication keys, or private network details.

## Sysmon
Sysmon provides detailed Windows system activity telemetry. Record which configuration you used and which event channels/events it enables. Review the configuration before applying it.

## Validation
- [ ] Endpoint appears in the Wazuh agent list
- [ ] Agent status is active
- [ ] Sysmon service is running
- [ ] Sysmon Operational events are being generated
- [ ] Relevant events arrive in Wazuh

References:
- https://learn.microsoft.com/sysinternals/downloads/sysmon
- https://documentation.wazuh.com/current/user-manual/capabilities/log-data-collection/index.html
