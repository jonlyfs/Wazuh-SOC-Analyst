# 07 — File Integrity Monitoring and rules (Part 5)

## File Integrity Monitoring test
Use a disposable test directory/file in your isolated endpoint. Do not monitor sensitive system paths without understanding the performance and privacy impact.

| Field | Value |
|---|---|
| Test path | TODO |
| FIM configuration location | TODO |
| Change made | TODO |
| Expected event | TODO |
| Observed alert | TODO |

## Custom rule development
Before adding a rule:
1. Define the behavior you want to detect.
2. Identify the event fields and existing rule/decoder context.
3. Choose an unused local rule ID in the supported range for your version.
4. Write a narrowly scoped rule.
5. Validate syntax with the Wazuh tools/documentation for your version.
6. Test both matching and non-matching events.
7. Document false positives and tuning.

Place tested local rules in `../rules/`. The included rules directory contains guidance only; no tutorial-specific rule is invented here.
