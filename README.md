# Enterprise Monitoring & Observability Platform

## Overview

This repository documents a production-style monitoring and observability platform built in a Proxmox-based homelab environment.

The project demonstrates hands-on experience with:

- infrastructure and network monitoring
- Linux systems administration
- SNMP-based observability
- containerized services
- reverse proxy and HTTPS integration
- operational validation
- backup and recovery
- troubleshooting
- change management
- incident response
- technical documentation

The platform is designed around the operational practices of a small enterprise environment rather than a standalone monitoring server.

---

## Current Production Platform

LibreNMS is the first deployed monitoring platform in the observability stack.

The production deployment uses four containers:

- LibreNMS web application
- LibreNMS dispatcher
- MariaDB
- Redis

The platform provides:

- infrastructure discovery
- SNMP polling
- availability monitoring
- performance history
- network alerting
- operational health validation

Detailed production design:

[`docs/deployment/librenms-production-design.md`](./docs/deployment/librenms-production-design.md)

---

## Secure Application Publishing

LibreNMS is published through a dedicated Caddy reverse proxy.

Security controls include:

- TLS termination at the reverse proxy
- internal certificate authority
- explicitly trusted proxy configuration
- HTTPS-only session cookies
- database credentials supplied through an ignored environment file
- no credentials committed to source control
- persistent application and database storage outside container layers
- staged validation before deployment

---

## Platform Health Validation

The LibreNMS deployment is considered healthy when:

- all four containers are running
- MariaDB health checks pass
- Redis health checks pass
- the LibreNMS validation utility reports no critical failures
- active pollers are detected
- the dispatcher is active
- the published HTTPS endpoint returns the login page
- application redirects remain HTTPS
- session cookies include the Secure attribute

Operational validation:

[`docs/runbooks/librenms-health-validation.md`](./docs/runbooks/librenms-health-validation.md)

---

## Deployment and Change Workflow

Changes follow a staged operational workflow:

1. Validate the Compose model.
2. Confirm required environment variables.
3. Back up the database and persistent data.
4. Recreate only affected services.
5. Wait for the application endpoint.
6. Run the LibreNMS validation utility.
7. Test the external HTTPS route.
8. Confirm polling resumes.

LibreNMS updates are delivered through the official Docker image and are intended to be performed through documented maintenance changes with a verified rollback point.

---

## Change Management Case Study

### LibreNMS HTTPS and Caddy Integration

A documented infrastructure change was used to publish LibreNMS through the internal Caddy reverse-proxy platform.

The change included:

- internal DNS records
- shared Caddy configuration
- LibreNMS backend publishing
- public base URL configuration
- trusted proxy restrictions
- HTTPS-only session cookies
- staged validation
- deployment checks

During deployment, the pipeline atomically replaced the host Caddyfile. Because the configuration was mounted into the container as a single-file bind mount, the running container remained attached to the previous inode and continued reading the old configuration.

The deployment process was corrected to overwrite the existing file in place, preserving the bind-mounted inode.

Validation confirmed:

- DNS resolution
- TLS certificate issuance
- HTTPS login availability
- HTTPS redirects
- Secure session cookies
- successful Caddy deployment
- healthy LibreNMS validation
- healthy MariaDB and Redis
- active dispatcher and pollers
- checksum-verified backup creation

Full change record:

[`docs/changes/change-001-librenms-caddy-integration.md`](./docs/changes/change-001-librenms-caddy-integration.md)

---

## Incident Response Case Study

### Incident 001 — DNS Connectivity Failure

An IoT device experienced intermittent connectivity because firewall segmentation prevented access to centralized DNS services.

**Symptoms**

- intermittent device availability
- application discovery failures
- streaming interruptions

**Investigation tools**

- UniFi Flow Analytics
- firewall logs

Flow data showed DNS requests being denied between the IoT network and the infrastructure services network.

**Root cause**

The segmentation policy lacked the required DNS exception.

**Resolution**

Controlled DNS access was permitted from the IoT network to approved DNS services.

**Lesson learned**

Network segmentation must account for required shared infrastructure services such as DNS and NTP.

Full incident record:

[`docs/incidents/incident-001-sonos-dns-failure.md`](./docs/incidents/incident-001-sonos-dns-failure.md)

---

## Runbooks

Operational runbooks include:

- LibreNMS health validation
- DNS troubleshooting
- firewall troubleshooting
- host-down response
- switch-down response
- VLAN troubleshooting
- VPN troubleshooting
- Plex troubleshooting
- Sonos troubleshooting

[`docs/runbooks/`](./docs/runbooks/)

---

## Backup and Recovery

The project includes documentation for:

- LibreNMS backup strategy
- application recovery
- persistent-data recovery
- database recovery
- recovery procedures

[`docs/backup-recovery/`](./docs/backup-recovery/)

---

## Monitoring Standards

The repository includes standards for:

- monitoring
- SNMP
- alerting
- dashboards
- syslog
- NetFlow/IPFIX

[`docs/standards/`](./docs/standards/)

---

## Technology Stack

### Deployed

- LibreNMS
- MariaDB
- Redis
- Docker
- Linux
- Caddy
- internal PKI / TLS
- SNMP
- Git
- GitHub

### Planned Expansion

The broader observability roadmap includes:

- Prometheus
- Grafana
- Loki
- ntopng
- expanded alerting and incident-response automation

These remain roadmap items until implemented and validated.

---

## Skills Demonstrated

This project demonstrates practical experience with:

- infrastructure monitoring
- network monitoring
- Linux administration
- Docker operations
- service health validation
- SNMP-based monitoring
- reverse proxy configuration
- TLS and internal PKI
- application publishing
- backup and recovery
- change management
- root-cause analysis
- incident response
- operational runbooks
- technical documentation

---

## Repository Structure

- `docs/deployment/` — platform deployment documentation
- `docs/runbooks/` — operational response procedures
- `docs/incidents/` — incident records and root-cause analysis
- `docs/changes/` — infrastructure change records
- `docs/backup-recovery/` — backup and recovery procedures
- `docs/standards/` — monitoring and observability standards
- `monitoring/librenms/` — sanitized LibreNMS deployment examples
- `dashboards/` — dashboard-related content
- `diagrams/` — architecture and topology assets
- `templates/` — change, incident, and maintenance templates

---

## Portfolio Purpose

This repository demonstrates operational experience relevant to roles such as:

- NOC Analyst
- Systems Administrator
- Infrastructure Engineer
- Network Engineer
- Technical Support Engineer
- IT Operations Engineer

The project emphasizes deployed systems, operational validation, troubleshooting, change control, and documentation rather than theoretical architecture alone.

---

## Public Repository Notice

This repository contains sanitized documentation and example configuration.

Credentials, production secrets, private keys, authentication material, and other sensitive operational data are intentionally excluded from source control.
