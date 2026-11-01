# Bunstation MCP

Remote MCP server for [Bunstation](https://bunstation.com). It lets an AI agent build websites and apps from a prompt, edit and commit project files, publish builds, and read credits, invoices, and usage for the signed-in account.

- **URL:** `https://api.bunstation.com/mcp`
- **Transport:** Streamable HTTP (stateless, `POST` only)
- **Auth:** OAuth 2.1 with dynamic client registration, scope `mcp`. The first connection opens Bunstation sign-in.
- **Protected resource metadata:** `https://api.bunstation.com/.well-known/oauth-protected-resource/mcp`

The server is hosted by Bunstation. There is nothing to install or run locally.

## Connect

### Claude Code

```bash
claude mcp add --transport http bunstation https://api.bunstation.com/mcp
```

### Claude (claude.ai and desktop)

**Settings → Connectors → Add custom connector**, then paste `https://api.bunstation.com/mcp`.

### Cursor

Install the [Bunstation Cursor plugin](https://github.com/bunstation/cursor-plugin), or add this to `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "bunstation": {
      "url": "https://api.bunstation.com/mcp"
    }
  }
}
```

### VS Code

```bash
code --add-mcp '{"name":"bunstation","type":"http","url":"https://api.bunstation.com/mcp"}'
```

### Other clients

Any client that supports remote Streamable HTTP servers with OAuth works. Clients that only speak stdio can use [`mcp-remote`](https://www.npmjs.com/package/mcp-remote):

```json
{
  "mcpServers": {
    "bunstation": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://api.bunstation.com/mcp"]
    }
  }
}
```

## Tools

### Projects

| Tool | Description |
| --- | --- |
| `projects_create` | Create a project from a prompt and start building it. Stack is `vite` or `nextjs`, model is an OpenRouter model id. At most 3 projects per request. |
| `projects_list` | List the user's projects, newest activity first. |
| `projects_get` | Get one project. |
| `projects_update` | Rename a project or change the model used for later messages. |
| `projects_publish` | Publish a successful build on a `bunstation.app` subdomain. |
| `projects_generation_stop` | Stop the generation running on a project. |
| `projects_chat_get` | Read the recent conversation, generation status, and queued follow-ups. |
| `projects_builds_list` | List recent builds, newest first. |
| `projects_builds_log` | Read the build log for a commit. |

### Templates

| Tool | Description |
| --- | --- |
| `templates_list` | List public templates from the app library, optionally by category. |
| `templates_use` | Copy a template into a new project without starting a prompt build. |

### Files and changes

Edits are staged as uncommitted changes until `projects_changes_commit`. Reads include uncommitted changes.

| Tool | Description |
| --- | --- |
| `projects_files_list` | List file paths in a project. |
| `projects_files_read` | Read a text file. |
| `projects_files_search` | Search text in a project. |
| `projects_files_create` | Stage a new text file. |
| `projects_files_update` | Stage a full replacement of a text file. |
| `projects_files_delete` | Stage deletion of a file. |
| `projects_changes_list` | List uncommitted changes. |
| `projects_changes_commit` | Commit every pending change. |
| `projects_changes_discard` | Drop one uncommitted change. |

### Media

| Tool | Description |
| --- | --- |
| `projects_media_list` | List files in the project media library. |
| `projects_media_read` | Preview a media file. Images up to 4 MB are returned so the model can see them. |
| `projects_media_upload` | Return a one-time `curl` command that uploads local files to the media library. The token expires in 10 minutes. |

### Account

| Tool | Description |
| --- | --- |
| `account_credits_get` | Credit balance and current plan. |
| `account_invoices_list` | List invoices, newest first. |
| `account_invoices_get` | Get one invoice and its hosted Stripe URL. |
| `account_usage_get` | Token usage for `this_week`, `this_month`, or `all_time`. |

## Example prompts

- "Create a landing page for my bakery with a menu and contact form."
- "List my Bunstation projects and show the build log for the newest one."
- "Change the hero heading in my portfolio project, commit it, and publish."
- "How many credits do I have left this month?"

## Support

Email [hi@bunstation.com](mailto:hi@bunstation.com) or open an issue in this repository.

## License

[MIT](LICENSE)
