# Pre-Work: Installation & Setup

## 1. System Requirements

- macOS, Linux, or Windows (via WSL)
- Node.js 18+ (we recommend installing via [nvm](https://github.com/nvm-sh/nvm))
- A terminal you're comfortable with (Terminal, iTerm2, Ghostty, Windows Terminal, etc.)

### Install Node.js with nvm

```bash
# Install nvm (see https://github.com/nvm-sh/nvm for latest instructions)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash

# Restart your terminal, then install Node.js
nvm install 18
nvm use 18
```

Verify your Node.js version:

```bash
node --version
# Should print v18.x.x or higher
```

## 2. Install Claude Code

```bash
npm install -g @anthropic-ai/claude-code
```

Verify the installation:

```bash
claude --version
```

## 3. Authenticate

Launch Claude Code and follow the authentication prompts:

```bash
claude
```

This will open your browser to authenticate. Follow the instructions to complete login.

## 4. Verify Everything Works

Create a test directory and run a quick check:

```bash
mkdir ~/claude-test && cd ~/claude-test
claude
```

Once inside Claude Code, type:

```
What is 2 + 2?
```

If you get a response, you're all set. Type `/exit` to quit.

Clean up:

```bash
rm -rf ~/claude-test
```

## 5. Optional: Choose Your Editor

Claude Code works great alongside any editor. If you use VS Code or JetBrains IDEs, Claude Code can also run as an extension within your editor. Install whichever setup you prefer:

- **Terminal:** No extra setup needed — just run `claude` in your terminal
- **VS Code:** Install the "Claude Code" extension from the marketplace
- **JetBrains:** Install the "Claude Code" plugin from the JetBrains marketplace

---

## Troubleshooting

**"command not found: claude"**
Make sure npm's global bin directory is in your PATH. Run `npm config get prefix` and add the `/bin` subdirectory to your PATH. Ask your neighbour for help if you are unsure on how to do this.

**Authentication fails**
Try `claude auth logout` then `claude auth login` to start fresh.

