# Tuney MCP server

Tuney is royalty-free music for video, ads, podcasts, games and apps, built from musical elements composed, performed and recorded by musicians and fitted to the exact length you need. This repository is the home of the Tuney MCP server: one remote server that any MCP client can use, and install instructions for the assistants and editors people ask about most.

- **Server URL:** `https://api.tuney.io/cue-engine2/connector/mcp`
- **Transport:** MCP streamable HTTP
- **Authentication:** OAuth 2.0 (authorization code with PKCE, dynamic client registration). You sign in with your Tuney account in the browser; the server never sees your password.
- **Account:** a Tuney account, free to create at [app.tuney.io](https://app.tuney.io). Searching, generating, adjusting and listening to previews is free. Downloading a final file uses the credits of your account, exactly as in the Tuney app.
- **Documentation:** [app.tuney.io/claude](https://app.tuney.io/claude) · **Privacy notice:** [app.tuney.io/claude/privacy](https://app.tuney.io/claude/privacy) · **Support:** [hello@tuney.io](mailto:hello@tuney.io)

## What the server can do

| Tool | What it does | Uses credits |
|---|---|---|
| `list_styles` | Lists the genres, moods and tempos Tuney works with | no |
| `find_tracks` | Finds ready-made tracks for a brief, or by genre, mood and tempo, with instant previews | no |
| `generate_track` | Composes a new track from Tuney's hand-made elements, fitted to an exact length (5 s to 10 min) | no |
| `get_task` / `wait_for_task` | Follows a generation or download until it is done | no |
| `get_track` | Shows a track: name, preview link, length, bpm, key, style and sections | no |
| `adjust_track` | Re-renders a track with a new length, a turning point, a fade-out or short ending, a tempo or key change, stems muted or regenerated | no |
| `download_track` | Gives a download link for the final file as mp3, wav or stems (melody, chords, bass, drums, effects) | yes |
| `get_account` | Shows which Tuney account is connected and its credit balance | no |
| `import_video` | Brings in a video from a public link to the file | no |
| `get_video_upload_link` | Gives a one-time link to send a local video file (curl `-T`), up to 100 MB | no |
| `analyze_video` | Shows the picture cuts, where people speak, and a sonic-logo guess | no |
| `score_video` | Composes music to the video's cuts, ending before the sonic logo, and renders the video with it | no |
| `render_video` | Puts one of your tracks under a video, lowered while people speak | no |

You never call the tools yourself. Ask your assistant for what you need, for example:

- "Find me a warm acoustic track for a 45 second travel vlog intro."
- "Generate an upbeat 30 second pop track for a product video."
- "Make it 60 seconds with a turning point at 30 and a short ending."
- "Drop the drums for the voice-over part."
- "Download it as stems."
- "Score this ad: upbeat pop, end before the logo." (with a video link or file)

File links in results (previews, downloads, rendered videos) are on `api.tuney.io` and expire after six
hours. If your assistant runs in a sandbox with restricted network access, allow `api.tuney.io`.

## Connect from your client

### Claude (claude.ai, Claude Desktop, Claude mobile)

Settings → Connectors → **Add custom connector**, paste the server URL, then **Connect** and sign in with your Tuney account. Once Tuney is listed in the Claude connector directory you can also add it from there with one click.

### Claude Code

Install the Tuney plugin, which adds the server together with a skill that teaches Claude how to use it well:

```bash
claude plugin marketplace add library-x/tuney_claude_plugin
claude plugin install tuney@tuney
```

Or add just the server:

```bash
claude mcp add --transport http tuney https://api.tuney.io/cue-engine2/connector/mcp
```

Then run `/mcp` inside Claude Code and sign in in the browser window that opens. Plugin source: [library-x/tuney_claude_plugin](https://github.com/library-x/tuney_claude_plugin).

### ChatGPT

ChatGPT connects to MCP servers through developer mode:

1. Settings → **Connectors** → **Advanced** → turn on **Developer mode**.
2. Back in Connectors, choose **Create**, give it the name `Tuney`, paste the server URL, set **Authentication** to **OAuth**, and create it.
3. Sign in with your Tuney account when asked.
4. In a chat, open the tools menu ("+" or **More**) and enable **Tuney**.

Developer mode is available on paid ChatGPT plans. On Team, Enterprise and Edu plans an admin may need to enable it first.

### Cursor

Click to install:

[![Install in Cursor](https://cursor.com/deeplink/mcp-install-dark.svg)](cursor://anysphere.cursor-deeplink/mcp/install?name=tuney&config=eyJ1cmwiOiAiaHR0cHM6Ly9hcGkudHVuZXkuaW8vY3VlLWVuZ2luZTIvY29ubmVjdG9yL21jcCJ9)

Or add it to `~/.cursor/mcp.json` (global) or `.cursor/mcp.json` (project):

```json
{
  "mcpServers": {
    "tuney": {
      "url": "https://api.tuney.io/cue-engine2/connector/mcp"
    }
  }
}
```

Cursor detects that the server uses OAuth and shows a **Sign in** action next to it in Settings → MCP.

### VS Code (GitHub Copilot)

Add it from the command line:

```bash
code --add-mcp '{"name":"tuney","type":"http","url":"https://api.tuney.io/cue-engine2/connector/mcp"}'
```

Or put this in `.vscode/mcp.json` in your workspace (or in your user profile's `mcp.json`):

```json
{
  "servers": {
    "tuney": {
      "type": "http",
      "url": "https://api.tuney.io/cue-engine2/connector/mcp"
    }
  }
}
```

Start the server from the MCP view or the **MCP: List Servers** command; VS Code opens the Tuney sign-in page the first time.

### Codex CLI

```bash
codex mcp add tuney --url https://api.tuney.io/cue-engine2/connector/mcp
codex mcp login tuney
```

`codex mcp list` shows the server and whether you are signed in. The same entry can be written by hand in `~/.codex/config.toml`:

```toml
[mcp_servers.tuney]
url = "https://api.tuney.io/cue-engine2/connector/mcp"
```

### Gemini CLI

```bash
gemini mcp add --transport http tuney https://api.tuney.io/cue-engine2/connector/mcp
```

Then, inside Gemini CLI, run `/mcp auth tuney` to sign in. The equivalent entry in `~/.gemini/settings.json`:

```json
{
  "mcpServers": {
    "tuney": {
      "httpUrl": "https://api.tuney.io/cue-engine2/connector/mcp",
      "oauth": { "enabled": true }
    }
  }
}
```

### Windsurf

Add to `~/.codeium/windsurf/mcp_config.json`:

```json
{
  "mcpServers": {
    "tuney": {
      "serverUrl": "https://api.tuney.io/cue-engine2/connector/mcp"
    }
  }
}
```

### Any other MCP client

Use the server URL with the streamable HTTP transport. The server follows the MCP authorization specification, so a client that supports OAuth 2.0 with dynamic client registration needs nothing else. For implementers, discovery lives at:

- Protected resource metadata: `https://api.tuney.io/cue-engine2/connector/.well-known/oauth-protected-resource`
- Authorization server metadata: `https://api.tuney.io/cue-engine2/connector/.well-known/oauth-authorization-server`
- Host-root copies of both, for clients that look there: `https://api.tuney.io/.well-known/oauth-authorization-server/cue-engine2/connector` and `https://api.tuney.io/.well-known/oauth-protected-resource/cue-engine2/connector/mcp`

Clients that need a pre-registered client ID or a static token are not supported; registration is dynamic and open.

## MCP Registry

The server is published in the [official MCP Registry](https://registry.modelcontextprotocol.io) as `io.github.library-x/tuney`; [`server.json`](server.json) in this repository is the listing source.

## Disconnecting and data

Disconnecting in your client revokes its access to your Tuney account; tracks you created stay in your account. The [privacy notice](https://app.tuney.io/claude/privacy) describes what the server stores (short-lived sessions and tokens) and what it does not (your conversations). Questions: [hello@tuney.io](mailto:hello@tuney.io).

## License

The contents of this repository are MIT licensed. Music from Tuney is licensed under the [Tuney terms](https://app.tuney.io/terms).
