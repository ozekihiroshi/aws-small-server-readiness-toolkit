# Example: Small Business Server on AWS

This example shows how to use the **AWS Small Server Readiness Toolkit** to review a small business server running on AWS EC2.

It is intended for practical use cases such as:

* Small business website hosting
* Internal business tools
* Booking systems
* Customer portals
* Small CRM systems
* File upload portals
* Lightweight Docker-based applications
* Legacy EC2 server handover
* Pre-migration server review
* Production readiness review

## Important Notice

This example does **not** provide legal compliance certification, security certification, penetration testing, or a guarantee against incidents.

It is a practical example for reviewing a small business server on AWS.

For regulated data, financial data, healthcare data, government workloads, or formal compliance requirements, consult qualified AWS, security, legal, or compliance professionals.

---

## 1. Scenario

A small business has an application or website running on an Ubuntu EC2 server.

The server may be used for:

```text
Company website
Customer inquiry form
Booking system
Internal dashboard
Small CRM
Document upload portal
Admin tool
API server
Docker-based business application
Legacy web application
```

The server may have been built quickly, inherited from another administrator, or migrated from another hosting provider.

The goal is to review whether the server is ready for production use, migration, troubleshooting, hardening, backup review, or handover.

---

## 2. Common Deployment Patterns

### Pattern A: Traditional Linux Web Server

```text
EC2 Ubuntu
├── Nginx or Apache
├── PHP / Python / Node.js / Ruby / Java application
├── MySQL / MariaDB / PostgreSQL on the same server
└── Application files under /var/www, /srv, or /opt
```

### Pattern B: Docker-Based Small Business Server

```text
EC2 Ubuntu
├── Docker Compose
├── Reverse proxy container
├── Application container
├── Database container
├── Redis or cache container if used
└── Docker volumes for persistent data
```

### Pattern C: Hybrid AWS Server

```text
EC2 Ubuntu
├── Web or application server
├── RDS database
├── S3 for uploads or backups
├── CloudWatch for monitoring
└── Route 53 or external DNS
```

### Pattern D: Legacy EC2 Server

```text
EC2 Ubuntu
├── Unknown application history
├── Unknown administrator access
├── Unclear backup status
├── Old packages or services
└── Incomplete handover documentation
```

---

## 3. Review Goals

This example focuses on the following questions:

* Is the EC2 server clearly identified?
* Is the business purpose documented?
* Is SSH access controlled?
* Are AWS Security Groups safe?
* Are only required services publicly exposed?
* Is the operating system supported and updated?
* Are important application files known?
* Is the database protected and backed up?
* Are uploads or user-generated files backed up?
* Is HTTPS configured and renewable?
* Are logs available for troubleshooting?
* Is monitoring configured?
* Can the server be restored on a new EC2 instance?
* Can another administrator understand and operate the server?

---

## 4. Recommended Toolkit Documents

Use the following documents together:

```text
docs/ec2-readiness-checklist.md
docs/security-baseline-checklist.md
docs/backup-and-restore-checklist.md
docs/ubuntu-hardening-notes.md
docs/docker-server-checklist.md
```

Use the Docker checklist only if Docker or Docker Compose is used.

---

## 5. Step 1: Run the Ubuntu Audit Script

From the repository root:

```bash
./scripts/ubuntu-server-audit.sh
```

For more complete information:

```bash
sudo ./scripts/ubuntu-server-audit.sh
```

The report will be created under:

```text
reports/
```

Example:

```text
reports/ubuntu-server-audit-20260609-091500.md
```

Review the generated report before making changes.

---

## 6. Step 2: Identify the Server Role

Before reviewing technical details, identify the business role of the server.

### Key Questions

* [ ] What business function does this server support?
* [ ] Is it production, staging, development, test, or temporary?
* [ ] Who owns the application?
* [ ] Who owns the AWS account?
* [ ] Who is responsible for operations?
* [ ] Who should be contacted during an incident?
* [ ] What happens if the server is down for one hour?
* [ ] What happens if the server is down for one day?
* [ ] What data would be lost if the server failed now?
* [ ] Is there a known migration or replacement plan?

### Notes Template

```text
Server purpose:

Business owner:

Technical owner:

Environment:

Criticality:

Known users:

Known risks:
```

---

## 7. Step 3: EC2 Readiness Review

Use:

```text
docs/ec2-readiness-checklist.md
```

Focus on:

* AWS account
* AWS region
* EC2 instance ID
* Instance type
* EBS volumes
* Elastic IP
* DNS records
* Security Groups
* IAM role
* Backup
* Monitoring
* Cost hygiene

### Key Questions

* [ ] Is the EC2 instance ID documented?
* [ ] Is the AWS region correct?
* [ ] Is the instance type appropriate?
* [ ] Is the disk size appropriate?
* [ ] Is EBS encryption reviewed?
* [ ] Is public IP usage intentional?
* [ ] Is Elastic IP used if stable IP is required?
* [ ] Are DNS records documented?
* [ ] Is there a rollback plan for migration?
* [ ] Are unused AWS resources reviewed?

---

## 8. Step 4: Network and Security Group Review

Use:

