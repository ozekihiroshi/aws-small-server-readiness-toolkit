# EC2 Readiness Checklist

This checklist is part of the **AWS Small Server Readiness Toolkit**.

It is intended for practical review of small AWS EC2-based Linux servers before production use, migration, troubleshooting, or handover.

This checklist focuses on AWS EC2-level readiness, including instance configuration, storage, networking, access, backup, monitoring, and operational documentation.

## Important Notice

This checklist does **not** provide legal compliance certification, security certification, penetration testing, or a guarantee against incidents.

It is a practical readiness review document for small business AWS servers.

For regulated workloads, financial data, healthcare data, government workloads, or formal compliance requirements, consult qualified AWS, security, legal, or compliance professionals.

## When to Use This Checklist

Use this checklist before:

* Launching a new EC2 production server
* Migrating WordPress, Laravel, or another web application to AWS
* Handing over an EC2 server to another administrator
* Reviewing an existing EC2 server
* Troubleshooting operational risk
* Preparing a client-facing server readiness report

---

## 1. EC2 Instance Identity

Confirm that the instance is clearly identified.

* [ ] AWS account is identified.
* [ ] AWS region is identified.
* [ ] EC2 instance ID is documented.
* [ ] Instance name tag is meaningful.
* [ ] Hostname is meaningful.
* [ ] Environment is identified: production, staging, development, test, or temporary.
* [ ] Server purpose is documented.
* [ ] Business owner is identified.
* [ ] Technical owner is identified.
* [ ] Launch date or migration date is documented.

### Notes

```text
```

---

## 2. AWS Region and Availability Zone

Confirm that the server location is intentional.

* [ ] AWS region is appropriate for the business requirement.
* [ ] Availability Zone is documented.
* [ ] Data residency expectations are understood if applicable.
* [ ] Backups and snapshots are stored in acceptable regions.
* [ ] Cross-region replication is documented if used.
* [ ] Latency requirements are considered.
* [ ] Region-specific cost differences are considered.
* [ ] Region choice is documented in handover notes.

### Common Risk Indicators

* Server is running in an unintended region.
* Backups are copied to an unexpected region.
* Client expects local data residency but region was not confirmed.
* Region choice is not documented.

### Notes

```text
```

---

## 3. Instance Type and Capacity

Confirm that the instance size matches the workload.

* [ ] Instance type is documented.
* [ ] vCPU and memory are appropriate for the workload.
* [ ] CPU usage is reviewed.
* [ ] Memory usage is reviewed.
* [ ] Swap configuration is reviewed.
* [ ] Instance type is not oversized without reason.
* [ ] Instance type is not undersized for production use.
* [ ] Burstable instance credit behavior is understood if using T-family instances.
* [ ] Scaling plan is documented if future growth is expected.
* [ ] Cost impact of instance size is reviewed.

### Common Risk Indicators

* Production server runs out of memory.
* CPU credits are exhausted on burstable instances.
* Instance is much larger than needed.
* No one knows why the instance type was selected.

### Notes

```text
```

---

## 4. Operating System and AMI

Confirm that the base operating system is supportable.

* [ ] Operating system is documented.
* [ ] OS version is currently supported.
* [ ] AMI source is known.
* [ ] AMI ID is documented if needed.
* [ ] Kernel version is reviewed.
* [ ] Package update status is reviewed.
* [ ] Reboot requirement is checked.
* [ ] OS upgrade path is understood.
* [ ] Custom AMI usage is documented if applicable.
* [ ] Unsupported or end-of-life OS versions are avoided.

### Notes

```text
```

---

## 5. EBS Volumes and Storage

Confirm that storage is correctly sized, encrypted, and documented.

* [ ] Root EBS volume is documented.
* [ ] Additional EBS volumes are documented.
* [ ] Volume size is appropriate.
* [ ] Volume type is appropriate.
* [ ] EBS encryption status is reviewed.
* [ ] Filesystem usage is reviewed.
* [ ] Mount points are documented.
* [ ] Filesystem type is documented.
* [ ] Disk expansion procedure is documented if needed.
* [ ] Data volume separation is considered for important workloads.
* [ ] Delete-on-termination setting is reviewed.
* [ ] Old unattached EBS volumes are reviewed.

### Common Risk Indicators

