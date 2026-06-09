# Docker Server Checklist

This checklist is part of the **AWS Small Server Readiness Toolkit**.

It is intended for practical review of Docker-based workloads running on small AWS EC2 Ubuntu servers.

This checklist is especially useful for servers running:

* Docker Compose applications
* WordPress containers
* Laravel containers
* Nginx / Apache / Traefik reverse proxy containers
* MySQL / MariaDB / PostgreSQL containers
* Redis containers
* Internal business tools
* Small production or staging environments

## Important Notice

This checklist does **not** provide legal compliance certification, security certification, penetration testing, or a guarantee against incidents.

It is a practical review document for small Docker-based servers. Any change to a production Docker server should be tested, documented, and backed up before applying.

---

## 1. Docker Server Identity

Confirm that the Docker server is clearly identified.

* [ ] Server hostname is documented.
* [ ] AWS region is documented.
* [ ] EC2 instance ID is documented.
* [ ] Environment is identified: production, staging, development, test, or temporary.
* [ ] Main Docker applications are listed.
* [ ] Business owner is identified.
* [ ] Technical owner is identified.
* [ ] Docker administrator is identified.
* [ ] Server purpose is documented.
* [ ] Criticality is classified: low, medium, high, or business-critical.

### Notes

```text
```

---

## 2. Docker Installation

Confirm that Docker installation is understood and supportable.

* [ ] Docker is installed intentionally.
* [ ] Docker version is documented.
* [ ] Docker Compose version is documented.
* [ ] Installation method is documented.
* [ ] Docker service status is reviewed.
* [ ] Docker starts automatically after reboot if required.
* [ ] Docker data root location is documented.
* [ ] Docker disk usage is reviewed.
* [ ] Docker upgrade responsibility is documented.
* [ ] Docker restart impact is understood.

### Review Commands

```bash
docker version
docker compose version
systemctl status docker --no-pager
docker info
docker system df
```

### Notes

```text
```

---

## 3. Docker Compose Projects

Confirm that Docker Compose projects are known and documented.

* [ ] Docker Compose project directories are documented.
* [ ] `docker-compose.yml` or `compose.yml` files are located.
* [ ] Override files are documented if used.
* [ ] Project ownership is documented.
* [ ] Application purpose is documented.
* [ ] Start procedure is documented.
* [ ] Stop procedure is documented.
* [ ] Restart procedure is documented.
* [ ] Update procedure is documented.
* [ ] Rollback procedure is documented if available.

### Review Commands

```bash
find /home /opt /srv /var/www -name "docker-compose.yml" -o -name "compose.yml" 2>/dev/null
docker compose ps
```

### Notes

```text
```

---

## 4. Running and Stopped Containers

Confirm that running and stopped containers are expected.

* [ ] Running containers are reviewed.
* [ ] Stopped containers are reviewed.
* [ ] Unknown containers are investigated.
* [ ] Restart policies are reviewed.
* [ ] Container names are meaningful.
* [ ] Container images are documented.
* [ ] Container creation dates are reviewed.
* [ ] Containers from old projects are removed if no longer needed.
* [ ] Critical containers restart after reboot.
* [ ] Health checks are reviewed if configured.

### Review Commands

```bash
docker ps
docker ps -a
docker inspect <container>
```

### Common Risk Indicators

* Unknown containers running.
* Important containers have no restart policy.
* Old stopped containers remain from previous migrations.
* Container names are unclear.
* Production containers were started manually without Compose.

### Notes

```text
```

---

## 5. Images and Version Control

Confirm that container image versions are manageable.

* [ ] Images are reviewed.
* [ ] Image tags are documented.
* [ ] Avoid relying only on `latest` for production.
* [ ] Image source is trusted.
* [ ] Custom Dockerfiles are stored in Git.
* [ ] Build procedure is documented.
* [ ] Update procedure is documented.
* [ ] Rollback image version is documented if possible.
* [ ] Unused images are reviewed before cleanup.
* [ ] Image cleanup policy is documented.

### Review Commands

```bash
docker images
docker history <image>
docker system df
```

### Common Risk Indicators

* Production service uses `latest` without version control.
* Custom image was built manually and Dockerfile is missing.
* Old images consume large disk space.
* Image source is unknown.
* No rollback image tag is available.

### Notes

```text
```

---

## 6. Docker Networks and Port Exposure

Confirm that container network exposure is intentional.

* [ ] Docker networks are reviewed.
* [ ] Published ports are reviewed.
* [ ] Public-facing containers are identified.
* [ ] Internal-only services are not exposed publicly.
* [ ] Database containers are not exposed publicly.
* [ ] Redis or cache containers are not exposed publicly.
* [ ] Reverse proxy routing is documented.
* [ ] Host firewall and AWS Security Groups match Docker exposure.
* [ ] Unused published ports are removed.
* [ ] Network names are meaningful.