```text
docs/security-baseline-checklist.md
```

Focus on network exposure.

### Key Questions

* [ ] Which ports are open in AWS Security Groups?
* [ ] Is SSH restricted by source IP where practical?
* [ ] Are HTTP and HTTPS intentionally open?
* [ ] Are database ports closed to the public internet?
* [ ] Are admin panels protected?
* [ ] Are old test ports still open?
* [ ] Are Security Group rule descriptions meaningful?
* [ ] Do host-level firewall rules match AWS Security Groups?

### Common High-Risk Exposure

```text
22/tcp open to 0.0.0.0/0 without strong access control
3306/tcp MySQL publicly exposed
5432/tcp PostgreSQL publicly exposed
6379/tcp Redis publicly exposed
9200/tcp Elasticsearch publicly exposed
8080/tcp admin or test application publicly exposed
```

---

## 9. Step 5: SSH and User Access Review

Administrative access should be controlled and documented.

### Key Questions

* [ ] Who can SSH into the server?
* [ ] Is password authentication disabled?
* [ ] Is root SSH login disabled?
* [ ] Are old SSH keys removed?
* [ ] Are sudo-capable users documented?
* [ ] Are former administrator accounts disabled?
* [ ] Is emergency access documented?
* [ ] Is AWS Systems Manager Session Manager considered?

### Useful Commands

```bash
awk -F: '($7 !~ /(nologin|false)$/) {print $1 ":" $6 ":" $7}' /etc/passwd
getent group sudo
find /home /root -maxdepth 3 -name authorized_keys -type f -print 2>/dev/null
last -n 20
```

---

## 10. Step 6: Application Inventory

Identify application files and runtime.

### Common Application Paths

```text
/var/www
/var/www/html
/srv
/opt
/home/ubuntu
```

### Key Questions

* [ ] Where is the application root directory?
* [ ] What runtime is used: PHP, Python, Node.js, Ruby, Java, Go, or other?
* [ ] Is source code stored in Git?
* [ ] Are configuration files documented?
* [ ] Are environment files identified?
* [ ] Are uploaded files or generated files identified?
* [ ] Are scheduled jobs documented?
* [ ] Are background workers documented?
* [ ] Is the deployment process documented?
* [ ] Is the rollback process documented?

### Useful Commands

```bash
find /var/www /srv /opt /home -maxdepth 3 -type f \( -name ".env" -o -name "package.json" -o -name "composer.json" -o -name "requirements.txt" -o -name "Gemfile" \) 2>/dev/null
systemctl list-timers --all --no-pager
```

---

## 11. Step 7: Database Review

Most business applications depend on a database.

### Common Database Types

```text
MySQL / MariaDB
PostgreSQL
SQLite
Redis
RDS
External managed database
Docker-based database
```

### Key Questions

* [ ] What database is used?
* [ ] Where is the database located?
* [ ] Is the database publicly exposed?
* [ ] Are database credentials protected?
* [ ] Is database backup configured?
* [ ] Is database restore documented?
* [ ] Has database restore been tested?
* [ ] Is database size reviewed?
* [ ] Is database maintenance responsibility documented?

### Useful Commands

```bash
ss -tulpen | grep -E "3306|5432|6379" || true
systemctl status mysql --no-pager
systemctl status mariadb --no-pager
systemctl status postgresql --no-pager
docker ps --format "table {{.Names}}\t{{.Image}}\t{{.Ports}}"
```

---

## 12. Step 8: Backup and Restore Review

Use:

```text
docs/backup-and-restore-checklist.md
```

A useful backup should include all data required to restore the business service.

### Minimum Backup Questions

* [ ] Are application files backed up?
* [ ] Is the database backed up?
* [ ] Are uploaded files backed up?
* [ ] Are configuration files backed up?
* [ ] Are secrets included in restore planning without being exposed?
* [ ] Are Docker volumes backed up if Docker is used?
* [ ] Is backup stored outside the original server?
* [ ] Is backup encrypted where appropriate?
* [ ] Is retention documented?
* [ ] Has restore been tested?

### Common Backup Mistakes

* Backups are stored only on the same EC2 instance.
* Database is not included.
* Uploads or generated files are not included.
* Docker volumes are not included.
* Backup exists but restore was never tested.
* Backup owner is unknown.
* Backup failure alerts are not configured.

---

## 13. Step 9: Docker Review if Applicable

Use:

```text
docs/docker-server-checklist.md
```

Focus on:

* Compose files
* Containers
* Images
* Port exposure
* Networks
* Volumes
* `.env`
* Database containers
* Reverse proxy
* Restart policies
* Backup and restore

### Key Questions

* [ ] Where are Compose files located?
* [ ] Are running containers documented?
* [ ] Are published ports intentional?
* [ ] Are volumes documented?
* [ ] Are important volumes backed up?
* [ ] Are `.env` files protected?
* [ ] Are database containers protected?
* [ ] Do containers restart after reboot?
* [ ] Is Docker disk usage reviewed?

---

## 14. Step 10: Web Server and HTTPS Review

Small business servers often expose web applications.

### Common Web Servers

