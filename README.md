# Enterprise Monitoring & Observability Platform

Enterprise-grade monitoring and observability platform built in a Proxmox homelab for infrastructure, network engineering, systems administration, and DevOps learning.

## Goals

- Learn enterprise monitoring tools
- Build recruiter-facing portfolio documentation
- Monitor Proxmox, UniFi, DNS, VPN, Plex, Sonos, Docker, and VLAN infrastructure
- Practice alerting, dashboards, runbooks, and incident response

## Stack Roadmap

1. LibreNMS
2. Prometheus
3. Grafana
4. Loki
5. ntopng
6. Alerting and Incident Response

## Environment

### Site A

- UDM Pro
- Raspberry Pi 5: Pi-hole, Unbound, Caddy
- WD PR4100 NAS
- Proxmox host: Dell OptiPlex 9020, i7-4790, 32GB RAM
- Ubuntu and Docker workloads

### Site B

- UDR7
- Plex server
- Additional media and user workloads

## Network Segmentation

- VLAN 10 Admin
- VLAN 20 Trusted
- VLAN 30 IoT
- VLAN 60 Work
- VLAN 70 Kids
- VLAN 75 Media Acquisition
- VLAN 99 Infrastructure

## Project Phases

| Phase | Tooling | Purpose |
|---|---|---|
| 1 | LibreNMS | Network and infrastructure discovery |
| 2 | Prometheus + Grafana | Metrics and dashboards |
| 3 | Loki | Centralized logging |
| 4 | ntopng | Network flow visibility |
| 5 | Alerting | Incident response and operational maturity |

## Portfolio Skills Demonstrated

- Infrastructure monitoring
- Network monitoring
- SNMP
- Syslog
- NetFlow/IPFIX
- Time-series metrics
- Dashboards
- Alerting
- Runbooks
- Incident response
- Technical documentation
