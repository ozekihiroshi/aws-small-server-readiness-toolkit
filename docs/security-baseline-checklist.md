# Security Baseline Checklist

This checklist is part of the **AWS Small Server Readiness Toolkit**.

It is intended for practical review of small AWS-based Linux servers, especially Ubuntu EC2 instances used for WordPress, Laravel, Docker-based applications, internal tools, and small business web services.

This checklist helps identify common operational and security risks before production use, migration, troubleshooting, or handover.

## Important Notice

This checklist does **not** provide legal compliance certification, security certification, penetration testing, or a guarantee against security incidents.

It is a practical baseline review document. For regulated data, financial data, healthcare data, government workloads, or legal compliance requirements, consult qualified security, legal, or compliance professionals.

## How to Use This Checklist

Recommended workflow:

1. Run the audit script.

   ```bash
   ./scripts/ubuntu-server-audit.sh
   ```

   Or, for more complete results:

   ```bash
   sudo ./scripts/ubuntu-server-audit.sh
   ```

2. Open the generated report under:

   ```text
   reports/
   ```

3. Review each checklist section below.

4. Mark each item as:

   * `[x] OK`
   * `[ ] Needs review`
   * `[ ] Not applicable`
   * `[ ] Unknown`

5. Record findings, risks, and recommended actions.

---

## 1. Server Identity and Purpose

Confirm that the server's role is clearly understood.

* [ ] Server hostname is meaningful and documented.
* [ ] Server purpose is documented.
* [ ] Business owner or responsible person is identified.
* [ ] Technical administrator is identified.
* [ ] Environment is identified: production, staging, development, test, or temporary.
* [ ] Important applications running on the server are listed.
* [ ] Important data stored on the server is identified.
* [ ] Server criticality is classified: low, medium, high, or business-critical.

### Notes

```text
```

---

## 2. AWS Region and Data Location

Confirm that the server is running in the expected AWS region.

* [ ] AWS region is confirmed.
* [ ] Region choice is intentional and documented.
* [ ] Data location requirements are understood.
* [ ] Backups and snapshots are stored in approved regions.
* [ ] Any cross-region replication is documented.
* [ ] Public IP usage is intentional.
* [ ] Elastic IP usage is documented if applicable.
* [ ] DNS records point to the correct server or load balancer.

### Notes

```text
```

---

## 3. AWS Account and IAM Responsibility

Confirm that AWS account ownership and access responsibility are clear.

* [ ] AWS account owner is identified.
* [ ] Root account is not used for daily operations.
* [ ] MFA is enabled for root account.
* [ ] Administrative IAM users or roles are documented.
* [ ] IAM access follows least privilege where possible.
* [ ] EC2 instance role is attached only if needed.
* [ ] EC2 instance role permissions are minimal.
* [ ] Long-term access keys are avoided where possible.
* [ ] Unused IAM users, roles, and keys are reviewed.

### Notes

```text
```

---

## 4. Network Exposure and Security Groups

Confirm that only required network access is allowed.

* [ ] AWS Security Group inbound rules are reviewed.
* [ ] SSH port is not open to the entire internet unless explicitly justified.
* [ ] HTTP port 80 is open only if needed.
* [ ] HTTPS port 443 is open only if needed.
* [ ] Database ports are not publicly exposed.
* [ ] Admin panels are not publicly exposed unless required and protected.
* [ ] Source IP restrictions are applied where practical.
* [ ] Unused inbound rules are removed.
* [ ] Outbound rules are reviewed.
* [ ] Network ACLs are reviewed if custom rules are used.

### Common Risk Indicators

* SSH open to `0.0.0.0/0`
* MySQL, PostgreSQL, Redis, or Elasticsearch exposed publicly
* Multiple unused open ports
* Unknown services listening on public interfaces

### Notes

```text
```

---

## 5. SSH Access Control

Confirm that SSH access is restricted and manageable.