```text
Nginx
Apache
Caddy
Traefik
Application built-in server behind reverse proxy
```

### Key Questions

* [ ] Which web server or reverse proxy is used?
* [ ] Are active sites or routes documented?
* [ ] Is HTTPS enabled?
* [ ] Is certificate renewal configured?
* [ ] Is HTTP redirected to HTTPS where appropriate?
* [ ] Is the default site disabled or intentional?
* [ ] Are old domains or test routes still active?
* [ ] Are admin routes protected?
* [ ] Are web server configuration files backed up?

### Useful Commands

```bash
nginx -t
apache2ctl configtest
certbot certificates
docker ps --format "table {{.Names}}\t{{.Image}}\t{{.Ports}}"
```

---

## 15. Step 11: Monitoring and Logging Review

Monitoring should detect common failures before users report them.

### Key Questions

* [ ] Is uptime monitored?
* [ ] Is disk usage monitored?
* [ ] Is memory usage monitored?
* [ ] Is application health monitored?
* [ ] Are alerts sent to the correct person?
* [ ] Are system logs available?
* [ ] Are web server logs available?
* [ ] Are application logs available?
* [ ] Are Docker logs reviewed if Docker is used?
* [ ] Are logs rotated?

### Useful Commands

```bash
systemctl status amazon-cloudwatch-agent --no-pager
systemctl status amazon-ssm-agent --no-pager
journalctl -p err -n 80 --no-pager
ls -lah /var/log
```

---

## 16. Step 12: Ubuntu Hardening Review

Use:

```text
docs/ubuntu-hardening-notes.md
```

Focus on:

* Package updates
* SSH hardening
* Firewall and network exposure
* User and sudo access
* Failed services
* Log rotation
* Disk usage
* Swap and memory
* Secrets and sensitive files
* Backup before changes

### Key Questions

* [ ] Are security updates pending?
* [ ] Is a reboot required?
* [ ] Are failed services reviewed?
* [ ] Are unknown users reviewed?
* [ ] Are secrets protected?
* [ ] Is disk usage safe?
* [ ] Is there a backup before hardening changes?
* [ ] Are changes documented?

---

## 17. Step 13: Handover Documentation

For handover, document the following:

```text
Server Name:
AWS Account:
AWS Region:
EC2 Instance ID:
Instance Type:
Elastic IP:
Domain Name:
DNS Provider:
Primary Application:
Application Path:
Runtime:
Database:
Web Server:
Docker Used:
Backup Method:
Backup Location:
Restore Procedure:
Monitoring:
Admin Contact:
Technical Contact:
Known Risks:
Next Actions:
```

---

## 18. Example Risk Rating

| Area                       | Status  | Risk Level | Notes |
| -------------------------- | ------- | ---------- | ----- |
| Server identity            | Unknown | Medium     |       |
| AWS region and EC2 details | Unknown | Medium     |       |
| Security Group             | Unknown | High       |       |
| SSH access                 | Unknown | High       |       |
| Application inventory      | Unknown | Medium     |       |
| Database protection        | Unknown | High       |       |
| Backup and restore         | Unknown | High       |       |
| Docker readiness           | N/A     | Medium     |       |
| HTTPS                      | Unknown | Medium     |       |
| Monitoring                 | Unknown | Medium     |       |
| Ubuntu hardening           | Unknown | Medium     |       |
| Handover documentation     | Unknown | Medium     |       |

---

## 19. Example Recommended Action Plan

| Priority | Action                                        | Owner | Due Date | Status |
| -------- | --------------------------------------------- | ----- | -------- | ------ |
| High     | Confirm database backup and restore procedure |       |          |        |
| High     | Review Security Group inbound rules           |       |          |        |
| High     | Confirm SSH access control                    |       |          |        |
| Medium   | Document application root path and runtime    |       |          |        |
| Medium   | Confirm HTTPS renewal process                 |       |          |        |
| Medium   | Confirm monitoring and alert recipients       |       |          |        |
| Low      | Improve handover documentation                |       |          |        |

---

## 20. Final Small Business Server Review Summary

Complete this section after review.

```text
Server Name:

Business Purpose:

AWS Account:

AWS Region:

EC2 Instance ID:

Instance Type:

Deployment Type:
Host-based / Docker-based / RDS-based / Hybrid / Unknown

Web Server:
Nginx / Apache / Traefik / Caddy / Other / Unknown

Database:
Local DB / Docker DB / RDS / External / None / Unknown

Docker Used:
Yes / No / Unknown

Backup Method:

Backup Location:

Restore Tested:
Yes / No / Unknown

Monitoring:
Configured / Partial / None / Unknown

Main Risks Found:

Recommended Immediate Actions:

Recommended Next Actions:

Reviewer:

Review Date:
```

---

## Related Toolkit Documents

Recommended related documents:

```text
docs/ec2-readiness-checklist.md
docs/security-baseline-checklist.md
docs/backup-and-restore-checklist.md
docs/ubuntu-hardening-notes.md
docs/docker-server-checklist.md
```

Recommended current script:

```bash
./scripts/ubuntu-server-audit.sh
```

Use this example together with the generated audit report to prepare a practical small business server readiness review.
