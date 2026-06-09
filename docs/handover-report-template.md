# Handover Report Template

This template is part of the **AWS Small Server Readiness Toolkit**.

It is intended to help prepare a practical handover or readiness report for a small AWS EC2-based Linux server.

This template can be used after reviewing the server with:

```text
scripts/ubuntu-server-audit.sh
docs/ec2-readiness-checklist.md
docs/security-baseline-checklist.md
docs/backup-and-restore-checklist.md
docs/ubuntu-hardening-notes.md
docs/docker-server-checklist.md
```

## Important Notice

This report template does **not** provide legal compliance certification, security certification, penetration testing, disaster recovery certification, or a guarantee against incidents.

It is intended for practical server documentation, operational handover, migration preparation, and risk communication.

For regulated data, financial data, healthcare data, government workloads, or formal compliance requirements, consult qualified AWS, security, legal, compliance, or disaster recovery professionals.

---

## 1. Report Summary

```text
Report Title:

Server Name:

Client / Organization:

Prepared By:

Review Date:

Review Type:
Initial review / Migration readiness / Production readiness / Security baseline / Backup review / Handover / Troubleshooting preparation

Overall Status:
Good / Needs attention / High risk / Unknown
```

### Executive Summary

```text
Summary of the server condition:

Main positive findings:

Main risks:

Recommended immediate actions:

Recommended next actions:
```

---

## 2. Server Identification

```text
AWS Account:

AWS Region:

Availability Zone:

EC2 Instance ID:

Instance Name Tag:

Instance Type:

Operating System:

Hostname:

Environment:
Production / Staging / Development / Test / Temporary / Unknown

Primary Purpose:

Business Owner:

Technical Owner:

Administrator Contact:
```

---

## 3. Application Overview

```text
Main Application:

Application Type:
WordPress / Laravel / Docker application / Internal tool / Static website / API server / Other

Application Path:

Runtime:
PHP / Python / Node.js / Ruby / Java / Go / Other

Web Server:
Nginx / Apache / Caddy / Traefik / Other / Unknown

Database:
MySQL / MariaDB / PostgreSQL / SQLite / Redis / RDS / Docker DB / External / None / Unknown

Docker Used:
Yes / No / Unknown

Public URL:

Admin URL:

DNS Provider:

Domain Owner:
```

### Application Notes

```text
```

---

## 4. Network and Access Summary

```text
Public IP:

Elastic IP:
Yes / No / Unknown

Security Groups Reviewed:
Yes / No / Partial

SSH Exposure:
Restricted / Open to Internet / Unknown / Not applicable

HTTP Exposure:
Required / Not required / Unknown

HTTPS Exposure:
Required / Not required / Unknown

Database Public Exposure:
No / Yes / Unknown

Admin Panel Exposure:
Protected / Public / Unknown / Not applicable
```

### Key Findings

```text
```

### Recommended Actions

```text
```

---

## 5. SSH and Administrator Access

```text
SSH Users Reviewed:
Yes / No / Partial

Password Authentication:
Disabled / Enabled / Unknown

Root SSH Login:
Disabled / Enabled / Unknown

Authorized Keys Reviewed:
Yes / No / Partial

Sudo Users Reviewed:
Yes / No / Partial

Former Administrator Access Removed:
Yes / No / Unknown / Not applicable

Emergency Access Documented:
Yes / No / Partial
```

### Key Findings

```text
```

### Recommended Actions

```text
```

---

## 6. Operating System and Hardening Summary

```text
OS Version:

Kernel Version:

Package Updates Reviewed:
Yes / No / Partial

Security Updates Pending:
No / Yes / Unknown

Reboot Required:
No / Yes / Unknown

Firewall Reviewed:
Yes / No / Partial

Failed Services Reviewed:
Yes / No / Partial

Time Synchronization:
OK / Needs review / Unknown

Disk Usage:
OK / Needs attention / Critical / Unknown

Memory Usage:
OK / Needs attention / Critical / Unknown
```

