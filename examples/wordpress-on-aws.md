# Example: WordPress on AWS

This example shows how to use the **AWS Small Server Readiness Toolkit** to review a small WordPress server running on AWS EC2.

It is intended for practical use cases such as:

* WordPress migration to AWS
* Existing WordPress server review
* WordPress troubleshooting preparation
* WordPress backup and restore review
* WordPress handover documentation
* Small business website infrastructure review

## Important Notice

This example does **not** provide legal compliance certification, security certification, penetration testing, or a guarantee against incidents.

It is a practical example for reviewing a small WordPress server on AWS.

For regulated data, financial data, healthcare data, government workloads, or formal compliance requirements, consult qualified AWS, security, legal, or compliance professionals.

---

## 1. Scenario

A small business has a WordPress website running on an Ubuntu EC2 server.

The server may be using one of the following patterns:

```text
Pattern A: Traditional WordPress Server

EC2 Ubuntu
├── Nginx or Apache
├── PHP-FPM or mod_php
├── MySQL or MariaDB
└── WordPress files under /var/www/html
```

```text
Pattern B: Docker-Based WordPress Server

EC2 Ubuntu
├── Docker Compose
├── WordPress container
├── MySQL or MariaDB container
├── Nginx, Apache, Traefik, or Caddy reverse proxy
└── Docker volumes for database and uploads
```

```text
Pattern C: Hybrid WordPress Server

EC2 Ubuntu
├── Nginx or Apache on host
├── PHP/WordPress on host or container
├── RDS database
└── S3 or local storage for media files
```

The goal is to review whether the server is ready for production use, migration, troubleshooting, or handover.

---

## 2. Review Goals

This example focuses on the following questions:

* Is the EC2 server clearly identified?
* Is SSH access controlled?
* Are AWS Security Groups safe?
* Is WordPress running as expected?
* Are WordPress files and database backed up?
* Can the website be restored on a new server?
* Are Docker volumes documented if Docker is used?
* Is HTTPS configured and renewable?
* Are logs available for troubleshooting?
* Is handover documentation sufficient?

---

## 3. Recommended Toolkit Documents

Use the following documents together:

```text
docs/ec2-readiness-checklist.md
docs/security-baseline-checklist.md
docs/backup-and-restore-checklist.md
docs/ubuntu-hardening-notes.md
docs/docker-server-checklist.md
```

Use the Docker checklist only if WordPress is running with Docker or Docker Compose.

---

## 4. Step 1: Run the Ubuntu Audit Script

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

## 5. Step 2: Identify the WordPress Deployment Type

Check whether WordPress is installed directly on the host or inside Docker.

### Host-Based WordPress

Common paths:

```text
/var/www/html
/var/www/wordpress
/srv/www
```

Review commands:

```bash
ls -lah /var/www
find /var/www -name "wp-config.php" -type f 2>/dev/null
```

### Docker-Based WordPress

Review commands:

```bash
docker ps -a
docker compose ps
docker volume ls
find /home /opt /srv /var/www -name "docker-compose.yml" -o -name "compose.yml" 2>/dev/null
```

### RDS-Based WordPress

Check `wp-config.php`:

```bash
grep -E "DB_HOST|DB_NAME|DB_USER" /path/to/wp-config.php
```

Do not print or share database passwords in reports unless they are properly redacted.

---

## 6. Step 3: EC2 Readiness Review

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
* Backup and monitoring

### Key WordPress Questions

* [ ] Is this server production, staging, or development?
* [ ] Is the instance type appropriate for traffic?
* [ ] Is the root EBS volume large enough?
* [ ] Is EBS encryption enabled?
* [ ] Is the domain pointing to the correct server?
* [ ] Is Elastic IP used if stable IP is required?
* [ ] Are old snapshots and AMIs documented?
* [ ] Is there a migration rollback plan?

---

## 7. Step 4: Security Baseline Review

Use:

```text
docs/security-baseline-checklist.md
```

Focus on:

* SSH access
* Security Groups
* OS updates
* Firewall
* Listening ports
* Web server
* Secrets
* Database exposure
* Monitoring
* Handover

### Key WordPress Questions

