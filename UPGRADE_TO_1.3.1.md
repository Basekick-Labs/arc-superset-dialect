# Upgrading to v1.3.1

## Critical Fix: Module Name Conflict

Version 1.3.1 fixes a critical bug where both `arc-superset-dialect` and `arc-superset-arrow` packages were using the same module name (`arc_dialect.py`), causing file conflicts and SQLAlchemy entry point registration failures.

## What Changed

### Module Names
- **arc-superset-arrow**: `arc_dialect.py` → `arc_dialect_arrow.py`
- **arc-superset-dialect**: `arc_dialect.py` → `arc_dialect_json.py`

### SQLAlchemy Dialect Names
- **arc-superset-arrow**: `arc` → `arc.arrow`
- **arc-superset-dialect**: `arc` → `arc.json`

### Connection Strings
- **Arrow (recommended)**: `arc.arrow://API_KEY@HOST:PORT/DATABASE`
- **JSON**: `arc+json://API_KEY@HOST:PORT/DATABASE`

## Upgrade Steps

### 1. Uninstall Old Versions
```bash
pip uninstall arc-superset-dialect arc-superset-arrow -y
```

### 2. Install v1.3.1
```bash
pip install arc-superset-arrow==1.3.1
# or
pip install arc-superset-dialect==1.3.1
# or both (now works without conflicts!)
pip install arc-superset-arrow==1.3.1 arc-superset-dialect==1.3.1
```

### 3. Update Connection Strings

In Superset, update your Arc database connections:

**Old (v1.3.0):**
```
arc://your-api-key@localhost:8000/default
```

**New (v1.3.1):**
```
arc.arrow://your-api-key@localhost:8000/default
```

Or for JSON dialect:
```
arc+json://your-api-key@localhost:8000/default
```

### 4. Test Connection

After updating the connection string, click "Test Connection" in Superset to verify it works.

## Why This Fix Was Needed

The original implementation had both packages installing a file named `arc_dialect.py` to the same location:
```
/usr/local/lib/python3.10/site-packages/arc_dialect.py
```

When both packages were installed:
1. The second installation would overwrite the first package's file
2. Both packages registered the same entry point `arc`, causing conflicts
3. SQLAlchemy's plugin loader would fail with: `Can't load plugin: sqlalchemy.dialects:arc`

Now each package has a unique module name and entry point, allowing both to be installed simultaneously without conflicts.

## Which Dialect Should I Use?

**Use `arc-superset-arrow` (recommended)**
- Uses Apache Arrow IPC format for data transfer
- 3-5x faster query performance
- More efficient memory usage
- Binary columnar format
- Connection string: `arc.arrow://...`

**Use `arc-superset-dialect` (JSON)**
- Standard JSON response format
- Better for debugging (human-readable)
- Connection string: `arc+json://...`

Both dialects are functionally equivalent and support all Arc features (multi-database, SHOW DATABASES, SHOW TABLES, etc.).
