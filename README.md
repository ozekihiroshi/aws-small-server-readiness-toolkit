# AWS Small Server Readiness Toolkit

A practical checklist and script collection for reviewing small AWS-based Linux servers before production use, migration, troubleshooting, or handover.

This toolkit is designed for small business web servers, WordPress hosting, Laravel applications, Docker-based services, and internal tools running on AWS EC2.

It helps review common operational risks such as:

* SSH exposure
* OS update status
* Disk usage
* Memory status
* Firewall status
* Listening network ports
* Docker service status
* Web server status
* Backup readiness
* Logging and monitoring gaps
* Basic handover documentation

## Purpose

Many small AWS servers are created quickly for websites, business applications, internal tools, or temporary migration projects. Over time, important operational details can become unclear:

* Who can access the server?
* Is SSH safely restricted?
* Are OS packages updated?
* Is there a backup plan?
* Is Docker running production workloads?
* Are logs and monitoring configured?
* Is there enough disk space?
* Is the server ready to be handed over to another administrator?

This project provides practical checklists and simple scripts to help identify these issues before they become production risks.

## Important Notice

This toolkit does **not** provide legal compliance certification, security certification, penetration testing, or a guarantee against incidents.

It is intended to support practical server readiness review, documentation, and discussion before or after AWS server deployment.

For regulated data, financial data, healthcare data, government workloads, or legal compliance requirements, consult qualified security, legal, or compliance professionals.

## Initial Scope

The current version focuses on:

* AWS EC2 small server readiness
* Ubuntu server baseline review
* Basic Linux hardening checks
* Docker server checks
* Backup and restore readiness
* Operational handover preparation

Future versions may include:

* AWS CLI-based EC2 checks
* S3 backup review
* CloudTrail and GuardDuty checklist
* Terraform examples
* WordPress on AWS checklist
* Laravel on AWS checklist
* Report templates for clients

## Repository Structure

```text
aws-small-server-readiness-toolkit/
├── README.md
├── docs/
│   ├── security-baseline-checklist.md
│   ├── backup-and-restore-checklist.md
│   ├── ec2-readiness-checklist.md
│   ├── ubuntu-hardening-notes.md
│   ├── docker-server-checklist.md
│   ├── aws-region-and-data-residency-notes.md
│   └── handover-report-template.md
├── scripts/
│   ├── ubuntu-server-audit.sh
│   ├── docker-server-audit.sh
│   ├── backup-readiness-check.sh
│   └── generate-report.sh
├── examples/
│   ├── wordpress-on-aws.md
│   ├── laravel-on-aws.md
│   └── small-business-server.md
└── LICENSE
```

## First Script

The first script is:

```text
scripts/ubuntu-server-audit.sh
```

It performs a read-only baseline review of an Ubuntu server and generates a Markdown report.

It checks:

* Hostname and OS version
* Kernel version
* Uptime
* AWS EC2 metadata availability
* Disk usage
* Memory usage
* Package update status
* Firewall status
* SSH configuration
* Listening TCP/UDP ports
* Docker status
* Web server status
* Backup-related directories
* Monitoring agents
* Recent login information
* Sudo-capable users
* Systemd failed services

## Usage

Clone the repository:

```bash
git clone https://github.com/ozekihiro/aws-small-server-readiness-toolkit.git
cd aws-small-server-readiness-toolkit
```

Make the script executable:

```bash
chmod +x scripts/ubuntu-server-audit.sh
```

Run the audit:

```bash
./scripts/ubuntu-server-audit.sh
```

The script creates a report file under:

```text
reports/
```

Example:

```text
reports/ubuntu-server-audit-20260609-091500.md
```

## Safety

The initial audit script is read-only. It does not modify system configuration.

It does not:

* Change firewall rules
* Restart services
* Modify SSH settings
* Install packages
* Delete files
* Create AWS resources

Some checks may show more complete results when executed with `sudo`.

Example:

```bash
sudo ./scripts/ubuntu-server-audit.sh
```

## Typical Use Cases

### 1. Before Migrating a Website to AWS

Use this toolkit to check whether a new EC2 server has basic operational readiness before moving WordPress, Laravel, or other web applications.

### 2. Before Handing Over a Server

Generate a baseline report to document the server's current condition before handover.

### 3. Before Troubleshooting Production Issues

Check disk usage, memory status, failed services, listening ports, Docker status, and update status.

### 4. Before Discussing Security Improvements

Use the generated report as a starting point for a practical security review.

## Target Users

This toolkit is intended for:

* Freelance developers
* Small business IT administrators
* Web developers managing EC2 servers
* WordPress/Laravel maintainers
* Consultants preparing server handover documentation
* Engineers reviewing small AWS server environments

## Current Limitations

This toolkit does not currently perform:

* Full AWS account security review
* IAM policy analysis
* Penetration testing
* Vulnerability scanning
* CIS benchmark validation
* Legal or regulatory compliance assessment
* Automated remediation

## License

This project is intended to be released under an open-source license.

A license file should be added before wider distribution.
