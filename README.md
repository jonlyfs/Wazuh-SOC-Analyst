# Wazuh SOC Analyst 

A practical SOC home lab based on the **Wazuh SOC Analyst Challenge** playlist by [MyDFIR](https://www.youtube.com/@MyDFIR).

This repository documents the lab build and the analyst workflow: deploy Wazuh, connect Windows telemetry through an agent and Sysmon, generate and review events, create a SOC dashboard, configure File Integrity Monitoring (FIM), build detection rules, use Active Response, and investigate alerts.


## Learning objectives

- Understand Wazuh's role as a SIEM/XDR platform.
- Deploy and validate a Wazuh server.
- Enroll endpoint agents and collect Windows telemetry with Sysmon.
- Generate benign test activity and trace it into Wazuh.
- Build a dashboard that answers useful SOC questions.
- Monitor file changes and write/test custom rules.
- Configure and safely test Active Response.
- Triage an alert and document an investigation.

## Tutorial sequence

The playlist contains nine videos. The sequence below reflects the playlist titles; the final two topics are confirmed as Parts 6 and 7.

| # | Video | Repository notes |
|---|---|---|
| 1 | Build a SOC Lab with Wazuh (Free Challenge + Giveaway) | Scope, objectives, and lab plan |
| 2 | [Wazuh 101: What is Wazuh?](https://www.youtube.com/@MyDFIR) | Platform concepts and architecture |
| 3 | Build a SOC Lab with Wazuh: Deploy the Server (Part 1) | `docs/03-server-deployment.md` |
| 4 | Build a SOC Lab with Wazuh: Connect Agents + Sysmon (Part 2) | `docs/04-agents-sysmon.md` |
| 5 | Build a SOC Lab with Wazuh: Generate + Read Telemetry (Part 3) | `docs/05-telemetry-analysis.md` |
| 6 | Build a SOC Lab with Wazuh: Build Your SOC Dashboard (Part 4) | `docs/06-dashboard.md` |
| 7 | Build a SOC Lab with Wazuh: File Integrity Monitor + Rules (Part 5) | `docs/07-fim-and-rules.md` |
| 8 | Build a SOC Lab with Wazuh: Active Response (Part 6) | `docs/08-active-response.md` |
| 9 | Build a SOC Lab with Wazuh: The Investigation (Part 7) | `docs/09-investigation.md` |

Playlist: https://www.youtube.com/playlist?list=PLHmErnFy_2zk

## Lab architecture

Typical learning topology (adapt to the actual tutorial and your available resources):

```text
                 Isolated lab network
                         |
              +---------------------+
              | Wazuh server        |
              | Manager / Indexer / |
              | Dashboard           |
              +---------------------+
                         ^
                         | Agent events
                         |
              +---------------------+
              | Windows endpoint    |
              | Wazuh agent         |
              | Sysmon              |
              +---------------------+
```

Do not expose management ports or the dashboard to the public internet. Use host-only/NAT networking or an equivalently isolated environment unless you understand and have secured the network design.

## Repository layout

```text
.
├── README.md
├── LICENSE
├── .gitignore
├── docs/                 # Tutorial-aligned build and analyst notes
├── config/
│   ├── sysmon/           # Sysmon config guidance
│   └── wazuh/            # Wazuh config guidance
├── rules/                # Custom Wazuh rules (tested examples only)
├── scripts/              # Safe lab helper scripts
├── investigations/       # Alert investigation write-ups
└── screenshots/          # Redacted evidence from your own lab
```

## Getting started

1. Read `docs/01-project-overview.md` and define your isolated lab.
2. Follow the official Wazuh installation documentation for the release you are installing: https://documentation.wazuh.com/current/installation-guide/index.html
3. Record your exact versions, operating systems, network mode, and resource allocation.
4. Complete the tutorial stages in order and update the matching document with your own verified steps.
5. Test telemetry, FIM, rules, and response only with benign activity on systems you own or are authorized to administer.
6. Add redacted screenshots and an investigation report based on events generated in your lab.

## Validation checklist

- [ ] Wazuh components are healthy and dashboard is accessible inside the lab.
- [ ] Windows endpoint agent is enrolled and active.
- [ ] Sysmon events are generated and visible in Wazuh.
- [ ] A controlled test event can be found and explained.
- [ ] Dashboard panels answer defined SOC questions.
- [ ] FIM detects a controlled file modification.
- [ ] Custom rule is syntax-checked and tested against expected and non-matching events.
- [ ] Active Response is tested safely and its effect is verified.
- [ ] Investigation report includes evidence, timeline, conclusion, and remediation.

## Safety, privacy, and operational notes

- Only monitor systems you own or have explicit authorization to monitor.
- Keep attack simulation and response testing inside an isolated lab.
- Active Response can interrupt network access or processes. Do not enable disruptive actions on production endpoints.
- Never commit passwords, API keys, private keys, enrollment secrets, raw sensitive logs, or unredacted screenshots.
- Review and test all downloaded configurations and scripts before use.
- Do not represent simulated activity as a real-world incident.

## References

- [MyDFIR YouTube channel](https://www.youtube.com/@MyDFIR)
- [Wazuh documentation](https://documentation.wazuh.com/)
- [Wazuh installation guide](https://documentation.wazuh.com/current/installation-guide/index.html)
- [Wazuh user manual](https://documentation.wazuh.com/current/user-manual/index.html)
- [Wazuh ruleset documentation](https://documentation.wazuh.com/current/user-manual/ruleset/index.html)
- [Microsoft Sysmon](https://learn.microsoft.com/sysinternals/downloads/sysmon)

## Project status

This repository starts as a documentation and lab-tracking scaffold. Replace every `TODO` with details from your own environment. Add configuration exports and screenshots only after sanitizing them.