* [ ] Is SSH restricted by source IP where practical?
* [ ] Is password-based SSH disabled?
* [ ] Is root SSH login disabled?
* [ ] Are only HTTP and HTTPS publicly exposed?
* [ ] Is MySQL or MariaDB not publicly exposed?
* [ ] Are old WordPress admin users reviewed?
* [ ] Are unknown system users reviewed?
* [ ] Are failed login attempts reviewed?
* [ ] Are updates and reboot requirements reviewed?

---

## 8. Step 5: WordPress File Review

Identify WordPress files and important directories.

Common important paths:

```text
wp-config.php
wp-content/uploads
wp-content/themes
wp-content/plugins
.htaccess
```

Review commands:

```bash
find /var/www -name "wp-config.php" -type f 2>/dev/null
find /var/www -path "*/wp-content/uploads" -type d 2>/dev/null
find /var/www -path "*/wp-content/themes" -type d 2>/dev/null
find /var/www -path "*/wp-content/plugins" -type d 2>/dev/null
```

### Key Questions

* [ ] Is the WordPress root path documented?
* [ ] Is `wp-config.php` protected?
* [ ] Are uploads included in backup?
* [ ] Are custom themes included in backup or Git?
* [ ] Are custom plugins included in backup or Git?
* [ ] Are unused themes and plugins reviewed?
* [ ] Are file permissions reasonable?
* [ ] Are `.git`, `.env`, backup archives, or SQL dumps exposed under the web root?

---

## 9. Step 6: Database Review

WordPress requires database backup and restore planning.

### Host-Based MySQL or MariaDB

Review commands:

```bash
systemctl status mysql --no-pager
systemctl status mariadb --no-pager
ss -tulpen | grep -E "3306|mysql|mariadb" || true
```

### Docker-Based MySQL or MariaDB

Review commands:

```bash
docker ps --format "table {{.Names}}\t{{.Image}}\t{{.Ports}}"
docker volume ls
docker inspect <database-container>
```

### RDS-Based Database

Review in AWS console:

```text
RDS instance
Security Group
Backup retention
Snapshot policy
Public accessibility
Encryption
Maintenance window
```

### Key Questions

* [ ] Is the database location documented?
* [ ] Is the database not publicly exposed?
* [ ] Is database backup configured?
* [ ] Is the restore command documented?
* [ ] Has database restore been tested?
* [ ] Are database credentials protected?
* [ ] Is RDS backup retention reviewed if RDS is used?

---

## 10. Step 7: Backup and Restore Review

Use:

```text
docs/backup-and-restore-checklist.md
```

For WordPress, a useful backup must include both:

```text
WordPress files + WordPress database
```

### Minimum Backup Scope

* [ ] WordPress root directory
* [ ] `wp-content/uploads`
* [ ] Custom themes
* [ ] Custom plugins
* [ ] `wp-config.php`
* [ ] Database dump
* [ ] Web server configuration
* [ ] Docker Compose files if Docker is used
* [ ] Docker volumes if Docker is used
* [ ] TLS renewal method
* [ ] DNS and migration notes

### Common Backup Mistakes

* Only files are backed up, but database is not.
* Only database is backed up, but uploads are not.
* Backups are stored only on the same EC2 instance.
* Docker volumes are not included.
* Backup plugin exists but restore was never tested.
* No one knows where the latest backup is stored.
* No one knows the database password needed for restore.

---

## 11. Step 8: Docker Review if Applicable

Use:

```text
docs/docker-server-checklist.md
```

Focus on:

* Compose files
* Volumes
* `.env`
* Database container
* Reverse proxy
* Port exposure
* Restart policies
* Backup and restore

### Key Docker Questions

* [ ] Where is the Compose project directory?
* [ ] Is the `.env` file protected?
* [ ] Which volume contains WordPress files?
* [ ] Which volume contains database data?
* [ ] Are volumes backed up?
* [ ] Is the database port exposed publicly?
* [ ] Do containers restart after reboot?
* [ ] Is the reverse proxy configuration documented?

---

## 12. Step 9: Web Server and HTTPS Review

WordPress production websites should normally use HTTPS.

### Nginx Review Commands

