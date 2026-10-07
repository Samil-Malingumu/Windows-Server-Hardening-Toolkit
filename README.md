# Windows Server Hardening Toolkit

Audit a Windows Server against a CIS Benchmark hardening checklist, fix what is safe to fix, and report the before and after compliance score. Written in PowerShell.

> **Status:** [In development / v1.0] · **License:** [MIT] · **Not affiliated with or endorsed by CIS, Microsoft or any vendor.**

## Why this exists

Hardening servers by hand is slow, inconsistent and hard to prove. This toolkit turns a checklist into repeatable code: run an audit, see exactly what fails, apply fixes with a safety net, and re-run the audit to confirm the improvement.

It is based on my experience hardening Windows Server environments for client organizations. This repository was written from scratch and contains no client data, scripts or names.

## What it does

- **Audit:** checks [N] controls and returns Pass, Fail or Not applicable, with the current value
- **Remediate:** runs in dry-run mode by default, and saves a rollback file before every change
- **Report:** produces an HTML and CSV report with a compliance percentage and a before-and-after comparison

### Control areas covered

- Account and password policy
- Local users, groups and administrator rights
- Audit policy and event log settings
- Network settings and legacy protocols
- Services and features that should be disabled
- Remote Desktop and PowerShell remoting
- Windows Update and Defender settings
- Windows server firewalls

Controls are defined in `controls/controls.json` with my own IDs and plain-language descriptions.

## Requirements

- Windows Server 2019 and above
- Windows PowerShell 5.1 or later
- Administrator rights
- A **test machine**. Do not run remediation on production first.

## Quick start

```powershell
# 1. Open PowerShell as administrator on a TEST server
# 2. Run the audit
.\src\Invoke-Audit.ps1

# 3. Build the report
.\src\New-Report.ps1

# 4. Preview fixes without changing anything
.\src\Invoke-Remediate.ps1 -WhatIf

# 5. Apply fixes, then audit again to see the new score
.\src\Invoke-Remediate.ps1
.\src\Invoke-Audit.ps1
```



## Sample result

Run on a fresh Windows Server lab VM:

| Stage | Compliance |
|---|---|
| Before remediation | [X%] |
| After remediation | [Y%] |

See [`docs/sample-report.html`](docs/sample-report.html). This report comes from my own lab VM only.

## Safety

- Audit-only unless you ask for changes
- `-WhatIf` shows what would change
- A rollback file is saved before every change (see [`docs/rollback.md`](docs/rollback.md))
- Every change is logged
- Always take a snapshot or backup first

Some controls can break applications or remote access if applied blindly. Read each control description and test before using this in your own environment.

## Repository layout

```
src/            audit, remediation and report scripts
controls/       control definitions (controls.json)
tests/          Pester tests
docs/           sample report and rollback guide
terraform/      optional Azure Policy example
.github/        lint and test workflow
```

## Tests

```powershell
Invoke-Pester .\tests
```

A GitHub Actions workflow runs PSScriptAnalyzer and Pester on every push.

## Roadmap

- [ ] More controls and a Server-version selector
- [ ] Group Policy export comparison
- [ ] Azure Policy and Terraform example
- [ ] Remote scanning of multiple servers

## Disclaimer

This tool changes system settings. Use it at your own risk, test it first, and make sure you have authorization for any server you run it on. It is not an official CIS product, and CIS Benchmarks are a separate work with their own terms of use.

## About

Built by **Samil Malingumu**, systems and network security engineer. Fortinet, Sophos, Microsoft and Azure certified.

[LinkedIn](https://www.linkedin.com/in/malingumu-samil) · kabandasamil@outlook.com
