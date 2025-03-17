# MCP Oracle DB Context

A powerful Model Context Protocol (MCP) server that provides contextual database schema information for large Oracle databases, enabling AI assistants to understand and work with databases containing thousands of tables.

## Overview

The MCP Oracle DB Context server solves a critical challenge when working with very large Oracle databases: how to provide AI models with accurate, relevant database schema information without overwhelming them with tens of thousands of tables and relationships.

By intelligently caching and serving database schema information, this server allows AI assistants to:
- Look up specific table schemas on demand
- Search for tables that match specific patterns
- Understand table relationships and foreign keys
- Get database vendor information

## Features

- **Smart Schema Caching**: Builds and maintains a local cache of your database schema to minimize database queries
- **Targeted Schema Lookup**: Retrieve schema for specific tables without loading the entire database structure
- **Table Search**: Find tables by name pattern matching
- **Relationship Mapping**: Understand foreign key relationships between tables
- **Oracle Database Support**: Built specifically for Oracle databases
- **MCP Integration**: Works seamlessly with GitHub Copilot in VSCode, Claude, ChatGPT, and other AI assistants that support MCP

## Installation

### Prerequisites

- Python 3.12 or higher
- uv package manager (recommended over pip)
- Oracle database access
- Oracle instant client (required for the `oracledb` Python package)

### Installing uv

```bash
# Install uv using curl (macOS/Linux)
curl -LsSf https://astral.sh/uv/install.sh | sh

# Or using PowerShell (Windows)
irm https://astral.sh/uv/install.ps1 | iex
```

Make sure to restart your terminal after installing uv.

### Project Setup

1. Clone this repository:
   ```bash
   git clone https://github.com/yourusername/mcp-db-context.git
   cd mcp-db-context
   ```

2. Create and activate a virtual environment using uv:
   ```bash
   # Create virtual environment
   uv venv
   
   # Activate (On Unix/macOS)
   source .venv/bin/activate
   
   # Activate (On Windows)
   .venv\Scripts\activate
   ```

3. Install dependencies:
   ```bash
   # Install in development mode
   uv pip install -e .
   ```

4. Create a `.env` file with your Oracle connection details:
   ```bash
   ORACLE_CONNECTION_STRING=username/password@hostname:port/service_name
   CACHE_DIR=.cache  # Optional: defaults to .cache
   ```

## Usage

### Starting the Server

To run the MCP server directly:

```bash
uv run main.py
```

For development and testing:

```bash
# Install the MCP Inspector
uv pip install mcp-cli

# Test with MCP Inspector
mcp dev main.py

# Or install in Claude Desktop
mcp install main.py
```

### Available Tools

When connected to an AI assistant like Claude, the following tools will be available:

#### `get_table_schema`
Get detailed schema information for a specific table.

Example:
```
Can you show me the schema for the EMPLOYEES table?
```

#### `get_tables_schema`
Get schema information for multiple tables at once.

Example:
```
Please provide the schemas for both EMPLOYEES and DEPARTMENTS tables.
```

#### `search_tables_schema`
Search for tables by name pattern and retrieve their schemas.

Example:
```
Find all tables that might be related to customers and show their schemas.
```

#### `rebuild_schema_cache`
Force a rebuild of the schema cache. Use sparingly as this is resource-intensive.

Example:
```
The database structure has changed. Could you rebuild the schema cache?
```

#### `get_database_vendor_info`
Get information about the connected Oracle database version.

Example:
```
What Oracle database version are we running?
```

## Architecture

This MCP server employs a three-layer architecture optimized for large-scale Oracle databases:

1. **DatabaseConnector Layer**
   - Manages Oracle database connections and query execution
   - Implements connection pooling and retry logic
   - Handles raw SQL operations

2. **SchemaManager Layer**
   - Implements intelligent schema caching
   - Provides optimized schema lookup and search
   - Manages the persistent cache on disk

3. **DatabaseContext Layer**
   - Exposes high-level MCP tools and interfaces
   - Handles authorization and access control
   - Provides schema optimization for AI consumption

## System Requirements

- **Python**: Version 3.12 or higher (required for optimal performance)
- **Memory**: 4GB+ available RAM for large databases (10,000+ tables)
- **Disk**: Minimum 500MB free space for schema cache
- **Oracle**: Compatible with Oracle Database 11g and higher
- **Network**: Stable connection to Oracle database server

## Performance Considerations

- Initial cache building may take 5-10 minutes for very large databases
- Subsequent startups typically take less than 30 seconds
- Schema lookups are generally sub-second after caching
- Memory usage scales with active schema size

## Contributing

We welcome contributions! Please see our [Contributing Guidelines](CONTRIBUTING.md) for details.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Support

For issues and questions:
- Create an issue in our GitHub repository
- Check our [FAQ](docs/FAQ.md) for common questions
- Join our community discussions