# LibreNMS Backup and Recovery

## Backup scope

A complete recovery set contains:

- MariaDB logical dump
- LibreNMS persistent application data
- RRD performance data
- sanitized Compose configuration
- host-specific environment file stored separately
- SHA-256 checksums

Application logs are excluded because they are not required for restoration.

## Backup procedure

1. Create a timestamped backup directory.
2. Export MariaDB using a consistent transaction.
3. Stop the LibreNMS web and dispatcher services briefly.
4. Archive persistent LibreNMS data.
5. Restart the services.
6. Generate SHA-256 checksums.
7. Test the archive for readability.
8. Verify the HTTPS application endpoint.

## Recovery procedure

1. Provision a compatible Docker host.
2. Restore the Compose configuration and secret environment file.
3. Start MariaDB and Redis.
4. Restore the database dump.
5. Restore persistent LibreNMS data.
6. Start the web and dispatcher containers.
7. Run the LibreNMS validation script.
8. Confirm active polling and dispatcher operation.
9. Test the external HTTPS endpoint.
10. Document the recovery result.

## Validation requirements

A backup is not considered valid until:

- the SQL dump is non-empty;
- the archive can be listed successfully;
- all checksums pass;
- the service returns to a healthy state after backup;
- the restore procedure has been tested in an isolated environment.
