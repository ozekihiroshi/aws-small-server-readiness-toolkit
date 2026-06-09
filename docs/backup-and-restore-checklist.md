# Backup and Restore Checklist

This checklist is part of the **AWS Small Server Readiness Toolkit**.

It is intended for practical review of backup and restore readiness for small AWS-based Linux servers, especially Ubuntu EC2 instances used for WordPress, Laravel, Docker-based applications, internal tools, and small business web services.

## Important Notice

This checklist does **not** provide legal compliance certification, disaster recovery certification, or a guarantee against data loss.

It is a practical baseline document to help review whether important server data is backed up, recoverable, documented, and testable.

For regulated data, financial data, healthcare data, government workloads, or formal disaster recovery requirements, consult qualified security, legal, compliance, or disaster recovery professionals.

## Core Principle

A backup is not complete until restore has been tested.

The goal is not only to confirm that backup files exist, but also to confirm:

* What is backed up
* Where it is backed up
* How often it is backed up
* Who is responsible
* How long backups are retained
* Whether backups are encrypted
* Whether restore has been tested
* How long recovery would take

---

## 1. Backup Ownership

Confirm who is responsible for backup and restore.

* [ ] Business owner is identified.
* [ ] Technical owner is identified.
* [ ] Person responsible for checking backup success is identified.
* [ ] Person responsible for restoring the server is identified.
* [ ] Backup failure notification recipient is identified.
* [ ] Backup responsibility is documented.
* [ ] Backup review schedule is defined.

### Notes

```text
```

---

## 2. Server and Application Inventory

Confirm what needs to be protected.

* [ ] Server hostname is documented.
* [ ] AWS region is documented.
* [ ] EC2 instance ID is documented.
* [ ] Elastic IP is documented if used.
* [ ] Domain names are documented.
* [ ] Main applications are listed.
* [ ] Application root directories are documented.
* [ ] Database type and location are documented.
* [ ] Uploaded files or user-generated content locations are documented.
* [ ] Configuration files are documented.
* [ ] Secret files are identified.
* [ ] Scheduled jobs are documented.

### Notes

```text
```

---

## 3. Backup Scope

Confirm that all important data is included.

* [ ] Application source code is backed up or stored in Git.
* [ ] Application configuration files are backed up.
* [ ] Environment files such as `.env` are handled carefully.
* [ ] Uploaded files are backed up.
* [ ] User-generated content is backed up.
* [ ] Database data is backed up.
* [ ] Web server configuration is backed up.
* [ ] Docker Compose files are backed up if Docker is used.
* [ ] Docker volumes are backed up if Docker is used.
* [ ] SSL/TLS certificate renewal method is documented.
* [ ] Cron jobs or systemd timers are backed up or documented.
* [ ] Important scripts are backed up.
* [ ] Infrastructure notes are backed up.

### Common Risk Indicators

* Only source code is backed up, but uploads are not.
* Only database is backed up, but uploaded files are not.
* Docker volumes are not included.
* `.env` files are missing from restore documentation.
* Backup target is on the same server only.
* Nobody knows which files are required for full recovery.

### Notes

```text
```

---

## 4. AWS-Level Backup

Review EC2 and EBS-level protection.

* [ ] EBS volumes are identified.
* [ ] EBS volume encryption status is reviewed.
* [ ] EBS snapshot policy exists or is planned.
* [ ] Snapshot frequency is documented.
* [ ] Snapshot retention period is documented.
* [ ] Snapshot region is documented.
* [ ] Cross-region copy is reviewed if needed.
* [ ] AMI creation process is documented if used.
* [ ] AWS Backup is reviewed if used.
* [ ] Backup vault configuration is reviewed if AWS Backup is used.
* [ ] Backup access permissions are reviewed.
* [ ] Old snapshots are reviewed for cost and relevance.

### Notes

```text
```

---

## 5. File-Level Backup

Review application and system file backup.