### Key Findings

```text
```

### Recommended Actions

```text
```

---

## 7. Web Server and HTTPS Summary

```text
Web Server:

HTTPS Enabled:
Yes / No / Unknown

Certificate Provider:
Let's Encrypt / AWS ACM / Commercial CA / Self-signed / Unknown

Certificate Renewal Confirmed:
Yes / No / Unknown

HTTP to HTTPS Redirect:
Yes / No / Unknown

Default Site Reviewed:
Yes / No / Not applicable

Web Root Documented:
Yes / No / Partial

Web Config Backed Up:
Yes / No / Unknown
```

### Key Findings

```text
```

### Recommended Actions

```text
```

---

## 8. Database Summary

```text
Database Type:

Database Location:
Local / Docker / RDS / External / Unknown

Database Publicly Exposed:
No / Yes / Unknown

Database Backup Configured:
Yes / No / Unknown

Database Restore Tested:
Yes / No / Unknown

Database Credentials Protected:
Yes / No / Unknown

Database Size:

Database Owner:
```

### Key Findings

```text
```

### Recommended Actions

```text
```

---

## 9. Docker Summary

Complete this section only if Docker is used.

```text
Docker Used:
Yes / No / Unknown

Docker Version:

Docker Compose Version:

Compose Project Directory:

Main Containers:

Database Containers:

Reverse Proxy Container:

Important Volumes:

Published Ports Reviewed:
Yes / No / Partial

Docker Volumes Backed Up:
Yes / No / Unknown

Containers Restart After Reboot:
Yes / No / Unknown

Docker Disk Usage:
OK / Needs attention / Critical / Unknown
```

### Key Findings

```text
```

### Recommended Actions

```text
```

---

## 10. Backup and Restore Summary

```text
Backup Owner:

Backup Method:
EBS Snapshot / AWS Backup / S3 / External server / Application backup / Database dump / Plugin / Other

Backup Storage Location:

Backup Frequency:

Retention Period:

Backup Includes Application Files:
Yes / No / Unknown

Backup Includes Database:
Yes / No / Unknown

Backup Includes Uploaded Files:
Yes / No / Unknown / Not applicable

Backup Includes Docker Volumes:
Yes / No / Unknown / Not applicable

Backup Stored Outside Original Server:
Yes / No / Unknown

Backup Encrypted:
Yes / No / Unknown

Restore Procedure Documented:
Yes / No / Partial

Restore Tested:
Yes / No

Restore Test Date:

Estimated RTO:

Estimated RPO:
```

### Key Findings

```text
```

### Recommended Actions

```text
```

---

## 11. Monitoring and Logging Summary

```text
Uptime Monitoring:
Configured / Partial / Not configured / Unknown

Disk Usage Alert:
Configured / Partial / Not configured / Unknown

CPU and Memory Monitoring:
Configured / Partial / Not configured / Unknown

Application Health Check:
Configured / Partial / Not configured / Unknown

CloudWatch Agent:
Installed / Not installed / Unknown

SSM Agent:
Installed / Not installed / Unknown

Alert Recipient:

System Logs Reviewed:
Yes / No / Partial

Application Logs Reviewed:
Yes / No / Partial

Log Rotation Reviewed:
Yes / No / Partial
```

### Key Findings

```text
```

### Recommended Actions

```text
```

---

## 12. Secrets and Sensitive Files

```text
Environment Files Identified:
Yes / No / Partial

Environment File Permissions Reviewed:
Yes / No / Partial

Secrets Excluded From Git:
Yes / No / Unknown

AWS Credentials Found on Server:
No / Yes / Unknown

Database Credentials Protected:
Yes / No / Unknown

Private Keys Reviewed:
Yes / No / Partial

Former Credentials Rotated:
Yes / No / Unknown / Not applicable
```

### Key Findings

```text
```

### Recommended Actions

```text
```

---

## 13. Cost and Resource Hygiene

