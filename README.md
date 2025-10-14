# Claude Code Plugins Marketplace

A curated collection of reusable plugins for [Claude Code](https://docs.claude.com/en/docs/claude-code/plugins) to enhance your development workflow with custom commands, agents, hooks, and MCP servers.

## What are Claude Code Plugins?

Claude Code plugins are extensible tools that allow you to customize and enhance Claude Code's functionality. They can include:

- **Custom Commands**: Reusable slash commands for common tasks
- **Specialized Agents**: Task-specific AI agents with focused capabilities
- **Hooks**: Automated actions triggered by events
- **MCP Servers**: Integration with Model Context Protocol servers for extended functionality

## Available Plugins

### Documentation Researcher

A specialized agent that efficiently researches framework, library, and tool documentation using Context7 MCP integration.

**Features:**
- Fetches latest documentation from official sources
- Uses Context7 for intelligent documentation search
- Reduces token usage by delegating research to a separate agent
- Provides version-specific documentation when available

**Requirements:**
- Context7 API key (set as `CONTEXT7_API_KEY` environment variable)

**Use Cases:**
- Research how to use a specific framework or library
- Find best practices and usage examples
- Get up-to-date API documentation
- Understand migration guides and version differences

## Installation

### Install the Marketplace

Add this marketplace to your Claude Code configuration:

```bash
claude code marketplace add https://github.com/igor-gorohovsky/claude-code-plugins
```

Or manually add to your `.claude/settings.json`:

```json
{
  "marketplaces": [
    "https://github.com/igor-gorohovsky/claude-code-plugins"
  ]
}
```

### Install Individual Plugins

Once the marketplace is added, install plugins using:

```bash
claude code plugin install documentation-researcher
```

Or use the interactive menu:

```bash
claude code plugin add
```

Then select plugins from the marketplace.

## Using the Plugins

### Documentation Researcher Agent

After installation, you can invoke the documentation researcher agent from any Claude Code conversation:

```
Can you research how to use React Query v5 for data fetching?
```

Or explicitly request the agent:

```
Use the documentation researcher agent to find information about Next.js App Router
```

The agent will:
1. Check if the library is already in your project dependencies
2. Fetch documentation from official sources
3. Provide version-specific guidance
4. Include code examples and best practices
