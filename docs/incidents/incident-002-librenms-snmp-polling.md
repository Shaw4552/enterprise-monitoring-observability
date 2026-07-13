# Incident 002: LibreNMS SNMP Polling Failure

## Summary

A Linux infrastructure device responded successfully to manual SNMP queries,
but LibreNMS continued to mark the device as unavailable.

The investigation followed the monitoring path from network connectivity
through SNMP authentication and application-level polling.

## Symptoms

- the monitored host responded to ICMP
- UDP port 161 was reachable
- the SNMP daemon was active and listening on all interfaces
- manual SNMP queries from the monitoring container succeeded
- automated LibreNMS polling timed out
- the device remained marked down

## Investigation

Testing confirmed that:

1. Layer 3 connectivity was functional.
2. The SNMP service was listening on UDP port 161.
3. Source-restricted SNMP access permitted the monitoring server.
4. The LibreNMS application container could reach the monitored host.
5. Manual SNMP polling worked from the same container used by LibreNMS.

Debug polling revealed that LibreNMS was attempting to query the device with
an empty SNMP community value.

## Root Cause

The monitored device record in LibreNMS did not contain the required SNMP
community credential.

The network, firewall, container routing, and SNMP daemon were operating
correctly.

## Resolution

- configured the correct SNMP version and credential in the device record
- reran device discovery
- forced an immediate polling cycle
- verified successful collection of system, interface, storage, CPU, memory,
  process, and disk I/O metrics
- confirmed RRD data updates and graph generation

## Security Controls

- SNMP access was restricted to the monitoring server and localhost
- the firewall allowed only the required monitoring flow
- the intrusion-prevention exception was scoped to the trusted monitoring
  source rather than disabling the signature globally
- secrets and internal addressing were excluded from public documentation

## Lessons Learned

- successful ping tests do not prove application monitoring is functional
- UDP reachability tests do not prove valid SNMP authentication
- troubleshooting should test from the same execution environment as the
  production poller
- application debug output can expose credential or configuration problems
  hidden by successful manual tests
- monitoring credentials must be validated as part of device onboarding

## Skills Demonstrated

- LibreNMS administration
- SNMP monitoring
- Linux service management
- Docker networking
- Redis-backed polling
- firewall and IPS policy analysis
- layered network troubleshooting
- root-cause analysis
- infrastructure documentation