```text
Instance Type Reviewed:
Yes / No / Partial

EBS Volume Size Reviewed:
Yes / No / Partial

Old Snapshots Reviewed:
Yes / No / Partial

Old AMIs Reviewed:
Yes / No / Partial

Unattached EBS Volumes Reviewed:
Yes / No / Partial

Elastic IP Usage Reviewed:
Yes / No / Partial

Unused Load Balancers Reviewed:
Yes / No / Not applicable

Cost Owner Identified:
Yes / No / Unknown
```

### Key Findings

```text
```

### Recommended Actions

```text
```

---

## 14. Risk Rating

| Area                      | Status  | Risk Level          | Notes |
| ------------------------- | ------- | ------------------- | ----- |
| EC2 identification        | Unknown | Low / Medium / High |       |
| Application inventory     | Unknown | Low / Medium / High |       |
| Network exposure          | Unknown | Low / Medium / High |       |
| SSH access                | Unknown | Low / Medium / High |       |
| OS updates and hardening  | Unknown | Low / Medium / High |       |
| Web server and HTTPS      | Unknown | Low / Medium / High |       |
| Database protection       | Unknown | Low / Medium / High |       |
| Docker readiness          | N/A     | Low / Medium / High |       |
| Backup and restore        | Unknown | Low / Medium / High |       |
| Monitoring and logging    | Unknown | Low / Medium / High |       |
| Secrets handling          | Unknown | Low / Medium / High |       |
| Cost and resource hygiene | Unknown | Low / Medium / High |       |
| Handover documentation    | Unknown | Low / Medium / High |       |

---

## 15. Recommended Action Plan

| Priority | Action | Owner | Due Date | Status |
| -------- | ------ | ----- | -------- | ------ |
| High     |        |       |          |        |
| High     |        |       |          |        |
| High     |        |       |          |        |
| Medium   |        |       |          |        |
| Medium   |        |       |          |        |
| Medium   |        |       |          |        |
| Low      |        |       |          |        |
| Low      |        |       |          |        |

### Priority Guide

#### High Priority

Issues that may directly expose the server, interrupt service, or cause data loss.

Examples:

* Database publicly exposed
* SSH open to the world without strong access control
* No backup for production data
* Restore procedure missing
* Critical disk usage
* Unsupported operating system
* Unknown administrator access
* Secrets exposed under web root or in Git

#### Medium Priority

Issues that increase operational or recovery risk.

Examples:

* Restore has never been tested
* Monitoring incomplete
* Many pending updates
* Docker volumes not clearly documented
* Deployment procedure unclear
* No handover documentation
* HTTPS renewal not confirmed

#### Low Priority

Useful improvements that are not urgent.

Examples:

* Improve naming conventions
* Clean up old snapshots after review
* Add diagrams
* Improve log retention documentation
* Add more detailed operational notes

---

## 16. Handover Notes

```text
Important Paths:

Important Services:

Start Procedure:

Stop Procedure:

Restart Procedure:

Deployment Procedure:

Backup Procedure:

Restore Procedure:

Monitoring Procedure:

Known Risks:

Pending Tasks:

Useful Commands:
```

---

## 17. Final Conclusion

```text
Overall Assessment:

The server is ready for handover:
Yes / No / Conditional / Unknown

The server is ready for production use:
Yes / No / Conditional / Unknown

The server is ready for migration:
Yes / No / Conditional / Unknown

Main reason for assessment:

Recommended next step:
```

---

## 18. Appendix: Related Files

Recommended toolkit files:

```text
scripts/ubuntu-server-audit.sh
docs/ec2-readiness-checklist.md
docs/security-baseline-checklist.md
docs/backup-and-restore-checklist.md
docs/ubuntu-hardening-notes.md
docs/docker-server-checklist.md
examples/wordpress-on-aws.md
examples/laravel-on-aws.md
examples/small-business-server.md
```

Generated audit reports:

```text
reports/
```