### Review Commands

```bash
docker network ls
docker network inspect <network>
docker ps --format "table {{.Names}}\t{{.Image}}\t{{.Ports}}"
ss -tulpen
```

### Common Risk Indicators

* Database container publishes port `3306`, `5432`, or `6379` to the public interface.
* Redis is exposed outside the Docker network.
* Docker publishes ports that are not documented.
* AWS Security Group allows ports that are only for internal services.
* Multiple reverse proxies are active without clear routing.

### Notes

```text
```

---

## 7. Volumes and Persistent Data

Confirm where persistent data is stored.

* [ ] Named volumes are listed.
* [ ] Bind mounts are listed.
* [ ] Application upload directories are identified.
* [ ] Database volumes are identified.
* [ ] Storage paths are documented.
* [ ] Important data is not stored only inside container writable layers.
* [ ] Volume ownership and permissions are reviewed.
* [ ] Volume backup method is documented.
* [ ] Restore method is documented.
* [ ] Unused volumes are reviewed before removal.

### Review Commands

```bash
docker volume ls
docker volume inspect <volume>
docker inspect <container>
```

### Common Risk Indicators

* Important data exists only inside a container filesystem.
* Database data is in an unnamed or undocumented volume.
* No one knows which volume belongs to which application.
* `docker volume prune` would remove important data.
* Bind mounts point to undocumented host paths.

### Notes

```text
```

---

## 8. Environment Files and Secrets

Confirm that secrets are protected.

* [ ] `.env` files are identified.
* [ ] `.env` files are not committed to public Git repositories.
* [ ] `.env` file permissions are reviewed.
* [ ] Database passwords are stored carefully.
* [ ] API keys are reviewed.
* [ ] AWS credentials are not stored unnecessarily in containers.
* [ ] Former administrator credentials are rotated after handover.
* [ ] Docker secrets are considered if appropriate.
* [ ] Secret values are not printed in logs.
* [ ] Backup handling of secret files is reviewed.

### Review Commands

```bash
find /home /opt /srv /var/www -name ".env" -type f -print 2>/dev/null
ls -lah /path/to/.env
```

### Common Risk Indicators

* `.env` file is world-readable.
* `.env` is committed to Git.
* AWS access keys are stored in application containers.
* Shared credentials are used by multiple people.
* Secrets are copied into documentation or logs.

### Notes

```text
```

---

## 9. Database Containers

Confirm that containerized databases are protected and backed up.

* [ ] Database container is identified.
* [ ] Database engine and version are documented.
* [ ] Database volume is documented.
* [ ] Database credentials are protected.
* [ ] Database port is not publicly exposed.
* [ ] Backup command is documented.
* [ ] Backup schedule is documented.
* [ ] Restore command is documented.
* [ ] Restore has been tested.
* [ ] Upgrade procedure is documented.

### Example Review Commands

```bash
docker ps --format "table {{.Names}}\t{{.Image}}\t{{.Ports}}"
docker volume ls
docker inspect <database-container>
```

### Common Risk Indicators

* Database is running in Docker but no volume backup exists.
* Database port is exposed to the internet.
* Database container uses an old unsupported version.
* Restore command is unknown.
* Database password is unknown or stored insecurely.

### Notes

```text
```

---

## 10. Reverse Proxy and HTTPS

Confirm how web traffic reaches containers.

* [ ] Reverse proxy is identified: Nginx, Apache, Traefik, Caddy, or other.
* [ ] HTTP routing is documented.
* [ ] HTTPS routing is documented.
* [ ] Domain names are documented.
* [ ] TLS certificate issuer is documented.
* [ ] Certificate renewal method is documented.
* [ ] HTTP to HTTPS redirect is configured where appropriate.
* [ ] Proxy network is documented.
* [ ] Backend container ports are not exposed unnecessarily.
* [ ] Default route behavior is reviewed.

### Review Commands

```bash
docker ps --format "table {{.Names}}\t{{.Image}}\t{{.Ports}}"
docker network ls
```

### Common Risk Indicators

* Application container exposes public port directly when reverse proxy should handle traffic.
* TLS renewal method is unknown.
* Multiple reverse proxies conflict.
* Old domains still route to containers.
* Admin tools are exposed publicly.

### Notes

```text
```

---

## 11. Container User and Privilege

Confirm containers do not run with unnecessary privileges.

