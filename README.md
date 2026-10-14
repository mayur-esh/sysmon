# sysmon
My sysmon configs

| Config                                                                                                                       | Purpose                                          |
| ---------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------ |
| [BigDiskBuster.xml](BigDiskBuster.xml)                                                                                       | Temp and Defender staging file create/delete     |
| [nightmare-eclipse-HardBreacher-GreenSection-PrettyPrague.xml](nightmare-eclipse-HardBreacher-GreenSection-PrettyPrague.xml) | AV and NVIDIA PoC overlay                        |
| [PatchTuesday-2026-10.xml](PatchTuesday-2026-10.xml)                                                                         | Microsoft Patch Tuesday, October 2026 overlay    |
             

Sigma rules that use these configs: [mayur-esh/sigma-rules](https://github.com/mayur-esh/sigma-rules).

# Patch Tuesday, October 2026: Sysmon overlay

[`PatchTuesday-2026-10.xml`](PatchTuesday-2026-10.xml) is an include-only overlay (schema 4.90, Sysmon v15+). It collects telemetry and tags it with a `RuleName`; alert in your SIEM. Sysmon runs one config at a time, so merge these RuleGroups into your baseline instead of loading this file alone. Companion to the PenTestIT October 2026 Patch Tuesday post - https://pentestit.com/microsoft-patch-tuesday-october-2026-exploitability-and-patching-priority/

| RuleName                                     | Event | CVE                                                                                                                 |
| -------------------------------------------- | ----- | ------------------------------------------------------------------------------------------------------------------- |
| CVE-2026-96951-scripthost-from-download-path | 1     | CVE-2026-96951                                                                                                      |
| Outlook-RCE-any-child                        | 1     | CVE-2026-70242, -70291, -88156, -97016, -97749                                                                      |
| CriticalService-crash-WerFault               | 1     | network RCEs (LSASS, DNS, MSMQ, Print, Exchange, Hyper-V, sshd)                                                     |
| NetworkService-spawns-shell                  | 1     | CVE-2026-83869, -96113 (DNS), -100202, -108460 (MSMQ), -70147, -70153 (Print), -70571 (Exchange), -105733 (Hyper-V) |
| CVE-2026-70571-Exchange-worker-shell         | 1     | CVE-2026-70571                                                                                                      |
| MSMQ-mqsvc-connection                        | 3     | CVE-2026-100202, -108460                                                                                            |
| DHCP-failover-647                            | 3     | CVE-2026-96934                                                                                                      |
| CVE-2026-97816-sshd-connection               | 3     | CVE-2026-97816                                                                                                      |
| CVE-2026-108398-unsigned-userpath-load       | 7     | CVE-2026-108398                                                                                                     |
| CVE-2026-96951-outlook-attachment-write      | 11    | CVE-2026-96951                                                                                                      |
| CVE-2026-96951-browser-download-write        | 11    | CVE-2026-96951, CVE-2026-108463                                                                                     |
| CVE-2026-70571-Exchange-webdir-script-write  | 11    | CVE-2026-70571                                                                                                      |
| CVE-2026-96951-motw-stream                   | 15    | CVE-2026-96951                                                                                                      |

Missing-MOTW hunt: an EID 11 from the two `*-write` rules with no EID 15 `CVE-2026-96951-motw-stream` for the same `TargetFilename` within a few seconds.

Alerting logic and tests: [mayur-esh/sigma-rules](https://github.com/mayur-esh/sigma-rules/tree/main/PatchTuesday-2026-10).
