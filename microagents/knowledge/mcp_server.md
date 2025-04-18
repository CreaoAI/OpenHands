---
name: mcp_server
type: knowledge
version: 1.0.0
agent: CodeActAgent
triggers:
    - mcp server
    - mcp
    - model context protocol server
    - modelcontextprotocol server
---

# Knowledge Base: MCP Server Node.js Template

This knowledge provides instructions for setting up, customizing, and testing an MCP (Model Context Protocol) server based on the Node.js template project `https://github.com/HaiyiMei/mcp-node-template.git`.

## Setup Procedure

To set up the MCP server from the template:

1.  **Clone Repository**:
    ```bash
    git clone https://github.com/HaiyiMei/mcp-node-template.git
    cd mcp-node-template
    ```
2.  **Install Dependencies**:
    ```bash
    npm install
    ```
3.  **Build Project**:
    ```bash
    npm run build
    ```
    *   The compiled server code will be located in `build/index.js`.

*For comprehensive details about the template structure and advanced customization, consult the `README.md` within the template project.*

## Customization Guidelines

Modify the server's features by editing files in the `src/` directory:

*   `src/resources/index.ts`: Define and manage **Resources** (data/content exposed to LLMs).
*   `src/tools/index.ts`: Define and manage **Tools** (actions LLMs can perform via the server).
*   `src/prompts/index.ts`: Define and manage **Prompts** (reusable prompt templates/workflows).

*Refer to the template project's `README.md` for specific code examples.*

## Testing Protocol: MCP Inspector **ONLY**

**Rule**: Testing **MUST** be performed using the **MCP Inspector** (`@modelcontextprotocol/inspector`). Do **NOT** create custom test scripts, harnesses, web servers, or HTML files.

### Inspector Usage Modes & Features

1.  **CLI Mode (`--cli`) - PRIMARY METHOD**:
    *   **Purpose**: Essential for automation, scripting, and programmatic testing (e.g., integration with coding agents).
    *   **Usage**:
        ```bash
        # Basic CLI interaction
        npx @modelcontextprotocol/inspector --cli node build/index.js

        # List available tools
        npx @modelcontextprotocol/inspector --cli node build/index.js --method tools/list

        # Call a specific tool
        npx @modelcontextprotocol/inspector --cli node build/index.js --method tools/call --tool-name <TOOL_NAME> --tool-arg <PARAM>=<VALUE>
        ```
2.  **UI Mode (Browser)**:
    *   **Purpose**: Use for interactive debugging **only if CLI mode encounters persistent issues**.
    *   **How to Launch & Access**:
        1.  Start the inspector using one of the following commands:
            ```bash
            # Recommended command (uses project script)
            npm run inspect

            # Explicit command
            npx @modelcontextprotocol/inspector node build/index.js
            ```
        2.  This command starts the necessary background services:
            *   MCP Inspector (MCPI) UI Backend: Serves the UI.
            *   MCP Proxy (MCPP) Server: Handles MCP communication (Default port: 6277).
        3.  Open the MCPI UI URL in your browser (Default: `http://127.0.0.1:6274`) to interact with the server.
    *   **Passing Arguments/Environment Variables**:
        ```bash
        # Pass server arguments (use -- separator)
        npx @modelcontextprotocol/inspector node build/index.js -- arg1 arg2

        # Pass environment variables (-e)
        npx @modelcontextprotocol/inspector -e KEY=VALUE node build/index.js

        # Pass both env vars and server args
        npx @modelcontextprotocol/inspector -e KEY=VALUE -- node build/index.js --server-flag arg1
        ```
    *   **Configuration Files**: For complex setups.
        ```bash
        npx @modelcontextprotocol/inspector --config path/to/config.json --server <serverNameDefinedInConfig>
        ```


**Testing Workflow Summary:**

*   **ALWAYS** use the **MCP Inspector**.
*   **PRIORITIZE** the **CLI mode (`--cli`)** for all automated and standard testing.
*   **IF** CLI mode fails repeatedly, **THEN** switch to the **UI mode** for interactive debugging.
*   **NEVER** create separate testing infrastructure.