* Root filesystem is over 80% used.
* Important application data is stored on the root volume without backup.
* EBS encryption status is unknown.
* Unattached volumes are accumulating cost.
* Delete-on-termination behavior is not understood.

### Notes

```text
```

---

## 6. Public IP and DNS

Confirm how users and administrators reach the server.

* [ ] Public IP usage is intentional.
* [ ] Elastic IP is used if stable IP is required.
* [ ] Elastic IP allocation is documented.
* [ ] DNS records are documented.
* [ ] DNS TTL is reviewed before migration.
* [ ] Domain owner is identified.
* [ ] DNS provider is identified.
* [ ] Reverse proxy or load balancer usage is documented.
* [ ] Old DNS records are reviewed.
* [ ] Cutover and rollback plan is documented for migrations.

### Common Risk Indicators

* Public IP changes after stop/start because Elastic IP is not used.
* DNS points to an old server.
* No one knows who controls the domain.
* Migration is performed without DNS rollback plan.

### Notes

```text
```

---

## 7. Security Group Review

Confirm that AWS Security Groups expose only required services.

* [ ] Attached Security Groups are documented.
* [ ] Inbound rules are reviewed.
* [ ] Outbound rules are reviewed.
* [ ] SSH access is restricted where practical.
* [ ] HTTP access is intentional.
* [ ] HTTPS access is intentional.
* [ ] Database ports are not publicly exposed.
* [ ] Admin tools are not publicly exposed unless protected.
* [ ] Source IP restrictions are applied where appropriate.
* [ ] Unused rules are removed.
* [ ] Rule descriptions are meaningful.

### Common Risk Indicators

* SSH open to `0.0.0.0/0`.
* MySQL, PostgreSQL, Redis, or Elasticsearch exposed publicly.
* Security Group rules have no descriptions.
* Old migration/test ports remain open.
* Multiple Security Groups make exposure unclear.

### Notes

```text
```

---

## 8. SSH and Administrative Access

Confirm that administrative access is controlled.

* [ ] SSH users are documented.
* [ ] SSH private key owner is known.
* [ ] Password authentication is disabled where possible.
* [ ] Root SSH login is disabled.
* [ ] `authorized_keys` files are reviewed.
* [ ] Old administrator keys are removed.
* [ ] SSH source IP restriction is configured where practical.
* [ ] Bastion host or VPN is considered if appropriate.
* [ ] AWS Systems Manager Session Manager is considered.
* [ ] Emergency access procedure is documented.

### Notes

```text
```

---

## 9. IAM Role and AWS Permissions

Confirm that instance permissions are minimal and intentional.

* [ ] EC2 instance profile is documented.
* [ ] IAM role is attached only if needed.
* [ ] IAM role permissions are reviewed.
* [ ] Permissions follow least privilege where practical.
* [ ] S3 access is limited to required buckets.
* [ ] CloudWatch permissions are limited to required actions.
* [ ] Backup-related permissions are reviewed.
* [ ] Long-term AWS access keys are not stored unnecessarily on the server.
* [ ] AWS CLI configuration is reviewed if used.
* [ ] Former administrator access is removed.

### Common Risk Indicators

* EC2 instance has broad administrator permissions.
* AWS access keys are stored under application directories.
* Unknown IAM role attached to the server.
* Instance can access unrelated S3 buckets.

### Notes

```text
```

---

## 10. Network Architecture

Confirm that network placement is understood.

* [ ] VPC ID is documented.
* [ ] Subnet ID is documented.
* [ ] Public or private subnet placement is documented.
* [ ] Route table is understood.
* [ ] Internet Gateway usage is documented.
* [ ] NAT Gateway usage is documented if applicable.
* [ ] Load balancer usage is documented if applicable.
* [ ] VPC endpoints are reviewed if used.
* [ ] Network ACLs are reviewed if custom rules exist.
* [ ] Network architecture is included in handover documentation.

### Notes

```text
```

---

## 11. Web Server Readiness

Confirm that web traffic is handled properly.

* [ ] Web server type is identified: Nginx, Apache, Caddy, Traefik, or other.
* [ ] Web server configuration paths are documented.
* [ ] Active virtual hosts or server blocks are reviewed.
* [ ] Default site is disabled or intentionally configured.
* [ ] HTTPS is enabled for production websites.
* [ ] HTTP to HTTPS redirect is configured where appropriate.
* [ ] TLS certificate renewal method is documented.
* [ ] Web root directories are documented.
* [ ] Reverse proxy settings are documented if used.
* [ ] Upload size and timeout settings are reviewed if relevant.

