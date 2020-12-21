---
title: "CAR-2020-12-001:Detecting windows log clearing with wevtutil"
layout: analytic
submission_date: 2020/12/02
information_domain: Host
subtypes: Process
analytic_type: TTP
contributors: Cyware Labs
applicable_platforms: Windows
---

In an attempt to clear traces after compromising a machine, threat actors often try to clear Windows Event logs. This is often done using “wevtutil”, a legitimate tool provided by Microsoft. This action interferes with event collection and notification, and may lead to a security event going undetected, thereby potentially leading to further compromise of the network.


### ATT&CK Detection

|Technique|Subtechnique(s)|Tactic(s)|Level of Coverage|
|---|---|---|---|
|[Indicator Removal on Host](https://attack.mitre.org/techniques/T1070/)|[Clear Windows Event Logs](https://attack.mitre.org/techniques/T1070/001)|[Defense Evasion](https://attack.mitre.org/tactics/TA0005 )|Low|

### Data Model References

|Object|Action|Field|
|---|---|---|
|[process](/data_model/process) | [start](/data_model/process#create) | [user](/data_model/process#command_line) |

### Implementations

#### Splunk search - Detecting log clearing with wevtutil

This search query looks for an instance where wevtutil is invoked along with a command that may cause the system to remove Windows Event logs.

```
index=* sourcetype=_your__windows__sysmon_sourcetype EventCode=1 Image=*wevtutil* CommandLine=*cl* (CommandLine=*System* OR CommandLine=*Security* OR CommandLine=*Setup* OR CommandLine=*Application*) 
| table _time host Image CommandLine
```