# Networking and Volumes

## 1. Purpose

This document describes the proposed Docker networking and persistent storage configuration for Manager Library.

Its purpose is to define how infrastructure services communicate with each other and how database information is preserved across container restarts and recreation.

These guidelines describe the intended configuration and do not confirm that the network or volumes have already been created.

## 2. Network Architecture

Manager Library should use a dedicated Docker network to enable communication between its infrastructure services.

The network should provide:

- Internal communication between containers.
- Service discovery through container service names.
- Isolation from unrelated containers.
- Controlled access to exposed ports.
- A clear separation between internal and external traffic.

A Docker bridge network is a suitable option for a single-host deployment.

## 3. Service Communication

The proposed communication flow is:

| Source | Destination | Purpose |
|---|---|---|
| User's Browser | Frontend | Access the web application |
| Frontend | Backend | Send HTTP API requests |
| Backend | PostgreSQL | Execute database operations |

The backend should connect to PostgreSQL using the database service name as its hostname when both services share the same Docker network.

The frontend should communicate with the backend through the configured API address.

The database should not be directly accessible from the public browser.

## 4. Port Configuration

The final port configuration should be defined according to the selected deployment environment.

The following ports are typical defaults and should be reviewed before implementation:

| Service | Default Port | Exposure Consideration |
|---|---:|---|
| React development server | 3000 | Development access |
| Laravel application server | 8000 | Application access |
| PostgreSQL | 5432 | Internal database access |

Only ports required by the deployment should be published to the host machine.

The PostgreSQL port should remain internal unless controlled administrative access is explicitly required.

## 5. Docker Network Configuration

A dedicated network may be declared in the Docker Compose configuration.

Illustrative example:

```yaml
networks:
  library-network:
    driver: bridge
```

The frontend, backend, and database services may be attached to this network according to their communication requirements.

The final network definition should be coordinated with the actual service configuration.

## 6. Persistent Volume Strategy

PostgreSQL requires persistent storage to preserve database information independently of the container lifecycle.

A named Docker volume is proposed for database files.

Its responsibilities include:

- Preserving database information.
- Supporting container recreation.
- Separating persistent data from temporary container files.
- Facilitating backup and recovery operations.

## 7. Volume Configuration

An illustrative Docker Compose volume definition is:

```yaml
volumes:
  library-postgres-data:
    driver: local
```

The PostgreSQL service should mount this volume at the database image's designated data directory.

The exact mount path must follow the selected PostgreSQL image configuration.

## 8. Data Persistence Considerations

The infrastructure should follow these recommendations:

- Use named volumes for database persistence.
- Avoid storing important database information only in a container's writable layer.
- Do not delete volumes during routine container maintenance.
- Document volume names and purposes.
- Restrict access to database files.
- Include persistent data in the backup and recovery strategy.

Removing a container should not unintentionally remove its associated persistent database volume.

## 9. Security Considerations

Network and volume configuration should follow these security principles:

- Keep database communication on an internal network.
- Expose only necessary application ports.
- Avoid public database access.
- Restrict access to persistent storage.
- Protect database credentials.
- Avoid using privileged containers without a justified requirement.

## 10. Maintenance Considerations

Network and volume maintenance should include:

- Reviewing active containers and network connections.
- Checking database volume availability.
- Monitoring disk space.
- Verifying backup completion.
- Testing data restoration.
- Reviewing access permissions.
- Documenting configuration changes.

## 11. Related Documentation

- `README.md`
- `infrastructure-architecture.md`
- `container-configuration.md`
- `database-configuration.md`
- `deployment-and-maintenance.md`

---

**Document status:** Proposed networking and persistent storage guidelines for Manager Library.