# Ubuntu Hardening Notes

This document is part of the **AWS Small Server Readiness Toolkit**.

It provides practical Ubuntu hardening notes for small AWS EC2 servers used for WordPress, Laravel, Docker-based applications, internal tools, and small business web services.

The goal is to help review and improve a server gradually and safely after a baseline audit.

## Important Notice

This document does **not** provide legal compliance certification, security certification, penetration testing, or a guarantee against incidents.

It is not a complete CIS benchmark implementation.

It is a practical hardening reference for small server environments. Any change to a production server should be tested, documented, and backed up before applying.

## Recommended Workflow

Before applying hardening changes:

1. Run the audit script.

   ```bash
   ./scripts/ubuntu-server-audit.sh
   ```

2. Review the generated report.

3. Confirm backup and restore readiness.

   ```text
   docs/backup-and-restore-checklist.md
   ```

4. Review the security baseline checklist.

   ```text
   docs/security-baseline-checklist.md
   ```

5. Apply changes gradually.

6. Re-run the audit script.

7. Document what changed.

---

## 1. Before Hardening

Do not start hardening before understanding the server.

* [ ] Confirm this is the correct server.
* [ ] Confirm whether it is production, staging, development, or test.
* [ ] Confirm who owns the server.
* [ ] Confirm who can approve changes.
* [ ] Confirm maintenance window.
* [ ] Confirm backup exists.
* [ ] Confirm restore procedure exists.
* [ ] Confirm emergency access method.
* [ ] Confirm current SSH access method.
* [ ] Confirm critical services running on the server.

### Important

Before changing SSH, firewall, or network settings, make sure you have a recovery method.

For AWS EC2, this may include:

* AWS console access
* EC2 serial console if enabled
* Systems Manager Session Manager
* Existing open SSH session
* EBS volume detach/attach recovery procedure
* Recent AMI or EBS snapshot

---

## 2. Package Updates

Keeping the operating system updated is one of the most basic and important hardening steps.

### Review Commands

```bash
apt list --upgradable
cat /var/run/reboot-required 2>/dev/null || echo "No reboot required"
lsb_release -a
uname -a
```

### Recommended Actions

* [ ] Review pending updates.
* [ ] Apply security updates regularly.
* [ ] Reboot after kernel or critical library updates when required.
* [ ] Define a maintenance window for production systems.
* [ ] Consider unattended security updates if appropriate.
* [ ] Document update responsibility.

### Example Commands

```bash
sudo apt update
sudo apt upgrade
```

For production systems, avoid applying updates blindly. Review impact first.

### Notes

```text
```

---

## 3. Unattended Security Updates

Ubuntu can automatically install security updates using `unattended-upgrades`.

### Review Commands

```bash
dpkg -l | grep unattended-upgrades
systemctl status unattended-upgrades --no-pager
```

### Recommended Actions

* [ ] Decide whether unattended security updates are appropriate.
* [ ] Confirm whether automatic reboot is enabled or disabled.
* [ ] Confirm update logs are reviewed.
* [ ] Confirm production systems have a maintenance policy.

### Notes

Automatic updates reduce exposure to known vulnerabilities, but they may also introduce service restart behavior. For business-critical systems, define the policy clearly.

### Notes

```text
```

---

## 4. SSH Hardening

SSH is often the most important administrative entry point.

### Review Commands

```bash
sudo sshd -T | grep -Ei 'permitrootlogin|passwordauthentication|pubkeyauthentication|kbdinteractiveauthentication|challengeresponseauthentication|port|listenaddress'
grep -Ei '^(PermitRootLogin|PasswordAuthentication|PubkeyAuthentication|KbdInteractiveAuthentication|ChallengeResponseAuthentication|Port|AllowUsers|AllowGroups)' /etc/ssh/sshd_config
find /home /root -maxdepth 3 -name authorized_keys -type f -print
```

### Recommended Settings to Review

* [ ] Disable root SSH login where possible.
* [ ] Disable password authentication where possible.
* [ ] Use SSH key-based authentication.
* [ ] Remove old administrator keys.
* [ ] Restrict SSH by Security Group source IP where practical.
* [ ] Consider `AllowUsers` or `AllowGroups` for stricter access.
* [ ] Consider AWS Systems Manager Session Manager.
* [ ] Keep an emergency access path before making changes.

### Common Settings

```text
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
```

### Safe Change Procedure

1. Open a second SSH session before changing SSH settings.
2. Edit SSH configuration.
3. Validate syntax.
4. Reload SSH.
5. Test new login in a new terminal.
6. Keep the old session open until confirmed.

### Example Commands

```bash
sudo sshd -t
sudo systemctl reload ssh
```

