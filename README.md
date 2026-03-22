# Using Claude Code + Stata in VS Code for Economic Research
### A setup guide for economists — no developer experience required

Built by a health economics researcher at KU Leuven.
Companion to: Evaluation of the French Sugar Tax on BMI and Health: An Agentic Research Framework — 2026

---

## What this gives you

Once set up, you can type plain English in VS Code and Claude will:
- Write and run Stata code on your live data
- Inspect your variables and dataset structure
- Run regressions and show you the output instantly
- Debug errors in your do-files
- Build and save complete analysis scripts

This is not ChatGPT describing what code *would* do. Claude is actually
running commands in your local Stata installation and returning real results.

---

## What you need before starting

- A Mac running macOS 12 or later
- Stata 17 or later (MP, SE, or BE — any licence works)
- About 20 minutes

That's it. You do not need Python or any prior experience with developer tools.

---

## Step 1 — Install VS Code

If you don't have VS Code yet:
1. Go to https://code.visualstudio.com
2. Download and install it like any Mac application

---

## Step 2 — Install Node.js

Claude Code requires Node.js to run.

1. Go to https://nodejs.org
2. Download the **LTS** version
3. Install it

Verify it worked by opening Terminal (`Cmd + Space`, type "Terminal", press
Enter) and running:

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

In Terminal, run:

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Restart Terminal completely (quit and reopen it), then verify:

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

Step 6 — Install the Stata Workbench extension
This connects VS Code to your local Stata installation.

In VS Code, press Cmd + Shift + X
Search for "Stata Workbench" (by Thomas Monk, LSE)
Click Install

Once installed, you need to turn on one setting to connect it to Claude Code.
This step is easy to miss — without it, Claude will not see Stata.

Press Cmd + Shift + X to open the Extensions panel again
Search for "Stata Workbench" and click on it to open its page
Click the small gear icon next to the extension name
Click "Extension Settings"
Look for the setting called "Stata Mcp: Configure Claude Code"
Tick the checkbox to turn it on
Fully quit VS Code (Cmd + Q) and reopen it

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

## Step 9 — Add the causal inference agent (optional but recommended)

Download `CLAUDE.md` from this repository and place it in your project
folder. Claude Code reads it automatically at the start of every session,
giving you a pre-configured research assistant that knows DiD, IV, and RD
methodology without you having to explain anything.

With the agent loaded, you can use shortcuts like:
- `/did` — run a full Difference-in-Differences workflow
- `/iv` — run a full Instrumental Variables workflow
- `/rd` — run a full Regression Discontinuity workflow
- `/robustness` — run all standard robustness checks
- `/writeup` — produce a results summary and commented do-file

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

Run this in Terminal:

```bash
echo 'export PATH="$HOME/.cargo/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

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
└── replication/           ← Full replication package for the thesis
    ├── README.md
    └── code/
```

---

## Credits

- **Stata MCP** by Thomas Monk (LSE) — https://github.com/tmonk/mcp-stata
- **Stata Workbench** by Thomas Monk (LSE) — https://github.com/tmonk/stata-workbench
- **Claude Code** by Anthropic — https://claude.ai/code
- **VS Code** by Microsoft — https://code.visualstudio.com

---

## Citation

If you use this setup in your research, please cite:

> Carlo Wiede (2026). *Evaluation of the French Sugar Tax on BMI and Health: An Agentic Research Framework*. KU Leuven.
> GitHub: https://github.com/carlowiede/stata-claude-agent
