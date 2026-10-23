# Container Configuration

## 1. Purpose

This document describes the proposed Docker container configuration for the Manager Library infrastructure.

Its purpose is to define the services, responsibilities, configuration requirements, and operational considerations for containerizing the application.

The configuration described here is a proposal and does not confirm that Docker containers have already been created or deployed.

## 2. Containerization Strategy

Docker is proposed as the containerization platform for Manager Library.

The main objectives are:

- Isolate application services.
- Provide consistent execution environments.
- Simplify service configuration.
- Facilitate local development.
- Support reproducible deployments.
- Separate application processes from persistent data.

Docker Compose may be used to coordinate multiple services through a single configuration file.

## 3. Proposed Container Services

The infrastructure may include the following services:

| Service | Technology | Responsibility |
|---|---|---|
| Frontend | React | Provides the application interface |
| Backend | PHP with Laravel | Processes application requests |
| Database | PostgreSQL 16 | Stores application information |

Each service should have its own configuration and responsibilities.

## 4. Container Responsibilities

### 4.1 Frontend Container

The frontend container should provide the environment required to serve the React application.

Configuration considerations:

- Appropriate Node.js version for the selected React setup.
- Installation of project dependencies.
- Application build or development command.
- Required environment variables.
- Access to the backend API.

The final execution strategy should be selected according to the frontend implementation.

### 4.2 Backend Container

The backend container should provide the environment required to execute the Laravel application.

Configuration considerations:

- Compatible PHP version.
- Required PHP extensions.
- Composer dependency management.
- Application configuration.
- Database connection settings.
- Appropriate application execution process.

Laravel environment configuration should be managed through environment variables or protected environment files.

### 4.3 Database Container

The database container should use PostgreSQL 16.

Configuration considerations:

- PostgreSQL image version.
- Database name.
- Database user and password.
- Connection port.
- Persistent storage volume.
- Health check.
- Initialization configuration.

The database should use the `library` schema.

## 5. Docker Compose Organization

Docker Compose is proposed to define and coordinate the infrastructure services.

A future Compose configuration may include:

- A frontend service.
- A backend service.
- A PostgreSQL database service.
- A private application network.
- A persistent database volume.
- Environment configuration.
- Service dependencies and health checks.

The exact service names, ports, image versions, and commands must be confirmed when the actual configuration is implemented.

## 6. Environment Variables

Sensitive and environment-specific settings should be provided through environment variables.

Examples of configuration values include:

| Variable | Purpose |
|---|---|
| `APP_ENV` | Defines the application environment |
| `APP_DEBUG` | Controls debug mode |
| `APP_KEY` | Laravel application encryption key |
| `DB_CONNECTION` | Database driver |
| `DB_HOST` | Database service hostname |
| `DB_PORT` | Database connection port |
| `DB_DATABASE` | Database name |
| `DB_USERNAME` | Database username |
| `DB_PASSWORD` | Database password |

The actual values must be defined according to the deployment environment.

Secrets must not be committed to the public repository.

## 7. Container Networking

Containers should communicate through a dedicated Docker network.

Recommended considerations:

- The frontend must reach the backend through its configured API address.
- The backend must connect to PostgreSQL using the database service hostname.
- Database access should remain restricted.
- Only necessary ports should be published to the host machine.

The detailed network design is described in `networking-and-volumes.md`.

## 8. Persistent Storage

The PostgreSQL service should use a named Docker volume to preserve database information.

The database volume must remain independent of the database container lifecycle.

Removing or recreating a container should not unintentionally remove persistent database information.

## 9. Health Checks and Dependencies

The final container configuration should consider service health checks.

Recommended practices:

- Verify PostgreSQL readiness before dependent database operations.
- Configure backend startup to handle database connection availability.
- Avoid assuming that a running container means its service is ready.
- Review service logs when startup problems occur.

## 10. Security Considerations

The following practices are recommended:

- Use specific and maintained image versions.
- Avoid unnecessary privileged containers.
- Do not expose internal database ports publicly.
- Protect environment variables and credentials.
- Apply appropriate container permissions.
- Keep dependencies and base images updated.

## 11. Related Documentation

- `README.md`
- `infrastructure-architecture.md`
- `database-configuration.md`
- `networking-and-volumes.md`
- `deployment-and-maintenance.md`

---

**Document status:** Proposed Docker container configuration for Manager Library.