Depending on the Ubuntu version, the service may be named `ssh` or `sshd`.

### Common Risk Indicators

* `PermitRootLogin yes`
* `PasswordAuthentication yes`
* Unknown keys in `authorized_keys`
* SSH open to `0.0.0.0/0`
* Shared administrator account
* No emergency recovery method

### Notes

```text
```

---

## 5. Firewall and Network Exposure

On AWS EC2, network exposure is usually controlled by Security Groups, but host-level firewall rules may also be used.

### Review Commands

```bash
sudo ufw status verbose
ss -tulpen
ip addr show
ip route show
```

### Recommended Actions

* [ ] Review AWS Security Group inbound rules.
* [ ] Review host-level firewall status.
* [ ] Confirm required public ports.
* [ ] Remove unused open ports.
* [ ] Restrict SSH by source IP where practical.
* [ ] Ensure database ports are not publicly exposed.
* [ ] Document all intentionally exposed ports.

### Typical Public Ports

```text
22/tcp    SSH, preferably restricted
80/tcp    HTTP
443/tcp   HTTPS
```

### Common Risk Indicators

* SSH open to the world
* Database port open to the world
* Unknown process listening on `0.0.0.0`
* Old test ports still open
* UFW disabled while Security Group is too permissive

### Notes

```text
```

---

## 6. User and Sudo Access

Review who can log in and who can become root.

### Review Commands

```bash
awk -F: '($7 !~ /(nologin|false)$/) {print $1 ":" $6 ":" $7}' /etc/passwd
getent group sudo
sudo -l
last -n 20
```

### Recommended Actions

* [ ] Document administrator users.
* [ ] Remove unused users.
* [ ] Lock former administrator accounts.
* [ ] Review sudo group members.
* [ ] Avoid shared accounts where possible.
* [ ] Review recent login history.
* [ ] Rotate credentials after handover.

### Example Commands

Lock a user account:

```bash
sudo usermod -L username
```

Remove a user only after confirming ownership of files and jobs:

```bash
sudo deluser username
```

### Notes

```text
```

---

## 7. System Services

Review enabled and failed services.

### Review Commands

```bash
systemctl --failed --no-pager
systemctl list-unit-files --type=service --state=enabled --no-pager
systemctl list-timers --all --no-pager
```

### Recommended Actions

* [ ] Investigate failed services.
* [ ] Disable unused services.
* [ ] Document required services.
* [ ] Document custom systemd services.
* [ ] Review systemd timers.
* [ ] Confirm application services restart after reboot.

### Example Commands

Disable an unused service only after confirming it is not required:

```bash
sudo systemctl disable service-name
sudo systemctl stop service-name
```

### Common Risk Indicators

* Failed services ignored
* Unknown custom services
* Application does not restart after reboot
* Old migration services still enabled

### Notes

```text
```

---

## 8. Log Review and Log Rotation

Logs are required for troubleshooting and incident review.

### Review Commands

```bash
journalctl -p err -n 80 --no-pager
ls -lah /var/log
cat /etc/logrotate.conf
ls -lah /etc/logrotate.d
```

### Recommended Actions

* [ ] Confirm system logs are available.
* [ ] Confirm authentication logs are available.
* [ ] Confirm web server logs are available.
* [ ] Confirm application logs are available.
* [ ] Confirm log rotation is configured.
* [ ] Confirm logs do not fill the disk.
* [ ] Avoid logging secrets or sensitive data.
* [ ] Document important log paths.

### Common Log Paths

```text
/var/log/syslog
/var/log/auth.log
/var/log/nginx
/var/log/apache2
/var/log/mysql
/var/log/postgresql
```

### Notes

```text
```

---

## 9. Fail2ban

Fail2ban can help reduce repeated brute-force attempts against services such as SSH.

### Review Commands

```bash
systemctl status fail2ban --no-pager
sudo fail2ban-client status
```

### Recommended Actions

* [ ] Decide whether Fail2ban is appropriate.
* [ ] Enable SSH jail if direct SSH is exposed.
* [ ] Review ban policy.
* [ ] Confirm it does not interfere with legitimate administrators.
* [ ] Document configuration.

### Example Installation

```bash
sudo apt update
sudo apt install fail2ban
```

### Notes

Fail2ban is not a substitute for restricting SSH by Security Group, VPN, or SSM Session Manager.

### Notes

```text
```

---

## 10. Time Synchronization

Correct system time is important for logs, TLS, authentication, and troubleshooting.

### Review Commands

```bash
timedatectl
systemctl status systemd-timesyncd --no-pager
```

### Recommended Actions

