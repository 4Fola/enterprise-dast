# Enterprise Dynamic Application Security Testing (DAST) Lab

## Overview
This repository demonstrates an enterprise-grade Dynamic Application Security Testing (DAST) framework designed to identify, validate, and remediate application and API security vulnerabilities across CI/CD pipelines and staging environments.

## Architecture & Infrastructure
- **Attacker Suite (Kali Linux):** OWASP ZAP, Burp Suite Pro, Kali CLI tools, Custom Python PoC Framework.
- **Target Microservices (Ubuntu/Docker):** 
  - Web Tier: OWASP Juice Shop (Node.js/Express)
  - API Tier: vAPI (PHP/Laravel REST API)
- **Enterprise Tool Configurations:** Policy mappings and rules for IBM AppScan Enterprise and Acunetix.

## Testing Scope & Security Coverage
| Target | Vector Category | Primary Tooling |
| :--- | :--- | :--- |
| Web Application | OWASP Top 10 (SQLi, XSS, BAC, Auth) | Burp Suite, ZAP, AppScan |
| REST API | OWASP API Top 10 (BOLA, Auth Bypass) | ZAP API Scan, Postman, Burp |
| Pipeline Security | DevSecOps Integration | ZAP Automation Framework (GitHub Actions) |

## Repository Structure
```text
├── .github/workflows/   # CI/CD Security Gate Pipelines
├── configs/            # Scanner Policies (AppScan, Acunetix, Burp, ZAP)
├── environment/        # Infrastructure as Code (Docker Compose)
├── exploits-poc/       # Python/Bash Vulnerability Validation Scripts
└── reports/            # Triage Reports, Executive Summaries, & Remediation Memos

---

### Step 3: OWASP ZAP Automation Framework Policy

To demonstrate automated enterprise DAST capabilities, we will use the **OWASP ZAP Automation Framework**. This allows ZAP to run headlessly using structured standard YAML files.

Create `configs/zap/baseline-scan.yaml` in your **Kali VM**:

```yaml
env:
  contexts:
    - name: "JuiceShop-Context"
      urls:
        - "http://<UBUNTU_IP>:3000"
      includePaths:
        - "http://<UBUNTU_IP>:3000/.*"
      excludePaths:
        - "http://<UBUNTU_IP>:3000/ftp/.*"
      authentication:
        method: "manual"
  parameters:
    failOnError: true
    progressToStdout: true

jobs:
  - type: spider
    parameters:
      maxDuration: 5
      maxDepth: 5
    name: "Spider Crawl"

  - type: activeScan
    parameters:
      maxRuleDurationInMins: 2
      concurrentScanningThreads: 4
    policyDefinition:
      defaultThreshold: "Medium"
      defaultStrength: "Medium"
    name: "Active Security Scan"

  - type: report
    parameters:
      template: "traditional-html"
      reportDir: "../../reports/raw"
      reportFile: "zap-raw-results"
    name: "Generate HTML Report"
