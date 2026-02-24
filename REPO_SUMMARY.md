# Vault Plugin 2 - Repository Summary

## Overview
This repository contains an **OpenCode custom tool plugin** for the Vault Plugin 2 system. The project implements a custom "create-pull-request" tool that integrates with the OpenCode AI platform for automated pull request creation.

## Project Purpose
The repository serves as a plugin extension for OpenCode, enabling users to create pull requests directly through the OpenCode interface with pre-configured authentication and automatic git push functionality.

## Directory Structure

```
vault-plugin-2/
├── .git/                          # Git repository metadata
├── .opencode/                     # OpenCode plugin configuration
│   ├── package.json              # Node.js dependencies
│   ├── bun.lock                  # Bun package manager lockfile
│   ├── .gitignore                # Git ignore rules
│   └── tool/
│       └── create-pull-request.js # Main plugin tool implementation
├── version1.txt                   # Version tracking file
└── [configuration files]          # Various config files

```

## Key Files

### `.opencode/tool/create-pull-request.js`
The main plugin tool implementation. This file:
- Exports a custom tool using the OpenCode `@opencode-ai/plugin` SDK
- Handles authentication via GitHub tokens
- Creates pull requests through a centralized bridge service
- Implements error handling for various HTTP status codes (401, 404, 409)
- Supports optional base branch specification
- Returns PR URLs and manual creation links when needed

**Key Features:**
- Uses Zod schema validation for tool arguments
- Integrates with control plane via bridge URL and auth tokens
- Automatically detects current git branch
- Provides user-friendly error messages
- Comprehensive logging for debugging

### `.opencode/package.json`
Node.js package configuration with the OpenCode AI plugin dependency.

### `version1.txt`
Contains version history and updates.

## Technology Stack
- **Runtime:** Node.js with ES modules
- **Package Manager:** Bun
- **Key Dependency:** `@opencode-ai/plugin` (v1.1.53)
- **Validation:** Zod schema library

## How It Works

1. **Tool Registration:** The create-pull-request tool is registered with OpenCode via the plugin SDK
2. **Arguments:** Accepts PR title, body, and optional base branch
3. **Authentication:** Uses environment variables for bridge URL, auth tokens, and session configuration
4. **Execution:** 
   - Retrieves current git branch
   - Makes authenticated HTTP request to bridge service
   - Returns PR URL or manual creation link
5. **Error Handling:** Provides specific error messages for authentication, session, and conflict issues

## Environment Variables
- `CONTROL_PLANE_URL` - Bridge service endpoint (defaults to http://localhost:8787)
- `SANDBOX_AUTH_TOKEN` - GitHub authentication token
- `SESSION_CONFIG` - JSON-formatted session configuration with sessionId

## Integration
This plugin integrates with the OpenCode platform's larger ecosystem:
- Receives session context from OpenCode
- Communicates with a central bridge service for PR creation
- Returns results back to the OpenCode UI for user visibility

## Development Notes
- Tool uses async/await for all I/O operations
- All errors are caught and formatted as user-friendly messages
- Session ID extraction handles both camelCase and snake_case formats
- Timeout protection on git command (5000ms)