* [ ] Confirm time synchronization is active.
* [ ] Confirm timezone is documented.
* [ ] Use UTC unless there is a clear operational reason not to.
* [ ] Confirm logs use expected timezone.

### Notes

```text
```

---

## 11. Disk Usage and Cleanup

Disk exhaustion is one of the most common causes of small server outages.

### Review Commands

```bash
df -hT
lsblk -f
sudo du -xhd1 / | sort -h | tail -20
```

### Recommended Actions

* [ ] Review root filesystem usage.
* [ ] Review large directories.
* [ ] Review log growth.
* [ ] Review Docker disk usage if Docker is used.
* [ ] Review local backup files.
* [ ] Set up disk usage monitoring.
* [ ] Document disk expansion procedure.

### Common Risk Indicators

* Root filesystem over 80% used
* Docker images and volumes consuming large space
* Backups stored only on the same server
* Logs growing without rotation
* Unknown large files under `/var`, `/opt`, or `/home`

### Notes

```text
```

---

## 12. Swap and Memory

Small EC2 instances may fail under memory pressure.

### Review Commands

```bash
free -h
swapon --show
ps aux --sort=-%mem | head -15
```

### Recommended Actions

* [ ] Review memory usage.
* [ ] Review whether swap exists.
* [ ] Consider swap for small instances if appropriate.
* [ ] Review application memory requirements.
* [ ] Monitor out-of-memory events.
* [ ] Consider instance resizing if memory is consistently insufficient.

### Notes

Swap is not a replacement for proper instance sizing, but it can reduce sudden failures on small servers.

### Notes

```text
```

---

## 13. Docker Hardening Notes

If Docker is used, review container exposure and data persistence.

### Review Commands

```bash
docker ps -a
docker images
docker volume ls
docker system df
docker network ls
```

### Recommended Actions

* [ ] Document Docker Compose project directories.
* [ ] Review exposed ports.
* [ ] Avoid running containers in privileged mode unless required.
* [ ] Document named volumes.
* [ ] Document bind mounts.
* [ ] Protect `.env` files.
* [ ] Confirm Docker volumes are backed up.
* [ ] Confirm container restart policies.
* [ ] Review unused images and stopped containers.
* [ ] Document container update procedure.

### Common Risk Indicators

* Important data exists only inside container filesystem
* Database volume not backed up
* `.env` readable by unnecessary users
* Containers exposing unnecessary public ports
* Old images consuming disk space

### Notes

```text
```

---

## 14. Web Server Hardening Notes

Review web server exposure and basic configuration.

### Review Commands

For Nginx:

```bash
nginx -t
systemctl status nginx --no-pager
ls -lah /etc/nginx/sites-enabled
```

For Apache:

```bash
apache2ctl configtest
systemctl status apache2 --no-pager
ls -lah /etc/apache2/sites-enabled
```

### Recommended Actions

* [ ] Confirm active sites.
* [ ] Disable default site if not needed.
* [ ] Confirm HTTPS is enabled.
* [ ] Confirm HTTP to HTTPS redirect where appropriate.
* [ ] Confirm certificate renewal.
* [ ] Review web root permissions.
* [ ] Avoid exposing backup files under web root.
* [ ] Avoid exposing `.env`, `.git`, or config files.
* [ ] Review upload size and timeout settings.
* [ ] Document server block or virtual host configuration.

### Common Risk Indicators

* `.env` accessible from browser
* `.git` directory under web root
* Default site still exposed
* Expired TLS certificate
* No redirect from HTTP to HTTPS
* Web root writable by too many users

### Notes

```text
```

---

## 15. Secrets and Sensitive Files

Secrets should be protected from accidental exposure.

### Review Commands

```bash
find /var/www /opt /srv /home -name ".env" -type f -print 2>/dev/null
find /var/www /opt /srv /home -name ".git" -type d -print 2>/dev/null
```

### Recommended Actions

* [ ] Identify `.env` files.
* [ ] Restrict file permissions.
* [ ] Avoid storing AWS access keys on the server.
* [ ] Avoid committing secrets to Git.
* [ ] Rotate credentials after handover.
* [ ] Protect database passwords.
* [ ] Protect private keys.
* [ ] Review backup files for sensitive data.

### Example Permission Review

```bash
ls -lah /path/to/.env
```

Possible restrictive permission:

```bash
chmod 600 /path/to/.env
```

Apply permissions carefully depending on application user requirements.

### Notes

```text
```

---

## 16. Malware and Intrusion Signals

This is not a full incident response procedure, but basic signs should be reviewed.

### Review Commands

```bash
last -n 30
sudo lastb -n 30
ps aux --sort=-%cpu | head -15
ps aux --sort=-%mem | head -15
systemctl --failed --no-pager
```