### Notes

```text
```

---

## 12. Application Readiness

Confirm that the application can be operated and restored.

* [ ] Application name and purpose are documented.
* [ ] Application root path is documented.
* [ ] Runtime version is documented: PHP, Node.js, Python, Ruby, Java, or other.
* [ ] Framework version is documented if applicable.
* [ ] Deployment procedure is documented.
* [ ] Rollback procedure is documented if available.
* [ ] Environment variables are documented securely.
* [ ] Application logs are documented.
* [ ] Scheduled jobs are documented.
* [ ] Background workers are documented if used.
* [ ] Application health check is defined.
* [ ] Application administrator account recovery process is understood.

### Notes

```text
```

---

## 13. Database Readiness

Confirm that database location and protection are clear.

* [ ] Database type is documented.
* [ ] Database location is documented: local, Docker, RDS, external, or managed service.
* [ ] Database port exposure is reviewed.
* [ ] Database credentials are stored securely.
* [ ] Database backup is configured.
* [ ] Database restore procedure is documented.
* [ ] Database size is reviewed.
* [ ] Database performance risk is reviewed.
* [ ] Database upgrade responsibility is documented.
* [ ] RDS backup and security group settings are reviewed if RDS is used.

### Common Risk Indicators

* Database is publicly exposed.
* Database backup is not configured.
* Database is inside Docker volume but volume backup is unclear.
* Database credentials are unknown.
* Database is on the same server but not included in restore plan.

### Notes

```text
```

---

## 14. Docker Readiness

Confirm Docker usage if applicable.

* [ ] Docker is installed only if needed.
* [ ] Docker Compose files are documented.
* [ ] Running containers are reviewed.
* [ ] Persistent volumes are documented.
* [ ] Bind mounts are documented.
* [ ] Port mappings are reviewed.
* [ ] Restart policies are configured where needed.
* [ ] `.env` files are protected.
* [ ] Container logs are reviewed.
* [ ] Docker disk usage is reviewed.
* [ ] Container update procedure is documented.
* [ ] Docker backup and restore procedure is documented.

### Notes

```text
```

---

## 15. Backup Readiness

Confirm that EC2 and application data can be recovered.

* [ ] EBS snapshot policy is configured or planned.
* [ ] Snapshot retention is documented.
* [ ] Application file backup is configured.
* [ ] Database backup is configured.
* [ ] Docker volume backup is configured if Docker is used.
* [ ] Backup destination is outside the original server.
* [ ] Backup encryption is reviewed.
* [ ] Backup failure alerting exists or is planned.
* [ ] Restore procedure is documented.
* [ ] Restore test has been performed or scheduled.

### Notes

```text
```

---

## 16. Monitoring and Alerting

Confirm that problems can be detected.

* [ ] Basic uptime monitoring exists.
* [ ] CPU monitoring exists or is planned.
* [ ] Memory monitoring exists or is planned.
* [ ] Disk usage monitoring exists or is planned.
* [ ] Application health monitoring exists or is planned.
* [ ] CloudWatch Agent is installed or intentionally not used.
* [ ] AWS SSM Agent is installed or intentionally not used.
* [ ] Alert recipients are documented.
* [ ] Monitoring owner is documented.
* [ ] Log retention is reviewed.

### Common Risk Indicators

* No disk alert on production server.
* No one receives failure notifications.
* Monitoring exists but alerts go to a former administrator.
* Application can fail while EC2 still appears healthy.

### Notes

```text
```

---

## 17. Logging

Confirm that logs are available for troubleshooting.

* [ ] System logs are available.
* [ ] Authentication logs are available.
* [ ] Web server access logs are available.
* [ ] Web server error logs are available.
* [ ] Application logs are available.
* [ ] Docker logs are reviewed if Docker is used.
* [ ] Log rotation is configured.
* [ ] Log retention is appropriate.
* [ ] Sensitive data is not unnecessarily logged.
* [ ] Centralized logging is considered if needed.

### Notes

```text
```

---

## 18. Cost and Resource Hygiene

Confirm that unnecessary cost is avoided.

