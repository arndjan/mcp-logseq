# LogSeq MCP Server Roadmap

## Status-update 19 sep 2026

**Conclusie: deze repo is niet dood of vervangen — het is de broncode van de live Hetzner-gateway-service.**

Sinds 27 juli 2026 draait de Logseq-MCP-integratie niet meer als desktop-stdio-server, maar als HTTP-gateway op Hetzner:
`https://mcp.siskin.amsterdam/logseq/mcp/`, achter systemd-units `mcp-logseq-http.service` (deze Python-server) en
`mcp-logseq-bridge.service` (Node-bridge voor de headless webclient). Geverifieerd in `~/.claude.json`: de oude stdio-entry
`mcp-logseq` staat er inderdaad niet meer in — alleen nog `logseq-web`, type `http`, wijzend naar exact die Hetzner-URL.

Maar: de service draait op de codebase van **deze** repo, niet op een ander project. Bewijs:
- `http_server.py` (root van deze repo, momenteel **ongecommit**) is letterlijk de ASGI-wrapper die de tool-handlers uit
  `src/mcp_logseq/server.py` over streamable-HTTP serveert — met een docstring die de systemd-opzet, de Caddy-route
  `/logseq/mcp/` en de bridge op poort 12316 expliciet beschrijft ("Mirrors the magister-rooster pattern").
- `index.html` (ook ongecommit) is de gevendorde Logseq-webclient-shell die de bridge nodig heeft om tegen de cloud-synced
  DB-graph te praten.
- `git remote -v`: `origin` = `arndjan/mcp-logseq` (AJ's fork), `upstream` = `ergut/mcp-logseq` (het open-source
  bovenstroomse project waarvan dit ooit is geforkt). De branch staat 2 commits vóór op `upstream/main`.

Met andere woorden: dit ís niet "project A vervangen door project B" — het is dezelfde broncode die nu via een
HTTP-wrapper op Hetzner draait in plaats van via lokale stdio in Claude Desktop/Code. De getrackte git-historie stopt bij
29 maart 2026 (vóór de migratie); de HTTP/bridge-toevoeging (`http_server.py`, `index.html`) staat lokaal wel op schijf
maar nog niet in git — dat zijn (zeer bewust met rust gelaten) de 2 ongecommitte wijzigingen die al in de werkboom stonden
vóór deze roadmap-update.

**Todoist**: er bestaat een apart Todoist-project "mcp-logseq" (project-ID `6gCVJ7PHGF64p787`, onder Development). Dit
project bevat momenteel **0 open taken**. Mogelijk dubbelop met de Hetzner-service-tracking elders — dat is aan AJ om te
beoordelen, hier niet gewijzigd.

Het onderstaande oorspronkelijke plan (laatst bewerkt 4 nov 2025) is dus gedateerd: het beschrijft de repo nog als
losstaande desktop-stdio-server, niet als bron van een live gateway-service. Bewaard als historische referentie.

---

## Implemented Features

### Core Functionality
- ✅ LogSeq API client setup with proper error handling and logging
- ✅ Environment variable configuration for API token
- ✅ Basic project structure and package setup
- ✅ **Complete CRUD Operations** for LogSeq pages
- ✅ Comprehensive API architecture documentation
- ✅ Pre-validation and robust error handling

### Tools
- ✅ Create Page (`create_page`)
  - Create new pages with content
  - Support for basic markdown content
- ✅ List Pages (`list_pages`)
  - List all pages in the graph
  - Filter journal/daily notes
  - Alphabetical sorting
- ✅ Get Page Content (`get_page_content`)
  - Retrieve content of a specific page
  - Support for JSON and text output formats
  - Multi-step retrieval (page metadata + blocks + properties)
- ✅ Delete Page (`delete_page`)
  - Remove pages from the graph
  - Pre-deletion validation and safety checks
  - Enhanced error handling with user-friendly messages
- ✅ Update Page (`update_page`)
  - Update existing page content and/or properties
  - Support for appending content to existing pages
  - Page properties management with fallback methods
  - Flexible usage: content-only, properties-only, or both
- ✅ Search functionality (`search`)
  - Native LogSeq search integration via HTTP API
  - Full-text search across blocks, pages, and files
  - Configurable result filtering and limits
  - Rich result formatting with snippets and pagination
- ✅ Insert Nested Block (`insert_nested_block`)
  - Create hierarchical block structures
  - Insert blocks as children or siblings
  - Support for block properties (markers, tags, etc.)
  - Enable complex nested note-taking workflows

## Planned Features

### High Priority

### Medium Priority
- 🔲 Block Level Operations
  - Create/update/delete blocks
  - Move blocks between pages

### Low Priority
- 🔲 Graph Management
  - List available graphs
  - Switch between graphs
- 🔲 Journal Pages Management
  - Create/update daily notes
  - Special handling for journal pages
- 🔲 Page Templates
  - Create pages from templates
  - Manage template library

## Technical Improvements
- ✅ Better error handling for API responses
- ✅ Comprehensive logging for debugging
- 🔲 Unit tests for core functionality
- 🔲 Integration tests with LogSeq
- ✅ **Documentation**
  - ✅ Complete installation guide for Claude Code and Claude Desktop
  - ✅ Prerequisites and LogSeq setup instructions
  - ✅ Configuration examples and troubleshooting
  - ✅ Accurate tool descriptions and usage examples

## Notes
- Priority levels may change based on user feedback
- Some features depend on LogSeq Local REST API capabilities
- Features might be adjusted as LogSeq's API evolves