* [ ] SSH users are documented.
* [ ] SSH key-based authentication is used.
* [ ] Password-based SSH login is disabled where possible.
* [ ] Root SSH login is disabled.
* [ ] SSH access is restricted by source IP, VPN, or bastion host where practical.
* [ ] Unused user accounts are disabled or removed.
* [ ] `authorized_keys` files are reviewed.
* [ ] Old or unknown SSH keys are removed.
* [ ] SSH port change is documented if a non-standard port is used.
* [ ] AWS Systems Manager Session Manager is considered as an alternative to direct SSH.

### Notes

```text
```

---

## 6. Operating System Updates

Confirm that the server is reasonably up to date.

* [ ] Ubuntu version is supported.
* [ ] Package update status is reviewed.
* [ ] Security updates are applied regularly.
* [ ] Reboot requirement is checked.
* [ ] Kernel update status is reviewed.
* [ ] Unattended upgrades are considered or configured.
* [ ] Update responsibility is documented.
* [ ] Maintenance window is defined for production systems.

### Common Risk Indicators

* Many pending security updates
* Unsupported Ubuntu release
* Server requires reboot but has not been rebooted for a long time
* No defined patching responsibility

### Notes

```text
```

---

## 7. Local Firewall

Confirm that host-level firewall policy is understood.

* [ ] UFW or another host firewall is installed or intentionally not used.
* [ ] Firewall status is documented.
* [ ] Allowed ports are reviewed.
* [ ] Firewall rules match the application requirements.
* [ ] Firewall configuration does not conflict with AWS Security Groups.
* [ ] Local firewall policy is included in handover documentation.

### Notes

```text
```

---

## 8. Listening Ports and Services

Confirm that running services are expected.

* [ ] Listening TCP and UDP ports are reviewed.
* [ ] Each public-facing service has a known purpose.
* [ ] Unused services are disabled.
* [ ] Failed systemd services are reviewed.
* [ ] Enabled services are reviewed.
* [ ] Timers and scheduled services are reviewed.
* [ ] Unknown services are investigated.

### Common Risk Indicators

* Unknown service listening on `0.0.0.0`
* Failed systemd services ignored
* Old application services still enabled
* Development tools exposed on public ports

### Notes

```text
```

---

## 9. Web Server and TLS

Confirm that web services are configured safely.

* [ ] Web server type is identified: Nginx, Apache, Caddy, Traefik, or other.
* [ ] Active virtual hosts or server blocks are reviewed.
* [ ] HTTPS is enabled for production websites.
* [ ] TLS certificate issuer and expiry date are checked.
* [ ] Certificate renewal process is confirmed.
* [ ] HTTP to HTTPS redirect is configured where appropriate.
* [ ] Default site is disabled or intentionally configured.
* [ ] Web root paths are documented.
* [ ] Web server configuration files are backed up.
* [ ] Admin interfaces are protected.

### Notes

```text
```

---

## 10. Docker and Containers

Confirm that Docker usage is understood and documented.

* [ ] Docker is installed only if needed.
* [ ] Running containers are reviewed.
* [ ] Stopped containers are reviewed.
* [ ] Docker images are reviewed.
* [ ] Docker Compose files are located and documented.
* [ ] Application data volumes are identified.
* [ ] Bind mounts are reviewed.
* [ ] Secrets and environment files are handled carefully.
* [ ] Containers are not running with unnecessary privileged mode.
* [ ] Docker disk usage is reviewed.
* [ ] Restart policies are configured where needed.
* [ ] Container update process is documented.

### Common Risk Indicators

* Unknown containers running
* Important data stored only inside containers
* No backup of Docker volumes
* `.env` files containing secrets without access control
* Containers exposing unnecessary ports

### Notes

```text
```

---

## 11. Application and Data Location

Confirm that application files and important data are known.

