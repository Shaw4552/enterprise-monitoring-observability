# Incident 001 – DNS Connectivity Failure

## Summary

An IoT device experienced intermittent connectivity due to a firewall policy preventing access to centralized DNS services.

## Symptoms

- Device intermittently unavailable
- Application discovery failures
- Streaming interruptions

## Investigation

Tools Used:

- UniFi Flow Analytics
- Firewall Logs

Findings:

Flow data revealed DNS requests being denied between the IoT network and the infrastructure services network.

## Root Cause

Firewall segmentation policy lacked a DNS exception rule.

## Resolution

Implemented controlled DNS access from IoT devices to approved DNS servers.

## Lessons Learned

Network segmentation requires supporting service exceptions for DNS, NTP, and other shared infrastructure services.