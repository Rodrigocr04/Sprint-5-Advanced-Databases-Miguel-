# Database Migration Guide

This project uses Liquibase for managing database migrations across development and production environments. The migrations are written in Oracle SQL and can be tested using Oracle SQL Developer.

## Prerequisites

Ensure you have:
- Access to both development and production database environments
- Liquibase installed and configured
- Oracle SQL Developer installed for testing migrations
- Proper database credentials for both environments
- Oracle JDBC driver in your classpath

## Project Structure

```
db-migrations/
├── changelog/
│   ├── changelog.xml
│   └── changesets/
├── liquibase-dev.properties
└── liquibase-prod.properties
```

## Configuration Files

### Development Environment (liquibase-dev.properties)
- Database URL: jdbc:oracle:thin:@//localhost:1521/free
- Username: DEVELOP
- Driver: oracle.jdbc.OracleDriver

### Production Environment (liquibase-prod.properties)
- Contains production database connection details
- Should be kept secure and not committed to version control

## Common Commands

### Check Migration Status
```bash
# Development environment
liquibase --defaultsFile=liquibase-dev.properties status

# Production environment
liquibase --defaultsFile=liquibase-prod.properties status
```

### Apply Migrations
```bash
# Development environment
liquibase --defaultsFile=liquibase-dev.properties update

# Production environment
liquibase --defaultsFile=liquibase-prod.properties update
```

### Rollback Migrations
```bash
# Rollback last change
liquibase --defaultsFile=liquibase-dev.properties rollback 1

# Rollback to specific tag
liquibase --defaultsFile=liquibase-dev.properties rollback <tag>
```

### Generate SQL Script
```bash
# Generate SQL without executing
liquibase --defaultsFile=liquibase-dev.properties updateSQL
```

## Best Practices

1. **Testing Migrations**
   - Always test migrations in development environment first
   - Use Oracle SQL Developer to verify the changes
   - Test both forward and rollback scenarios

2. **Version Control**
   - Keep all changesets in version control
   - Use meaningful names for changeset files
   - Document complex changes in the changeset comments

3. **Production Deployment**
   - Always review the generated SQL before production deployment
   - Take database backups before applying migrations
   - Schedule migrations during maintenance windows

4. **Changeset Guidelines**
   - Each changeset should be atomic and focused
   - Include proper rollback statements
   - Use meaningful IDs and comments
   - Test rollback scenarios

## Troubleshooting

1. **Connection Issues**
   - Verify database credentials
   - Check network connectivity
   - Ensure Oracle JDBC driver is in classpath

2. **Migration Failures**
   - Check Liquibase logs for detailed error messages
   - Verify changeset syntax
   - Ensure proper rollback statements are defined

3. **Lock Issues**
   - If Liquibase lock is stuck, use:
   ```bash
   liquibase --defaultsFile=liquibase-dev.properties releaseLocks
   ```

## Additional Resources

- [Liquibase Documentation](https://docs.liquibase.com/)
- [Oracle SQL Developer Documentation](https://docs.oracle.com/en/database/oracle/sql-developer/)
- [Oracle JDBC Driver Documentation](https://docs.oracle.com/en/database/oracle/oracle-database/21/jjdbc/)