* [ ] Application root directory is documented.
* [ ] Configuration file locations are documented.
* [ ] Environment variable files are identified.
* [ ] Uploaded files or user-generated content locations are identified.
* [ ] Database type and location are documented.
* [ ] Log file locations are documented.
* [ ] Cron jobs or scheduled application tasks are documented.
* [ ] Deployment process is documented.
* [ ] Rollback process is documented if available.

### Notes

```text
```

---

## 12. Backup Readiness

Confirm that backup exists, covers important data, and can be restored.

* [ ] Backup owner is identified.
* [ ] Backup target is documented.
* [ ] Backup schedule is documented.
* [ ] Backup retention period is documented.
* [ ] EBS snapshot policy is reviewed.
* [ ] Application file backup is reviewed.
* [ ] Database backup is reviewed.
* [ ] Docker volume backup is reviewed if Docker is used.
* [ ] Backup encryption is reviewed.
* [ ] Backup region/location is reviewed.
* [ ] Restore procedure is documented.
* [ ] Restore test has been performed or scheduled.
* [ ] Backup failure alerting is configured or planned.

### Common Risk Indicators

* Backup exists but restore has never been tested
* Only application files are backed up, but database is not
* Only database is backed up, but uploaded files are not
* Docker volumes are not included
* Backups are stored only on the same server
* No one knows who receives backup failure alerts

### Notes

```text
```

---

## 13. Disk, Memory, and Capacity

Confirm that the server has enough capacity and room for growth.

* [ ] Disk usage is reviewed.
* [ ] Large directories are identified.
* [ ] Log growth risk is reviewed.
* [ ] Docker disk usage is reviewed if Docker is used.
* [ ] Memory usage is reviewed.
* [ ] Swap configuration is reviewed.
* [ ] CPU usage is reviewed.
* [ ] Instance type is appropriate for workload.
* [ ] EBS volume size is appropriate.
* [ ] EBS volume type is appropriate.
* [ ] Growth risk is documented.

### Common Risk Indicators

* Root filesystem over 80% used
* Docker consuming large disk space
* Logs growing without rotation
* No swap on a small-memory instance
* Instance frequently running out of memory

### Notes

```text
```

---

## 14. Monitoring and Logging

Confirm that failures can be detected.

* [ ] Basic uptime monitoring exists.
* [ ] Disk usage alert exists or is planned.
* [ ] CPU and memory monitoring exists or is planned.
* [ ] Application health check exists or is planned.
* [ ] Web server logs are available.
* [ ] Application logs are available.
* [ ] System logs are available.
* [ ] Log retention is understood.
* [ ] CloudWatch Agent is installed or intentionally not used.
* [ ] AWS SSM Agent status is reviewed.
* [ ] Alert recipients are documented.
* [ ] Monitoring ownership is documented.

### Notes

```text
```

---

## 15. Authentication, Secrets, and Sensitive Files

Confirm that secrets are handled carefully.

* [ ] Application secrets are not committed to Git.
* [ ] `.env` files are protected by file permissions.
* [ ] Database passwords are not exposed in public files.
* [ ] API keys are reviewed.
* [ ] Old credentials are rotated if ownership changed.
* [ ] AWS credentials are not stored unnecessarily on the server.
* [ ] Private keys are protected.
* [ ] Access to backup files is restricted.
* [ ] Secrets rotation responsibility is documented.

### Common Risk Indicators

* AWS keys stored in application directory
* `.env` readable by unnecessary users
* Secrets included in backups without encryption
* Shared administrator account
* Unknown former users still have access

### Notes

```text
```

---

## 16. Database Exposure and Protection

Confirm that database access is controlled.

* [ ] Database type is identified.
* [ ] Database location is documented: local, Docker, RDS, external, or managed service.
* [ ] Database is not publicly exposed unless explicitly required and protected.
* [ ] Database user accounts are reviewed.
* [ ] Database backup is configured.
* [ ] Database restore process is documented.
* [ ] Database credentials are stored securely.
* [ ] Database logs are reviewed if needed.
* [ ] RDS security group is reviewed if RDS is used.
* [ ] RDS backup retention is reviewed if RDS is used.