* [ ] Application directory is backed up.
* [ ] Upload directory is backed up.
* [ ] Web server configuration is backed up.
* [ ] System service files are backed up or documented.
* [ ] Cron files are backed up or documented.
* [ ] Custom scripts are backed up.
* [ ] Backup excludes temporary or cache directories where appropriate.
* [ ] File backup can be restored without overwriting unrelated data accidentally.
* [ ] File permissions are preserved or documented.
* [ ] Backup command or tool is documented.

### Example Directories to Review

```text
/var/www
/var/www/html
/srv
/opt
/etc/nginx
/etc/apache2
/etc/systemd/system
/etc/cron.d
/home/ubuntu
```

### Notes

```text
```

---

## 6. Database Backup

Review database backup separately from file backup.

* [ ] Database type is identified: MySQL, MariaDB, PostgreSQL, SQLite, RDS, or other.
* [ ] Database location is documented: local, Docker, RDS, or external.
* [ ] Database backup method is documented.
* [ ] Database backup schedule is documented.
* [ ] Database backup retention period is documented.
* [ ] Database backup encryption is reviewed.
* [ ] Database restore command is documented.
* [ ] Database backup is included in monitoring or alerting.
* [ ] Database backup size is reviewed.
* [ ] Database dump consistency is considered for production workloads.
* [ ] RDS automated backups are reviewed if RDS is used.
* [ ] RDS snapshot policy is reviewed if RDS is used.

### Common Risk Indicators

* Database is running, but only application files are backed up.
* Database dump exists but restore has never been tested.
* Database backup password is not documented securely.
* Database backup is stored on the same server only.
* Docker database volume is not included in backup.

### Notes

```text
```

---

## 7. Docker Backup

Review Docker-specific backup requirements.

* [ ] Docker Compose project directories are documented.
* [ ] `docker-compose.yml` or `compose.yml` files are backed up.
* [ ] `.env` files are identified and protected.
* [ ] Named volumes are listed.
* [ ] Bind mounts are listed.
* [ ] Database containers are identified.
* [ ] Persistent application data locations are identified.
* [ ] Docker volume backup method is documented.
* [ ] Container image versions are documented.
* [ ] Restart procedure is documented.
* [ ] Restore procedure for Docker services is documented.

### Useful Commands for Review

```bash
docker ps -a
docker volume ls
docker inspect <container>
docker compose ps
docker system df
```

### Common Risk Indicators

* Important data exists only inside a container filesystem.
* Named volumes are used but not backed up.
* `.env` file is missing or undocumented.
* Compose file is not stored in Git.
* No one knows how to restart the Docker application after restore.

### Notes

```text
```

---

## 8. WordPress Backup

Review WordPress-specific backup items if applicable.

* [ ] WordPress root directory is documented.
* [ ] `wp-config.php` is protected and included in restore planning.
* [ ] `wp-content/uploads` is backed up.
* [ ] `wp-content/themes` is backed up or stored in Git.
* [ ] `wp-content/plugins` is backed up or documented.
* [ ] WordPress database is backed up.
* [ ] WordPress admin account recovery process is understood.
* [ ] Domain and HTTPS configuration are documented.
* [ ] Web server configuration is documented.
* [ ] Plugin-based backup tools are reviewed if used.
* [ ] Off-server backup exists.

### Notes

```text
```

---

## 9. Laravel Backup

Review Laravel-specific backup items if applicable.

* [ ] Laravel project directory is documented.
* [ ] Source code is stored in Git.
* [ ] `.env` file is protected and included in restore planning.
* [ ] `storage/app` is backed up if it contains user files.
* [ ] `storage/logs` retention is reviewed.
* [ ] Database is backed up.
* [ ] Queue worker configuration is documented if used.
* [ ] Scheduler configuration is documented if used.
* [ ] Web server configuration is documented.
* [ ] Deployment command sequence is documented.
* [ ] Required PHP version and extensions are documented.
* [ ] Docker configuration is backed up if used.

### Notes

```text
```

---

## 10. Backup Storage Location

Confirm backups are stored safely.

