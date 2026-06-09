# AWS Small Server Readiness Toolkit

A practical toolkit for reviewing small AWS EC2 Ubuntu servers before production use, migration, troubleshooting, hardening, backup review, Docker review, or handover.

This project is designed for small business web servers, WordPress hosting, Laravel applications, Docker Compose workloads, internal tools, and lightweight business applications running on AWS EC2.

It provides:

* A read-only Ubuntu server audit script
* Practical security baseline checklists
* Backup and restore readiness checklists
* EC2 readiness review guidance
* Ubuntu hardening notes
* Docker server review guidance
* WordPress, Laravel, and small business server examples
* A handover report template

## Purpose

Many small AWS servers are created quickly for websites, business applications, internal tools, or temporary migration projects.

Over time, important operational details can become unclear:

* Who can access the server?
* Is SSH safely restricted?
* Are AWS Security Groups too open?
* Are OS packages updated?
* Is there a backup plan?
* Has restore ever been tested?
* Is Docker running production workloads?
* Are Docker volumes backed up?
* Are logs and monitoring configured?
* Is there enough disk space?
* Can another administrator understand and operate the server?

This toolkit helps identify these issues before they become production risks.

## Important Notice

This toolkit does **not** provide legal compliance certification, security certification, penetration testing, disaster recovery certification, or a guarantee against incidents.

It is intended to support practical server readiness review, documentation, migration preparation, operational handover, and discussion before or after AWS server deployment.

For regulated data, financial data, healthcare data, government workloads, or legal compliance requirements, consult qualified AWS, security, legal, compliance, or disaster recovery professionals.

## Target Use Cases

This toolkit is useful for:

* WordPress migration to AWS
* Laravel deployment to AWS
* Existing EC2 server review
* Ubuntu server hardening preparation
* Docker Compose server review
* Backup and restore readiness review
* Small business server handover
* Pre-migration infrastructure assessment
* Troubleshooting preparation
* Lightweight client-facing server documentation

## Repository Structure

```text
aws-small-server-readiness-toolkit/
├── README.md
├── LICENSE
├── scripts/
│   └── ubuntu-server-audit.sh
├── docs/
│   ├── security-baseline-checklist.md
│   ├── backup-and-restore-checklist.md
│   ├── ec2-readiness-checklist.md
│   ├── ubuntu-hardening-notes.md
│   ├── docker-server-checklist.md
│   └── handover-report-template.md
└── examples/
    ├── wordpress-on-aws.md
    ├── laravel-on-aws.md
    └── small-business-server.md
```

## Main Script

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

For more complete results:

```bash
sudo ./scripts/ubuntu-server-audit.sh
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

The audit script is read-only. It does not modify system configuration.

It does not:

* Change firewall rules
* Restart services
* Modify SSH settings
* Install packages
* Delete files
* Create AWS resources

Some checks may show more complete results when executed with `sudo`.

## Recommended Review Workflow

A practical review flow is:

```text
1. Run the audit script
2. Review the generated Markdown report
3. Complete the EC2 readiness checklist
4. Complete the security baseline checklist
5. Complete the backup and restore checklist
6. Complete Docker checklist if Docker is used
7. Review Ubuntu hardening notes
8. Prepare a handover report
```

Recommended documents:

```text
docs/ec2-readiness-checklist.md
docs/security-baseline-checklist.md
docs/backup-and-restore-checklist.md
docs/ubuntu-hardening-notes.md
docs/docker-server-checklist.md
docs/handover-report-template.md
```

## Examples

Use these examples depending on the server type:

```text
examples/wordpress-on-aws.md
examples/laravel-on-aws.md
examples/small-business-server.md
```

### WordPress on AWS

Use this example when reviewing:

* WordPress EC2 servers
* WordPress migration to AWS
* WordPress backup and restore readiness
* WordPress Docker deployments
* WordPress handover documentation

### Laravel on AWS

Use this example when reviewing:

* Laravel EC2 servers
* Docker-based Laravel deployments
* Laravel queue workers
* Laravel scheduler setup
* Laravel `.env` handling
* Laravel backup and restore readiness

### Small Business Server on AWS

Use this example when reviewing:

* Internal tools
* Small business web applications
* Booking systems
* Customer portals
* Legacy EC2 servers
* General-purpose small AWS servers

## Current Scope

The current version focuses on:

* AWS EC2 small server readiness
* Ubuntu server baseline review
* Basic Linux hardening preparation
* Docker server review
* Backup and restore readiness
* WordPress and Laravel examples
* Operational handover documentation

## Current Limitations

This toolkit does not currently perform:

* Full AWS account security review
* IAM policy analysis
* Automated AWS resource scanning
* Penetration testing
* Vulnerability scanning
* CIS benchmark validation
* Legal or regulatory compliance assessment
* Automated remediation
* Terraform-based provisioning

## Typical Findings This Toolkit Helps Document

Examples of risks this toolkit can help identify:

* SSH open to the internet
* Unknown administrator access
* Unsupported Ubuntu version
* Pending security updates
* Reboot required but not performed
* Root filesystem nearly full
* Docker volumes not backed up
* Database publicly exposed
* Backup exists but restore was never tested
* `.env` files not protected
* Unknown containers running
* No handover documentation
* Monitoring alerts not configured
* Old snapshots or unused EBS volumes increasing cost

## Who This Is For

This toolkit is intended for:

* Freelance developers
* Small business IT administrators
* Web developers managing EC2 servers
* WordPress maintainers
* Laravel maintainers
* Docker Compose users
* Consultants preparing server handover documentation
* Engineers reviewing small AWS server environments

## Suggested Portfolio Description

You may describe this project as:

```text
A practical AWS small server readiness toolkit for reviewing Ubuntu EC2 servers before production use, migration, troubleshooting, hardening, backup review, Docker review, or handover. It includes a read-only audit script, practical checklists, and examples for WordPress, Laravel, and small business servers.
```

## Roadmap

Possible future additions:

* `scripts/backup-readiness-check.sh`
* `scripts/docker-server-audit.sh`
* AWS CLI-based EC2 metadata and Security Group review
* S3 backup review checklist
* CloudTrail and GuardDuty checklist
* CloudWatch alarm checklist
* Terraform examples
* Markdown report generator
* Sample completed handover report

## License

This project is released under the MIT License.

See:

```text
LICENSE
```
