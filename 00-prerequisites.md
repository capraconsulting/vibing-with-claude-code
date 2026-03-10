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

You need to have been granted access to Claude Code from Tet Digital.
If you have been granted access, follow the guide on https://www.notion.so/Claude-Code-Bedrock-30399ecf4db480ee9420cd02405ac4bd.
Otherwise, team up with another participant.

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

## Troubleshooting

**"command not found: claude"**
Make sure npm's global bin directory is in your PATH. Run `npm config get prefix` and add the `/bin` subdirectory to your PATH. Ask your neighbour for help if you are unsure on how to do this.

**Authentication fails**
Try `claude auth logout` then `claude auth login` to start fresh.

