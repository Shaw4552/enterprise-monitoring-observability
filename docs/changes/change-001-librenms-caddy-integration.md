# Change 001: LibreNMS HTTPS and Caddy Integration

## Objective

Publish LibreNMS through the internal Caddy reverse-proxy platform using
trusted HTTPS and secure application sessions.

## Changes implemented

- Added internal DNS records for the LibreNMS frontend and backend.
- Added LibreNMS to the shared Caddy configuration.
- Configured the HTTP application backend on TCP port 8000.
- Configured the LibreNMS public base URL.
- Restricted trusted proxy handling to the reverse proxy.
- Enabled HTTPS-only session cookies.
- Added staged validation and deployment checks.
- Corrected Caddy single-file bind-mount deployment behavior.

## Incident discovered during deployment

The deployment pipeline atomically replaced the host Caddyfile. Because the
file was mounted into the container as a single-file bind mount, the running
container remained attached to the previous inode and continued reading the
old configuration.

## Resolution

The deployment process was changed to overwrite the existing file in place,
preserving the bind-mounted inode. Normal deployment and rollback paths were
both corrected.

## Validation

- DNS resolution succeeded.
- TLS certificate issuance succeeded.
- LibreNMS returned the HTTPS login page.
- Redirects remained HTTPS.
- Session cookies included the Secure attribute.
- The Caddy pipeline completed successfully on all nodes.
- LibreNMS application validation passed.
- Database, Redis, dispatcher, and pollers were healthy.
- A checksum-verified backup was created.
