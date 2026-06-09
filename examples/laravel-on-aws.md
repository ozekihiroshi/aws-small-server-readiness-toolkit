# Example: Laravel on AWS

This example shows how to use the **AWS Small Server Readiness Toolkit** to review a small Laravel application server running on AWS EC2.

It is intended for practical use cases such as:

* Laravel deployment to AWS
* Existing Laravel server review
* Docker-based Laravel server review
* Laravel migration preparation
* Laravel backup and restore review
* Laravel troubleshooting preparation
* Laravel handover documentation
* Small business application infrastructure review

## Important Notice

This example does **not** provide legal compliance certification, security certification, penetration testing, or a guarantee against incidents.

It is a practical example for reviewing a small Laravel server on AWS.

For regulated data, financial data, healthcare data, government workloads, or formal compliance requirements, consult qualified AWS, security, legal, or compliance professionals.

---

## 1. Scenario

A small business has a Laravel application running on an Ubuntu EC2 server.

The server may be using one of the following patterns:

```text
Pattern A: Traditional Laravel Server

EC2 Ubuntu
├── Nginx or Apache
├── PHP-FPM
├── Composer-managed Laravel application
├── MySQL / MariaDB / PostgreSQL on the same server
├── Redis on the same server if used
└── Laravel project under /var/www or /srv
```

```text
Pattern B: Docker-Based Laravel Server

EC2 Ubuntu
├── Docker Compose
├── Nginx or Traefik reverse proxy
├── PHP-FPM / Laravel app container
├── MySQL / MariaDB / PostgreSQL container
├── Redis container
├── Queue worker container if used
└── Docker volumes for database and persistent files
```

```text
Pattern C: Hybrid Laravel Server

EC2 Ubuntu
├── Nginx or Apache on host
├── Laravel on host or container
├── RDS database
├── ElastiCache or local Redis
└── S3 or local storage for uploaded files
```

The goal is to review whether the server is ready for production use, migration, troubleshooting, or handover.

---

## 2. Review Goals

This example focuses on the following questions:

* Is the EC2 server clearly identified?
* Is SSH access controlled?
* Are AWS Security Groups safe?
* Is Laravel deployed in a reproducible way?
* Is the `.env` file protected?
* Are database and storage files backed up?
* Are queue workers and scheduled tasks documented?
* Can the application be restored on a new server?
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

Use the Docker checklist only if Laravel is running with Docker or Docker Compose.

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

## 5. Step 2: Identify the Laravel Deployment Type

Check whether Laravel is installed directly on the host or inside Docker.

### Host-Based Laravel

Common paths:

```text
/var/www
/var/www/html
/var/www/laravel
/srv/app
/opt/app
```

Review commands:

```bash
find /var/www /srv /opt -name "artisan" -type f 2>/dev/null
find /var/www /srv /opt -name ".env" -type f 2>/dev/null
```

### Docker-Based Laravel

Review commands:

```bash
docker ps -a
docker compose ps
docker volume ls
find /home /opt /srv /var/www -name "docker-compose.yml" -o -name "compose.yml" 2>/dev/null
```

### RDS-Based Laravel

Check the Laravel `.env` file carefully:

```bash
grep -E "DB_CONNECTION|DB_HOST|DB_DATABASE|DB_USERNAME" /path/to/.env
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

### Key Laravel Questions

* [ ] Is this server production, staging, or development?
* [ ] Is the instance type appropriate for traffic and background jobs?
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

### Key Laravel Questions

* [ ] Is SSH restricted by source IP where practical?
* [ ] Is password-based SSH disabled?
* [ ] Is root SSH login disabled?
* [ ] Are only HTTP and HTTPS publicly exposed?
* [ ] Is MySQL, PostgreSQL, or Redis not publicly exposed?
* [ ] Are system users reviewed?
* [ ] Are failed login attempts reviewed?
* [ ] Are updates and reboot requirements reviewed?
* [ ] Are Laravel secrets protected?

---

## 8. Step 5: Laravel File and Directory Review

Identify Laravel files and important directories.

Common important paths:

```text
.env
artisan
composer.json
composer.lock
storage/
storage/app
storage/logs
bootstrap/cache
public/
routes/
config/
database/migrations/
```

Review commands:

```bash
find /var/www /srv /opt -name "artisan" -type f 2>/dev/null
find /var/www /srv /opt -name ".env" -type f 2>/dev/null
find /var/www /srv /opt -name "composer.json" -type f 2>/dev/null
```

### Key Questions

* [ ] Is the Laravel project root path documented?
* [ ] Is the `.env` file protected?
* [ ] Is source code stored in Git?
* [ ] Is `composer.lock` present?
* [ ] Is `storage/app` included in backup if it contains uploaded files?
* [ ] Are logs rotated or managed?
* [ ] Are file permissions reasonable?
* [ ] Are `.git`, `.env`, backup archives, or SQL dumps exposed under the web root?
* [ ] Is the `public/` directory the only web root?

---

## 9. Step 6: Environment and Secrets Review

Laravel `.env` is critical.

### Review Command

```bash
ls -lah /path/to/.env
```

Safe metadata review:

```bash
grep -E "APP_ENV|APP_DEBUG|APP_URL|DB_CONNECTION|DB_HOST|CACHE_STORE|QUEUE_CONNECTION|MAIL_MAILER|FILESYSTEM_DISK" /path/to/.env
```

Avoid printing or sharing secret values such as:

```text
APP_KEY
DB_PASSWORD
AWS_SECRET_ACCESS_KEY
MAIL_PASSWORD
STRIPE_SECRET
PAYPAL_SECRET
```

### Key Questions

* [ ] Is `.env` outside the public web root?
* [ ] Is `.env` readable only by appropriate users?
* [ ] Is `APP_ENV=production` for production?
* [ ] Is `APP_DEBUG=false` for production?
* [ ] Is `APP_URL` correct?
* [ ] Is `APP_KEY` present?
* [ ] Are old credentials rotated after handover?
* [ ] Are secrets excluded from Git?
* [ ] Are secrets included safely in restore planning?

---

## 10. Step 7: Database Review

Laravel usually depends on a database.

### Host-Based Database

Review commands:

```bash
systemctl status mysql --no-pager
systemctl status mariadb --no-pager
systemctl status postgresql --no-pager
ss -tulpen | grep -E "3306|5432|mysql|postgres" || true
```

### Docker-Based Database

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
* [ ] Are migrations documented?
* [ ] Is RDS backup retention reviewed if RDS is used?

---

## 11. Step 8: Queue Worker Review

Laravel applications may require queue workers.

### Review Commands

```bash
grep -E "QUEUE_CONNECTION" /path/to/.env
systemctl list-units --type=service | grep -i queue || true
systemctl list-units --type=service | grep -i horizon || true
ps aux | grep -E "queue:work|horizon" | grep -v grep || true
```

For Docker:

```bash
docker ps --format "table {{.Names}}\t{{.Image}}\t{{.Command}}"
```

### Key Questions

* [ ] Does the application use queues?
* [ ] Is `QUEUE_CONNECTION` documented?
* [ ] Are queue workers managed by systemd, Supervisor, Docker, or another tool?
* [ ] Do queue workers restart after reboot?
* [ ] Is Laravel Horizon used?
* [ ] Are failed jobs monitored?
* [ ] Is queue restart procedure documented?
* [ ] Is worker deployment procedure documented?

### Common Risk Indicators

* Application depends on queues but no worker is running.
* Queue worker was started manually and will not restart.
* Failed jobs are not monitored.
* Redis is exposed publicly.
* Queue configuration is not documented.

---

## 12. Step 9: Scheduler Review

Laravel applications may require scheduled tasks.

### Review Commands

Host-based cron:

```bash
crontab -l
sudo crontab -l
cat /etc/crontab
ls -lah /etc/cron.d
```

Look for:

```text
php /path/to/artisan schedule:run
```

Systemd timers:

```bash
systemctl list-timers --all --no-pager
```

Docker-based scheduler:

```bash
docker ps --format "table {{.Names}}\t{{.Image}}\t{{.Command}}"
```

### Key Questions

* [ ] Does the application use Laravel Scheduler?
* [ ] Is `schedule:run` configured?
* [ ] Is the scheduler managed by cron, systemd, Docker, or another tool?
* [ ] Does it restart after reboot?
* [ ] Are scheduled jobs documented?
* [ ] Are job logs reviewed?
* [ ] Is timezone behavior understood?

### Common Risk Indicators

* Scheduler is required but not configured.
* Scheduler is configured for the wrong path.
* Scheduler runs under the wrong user.
* Timezone behavior is undocumented.
* Scheduled jobs fail silently.

---

## 13. Step 10: Cache, Session, and Redis Review

Laravel may use file, database, Redis, Memcached, or other backends.

### Review Command

```bash
grep -E "CACHE_STORE|CACHE_DRIVER|SESSION_DRIVER|QUEUE_CONNECTION|REDIS_HOST" /path/to/.env
```

### Key Questions

* [ ] Cache backend is documented.
* [ ] Session backend is documented.
* [ ] Redis usage is documented if used.
* [ ] Redis is not publicly exposed.
* [ ] Redis persistence expectations are understood.
* [ ] Cache clear procedure is documented.
* [ ] Session impact during deployment is understood.

### Common Commands

Use the project’s correct execution method.

Host-based example:

```bash
php artisan cache:clear
php artisan config:clear
php artisan route:clear
php artisan view:clear
```

Docker-based example:

```bash
docker compose exec app php artisan cache:clear
docker compose exec app php artisan config:clear
docker compose exec app php artisan route:clear
docker compose exec app php artisan view:clear
```

### Notes

For production Docker-based Laravel projects, document the exact container/service name and working directory.

---

## 14. Step 11: Deployment Procedure Review

Laravel deployment should be reproducible.

### Key Questions

* [ ] Deployment method is documented.
* [ ] Git branch or release source is documented.
* [ ] Composer install command is documented.
* [ ] NPM/Vite build procedure is documented if used.
* [ ] Migration procedure is documented.
* [ ] Cache optimization procedure is documented.
* [ ] Queue restart procedure is documented.
* [ ] Rollback procedure is documented.
* [ ] Maintenance mode procedure is documented.
* [ ] Required PHP version and extensions are documented.

### Common Host-Based Commands

```bash
composer install --no-dev --optimize-autoloader
php artisan migrate --force
php artisan config:cache
php artisan route:cache
php artisan view:cache
php artisan queue:restart
```

### Common Docker-Based Commands

```bash
docker compose exec app composer install --no-dev --optimize-autoloader
docker compose exec app php artisan migrate --force
docker compose exec app php artisan config:cache
docker compose exec app php artisan route:cache
docker compose exec app php artisan view:cache
docker compose exec app php artisan queue:restart
```

### Common Risk Indicators

* Deployment depends on undocumented manual steps.
* Production uses local uncommitted changes.
* `.env` is overwritten during deployment.
* Migrations run without backup.
* Queue workers are not restarted after deployment.
* Vite assets are missing after deployment.

---

## 15. Step 12: Backup and Restore Review

Use:

```text
docs/backup-and-restore-checklist.md
```

For Laravel, a useful backup may include:

```text
Laravel source code or Git repository
.env restore plan
storage/app if it contains uploaded files
database dump or managed DB backup
Docker volumes if Docker is used
web server configuration
queue/scheduler configuration
deployment notes
```

### Key Questions

* [ ] Is the source code stored in Git?
* [ ] Is `.env` included in restore planning without exposing secrets?
* [ ] Is `storage/app` backed up if needed?
* [ ] Is database backup configured?
* [ ] Are Docker volumes backed up if Docker is used?
* [ ] Is restore procedure documented?
* [ ] Has restore been tested?
* [ ] Are queue and scheduler restored correctly?
* [ ] Are file permissions restored correctly?

### Common Backup Mistakes

* Only source code is stored in Git, but uploaded files are not backed up.
* Database backup exists, but `.env` is missing.
* Docker volumes are not included.
* Queue and scheduler are forgotten during restore.
* Restore test does not include login or file upload testing.

---

## 16. Step 13: Docker Review if Applicable

Use:

```text
docs/docker-server-checklist.md
```

Focus on:

* Compose files
* `.env`
* Volumes
* Database container
* Redis container
* Queue worker container
* Scheduler container
* Reverse proxy
* Port exposure
* Restart policies
* Backup and restore

### Key Docker Questions

* [ ] Where is the Compose project directory?
* [ ] Is the `.env` file protected?
* [ ] Which service is the Laravel app container?
* [ ] Which service runs PHP-FPM?
* [ ] Which service runs queue workers?
* [ ] Which service runs the scheduler?
* [ ] Which volume contains database data?
* [ ] Which volume contains uploaded files?
* [ ] Are volumes backed up?
* [ ] Do containers restart after reboot?
* [ ] Is the reverse proxy configuration documented?

---

## 17. Step 14: Web Server and HTTPS Review

Laravel production applications should normally use HTTPS.

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
* [ ] Is the web root set to Laravel `public/`?
* [ ] Is the default site disabled or intentionally configured?
* [ ] Are web server config files backed up?
* [ ] Are old domains or test sites still active?
* [ ] Are admin paths protected where appropriate?

---

## 18. Step 15: Logging and Troubleshooting

Useful logs for Laravel troubleshooting may include:

```text
storage/logs/laravel.log
/var/log/nginx/access.log
/var/log/nginx/error.log
/var/log/apache2/access.log
/var/log/apache2/error.log
/var/log/php*
/var/log/mysql
/var/log/postgresql
docker logs <container>
```

### Key Questions

* [ ] Are Laravel logs available?
* [ ] Are web server logs available?
* [ ] Are PHP logs available?
* [ ] Are database logs available if needed?
* [ ] Are Docker logs available if Docker is used?
* [ ] Are logs rotated?
* [ ] Are logs not filling the disk?
* [ ] Are secrets not written to logs?
* [ ] Is error reporting appropriate for production?

### Production Reminder

For production Laravel applications:

```text
APP_ENV=production
APP_DEBUG=false
```

---

## 19. Step 16: Handover Notes

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
Laravel Project Path:
Deployment Type:
Host-based / Docker-based / RDS-based / Hybrid
Web Server:
PHP Version:
Laravel Version:
Database Type:
Database Location:
Redis / Queue:
Scheduler:
Backup Method:
Backup Location:
Restore Procedure:
Deployment Procedure:
Admin Contact:
Technical Contact:
Known Risks:
Next Actions:
```