* [ ] Privileged containers are reviewed.
* [ ] Containers running as root are reviewed.
* [ ] Host socket mounts are reviewed.
* [ ] Host filesystem mounts are reviewed.
* [ ] Capabilities are reviewed if customized.
* [ ] Containers with access to `/var/run/docker.sock` are reviewed carefully.
* [ ] Unnecessary privileged mode is removed.
* [ ] Application file permissions are reviewed.
* [ ] Container user expectations are documented.
* [ ] Security tradeoffs are documented.

### Review Commands

```bash
docker inspect <container>
```

### Common Risk Indicators

* Container runs with `privileged: true`.
* Container mounts `/var/run/docker.sock`.
* Container mounts large parts of host filesystem.
* Container runs as root without reason.
* File permission fixes require repeated manual `chmod 777`.

### Notes

```text
```

---

## 12. Logging

Confirm container logs are useful and manageable.

* [ ] Container logs are reviewed.
* [ ] Log driver is documented.
* [ ] Log rotation is configured or planned.
* [ ] Application logs are stored appropriately.
* [ ] Web access logs are available.
* [ ] Web error logs are available.
* [ ] Sensitive data is not logged unnecessarily.
* [ ] Logs do not fill disk.
* [ ] Important logs are included in troubleshooting documentation.
* [ ] Centralized logging is considered if needed.

### Review Commands

```bash
docker logs <container> --tail 100
docker inspect <container> | grep -i LogConfig -A 20
```

### Common Risk Indicators

* Docker logs consume large disk space.
* Application logs only exist inside containers.
* No log rotation.
* Logs contain secrets.
* No one knows where to find error logs.

### Notes

```text
```

---

## 13. Backup and Restore

Confirm Docker workloads can be recovered.

* [ ] Compose files are backed up or stored in Git.
* [ ] `.env` files are included in restore planning.
* [ ] Named volumes are backed up.
* [ ] Bind mount paths are backed up.
* [ ] Database dump is backed up.
* [ ] Uploaded files are backed up.
* [ ] Reverse proxy configuration is backed up.
* [ ] TLS renewal method is documented.
* [ ] Restore procedure is documented.
* [ ] Restore has been tested.
* [ ] Restore target is defined: same server, new EC2, staging, or local test.
* [ ] Recovery time expectation is discussed.

### Common Risk Indicators

* Docker Compose file exists but `.env` is missing.
* Volumes exist but backup only covers source code.
* Database volume exists but database dump is not created.
* Restore was never tested.
* Backup is stored only on the same EC2 instance.

### Notes

```text
```

---

## 14. Updates and Patch Management

Confirm how Docker workloads are updated.

* [ ] Host OS update process is documented.
* [ ] Docker Engine update process is documented.
* [ ] Container image update process is documented.
* [ ] Application update process is documented.
* [ ] Database upgrade process is documented.
* [ ] Update testing process is documented.
* [ ] Rollback process is documented.
* [ ] Maintenance window is documented.
* [ ] Image tags are reviewed before update.
* [ ] Update owner is documented.

### Common Risk Indicators

* Containers have not been updated for a long time.
* Production uses `latest` and updates are unpredictable.
* No rollback procedure.
* Database container upgraded without backup.
* Host OS updates require reboot but reboot plan is missing.

### Notes

```text
```

---

## 15. Health Checks and Restart Policy

Confirm containers recover from common failures.

* [ ] Restart policies are configured for critical containers.
* [ ] Health checks are configured where appropriate.
* [ ] Application readiness check exists.
* [ ] Reverse proxy restart behavior is understood.
* [ ] Database restart behavior is understood.
* [ ] Containers restart after server reboot.
* [ ] Manual recovery procedure is documented.
* [ ] Alerting exists or is planned for failed services.
* [ ] Systemd service for Compose project is considered if appropriate.
* [ ] Reboot test is performed or scheduled.

### Review Commands

```bash
docker ps -a
docker inspect <container>
```

### Common Risk Indicators

* Critical containers do not restart after reboot.
* No health checks.
* Application is down but container still appears running.
* Manual `docker run` command is lost.
* No alert when container exits.

### Notes

```text
```

---

## 16. Disk Usage and Cleanup

Confirm Docker does not silently consume the server disk.

* [ ] Docker disk usage is reviewed.
* [ ] Unused images are reviewed.
* [ ] Stopped containers are reviewed.
* [ ] Build cache is reviewed.
* [ ] Unused volumes are reviewed carefully.
* [ ] Local backup files are reviewed.
* [ ] Log growth is reviewed.
* [ ] Disk usage alert exists or is planned.
* [ ] Cleanup procedure is documented.
* [ ] Cleanup does not remove important volumes.

### Review Commands

