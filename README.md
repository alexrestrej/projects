# NEON MCP Server

## Overview

This is an MCP (Model Context Protocol) Server designed to provide AI assistants with access to NEON documentation. The server implements a clean architecture pattern with dependency injection and follows domain-driven design principles.

## Key Features

The server provides three main MCP capabilities:

🔧 Tools

* find_app - Search for application on Maestro de Migracion by system name and store their metadata

💬 Prompts

* generate_search_prompt - Generate structured prompts for AI assistants to research

## Architecture

The project follows Clean Architecture principles with clear separation of concerns:

```ini
📁 src
├── 📁 mcp_server_neon
│   ├─ 📁 domain
│   │   ├── 📁 model
│   │   │   └── 📁 neon
│   │   │       └── 📁 gateway: Abstract repositories for tools, resources, and prompts
│   │   │           ├── 📄 neon_entity.py
│   │   │           ├── 📄 neon_repository.py
│   │   └── 📁 usecase: Business logic for tools, resources, and prompts
│   │       ├── 📁 abstract_usecase
│   │       |   ├── 📄 neon_usecase.py
│   │       └── 📄 neon_maestro_usecase.py
├── 📁 infrastructure
│   ├── 📁 entry_points: MCP server bindings for tools, resources, prompts, and routes
│   │   ├── 📄 tools.py
│   │   └── 📄 routes.py
│   ├── 📁 driven_adapters: API integration and file system operations
│   │   ├── 📁 local_files
│   │   |   └── 📄 neon_maestro_adapter.py
│   │   |   └── 📄 Maestro_de_Migracion.xlsx
├── 📁 application: FastMCP server configuration with dependency injection
│    ├── 📄 app.py
│    └── 📁 config
│        ├── 📄 config.py
│        └── 📄 container.py
└─── 📄 server.py: Call app.py to start or initialize MCP server
```

## Technology Stack

* Framework: FastMCP for [MCP](https://modelcontextprotocol.io/introduction) server implementation
* Dependencies: [Dependency Injector](https://python-dependency-injector.ets-labs.org/) for IoC container
* Storage: Local Excel files in local_files/ directory
* Testing: Comprehensive test suite with pytest and async support

## Prerequisites

- Install uv from [Astral](https://docs.astral.sh/uv/getting-started/installation/) or the [GitHub README](https://github.com/astral-sh/uv#installation)
- Run: `uv python install` to install python version from file `.python-version`
- Python: 3.12 or higher
- Pandas Library: https://pandas.pydata.org/docs/reference/api/pandas.read_excel.html
- Node.js 20 or higher (Inspector): https://nodejs.org/en/download


## Local development

To make changes to this MCP locally and run it:

```sh
uv venv
source .venv/bin/activate
uv sync
```

After update dependencies, you must generate or update the `uv.lock` file. It works `package.lock` from node, where lock the dependency versions to avoid conflicts. Run the following command to generate or update lock:

```sh
uv lock
```

Run Unit Tests with Pytest

```sh
uv run pytest
```

Run Coverage with html report

```sh
uv run coverage html
```

A report will be generated into `htmlcov` folder. Open file `htmlcov/index.html` with a browser to visualize the report.

Run MCP Server

```sh
uv run mcp-server-neon
```

To test the MCP Server, run the [Inspector](https://modelcontextprotocol.io/docs/tools/inspector#inspector) tool created by Anthropic.

⚠️ Note: You must install node version 22 or newer

```sh
npx @modelcontextprotocol/inspector
```

When you run the **Inspector**, it shows something like:

```sh
Starting MCP inspector...
⚙️ Proxy server listening on 127.0.0.1:6277
🔑 Session token: e35f9b0215e837b2058eab71a0d571a7855f0bee97670f806400eac40f612062
Use this token to authenticate requests or set DANGEROUSLY_OMIT_AUTH=true to disable auth

🔗 Open inspector with token pre-filled:
   http://localhost:6274/?MCP_PROXY_AUTH_TOKEN=e35f9b0215e837b2058eab71a0d571a7855f0bee97670f806400eac40f612062
   (Auto-open is disabled when authentication is enabled)

🔍 MCP Inspector is up and running at http://127.0.0.1:6274
```

To test the MCP Server using the Inspector console:

1. Open `http://127.0.0.1:6274` in your browser
2. Navigate to the **Configuration** section
3. Enter the **Session token** in the **Proxy Session Token** field