---

## 20. Example Risk Rating

| Area                   | Status  | Risk Level | Notes |
| ---------------------- | ------- | ---------- | ----- |
| EC2 identification     | Unknown | Medium     |       |
| Security Group         | Unknown | High       |       |
| SSH access             | Unknown | High       |       |
| Laravel `.env`         | Unknown | High       |       |
| Database backup        | Unknown | High       |       |
| Storage backup         | Unknown | High       |       |
| Queue worker           | Unknown | Medium     |       |
| Scheduler              | Unknown | Medium     |       |
| Docker volumes         | N/A     | Medium     |       |
| HTTPS                  | Unknown | Medium     |       |
| Monitoring             | Unknown | Medium     |       |
| Handover documentation | Unknown | Medium     |       |

---

## 21. Example Recommended Action Plan

| Priority | Action                                         | Owner | Due Date | Status |
| -------- | ---------------------------------------------- | ----- | -------- | ------ |
| High     | Confirm database backup and restore procedure  |       |          |        |
| High     | Confirm `.env` protection and restore handling |       |          |        |
| High     | Review Security Group inbound rules            |       |          |        |
| Medium   | Document queue worker and scheduler setup      |       |          |        |
| Medium   | Confirm HTTPS renewal process                  |       |          |        |
| Low      | Improve deployment and rollback documentation  |       |          |        |

---

## 22. Final Laravel on AWS Review Summary

Complete this section after review.

```text
Application Name:

Domain:

AWS Account:

AWS Region:

EC2 Instance ID:

Deployment Type:
Host-based / Docker-based / RDS-based / Hybrid

Web Server:
Nginx / Apache / Traefik / Caddy / Other

Runtime:
PHP version:

Laravel Version:

Database:
Local MySQL / Local MariaDB / PostgreSQL / Docker DB / RDS / Other

Laravel Project Path:

Queue:
None / Database / Redis / SQS / Other

Scheduler:
Configured / Not configured / Unknown

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

Use this example together with the generated audit report to prepare a practical Laravel on AWS readiness review.
