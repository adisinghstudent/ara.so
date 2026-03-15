---
name: chrome-cdp-live-browser
description: Connect AI agents to your live Chrome session via CDP — interact with open tabs, logged-in accounts, and real page state without launching a new browser.
triggers:
  - browse the web in chrome
  - interact with my open browser tabs
  - click elements on a webpage
  - take a screenshot of a browser tab
  - read page content from chrome
  - navigate to a url in chrome
  - evaluate javascript in browser
  - automate chrome with live session
---

# Chrome CDP Live Browser Skill

> Skill by [ara.so](https://ara.so) — Daily 2026 Skills collection

Give your AI agent access to your **live Chrome session** — the tabs you already have open, your logged-in accounts, and your current page state. Connects directly to Chrome's remote debugging WebSocket with no Puppeteer, no fresh browser instance, and no re-login required.

## What It Does

- **Read live pages** — access Gmail, GitHub, internal tools, anything you're already logged into
- **Interact with tabs** — click, type, navigate, evaluate JS in your active browser
- **Capture state** — screenshots, accessibility trees, full HTML, network timing
- **Persistent daemons** — one lightweight background process per tab, modal appears once, reused silently
- **Handles scale** — works reliably with 100+ open tabs where Puppeteer-based tools time out

## Installation

### Enable Remote Debugging in Chrome

Navigate to `chrome://inspect/#remote-debugging` and toggle the switch. That's it — no flags, no restart needed.

### As a pi skill

```bash
pi install git:github.com/pasky/chrome-cdp-skill@v1.0.1
```

### For other agents (Amp, Claude Code, Cursor, etc.)

Clone the repository and copy the `skills/chrome-cdp/` directory wherever your agent loads skills or context from:

```bash
git clone https://github.com/pasky/chrome-cdp-skill.git
cp -r chrome-cdp-skill/skills/chrome-cdp/ ./your-agent-skills/
```

**Requirements:** Node.js 22+ only. No `npm install` needed — zero runtime dependencies.

## Configuration

The CLI auto-detects Chrome, Chromium, Brave, Edge, and Vivaldi on macOS and Linux via the `DevToolsActivePort` file.

For non-standard browser locations, set:

```bash
export CDP_PORT_FILE="/path/to/your/browser/profile/DevToolsActivePort"
```

## Key Commands

```bash
# List all open tabs (shows targetId prefixes)
scripts/cdp.mjs list

# Take a screenshot of a tab
scripts/cdp.mjs shot <target>

# Get accessibility tree (compact, semantic — best for understanding page structure)
scripts/cdp.mjs snap <target>

# Get full HTML, optionally scoped to a CSS selector
scripts/cdp.mjs html <target>
scripts/cdp.mjs html <target> ".main-content"

# Evaluate JavaScript in the page context
scripts/cdp.mjs eval <target> "document.title"

# Navigate and wait for load
scripts/cdp.mjs nav <target> https://example.com

# Network resource timing
scripts/cdp.mjs net <target>

# Click by CSS selector
scripts/cdp.mjs click <target> "button.submit"

# Click at specific coordinates
scripts/cdp.mjs clickxy <target> 320 240

# Type text at the focused element (works across cross-origin iframes)
scripts/cdp.mjs type <target> "hello world"

# Click "load more" repeatedly until selector disappears
scripts/cdp.mjs loadall <target> ".load-more-btn"

# Raw CDP command passthrough
scripts/cdp.mjs evalraw <target> Page.captureScreenshot '{"format":"png"}'

# Open a new tab (triggers Allow prompt once)
scripts/cdp.mjs open https://example.com

# Stop daemon(s)
scripts/cdp.mjs stop
scripts/cdp.mjs stop <target>
```

`<target>` is a unique prefix of the `targetId` shown by `list`.

## Code Examples

### Listing Tabs and Getting a Target

```javascript
import { execSync } from 'child_process';

// List all open tabs
const tabs = execSync('scripts/cdp.mjs list').toString();
console.log(tabs);
// Output includes lines like:
// abc123  https://github.com/...  GitHub - some repo

// Use just the prefix (e.g., "abc") as <target>
const target = 'abc123';
```

### Reading Page Content

```javascript
// Get the accessibility tree for semantic page understanding
const tree = execSync(`scripts/cdp.mjs snap ${target}`).toString();

// Get full HTML
const html = execSync(`scripts/cdp.mjs html ${target}`).toString();

// Scope HTML to a specific section
const section = execSync(`scripts/cdp.mjs html ${target} "#content"`).toString();
```

### Interacting with a Page

```javascript
// Navigate to a URL and wait for load
execSync(`scripts/cdp.mjs nav ${target} https://example.com`);

// Click a button
execSync(`scripts/cdp.mjs click ${target} "button[type=submit]"`);

// Fill in a form field
execSync(`scripts/cdp.mjs click ${target} "input#search"`);
execSync(`scripts/cdp.mjs type ${target} "my search query"`);

// Take a screenshot after interaction
execSync(`scripts/cdp.mjs shot ${target}`);
```

### Evaluating JavaScript

```javascript
// Read data from the page
const title = execSync(`scripts/cdp.mjs eval ${target} "document.title"`).toString().trim();

// Extract structured data
const links = execSync(
  `scripts/cdp.mjs eval ${target} "JSON.stringify([...document.querySelectorAll('a')].map(a => ({text: a.textContent.trim(), href: a.href})))"`
).toString();

const parsed = JSON.parse(links);
```

### Handling Infinite Scroll / Load More

```javascript
// Keep clicking "Load more" until button disappears, then get full content
execSync(`scripts/cdp.mjs loadall ${target} ".load-more"`);
const fullContent = execSync(`scripts/cdp.mjs html ${target}`).toString();
```

### Raw CDP Passthrough

```javascript
// Capture a screenshot with specific format via raw CDP
const result = execSync(
  `scripts/cdp.mjs evalraw ${target} Page.captureScreenshot '{"format":"jpeg","quality":80}'`
).toString();

// Get cookies
const cookies = execSync(
  `scripts/cdp.mjs evalraw ${target} Network.getCookies`
).toString();
```

### Using in an Agent Workflow

```javascript
// Typical agent pattern: observe → reason → act → verify
async function agentStep(targetPrefix) {
  // 1. Observe current state
  const snapshot = execSync(`scripts/cdp.mjs snap ${targetPrefix}`).toString();
  
  // 2. Reason about the page (pass to LLM, etc.)
  // ...
  
  // 3. Act based on reasoning
  execSync(`scripts/cdp.mjs click ${targetPrefix} ".btn-primary"`);
  
  // 4. Verify result
  const after = execSync(`scripts/cdp.mjs shot ${targetPrefix}`).toString();
  return after; // path to screenshot file
}
```

## Integration Patterns

### With Claude Code / Cursor / Amp

Copy the `skills/chrome-cdp/` directory into your project and reference `SKILL.md` in your agent's system prompt or context. The agent can then invoke `scripts/cdp.mjs` as shell commands.

### Accessing Authenticated Services

Because the skill connects to your running Chrome session, no credential management is needed:

```javascript
// The agent can directly read your logged-in Gmail, GitHub PRs, etc.
execSync(`scripts/cdp.mjs nav ${target} https://github.com/notifications`);
const notifs = execSync(`scripts/cdp.mjs snap ${target}`).toString();
```

### Working with SPAs and Dynamic Content

```javascript
// Navigate and wait, then get rendered content (not source HTML)
execSync(`scripts/cdp.mjs nav ${target} https://app.example.com/dashboard`);

// Evaluate JS to wait for a specific element
execSync(
  `scripts/cdp.mjs eval ${target} "await new Promise(r => { const i = setInterval(() => { if (document.querySelector('.data-loaded')) { clearInterval(i); r(); } }, 100); })"`
);

const content = execSync(`scripts/cdp.mjs html ${target} ".dashboard"`).toString();
```

## Troubleshooting

### "Allow debugging" modal keeps appearing

This happens when tools reconnect on every command. `chrome-cdp` holds persistent daemons per tab — the modal should only appear **once per tab**. If it reappears, check that a previous daemon did not crash:

```bash
scripts/cdp.mjs stop
# Then retry your command — a fresh daemon will be spawned
```

### Chrome not detected

```bash
# Check if DevToolsActivePort exists
ls ~/Library/Application\ Support/Google/Chrome/Default/DevToolsActivePort  # macOS
ls ~/.config/google-chrome/Default/DevToolsActivePort                        # Linux

# If in a non-standard location, set the env var
export CDP_PORT_FILE="/custom/path/DevToolsActivePort"
```

### Remote debugging not enabled

Make sure you've toggled the switch at `chrome://inspect/#remote-debugging` — it must be **on**. The toggle persists across Chrome restarts.

### Target not found / prefix ambiguous

```bash
# Run list to get current targetIds
scripts/cdp.mjs list

# Use a longer prefix to disambiguate
scripts/cdp.mjs snap abc123def  # instead of just "abc"
```

### Node.js version error

The skill requires Node.js 22+. Check your version:

```bash
node --version  # must be v22.x.x or higher
```

Use [nvm](https://github.com/nvm-sh/nvm) or [fnm](https://github.com/Schniz/fnm) to upgrade if needed.

### Daemon inactivity timeout

Daemons auto-exit after **20 minutes of inactivity**. The next command to a tab simply spawns a new daemon transparently — no action needed.
