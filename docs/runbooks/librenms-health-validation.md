# LibreNMS Health Validation

## Container state

Confirm the web application, dispatcher, database, and Redis containers are
running.

## Application validation

Run the LibreNMS validation utility as the LibreNMS application user:

```bash
docker exec --user librenms librenms /opt/librenms/validate.php
```

Expected checks include:

- database connected;
- database schema current;
- Redis functional;
- dispatcher enabled;
- active pollers detected;
- RRD directory writable;
- locks functional.

## External HTTPS validation

Confirm the published monitoring endpoint:

- presents the expected internal CA certificate;
- redirects directly to HTTPS;
- returns the login page;
- sets Secure session cookies.

## Polling validation

Review dispatcher logs and confirm:

- devices are polled repeatedly;
- polling completes within the configured interval;
- no persistent timeout or authentication failures occur.

## Capacity validation

Review:

- filesystem utilization;
- database size;
- RRD data growth;
- backup retention;
- container restart counts.

## Failure handling

If the web interface works but polling stops:

1. Inspect the dispatcher container.
2. Verify Redis.
3. Verify database connectivity.
4. Inspect dispatcher logs.
5. Run the LibreNMS validation utility.
6. Confirm device SNMP reachability.