### Notes

```text
```

---

## 17. Malware, Intrusion, and Abuse Signals

Confirm that basic signs of compromise or abuse are checked.

* [ ] Recent failed login attempts are reviewed.
* [ ] Unknown users are investigated.
* [ ] Unknown cron jobs are investigated.
* [ ] Unknown listening ports are investigated.
* [ ] Unexpected high CPU processes are investigated.
* [ ] Unexpected high memory processes are investigated.
* [ ] Suspicious files in temporary directories are reviewed if needed.
* [ ] Mail queue or spam behavior is reviewed if relevant.
* [ ] Security agent status is reviewed if installed.

### Notes

```text
```

---

## 18. Handover Documentation

Confirm that another administrator can understand and operate the server.

* [ ] Server purpose is documented.
* [ ] AWS account and region are documented.
* [ ] Domain and DNS records are documented.
* [ ] Application paths are documented.
* [ ] Service start/stop procedures are documented.
* [ ] Deployment procedure is documented.
* [ ] Backup procedure is documented.
* [ ] Restore procedure is documented.
* [ ] Monitoring procedure is documented.
* [ ] Important contacts are documented.
* [ ] Known risks are documented.
* [ ] Next recommended actions are documented.

### Notes

```text
```

---

## 19. Risk Rating

Use this table to summarize findings.

| Area                          | Status  | Risk Level          | Notes |
| ----------------------------- | ------- | ------------------- | ----- |
| Server identity and purpose   | Unknown | Low / Medium / High |       |
| AWS region and data location  | Unknown | Low / Medium / High |       |
| IAM and access responsibility | Unknown | Low / Medium / High |       |
| Network exposure              | Unknown | Low / Medium / High |       |
| SSH access control            | Unknown | Low / Medium / High |       |
| OS updates                    | Unknown | Low / Medium / High |       |
| Firewall                      | Unknown | Low / Medium / High |       |
| Listening services            | Unknown | Low / Medium / High |       |
| Web server and TLS            | Unknown | Low / Medium / High |       |
| Docker and containers         | Unknown | Low / Medium / High |       |
| Application and data location | Unknown | Low / Medium / High |       |
| Backup readiness              | Unknown | Low / Medium / High |       |
| Disk and capacity             | Unknown | Low / Medium / High |       |
| Monitoring and logging        | Unknown | Low / Medium / High |       |
| Secrets handling              | Unknown | Low / Medium / High |       |
| Database protection           | Unknown | Low / Medium / High |       |
| Handover documentation        | Unknown | Low / Medium / High |       |

---

## 20. Recommended Action Plan

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

Issues that may expose the server or business data directly.

Examples:

* SSH open to the world with weak access control
* Database publicly exposed
* No backup for production data
* Expired TLS certificate
* Unknown administrator access
* Critical disk usage
* Unsupported operating system

#### Medium Priority

Issues that increase operational risk but may not be immediately exposed.

Examples:

* No restore test
* Incomplete monitoring
* Many pending updates
* Poor handover documentation
* Unclear Docker volume backup
* No documented maintenance window

#### Low Priority

Improvements that are useful but not urgent.

Examples:

* Improve naming conventions
* Add more detailed documentation
* Clean up old stopped containers
* Review unused packages
* Improve report formatting

---

## 21. Final Review Summary

Complete this section after review.

```text
Server Name:

AWS Region:

Primary Purpose:

Main Applications:

Main Risks Found:

Recommended Immediate Actions:

Recommended Next Actions:

Reviewer:

Review Date:
```

---

## Related Toolkit Scripts

Recommended script:

```bash
./scripts/ubuntu-server-audit.sh
```

Recommended output directory:

```text
reports/
```

Use the generated audit report together with this checklist to prepare a practical server readiness review.