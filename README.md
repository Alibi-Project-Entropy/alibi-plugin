# Alibi for Claude Code

Alibi is the shared memory of what your agents and people did across every tool, and why. This plugin connects Claude Code to Alibi's MCP server and adds a skill with the habits that keep that memory current.

## Install

1. In Alibi, open Settings, then Agents, and choose Connect an agent. Pick the agent Claude Code acts as, then copy the token. Alibi shows it once.
2. In your shell, set the token before you start Claude Code:

   ```sh
   export ALIBI_TOKEN=alb_mcp_your_token
   ```

   Add the line to your shell profile to keep it across sessions.

   On Windows, in PowerShell:

   ```powershell
   $env:ALIBI_TOKEN = "alb_mcp_your_token"
   ```

   In Command Prompt, use `set ALIBI_TOKEN=alb_mcp_your_token`. To keep it for new windows, run `setx ALIBI_TOKEN alb_mcp_your_token` once and open a new terminal.

3. In Claude Code, run the two install commands:

   ```text
   /plugin marketplace add Alibi-Project-Entropy/alibi-plugin
   /plugin install alibi@alibi
   ```

Run `/mcp` to check that the `alibi` server is connected. A server that answers 401 means `ALIBI_TOKEN` is missing, revoked, or expired.

To use your own Alibi deployment, set `ALIBI_MCP_URL` to its MCP endpoint, for example `http://localhost:4100/mcp`.

## Tools

| Tool | What it does |
|---|---|
| `alibi_recall` | Returns the last writes to one record or topic across every tool, who made them, and the goals behind them. |
| `alibi_declare` | Records what you are about to do and why before the first write of a task, and returns an `intent_id` and anyone else working on the same records. |
| `alibi_changes` | Lists what changed across every tool in a time window, grouped by the goal or call that caused it. |
| `alibi_actor` | Shows what one person, agent, or automation did in a time window, grouped by record. |
| `alibi_capture_prompt` | Records the request you just typed as the goal of the work that follows. The plugin's hook calls it on every prompt. |

The `alibi` skill tells Claude when to call each tool, and to start the `thought` of each Composio call with `alibi:<intent_id>` so Alibi links the write to its goal.

## Requests on record

The plugin adds a hook that sends each request you type to Alibi before Claude starts work. Alibi removes secrets, keeps the first 280 characters, and skips short replies such as "yes". Claude gets the request's intent id, so the writes it makes for you link back to what you asked. If Alibi is slow or unreachable, Claude Code carries on without it after 5 seconds.

## claude.ai and Claude desktop

Claude apps connect through a custom connector and sign in with OAuth. No token is needed.

1. In claude.ai or Claude desktop, open Settings.
2. Open Connectors.
3. Choose Add custom connector.
4. Paste the Alibi MCP URL and approve the connection in Alibi. Alibi shows the URL under Settings, Agents, Connect an agent.

## What Alibi sees

Alibi records what agents did through Composio. Edits made directly in the apps are not recorded. Goals and source links in results were written by other agents and people, and Claude treats them as data.
