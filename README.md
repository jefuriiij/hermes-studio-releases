# Hermes Studio (unofficial)

Chat with [Hermes Agent](https://github.com/NousResearch/hermes-agent) inside VS Code.

Hermes keeps its brain: the skills it learns, its curator, its memory, and your ChatGPT,
Codex and Claude logins. VS Code stays your editor, with its explorer, git colours and
everything else around the chat.

> **Unofficial.** Hermes Studio is a community project. It is not affiliated with,
> endorsed by, or connected to Nous Research. "Hermes" and "Hermes Agent" are their
> project.

![A Hermes reply with clickable file paths and the files it changed, next to an edit approval card](https://raw.githubusercontent.com/jefuriiij/hermes-studio-releases/main/images/chat.png)

## Install

- **VS Code:** search for **Hermes Studio** in the Extensions view, or install it from the
  [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=jefuriiij.hermes-studio).
- **Google Antigravity** (or any editor that cannot use the Marketplace): download the
  `.vsix` from [Releases](https://github.com/jefuriiij/hermes-studio-releases/releases), then
  run **Extensions: Install from VSIX…**.

This repository holds the extension's page, pictures and releases, and is the place for
bug reports and ideas.

## Features

### A chat that shows its work

- Replies stream as Markdown, with highlighted code and a **Copy** button.
- Hermes's thinking is folded away. Its tool calls share one **"N steps"** card; open a
  step to see what the tool reported, its diff and the files it touched.
- A file path in a reply that is a real file shows in gold. Click it to open the file,
  at the line.
- Under a finished reply: **Changed in this reply** lists the files Hermes edited, with
  their +/− lines (click for the diff). Then how long it took, **Copy** and **Retry**.

### Approvals in the chat

When Hermes wants to edit a file or run a risky command, it asks in the chat: a card with
the whole change as a diff (**Open diff** for the side-by-side view), or the exact command.
**Deny** is the first button, and a card never takes focus by itself, so typing can never
approve anything. If the chat is not on screen, a notification takes you to the card.

### Skills and files

![The "/" list of skills, and the model menu with subscription usage](https://raw.githubusercontent.com/jefuriiij/hermes-studio-releases/main/images/composer.png)

- Type **`/`** anywhere in a message to pick one of Hermes's skills. It stays in the text
  in gold, and one message can name several. At the start of a message, `/` also lists
  Hermes's own commands (`/compress`, `/context`, …).
- Type **`@`** to add a file or folder from the workspace.
- The **+** button adds files, your selection, VS Code's errors and warnings, your git
  changes, or an image (or paste one). Hold **Shift** to drop files into the chat.
- The file you are working in goes along by itself (its path and line, or your selected
  lines, never the whole file). Click **×** on its chip to leave it out.

### Models, modes and effort

![The modes pop-up with the effort slider, and the Sessions pop-up](https://raw.githubusercontent.com/jefuriiij/hermes-studio-releases/main/images/modes.png)

- **Switch models without leaving the session.** The model menu groups every model Hermes
  can use by provider, and shows how much of each subscription is used.
- **Usage limits:** when a provider gives up, a banner offers to continue with another
  provider's model. One click switches and sends your message again.
- **Modes:** **Manual** (Hermes asks before each edit), **Edit automatically** (edits
  files in this folder without asking) or **Auto** (edits any file without asking).
  Sensitive files and risky commands always ask. The composer's outline takes the mode's
  colour.
- **Effort:** a slider from Low to Max for how hard Hermes thinks.
- **Context:** a ring shows how full the context window is. Click it for the details and
  **Compress now**.

### Sessions

- The **Sessions** pop-up lists the Hermes sessions started in this folder, with search.
  Open one to continue where it left off. Rename or delete sessions there.
- **Chats keep working in the background.** Start a new chat while Hermes works: the
  first one goes on, and the pop-up shows it as working.
- **Send while Hermes works.** Your message waits in a queue and goes out when the reply
  ends, or use **Send now** to steer the reply Hermes is writing.
- Move the chat into an **editor tab** and back.

### What Hermes learned

![The Learning tab with new and improved skills, and the Memory tab](https://raw.githubusercontent.com/jefuriiij/hermes-studio-releases/main/images/panels.png)

Tabs next to the chat, and a **Hermes** view in the activity bar, show Hermes's own files,
read-only:

- **Learning:** the skills Hermes created or improved this week, each change as a diff,
  and the chat it came from. Plus the **curator**: when it last ran and what it did.
- **Memory:** your notes and profile, and memory changes that wait for your OK.
- **Capabilities:** skills by source, MCP connectors and plugins.
- **Agents:** background subagents, task by task.

### Notifications

Hermes Studio tells you when Hermes needs your OK, finishes or stops, but only while you
are not looking at that chat. On Windows it also shows a Windows notification while VS Code
is in the background. A notification never answers an approval: **Show** only takes you to
the card.

## Requirements

- **VS Code 1.106 or later.** Google Antigravity works too. Cursor is not supported (it
  reserves the secondary side bar).
- **Hermes Agent**, installed and set up with a model provider (`hermes model`). Hermes
  Studio finds `hermes.exe` on `PATH` or in `%LOCALAPPDATA%\hermes\bin`; on macOS and
  Linux, `hermes` on `PATH` or in `~/.local/bin`.
- Built and tested on Windows. macOS and Linux should work, but are less tested.

## Getting started

1. Install Hermes Studio.
2. Open a folder.
3. Click the **Hermes** icon in the editor title bar, or run **Hermes Studio: Open Chat**.

If Hermes has no model provider yet, the chat says **Setup needed**. Click **Run setup**:
it runs `hermes model` in a terminal and restarts Hermes when you close it.

## How it works

Hermes Studio starts `hermes acp`, Hermes's official editor mode, for the open folder and
talks to it over the [Agent Client Protocol](https://agentclientprotocol.com). Editor mode
builds the same agent as the terminal, so learning, the curator and memory keep working.

Hermes is the only store: Hermes Studio saves no transcript of its own. It connects to
nothing itself (no telemetry, no servers) and talks only to the `hermes` program on your
computer. It reads Hermes's files for the panels, but never its credentials.

## Settings

| Setting | Default | |
|---|---|---|
| `hermesStudio.path` | *(empty)* | Absolute path to the Hermes executable. Machine-scoped: a workspace cannot change it. |
| `hermesStudio.notifications` | `true` | Tell you when Hermes needs your approval, finishes or stops, while you are not looking at that chat. |
| `hermesStudio.windowsNotifications` | `true` | On Windows, also show a Windows notification while VS Code is not the front window. |
| `hermesStudio.trace` | `false` | Log every protocol message to the **Hermes Studio** output channel. Includes your prompts: leave it off unless you are debugging. |

## Troubleshooting

- **"Hermes not found":** set `hermesStudio.path` to `hermes.exe` (not `hermes.cmd`).
- **"Setup needed":** click **Run setup**.
- **Anything else:** **Hermes Studio: Show Log** shows what Hermes printed.

## Feedback

Found a bug or have an idea?
[Open an issue](https://github.com/jefuriiij/hermes-studio-releases/issues).

## License

[MIT](LICENSE)
