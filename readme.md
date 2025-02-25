# Keycloak Reverse Proxy Setup

This project provides a simple setup to run `Keycloak` in production mode under a reverse proxy. The motive behind this setup is to provide a simple and up-to-date configuration, as existing resources often cover older versions or lack simplicity.

## Overview and Features

In this workspace, `Caddy` is used as the reverse proxy and only the recommended paths to expose, as per the [Keycloak documentation](https://www.keycloak.org/server/reverseproxy#_exposed_path_recommendations), are exposed. We use a `PostgreSQL` database to persist the data.

Keycloak will be running under `https://localhost/auth`.

## Setup

1. **Clone the repository**

2. **Run the setup**:
   ```sh
   docker compose up -d
   ```

## Important Notes

- Caddy manages TLS by default, so no extra configuration is needed. If you want to use other reverse proxies like Nginx, you need to configure TLS manually.
- Caddy automatically manages and overrides the X-Forwarded-\* headers to ensure proper client address forwarding. If using other reverse proxies, take extra precautions to ensure that the client address is properly set by your reverse proxy via the `Forwarded` or `X-Forwarded-For` headers. If this header is incorrectly configured, malicious clients can manipulate it to deceive Keycloak into believing the client is connecting from a different IP address than the actual one. Please check the [Keycloak documentation on reverse proxy headers](https://www.keycloak.org/server/reverseproxy#_configure_the_reverse_proxy_headers).
- A PostgreSQL database is used to persist data. If using another database, please check the [Keycloak documentation on supported databases](https://www.keycloak.org/server/db#_supported_databases).
- The Keycloak and PostgreSQL containers do not expose any ports to the host machine. If you need to access them, you can add reverse proxy rules and then restart only the Caddy container without stopping the entire compose setup.

## File Structure

```
├── Caddyfile
├── docker-compose.yml
├── readme.md
└── static/
   └── 404.html        # Custom 404 error page used whenever we do not match any path exposed by the reverse proxy
```

## References

- [Keycloak Guides](https://www.keycloak.org/guides)
- [Caddy Documentation](https://caddyserver.com/docs)
