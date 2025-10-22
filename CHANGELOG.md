# Changelog

All notable changes to the Arc Superset Dialect will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.3.3] - 2024-10-22

### Fixed
- **Critical**: Changed driver from `api` to `json` for proper SQLAlchemy connection string format
  - Connection string now uses `arc+json://` instead of `arc.json://`
  - Follows SQLAlchemy `backend+driver://` convention
  - Matches the Arrow dialect pattern (`arc+arrow://`)

### Changed
- Driver name: `api` → `json`
- Connection string format: `arc.json://...` → `arc+json://...`
- Updated all documentation to reflect new connection string format

## [1.3.2] - 2024-10-22

### Fixed
- Documentation updates for connection string format

## [1.3.1] - 2024-10-22

### Fixed
- **Critical**: Fixed module name conflict with arc-superset-arrow
  - Renamed module from `arc_dialect` to `arc_dialect_json` to avoid file conflicts
  - Changed driver name from `api` to `json` for proper SQLAlchemy registration
  - Connection string format: `arc+json://...` (follows SQLAlchemy `backend+driver` convention)
  - Fixes SQLAlchemy "Can't load plugin" error when both dialects are installed
  - Fixes Superset validation error "Invalid connection string"

### Changed
- Module name: `arc_dialect.py` → `arc_dialect_json.py`
- Driver name: `api` → `json`
- Connection string format: `arc://...` → `arc+json://...`
- Entry point: `arc` → `arc.json`

## [1.3.0] - 2024-10-21

### Changed
- BREAKING: Updated query endpoint from `/query` to `/api/v1/query` for Arc v1.0.0 compatibility
- Requires Arc Core v1.0.0 or later
- Backwards incompatible with Arc Core < v1.0.0

### Migration
- Upgrade Arc Core to v1.0.0 or later before upgrading this dialect

## [1.2.0] - 2025-10-08

### Fixed
- **Critical**: `get_table_names()` now correctly uses `SHOW TABLES FROM {schema}` when querying specific databases
  - Previously, schema parameter was ignored and only showed tables from the default database
  - Now properly queries the specified database (e.g., `benchmark`, `production`) when user selects a schema in Superset
  - Fixes issue where selecting a non-default schema would show empty table list

### Changed
- Removed outdated comments about schema parameter not being used
- Cleaned up `get_table_names()` implementation for clarity

## [1.1.0] - 2025-10-08

### Added
- **Multi-Database Support**: Arc databases are now exposed as schemas in Superset
  - `SHOW DATABASES` command to list all available databases
  - Schema filtering in `get_table_names()` to show tables per database
  - Support for cross-database queries (`SELECT * FROM production.cpu`)
  - Documentation for multi-database usage in README

### Changed
- `get_schema_names()` now returns all Arc databases instead of just ["default"]
- `get_table_names()` now filters tables by schema (database) when specified
- Updated README with multi-database examples and usage guide
- Version bumped to 1.1.0 for feature release

### Fixed
- Table discovery now correctly parses SHOW TABLES output for multi-database structure
- Schema parameter is now properly used for database filtering

## [1.0.3] - 2025-10-07

### Fixed
- Table discovery showing partition years (2025) instead of measurement names
- `get_table_names()` now correctly uses first column (measurement name)
- Removed hardcoded fallback tables

## [1.0.2] - 2025-10-07

### Added
- `do_ping()` method for Superset connection testing
- Better error handling in table discovery

## [1.0.1] - 2025-10-07

### Fixed
- Package installation issue - switched from `packages=find_packages()` to `py_modules=["arc_dialect"]`
- Now correctly includes arc_dialect.py in PyPI package

## [1.0.0] - 2025-10-07

### Added
- Initial release of Arc Superset Dialect
- SQLAlchemy dialect implementation for Arc API
- Support for Arc authentication via API keys
- Auto-discovery of tables and measurements
- Docker image with pre-installed dialect
- Comprehensive README with setup instructions
