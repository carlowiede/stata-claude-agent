# Using Claude Code + Stata in VS Code for Economic Research
### A setup guide for economists — no developer experience required

Built by an economics researcher at [Your Institution].
Companion to: *[Your thesis title]* — [Year]

---

## A note on AI choice

This guide uses Claude Code, but the Stata MCP works with any AI that
supports the Model Context Protocol — including Cursor, Copilot, and others.
Claude Code is used here because it is free to start and requires no IDE
beyond VS Code. Swap it out for whatever AI you prefer.

---

## What this gives you

Once set up, you can type plain English in VS Code and Claude will:
- Write and run Stata and/or R code on your live data
- Inspect your variables and dataset structure
- Run regressions and show you the output instantly
- Debug errors in your do-files or R scripts
- Build and save complete analysis scripts

This is not ChatGPT describing what code *would* do. Claude is actually
running commands in your local Stata installation and returning real results.

**R is optional.** The setup works with Stata alone. If you also have R
installed, the agent can use it for event study plots and additional
visualisations — but it will ask you upfront what software you have before
writing any code.

---

## What you need before starting

- A Mac (macOS 12 or later) or Windows (10 or later) computer
- Stata 17 or later (MP, SE, or BE — any licence works)
- About 20 minutes
- R (optional — only needed for event study plots and additional visualisations)

That's it. You do not need Python or any prior experience with developer tools.

> **Windows users:** the steps below use Mac keyboard shortcuts and Terminal.
> Where you see `Cmd`, use `Ctrl` instead. Where you see "Terminal", use
> "Command Prompt" or "PowerShell". Any Mac-specific differences are noted
> in the relevant steps.

---

## Step 1 — Install VS Code

If you don't have VS Code yet:
1. Go to https://code.visualstudio.com
2. Download and install it (Mac or Windows installer available on the same page)

---

## Step 2 — Install Node.js

Claude Code requires Node.js to run.

1. Go to https://nodejs.org
2. Download the **LTS** version
3. Install it

Verify it worked by opening a terminal (Mac: `Cmd + Space`, type "Terminal" / Windows: press `Win + R`, type `cmd`) and running:

```bash
node --version
```

You should see a version number like `v20.x.x`.

---

## Step 3 — Install Claude Code

Claude Code is Anthropic's AI assistant. Even when using VS Code, you need
it installed on your system — the VS Code extension runs on top of it.

In Terminal, run:

```bash
npm install -g @anthropic-ai/claude-code
```

Verify it worked:

```bash
claude --version
```

You should see a version number printed.

---

## Step 4 — Install uv

`uv` is a small tool that runs the Stata MCP server. It takes about 10
seconds to install.

**Mac** — in Terminal, run:

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

**Windows** — in PowerShell, run:

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

Restart your terminal completely, then verify:

```bash
uvx --version
```

---

## Step 5 — Install the Claude Code extension in VS Code

1. Open VS Code
2. Press `Cmd + Shift + X` to open the Extensions panel
3. Search for **"Claude Code"** (by Anthropic)
4. Click Install

Once installed, you will see a Claude icon in your left sidebar.

---

## Step 6 — Install a Stata MCP extension

This connects VS Code to your local Stata installation. There are two
good options — both do the same fundamental job, so pick either one:

| Extension | Author | Search for |
|---|---|---|
| **Stata Workbench** | Thomas Monk, LSE | `Stata Workbench` |
| **Stata MCP** | Lu Han / DeepEcon.ai | `Stata MCP` |

This guide uses Stata Workbench in the steps below, but the setup is
nearly identical for the DeepEcon extension.

> Note: this guide uses Claude, but both extensions work with any AI
> that supports MCP — including GitHub Copilot, Cursor, and others.

1. In VS Code, press `Cmd + Shift + X` to open the Extensions panel
2. Search for **"Stata Workbench"** (by Thomas Monk, LSE)
3. Click Install

Once installed, you need to turn on one setting to connect it to Claude Code.
This step is easy to miss — without it, Claude will not see Stata.

1. Press `Cmd + Shift + X` to open the Extensions panel again
2. Search for **"Stata Workbench"** and click on it to open its page
3. Click the small **gear icon** next to the extension name
4. Click **"Extension Settings"**
5. Look for the setting called **"Stata Mcp: Configure Claude Code"**
6. **Tick the checkbox to turn it on**
7. Fully quit VS Code (`Cmd + Q` on Mac, `Alt + F4` on Windows) and reopen it

---

## Step 7 — Verify the connection

1. Open VS Code and click the Claude icon in the sidebar
2. Type `/mcp` in the chat box

You should see `mcp_stata` listed as **connected**. ✅

If Stata is not found automatically, see the troubleshooting section below.

---

## Step 8 — Test it

Type this in the Claude Code chat panel:

> *"Run the Stata command: display 'hello from Stata'"*

If you see `hello from Stata` in the output, everything is working.
Claude is now talking to your live Stata installation.

---

## Step 9 — Start your analysis

