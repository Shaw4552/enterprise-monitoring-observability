# Phase 1: LibreNMS Deployment

## Purpose

LibreNMS is the first phase of the monitoring platform. It provides network and infrastructure monitoring through SNMP discovery, polling, alerting, and device inventory.

## Goals

- Deploy LibreNMS
- Establish SNMP monitoring standards
- Add core infrastructure devices
- Validate polling and discovery
- Document operational runbooks

## Initial Monitoring Targets

| Device Type | Purpose |
|---|---|
| Gateway / Router | Network edge monitoring |
| Proxmox Host | Virtualization infrastructure |
| DNS Server | Core name resolution |
| NAS | Storage and media services |
| Linux Hosts | Service monitoring |

## Success Criteria

- LibreNMS web UI is reachable
- At least one device is successfully added
- SNMP polling works
- Graphs populate
- Device inventory is documented
- Troubleshooting notes are captured

## Skills Demonstrated

- SNMP monitoring
- Network discovery
- Infrastructure inventory
- Monitoring documentation
- Operational validation
