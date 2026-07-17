# LibreNMS Production Design

## Purpose

LibreNMS provides infrastructure discovery, SNMP polling, availability
monitoring, performance history, and network alerting for the homelab
environment.

## Architecture

The deployment uses four containers:

- LibreNMS web application
- LibreNMS dispatcher
- MariaDB
- Redis

HTTPS terminates at a dedicated Caddy reverse proxy. LibreNMS receives
forwarded HTTPS headers only from the explicitly trusted reverse proxy.

## Security controls

- TLS termination at the reverse proxy
- Internal certificate authority
- Explicit trusted-proxy configuration
- HTTPS-only session cookies
- Database credentials supplied through an ignored environment file
- No credentials committed to source control
- Persistent application and database data stored outside container layers
- Staged validation before deployment

## Availability and validation

The deployment is considered healthy when:

- all four containers are running;
- MariaDB and Redis health checks pass;
- the LibreNMS validation script reports no critical failures;
- active pollers and the dispatcher are detected;
- the published HTTPS endpoint returns the login page;
- application redirects remain HTTPS;
- session cookies include the Secure attribute.

## Deployment workflow

1. Validate the Compose model.
2. Confirm required environment variables exist.
3. Back up the database and persistent data.
4. Recreate only the affected services.
5. Wait for the application endpoint.
6. Run the LibreNMS validation utility.
7. Test the external HTTPS route.
8. Confirm polling resumes.

## Update strategy

LibreNMS updates are delivered through the official Docker image. Updates
should be tested and deployed through a documented maintenance change with a
verified rollback point.