Once you have cloned the repo and opened it in VS Code (see below), the
agent will automatically ask you five short setup questions at the start
of each session:

1. Do you have Stata, R, or both installed?
2. Which would you like to use for the main analysis?
3. What is your identification strategy (DiD, IV, or RD)?
4. What is your outcome variable, and is it continuous or binary?
5. Is this panel data or cross-sectional?

This prevents the agent from assuming the wrong software or workflow and
avoids getting stuck mid-analysis. Answer once and the agent will confirm
its understanding before writing any code.

---

## Step 10 — Clone this repository and open it in VS Code

Rather than downloading files individually, clone this entire repository
and open it as your project folder in VS Code. This is important — Claude
Code only reads the `CLAUDE.md` agent file if it is in the root of the
folder you have open. Without this step, the custom commands below will
not work.

In Terminal, run:

```bash
git clone https://github.com/[yourusername]/[reponame].git
```

If you get a "command not found: git" error:
- **Mac:** run `xcode-select --install` in Terminal
- **Windows:** download Git from https://git-scm.com and install it

Then retry the clone command.

**No Terminal experience?** You can also click the green **"Code"** button
on the GitHub repo page and select **"Download ZIP"**. Unzip it, then open
that folder in VS Code via **File → Open Folder**. The commands will work
either way — what matters is that the folder containing `CLAUDE.md` is the
one open in VS Code.

Then in VS Code:
1. Click **File → Open Folder**
2. Select the cloned (or unzipped) folder
3. Click **Open**

Claude Code will automatically read `CLAUDE.md` at the start of every
session. You will then have access to these shortcuts:

- `/did` — run a full Difference-in-Differences workflow
- `/iv` — run a full Instrumental Variables workflow
- `/rd` — run a full Regression Discontinuity workflow
- `/robustness` — run all standard robustness checks
- `/writeup` — produce a results summary and commented do-file

Your own research files (data, do-files, results) go inside this folder
alongside the existing files.

---

## Troubleshooting

**Stata not found automatically**

Open VS Code settings (`Cmd + ,`), search for "stataMcp", and add your
Stata path in the "Stata Path" field. Common paths on Mac:

- Stata MP: `/Applications/Stata/StataMP.app/Contents/MacOS/stata-mp`
- StataNow MP: `/Applications/StataNow/StataMP.app/Contents/MacOS/stata-mp`
- Stata SE: `/Applications/Stata/StataSE.app/Contents/MacOS/stata-se`

**`mcp_stata` not showing up in /mcp**

1. Make sure "Configure Claude Code" is on in Stata Workbench settings
2. Fully quit and reopen VS Code
3. If still missing, run this in Terminal:

```bash
claude mcp add-json mcp_stata --scope user \
  '{"type":"stdio","command":"uvx","args":["--refresh","--refresh-package",
  "mcp-stata","--from","mcp-stata@latest","mcp-stata"]}'
```

Then restart VS Code and check `/mcp` again.

**`uvx: command not found` after installing uv**

**Mac** — run this in Terminal:

```bash
echo 'export PATH="$HOME/.cargo/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

**Windows** — restart PowerShell after installation. If still missing,
search "Environment Variables" in the Start menu and add the uv install
path manually.

Then retry `uvx --version`.

**Claude Code extension not connecting**

Make sure you ran `npm install -g @anthropic-ai/claude-code` in Terminal
and that `claude --version` returns a version number. The VS Code extension
requires the CLI to be installed — it does not work standalone.

---

## What's in this repository

```
/
├── README.md              ← This setup guide
├── CLAUDE.md              ← Causal inference agent for Claude Code
└── .claude/
    └── commands/
        ├── did.md         ← /did command
        ├── iv.md          ← /iv command
        ├── rd.md          ← /rd command
        ├── robustness.md  ← /robustness command
        └── writeup.md     ← /writeup command
```

---

## Using a different AI

The Stata MCP server is not tied to Claude. If you prefer another AI,
the Stata Workbench extension supports:

- **Cursor** — replace Steps 3 and 5 with installing Cursor (https://cursor.com).
  The Stata Workbench extension works identically.
- **GitHub Copilot** — supported in VS Code from version 1.102+.
  Follow the same Steps 1–6, then configure Copilot to use the MCP server.
- **Any MCP-compatible AI** — the `CLAUDE.md` agent file can be adapted
  to any system prompt format. The methodology and slash commands are
  AI-agnostic — only the filename is Claude-specific.

---

## Credits


- **Stata MCP** by Thomas Monk (LSE) — https://github.com/tmonk/mcp-stata
- **Stata Workbench** by Thomas Monk (LSE) — https://github.com/tmonk/stata-workbench
- **Claude Code** by Anthropic — https://claude.ai/code
- **VS Code** by Microsoft — https://code.visualstudio.com

---

## Citation

If you use this setup in your research, please cite:

> [Your Name] ([Year]). *[Thesis title]*. [Institution].
> GitHub: https://github.com/[yourusername]/[reponame]