* [ ] Backup destination is documented.
* [ ] Backup is stored outside the original server.
* [ ] Backup storage region is documented.
* [ ] Backup storage account or bucket is documented.
* [ ] Backup access is restricted.
* [ ] Backup encryption is reviewed.
* [ ] Backup lifecycle policy is documented.
* [ ] Backup deletion protection is reviewed if needed.
* [ ] Backup storage cost is reviewed.
* [ ] Backup naming convention is documented.

### Common Backup Destinations

* AWS EBS snapshots
* AWS Backup
* S3 bucket
* External backup server
* Managed hosting backup
* Database managed backup
* Client-owned storage

### Common Risk Indicators

* Backups stored only under `/backup` on the same server
* Publicly accessible S3 bucket
* No lifecycle policy
* No retention policy
* No clear owner

### Notes

```text
```

---

## 11. Retention and Versioning

Confirm how long backups are kept.

* [ ] Daily retention is defined.
* [ ] Weekly retention is defined if needed.
* [ ] Monthly retention is defined if needed.
* [ ] Yearly retention is defined if needed.
* [ ] Retention policy matches business needs.
* [ ] Retention policy matches storage cost expectations.
* [ ] Old backups are cleaned up safely.
* [ ] S3 versioning is reviewed if S3 is used.
* [ ] Snapshot retention is reviewed.
* [ ] Backup deletion process is controlled.

### Example Retention Policy

```text
Daily backups: 7 days
Weekly backups: 4 weeks
Monthly backups: 6 months
Yearly backups: not required
```

### Notes

```text
```

---

## 12. Encryption and Access Control

Confirm backup confidentiality.

* [ ] Backup encryption at rest is reviewed.
* [ ] Backup encryption in transit is reviewed.
* [ ] Access to backup storage is restricted.
* [ ] Backup restore permissions are restricted.
* [ ] IAM permissions follow least privilege where possible.
* [ ] Backup credentials are stored securely.
* [ ] Shared credentials are avoided.
* [ ] Former administrators' access is removed.
* [ ] Backup download access is logged where possible.
* [ ] Sensitive backup files are not publicly accessible.

### Notes

```text
```

---

## 13. Backup Automation

Confirm backup is not dependent only on manual work.

* [ ] Backup job is automated where possible.
* [ ] Backup schedule is documented.
* [ ] Cron job or systemd timer is documented.
* [ ] Backup script location is documented.
* [ ] Backup logs are reviewed.
* [ ] Backup job exit status is checked.
* [ ] Backup failure notification exists or is planned.
* [ ] Backup job does not silently overwrite the only previous backup.
* [ ] Backup job handles disk space safely.
* [ ] Backup job does not expose secrets in logs.

### Notes

```text
```

---

## 14. Restore Procedure

Confirm that restore steps are documented.

* [ ] Restore target is defined: same server, new EC2, local test, or staging.
* [ ] Required server packages are documented.
* [ ] Required system users are documented.
* [ ] Required file paths are documented.
* [ ] File restore procedure is documented.
* [ ] Database restore procedure is documented.
* [ ] Docker restore procedure is documented if Docker is used.
* [ ] DNS cutover procedure is documented if needed.
* [ ] TLS certificate recovery or renewal procedure is documented.
* [ ] Application startup procedure is documented.
* [ ] Post-restore verification steps are documented.
* [ ] Rollback procedure is documented if applicable.

### Notes

```text
```

---

## 15. Restore Testing

Confirm that restore has actually been tested.

* [ ] Restore test has been performed.
* [ ] Restore test date is documented.
* [ ] Restore tester is documented.
* [ ] Restore target environment is documented.
* [ ] Restore duration is documented.
* [ ] Problems found during restore are documented.
* [ ] Application login was tested after restore.
* [ ] Database content was checked after restore.
* [ ] Uploaded files were checked after restore.
* [ ] HTTPS was checked after restore.
* [ ] Background jobs were checked after restore if applicable.
* [ ] Restore test is scheduled periodically.