```bash
docker system df
docker ps -a
docker images
docker volume ls
df -hT
```

### Warning

Do not run destructive cleanup commands such as `docker system prune` or `docker volume prune` on production servers unless important data has been identified and backed up.

### Notes

```text
```

---

## 17. Common Docker Cleanup Commands

Use cleanup commands carefully.

### Safer Review Commands

```bash
docker system df
docker image ls
docker container ls -a
docker volume ls
```

### Potentially Destructive Commands

The following commands may remove data or resources needed for recovery.

```bash
docker container prune
docker image prune
docker system prune
docker volume prune
```

### Recommended Procedure Before Cleanup

* [ ] Confirm production containers.
* [ ] Confirm important volumes.
* [ ] Confirm Compose project directories.
* [ ] Confirm backup exists.
* [ ] Confirm restore procedure.
* [ ] Confirm cleanup scope.
* [ ] Take notes before cleanup.
* [ ] Avoid volume cleanup unless absolutely certain.

### Notes

```text
```

---

## 18. Docker Compose Handover

Confirm another administrator can operate the Docker application.

* [ ] Project directory is documented.
* [ ] Compose file path is documented.
* [ ] Environment file path is documented.
* [ ] Start command is documented.
* [ ] Stop command is documented.
* [ ] Restart command is documented.
* [ ] Update command is documented.
* [ ] Backup command is documented.
* [ ] Restore command is documented.
* [ ] Log check command is documented.
* [ ] Important container names are documented.
* [ ] Known risks are documented.

### Example Commands

```bash
docker compose ps
docker compose logs --tail 100
docker compose pull
docker compose up -d
docker compose down
```

### Notes

```text
```

---

## 19. Docker Risk Rating

Use this table to summarize Docker readiness.

| Area                             | Status  | Risk Level          | Notes |
| -------------------------------- | ------- | ------------------- | ----- |
| Docker server identity           | Unknown | Low / Medium / High |       |
| Docker installation              | Unknown | Low / Medium / High |       |
| Docker Compose projects          | Unknown | Low / Medium / High |       |
| Running and stopped containers   | Unknown | Low / Medium / High |       |
| Images and version control       | Unknown | Low / Medium / High |       |
| Networks and port exposure       | Unknown | Low / Medium / High |       |
| Volumes and persistent data      | Unknown | Low / Medium / High |       |
| Environment files and secrets    | Unknown | Low / Medium / High |       |
| Database containers              | N/A     | Low / Medium / High |       |
| Reverse proxy and HTTPS          | N/A     | Low / Medium / High |       |
| Container user and privilege     | Unknown | Low / Medium / High |       |
| Logging                          | Unknown | Low / Medium / High |       |
| Backup and restore               | Unknown | Low / Medium / High |       |
| Updates and patch management     | Unknown | Low / Medium / High |       |
| Health checks and restart policy | Unknown | Low / Medium / High |       |
| Disk usage and cleanup           | Unknown | Low / Medium / High |       |
| Docker Compose handover          | Unknown | Low / Medium / High |       |

---

## 20. Recommended Action Plan

Use this section to convert Docker findings into action items.

| Priority | Action | Owner | Due Date | Status |
| -------- | ------ | ----- | -------- | ------ |
| High     |        |       |          |        |
| High     |        |       |          |        |
| Medium   |        |       |          |        |
| Medium   |        |       |          |        |
| Low      |        |       |          |        |

### Priority Guide

#### High Priority

Issues that may expose services, cause data loss, or prevent recovery.

Examples:

* Database container publicly exposed
* Important Docker volumes not backed up
* No restore procedure
* `.env` file exposed or committed to Git
* Critical containers do not restart after reboot
* Important data stored only inside container filesystem
* Unknown containers running in production

#### Medium Priority

Issues that increase operational risk.

Examples:

* No health checks
* No documented update procedure
* No Docker disk usage monitoring
* Unclear image version policy
* Stopped containers from old projects not reviewed
* Compose project not documented

#### Low Priority

Useful improvements that are not urgent.

Examples:

* Improve container naming
* Clean up unused images after backup
* Add comments to Compose files
* Improve handover notes
* Review non-critical old containers

---

## 21. Final Docker Review Summary

Complete this section after review.

```text
Server Name:

AWS Region:

Environment:

Main Docker Applications:

Docker Version:

Docker Compose Version:

Main Compose Directories:

Public Containers:

Database Containers:

Important Volumes:

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
docs/ec2-readiness-checklist.md
docs/ubuntu-hardening-notes.md
```

Recommended current script:

```bash
./scripts/ubuntu-server-audit.sh
```

Use the generated audit report together with this checklist to prepare a practical Docker server readiness review.