* [ ] Instance type cost is reviewed.
* [ ] EBS volume cost is reviewed.
* [ ] Snapshot cost is reviewed.
* [ ] Elastic IP cost is reviewed.
* [ ] Unattached EBS volumes are reviewed.
* [ ] Old AMIs are reviewed.
* [ ] Old snapshots are reviewed.
* [ ] Unused load balancers are reviewed.
* [ ] Unused NAT Gateways are reviewed.
* [ ] Cost ownership is documented.

### Notes

```text
```

---

## 19. Handover Documentation

Confirm that another administrator can operate the server.

* [ ] AWS account name or ID is documented.
* [ ] AWS region is documented.
* [ ] EC2 instance ID is documented.
* [ ] Instance purpose is documented.
* [ ] Domain and DNS records are documented.
* [ ] SSH access procedure is documented.
* [ ] Application paths are documented.
* [ ] Service start/stop procedure is documented.
* [ ] Deployment procedure is documented.
* [ ] Backup procedure is documented.
* [ ] Restore procedure is documented.
* [ ] Monitoring procedure is documented.
* [ ] Known risks are documented.
* [ ] Next recommended actions are documented.

### Notes

```text
```

---

## 20. EC2 Readiness Risk Rating

Use this table to summarize readiness.

| Area                          | Status  | Risk Level          | Notes |
| ----------------------------- | ------- | ------------------- | ----- |
| EC2 instance identity         | Unknown | Low / Medium / High |       |
| Region and Availability Zone  | Unknown | Low / Medium / High |       |
| Instance type and capacity    | Unknown | Low / Medium / High |       |
| Operating system and AMI      | Unknown | Low / Medium / High |       |
| EBS volumes and storage       | Unknown | Low / Medium / High |       |
| Public IP and DNS             | Unknown | Low / Medium / High |       |
| Security Group review         | Unknown | Low / Medium / High |       |
| SSH and administrative access | Unknown | Low / Medium / High |       |
| IAM role and AWS permissions  | Unknown | Low / Medium / High |       |
| Network architecture          | Unknown | Low / Medium / High |       |
| Web server readiness          | Unknown | Low / Medium / High |       |
| Application readiness         | Unknown | Low / Medium / High |       |
| Database readiness            | Unknown | Low / Medium / High |       |
| Docker readiness              | N/A     | Low / Medium / High |       |
| Backup readiness              | Unknown | Low / Medium / High |       |
| Monitoring and alerting       | Unknown | Low / Medium / High |       |
| Logging                       | Unknown | Low / Medium / High |       |
| Cost and resource hygiene     | Unknown | Low / Medium / High |       |
| Handover documentation        | Unknown | Low / Medium / High |       |

---

## 21. Recommended Action Plan

Use this section to convert findings into action items.

| Priority | Action | Owner | Due Date | Status |
| -------- | ------ | ----- | -------- | ------ |
| High     |        |       |          |        |
| High     |        |       |          |        |
| Medium   |        |       |          |        |
| Medium   |        |       |          |        |
| Low      |        |       |          |        |

### Priority Guide

#### High Priority

Issues that may directly expose the server, interrupt service, or cause data loss.

Examples:

* SSH open to the world without strong access control
* Database publicly exposed
* No backup for production data
* EBS volume almost full
* Unsupported operating system
* Unknown administrator access
* No restore procedure

#### Medium Priority

Issues that increase operational risk.

Examples:

* No restore test
* Incomplete monitoring
* No handover documentation
* Unclear Docker volume ownership
* Many pending package updates
* Security Group descriptions missing

#### Low Priority

Useful improvements that are not urgent.

Examples:

* Improve naming conventions
* Clean up old snapshots
* Add better diagrams
* Improve log retention documentation
* Review cost optimization later

---

## 22. Final EC2 Readiness Summary

Complete this section after review.

```text
Server Name:

AWS Account:

AWS Region:

Availability Zone:

EC2 Instance ID:

Instance Type:

Operating System:

Primary Application:

Public IP / Elastic IP:

Domain Name:

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
docs/security-baseline-checklist.md
docs/backup-and-restore-checklist.md
```

Recommended current script:

```bash
./scripts/ubuntu-server-audit.sh
```

Use the generated audit report together with this checklist to prepare a practical EC2 readiness review.
