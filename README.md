# Enterprise Dynamic Application Security Testing (DAST)

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