### Recommended Actions

* [ ] Review failed login attempts.
* [ ] Review unknown users.
* [ ] Review unexpected high CPU processes.
* [ ] Review unexpected listening ports.
* [ ] Review unknown cron jobs.
* [ ] Review suspicious temporary files if needed.
* [ ] Review outbound spam or mail queue if server sends mail.
* [ ] Escalate if compromise is suspected.

### Notes

If compromise is suspected, avoid modifying the server before preserving evidence. Consider isolating the instance, taking snapshots, and consulting an incident response specialist.

### Notes

```text
```

---

## 17. AWS SSM Agent

AWS Systems Manager Session Manager can reduce the need for public SSH exposure.

### Review Commands

```bash
systemctl status amazon-ssm-agent --no-pager
```

### Recommended Actions

* [ ] Confirm whether SSM Agent is installed.
* [ ] Confirm whether instance role permits SSM access.
* [ ] Confirm Session Manager is allowed by policy.
* [ ] Consider using SSM instead of public SSH.
* [ ] Document access procedure.
* [ ] Confirm logging of sessions if required.

### Notes

SSM setup requires correct IAM role, network access to SSM endpoints or internet, and AWS account configuration.

### Notes

```text
```

---

## 18. CloudWatch Agent

CloudWatch Agent can collect metrics and logs for monitoring.

### Review Commands

```bash
systemctl status amazon-cloudwatch-agent --no-pager
```

### Recommended Actions

* [ ] Confirm whether CloudWatch Agent is installed.
* [ ] Confirm metrics collected.
* [ ] Confirm logs collected if used.
* [ ] Confirm alarm configuration.
* [ ] Confirm alert recipients.
* [ ] Document monitoring ownership.

### Notes

At minimum, small production servers should usually have disk usage monitoring and service/application health checks.

### Notes

```text
```

---

## 19. Backup Before Hardening

Before significant changes, create or confirm a backup.

### Recommended Actions

* [ ] Confirm recent EBS snapshot.
* [ ] Confirm application file backup.
* [ ] Confirm database backup.
* [ ] Confirm Docker volume backup if Docker is used.
* [ ] Confirm restore procedure.
* [ ] Confirm emergency rollback plan.
* [ ] Record change date and operator.

### Notes

For production systems, never assume backup is working just because a backup job exists. Confirm restore readiness.

### Notes

```text
```

---

## 20. Change Log Template

Use this section to document changes.

| Date | Change | Reason | Operator | Result |
| ---- | ------ | ------ | -------- | ------ |
|      |        |        |          |        |
|      |        |        |          |        |
|      |        |        |          |        |

---

## 21. Hardening Risk Rating

Use this table to summarize hardening status.

| Area                          | Status  | Risk Level          | Notes |
| ----------------------------- | ------- | ------------------- | ----- |
| Package updates               | Unknown | Low / Medium / High |       |
| SSH configuration             | Unknown | Low / Medium / High |       |
| Firewall and network exposure | Unknown | Low / Medium / High |       |
| User and sudo access          | Unknown | Low / Medium / High |       |
| System services               | Unknown | Low / Medium / High |       |
| Log review and rotation       | Unknown | Low / Medium / High |       |
| Fail2ban                      | N/A     | Low / Medium / High |       |
| Time synchronization          | Unknown | Low / Medium / High |       |
| Disk usage                    | Unknown | Low / Medium / High |       |
| Swap and memory               | Unknown | Low / Medium / High |       |
| Docker hardening              | N/A     | Low / Medium / High |       |
| Web server hardening          | N/A     | Low / Medium / High |       |
| Secrets and sensitive files   | Unknown | Low / Medium / High |       |
| Malware and intrusion signals | Unknown | Low / Medium / High |       |
| SSM Agent                     | N/A     | Low / Medium / High |       |
| CloudWatch Agent              | N/A     | Low / Medium / High |       |
| Backup before hardening       | Unknown | Low / Medium / High |       |

---

## 22. Final Hardening Summary

Complete this section after review.

```text
Server Name:

AWS Region:

Environment:

Primary Application:

Hardening Review Date:

Main Risks Found:

Changes Applied:

Changes Deferred:

Recommended Immediate Actions:

Recommended Next Actions:

Reviewer:
```

---

## Related Toolkit Documents

Recommended related documents:

```text
docs/security-baseline-checklist.md
docs/backup-and-restore-checklist.md
docs/ec2-readiness-checklist.md
```

Recommended current script:

```bash
./scripts/ubuntu-server-audit.sh
```

Use the generated audit report together with this document to plan safe and practical Ubuntu hardening work.