### Common Risk Indicators

* Backup exists but restore was never tested.
* Restore test requires missing passwords.
* Restore test requires undocumented manual steps.
* Backup files exist but application does not start after restore.
* Database restores but uploaded files are missing.

### Notes

```text
```

---

## 16. Recovery Objectives

Confirm recovery expectations.

* [ ] Recovery Time Objective is discussed.
* [ ] Recovery Point Objective is discussed.
* [ ] Maximum acceptable downtime is documented.
* [ ] Maximum acceptable data loss is documented.
* [ ] Business owner understands current backup limitations.
* [ ] Current backup setup matches business expectations.
* [ ] If expectations are not met, next actions are documented.

### Simple Definitions

```text
RTO: How long it may take to restore service.
RPO: How much recent data may be lost.
```

### Example

```text
RTO: Restore within 4 hours
RPO: Lose no more than 24 hours of data
```

### Notes

```text
```

---

## 17. Backup Risk Rating

Use this table to summarize backup readiness.

| Area                             | Status  | Risk Level          | Notes |
| -------------------------------- | ------- | ------------------- | ----- |
| Backup ownership                 | Unknown | Low / Medium / High |       |
| Server and application inventory | Unknown | Low / Medium / High |       |
| Backup scope                     | Unknown | Low / Medium / High |       |
| AWS-level backup                 | Unknown | Low / Medium / High |       |
| File-level backup                | Unknown | Low / Medium / High |       |
| Database backup                  | Unknown | Low / Medium / High |       |
| Docker backup                    | Unknown | Low / Medium / High |       |
| WordPress backup                 | N/A     | Low / Medium / High |       |
| Laravel backup                   | N/A     | Low / Medium / High |       |
| Backup storage location          | Unknown | Low / Medium / High |       |
| Retention and versioning         | Unknown | Low / Medium / High |       |
| Encryption and access control    | Unknown | Low / Medium / High |       |
| Backup automation                | Unknown | Low / Medium / High |       |
| Restore procedure                | Unknown | Low / Medium / High |       |
| Restore testing                  | Unknown | Low / Medium / High |       |
| Recovery objectives              | Unknown | Low / Medium / High |       |

---

## 18. Recommended Action Plan

Use this section to convert backup findings into action items.

| Priority | Action | Owner | Due Date | Status |
| -------- | ------ | ----- | -------- | ------ |
| High     |        |       |          |        |
| High     |        |       |          |        |
| Medium   |        |       |          |        |
| Medium   |        |       |          |        |
| Low      |        |       |          |        |

### Priority Guide

#### High Priority

Issues that may cause direct data loss or failed recovery.

Examples:

* No backup for production database
* No backup for uploaded files
* Backups stored only on the same server
* No restore procedure
* No one has access to restore credentials
* Server disk almost full because of local backups
* Publicly exposed backup files

#### Medium Priority

Issues that increase recovery risk.

Examples:

* Restore has never been tested
* Backup retention is unclear
* Backup failure alerts are missing
* Docker volumes are not clearly documented
* Backup owner is unclear
* No written restore checklist

#### Low Priority

Useful improvements that are not urgent.

Examples:

* Improve backup naming convention
* Add monthly restore drill
* Add more detailed restore screenshots
* Clean up obsolete backup files
* Add cost review for snapshots

---

## 19. Final Backup Review Summary

Complete this section after review.

```text
Server Name:

AWS Region:

Primary Application:

Backup Owner:

Backup Method:

Backup Storage Location:

Backup Frequency:

Retention Period:

Restore Tested:
Yes / No

Restore Test Date:

Main Backup Risks Found:

Recommended Immediate Actions:

Recommended Next Actions:

Reviewer:

Review Date:
```

---

## Related Toolkit Scripts

Recommended current script:

```bash
./scripts/ubuntu-server-audit.sh
```

Recommended output directory:

```text
reports/
```

Use the generated audit report together with this checklist to prepare a practical backup and restore readiness review.