```bash
nginx -t
systemctl status nginx --no-pager
ls -lah /etc/nginx/sites-enabled
```

### Apache Review Commands

```bash
apache2ctl configtest
systemctl status apache2 --no-pager
ls -lah /etc/apache2/sites-enabled
```

### Certbot Review Commands

```bash
certbot certificates
systemctl list-timers | grep -i certbot || true
```

### Key Questions

* [ ] Is HTTPS enabled?
* [ ] Is certificate renewal configured?
* [ ] Is HTTP redirected to HTTPS?
* [ ] Is the default site disabled or intentionally configured?
* [ ] Are web server config files backed up?
* [ ] Are old domains or test sites still active?
* [ ] Are admin paths protected where appropriate?

---

## 13. Step 10: WordPress Admin and Application Review

This toolkit is not a WordPress vulnerability scanner, but basic operational checks are useful.

### Key Questions

* [ ] Is the WordPress admin URL known?
* [ ] Are administrator accounts reviewed?
* [ ] Are old administrator accounts removed?
* [ ] Are plugins updated?
* [ ] Are themes updated?
* [ ] Is WordPress core updated?
* [ ] Are unused plugins removed or disabled?
* [ ] Are unused themes removed?
* [ ] Is there a staging or test environment?
* [ ] Is there a rollback plan before major updates?

### Notes

Avoid updating WordPress core, plugins, or themes on production without backup and rollback planning.

---

## 14. Step 11: Logging and Troubleshooting

Useful logs for WordPress troubleshooting may include:

```text
/var/log/nginx/access.log
/var/log/nginx/error.log
/var/log/apache2/access.log
/var/log/apache2/error.log
/var/log/php*
/var/log/mysql
/var/log/mariadb
wp-content/debug.log
docker logs <container>
```

### Key Questions

* [ ] Are web server logs available?
* [ ] Are PHP logs available?
* [ ] Are database logs available if needed?
* [ ] Are Docker logs available if Docker is used?
* [ ] Is WordPress debug logging disabled in production unless needed?
* [ ] Are logs rotated?
* [ ] Are logs not filling the disk?

---

## 15. Step 12: Handover Notes

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
WordPress Root Path:
Database Type:
Database Location:
Web Server:
PHP Version:
Backup Method:
Backup Location:
Restore Procedure:
Admin Contact:
Technical Contact:
Known Risks:
Next Actions:
```

---

## 16. Example Risk Rating

| Area                   | Status  | Risk Level | Notes |
| ---------------------- | ------- | ---------- | ----- |
| EC2 identification     | Unknown | Medium     |       |
| Security Group         | Unknown | High       |       |
| SSH access             | Unknown | High       |       |
| WordPress files        | Unknown | Medium     |       |
| Database backup        | Unknown | High       |       |
| Uploads backup         | Unknown | High       |       |
| Docker volumes         | N/A     | Medium     |       |
| HTTPS                  | Unknown | Medium     |       |
| Monitoring             | Unknown | Medium     |       |
| Handover documentation | Unknown | Medium     |       |

---

## 17. Example Recommended Action Plan

| Priority | Action                                             | Owner | Due Date | Status |
| -------- | -------------------------------------------------- | ----- | -------- | ------ |
| High     | Confirm database backup and restore procedure      |       |          |        |
| High     | Confirm uploads backup                             |       |          |        |
| High     | Review Security Group inbound rules                |       |          |        |
| Medium   | Document WordPress root path and database location |       |          |        |
| Medium   | Confirm HTTPS renewal process                      |       |          |        |
| Low      | Clean up old themes, plugins, and unused files     |       |          |        |

---

## 18. Final WordPress on AWS Review Summary

Complete this section after review.

```text
Website Name:

Domain:

AWS Account:

AWS Region:

EC2 Instance ID:

Deployment Type:
Host-based / Docker-based / RDS-based / Hybrid

Web Server:
Nginx / Apache / Traefik / Caddy / Other

Database:
Local MySQL / Local MariaDB / Docker DB / RDS / Other

WordPress Root Path:

Backup Method:

Backup Location:

Restore Tested:
Yes / No

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

Use this example together with the generated audit report to prepare a practical WordPress on AWS readiness review.
