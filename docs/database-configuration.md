# Database Configuration

## 1. Purpose

This document describes the proposed PostgreSQL database configuration for Manager Library.

Its purpose is to define the database engine requirements, connection settings, schema organization, persistence, and security considerations.

The configuration described here is a reference for implementation and does not confirm that a database instance has already been created or configured.

## 2. Database Engine

The project specification establishes PostgreSQL 16 as the database management system.

The database environment should consider the following requirements:

| Property | Configuration |
|---|---|
| Database Engine | PostgreSQL |
| Version | 16 |
| Schema | `library` |
| Time Zone | UTC |
| Naming Convention | `snake_case` |
| Primary Key Strategy | UUID |
| Timestamp Fields | `timestamptz` |

These settings should remain consistent with the data model defined in the DOCS repository.

## 3. Database Organization

The database should use a dedicated database for the Manager Library application.

Within that database, the application tables should be organized under the `library` schema.

The schema provides a logical structure for the project's relational data.

The main entities are:

| Table | Responsibility |
|---|---|
| `category` | Stores the predefined book categories |
| `book` | Stores book information and inventory data |
| `member` | Stores library member information |
| `user` | Stores application user accounts |
| `loan` | Stores loan records |
| `loan_item` | Stores individual books associated with a loan |

The exact columns, relationships, constraints, and indexes must follow the official data model.

## 4. Database Schema

The application should use the following schema:

```sql
CREATE SCHEMA library;
```

The schema must contain the tables defined by the project data model.

The database initialization process should ensure that the schema exists before application migrations or database operations are executed.

The final schema creation and migration strategy must be coordinated with the Laravel backend.

## 5. Database Connection

The Laravel backend should connect to PostgreSQL through environment-based configuration.

The main connection parameters include:

| Parameter | Description |
|---|---|
| `DB_CONNECTION` | Database driver (`pgsql`) |
| `DB_HOST` | Database service hostname |
| `DB_PORT` | PostgreSQL connection port |
| `DB_DATABASE` | Application database name |
| `DB_USERNAME` | Database connection user |
| `DB_PASSWORD` | Database connection password |
| `DB_SCHEMA` | Application schema (`library`) |

The actual database name, credentials, and host must be defined for each environment.

Database credentials must not be stored directly in source code or committed to the repository.

## 6. Data Integrity

The database configuration and application migrations should support the integrity requirements defined in the specification.

Important considerations include:

- UUID primary keys.
- Foreign key relationships.
- Appropriate unique constraints.
- Required fields and validation constraints.
- Consistent timestamp handling.
- Transactional operations for loan creation and inventory updates.
- Protection against duplicate return operations.

Business rules should be enforced at the application level and supported by database constraints where appropriate.

## 7. Database Transactions

Operations that modify related records should use database transactions when required.

For example, creating a loan should ensure that the loan record, its loan items, and the corresponding inventory updates are handled atomically.

If an operation fails, the transaction should be rolled back to avoid inconsistent data.

## 8. Time Zone and Date Management

The database should use UTC as the standard time reference.

Timestamp fields should use PostgreSQL `timestamptz` to support consistent time handling.

The application should handle the conversion between UTC and the user's local time when displaying dates.

Loan deadlines and overdue calculations must follow the business rules defined in the project specification.

## 9. Persistent Storage

The PostgreSQL database should use a persistent Docker volume.

The volume should preserve database files when the container is stopped, removed, or recreated.

Recommended considerations:

- Use a named volume.
- Avoid storing important database files only inside the container's writable layer.
- Establish backup procedures.
- Validate restoration procedures.
- Restrict access to persistent storage.

## 10. Backup and Recovery

The database maintenance strategy should include regular backups.

Recommended practices:

- Define a backup frequency.
- Store backups in a protected location.
- Restrict access to backup files.
- Verify backup integrity.
- Test database restoration periodically.

Backup frequency and retention policies should be determined according to project requirements.

## 11. Security Considerations

The database environment should follow these recommendations:

- Use strong database credentials.
- Restrict database network access.
- Avoid exposing PostgreSQL directly to the public internet.
- Use a dedicated database user for the application.
- Grant only the permissions required by the backend.
- Protect environment variables and secrets.
- Keep PostgreSQL updated within the selected major version.

## 12. Related Documentation

- `README.md`
- `infrastructure-architecture.md`
- `container-configuration.md`
- `networking-and-volumes.md`
- `deployment-and-maintenance.md`

---

**Document status:** Proposed PostgreSQL database configuration for Manager Library.