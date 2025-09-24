# MCP Workshop Demo: Claude Desktop + Playwright Browser Automation

This guide shows how to set up a live demonstration of Model Context Protocol (MCP) using Claude Desktop connected to Microsoft's Playwright MCP server for browser automation that you can view in real-time.

## Prerequisites

- **Node.js** (version 18 or higher) - Download from [nodejs.org](https://nodejs.org/)
- **Claude Desktop** app installed and logged in
- **Web browser** for viewing the demo
- **Stable internet connection** for downloading packages
- Basic familiarity with terminal/command line

## What You'll Build

By the end of this demo, you'll have:
- Claude Desktop connected to Playwright MCP server
- Real browser automation capabilities for Claude
- Visual demo setup using NoVNC (optional)
- Live demonstration of AI-powered web interactions

## Quick Setup

### Step 1: Verify Node.js Installation

```bash
# Check if Node.js is installed (should be v18 or higher)
node --version
npm --version

# If not installed, download from https://nodejs.org/
```

### Step 2: Configure Claude Desktop with Playwright MCP

1. Open Claude Desktop
2. Go to **Settings** → **Developer**
3. Click **"Edit Config"** to open `claude_desktop_config.json`
4. Add the Playwright MCP server configuration:

```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["-y", "@playwright/mcp"]
    }
  }
}
```

5. **Save the file** and **restart Claude Desktop**

### Step 3: Verify MCP Connection

1. Open Claude Desktop after restart
2. Go to **Settings** → **Developer**
3. You should see the Playwright server listed as **"Connected"**
4. Look for a 🔨 (hammer) icon in the chat input area - this indicates MCP tools are available

### Step 4: Test Basic Browser Automation

Send this test message to Claude:
```
"Can you open a web browser and navigate to example.com, then tell me what you see on the page?"
```

**Expected Result:** Claude should acknowledge it can use browser tools and provide information about the webpage content.

## Optional: Visual Demo Enhancement

For maximum visual impact during your workshop, you can add a visual browser component:

### Option A: NoVNC Docker Container (Visual Demo)

```bash
# Run browser container for visual demonstration
docker run -d \
  --name visual-demo-browser \
  -p 6080:6080 \
  -e VNC_PASSWORD=demo123 \
  dorowu/ubuntu-desktop-lxde-vnc

# Access at: http://localhost:6080/vnc.html
```

**Demo Flow:**
1. Claude uses Playwright MCP (headless) for actual automation
2. Manually demonstrate similar actions in the visual browser for audience impact
3. Explain that Playwright runs efficiently in the background while providing the same capabilities

### Option B: Playwright with Headed Mode

You can configure Playwright to run in headed mode for direct visual demonstration:

```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["-y", "@playwright/mcp", "--headless=false"]
    }
  }
}
```

**Note:** Headed mode may require additional display setup depending on your operating system.

## Demo Script Ideas

### Basic Navigation and Content Analysis
```
"Please navigate to wikipedia.org and tell me what you see on the homepage"
```

### Search and Data Extraction
```
"Go to wikipedia.org, search for 'Model Context Protocol', and summarize the key points from the article"
```

### Form Interaction
```
"Navigate to google.com and search for 'Anthropic Claude'. What are the top 3 search results about?"
```

### Multi-step Workflow
```
"Go to github.com, search for repositories about 'MCP servers', and tell me about the most popular one"
```

### Screenshot and Analysis
```
"Take a screenshot of the current page and describe what you see"
```

### Advanced Web Interaction
```
"Navigate to a news website, find the top story, and provide a brief summary of the headline and first paragraph"
```

### E-commerce Demonstration
```
"Go to an e-commerce site, search for a specific product, and tell me about the pricing and availability"
```

## Presentation Tips

1. **Dual Screen Setup:** Show both Claude Desktop and the browser view side-by-side
2. **Narrate Actions:** Explain what Claude is doing as it happens
3. **Show Tool Discovery:** Demonstrate how Claude automatically discovers available browser tools
4. **Error Handling:** Try asking Claude to interact with non-existent elements to show graceful error handling
5. **Real-time Visual:** The audience can see exactly what Claude sees and does

## Troubleshooting

### MCP Server Won't Connect
```bash
# Check Node.js version (needs v18+)
node --version

# Test Playwright MCP manually
npx -y @playwright/mcp

# Check Claude Desktop developer console for error messages
```

### Playwright Installation Issues
```bash
# Clear npm cache and reinstall
npm cache clean --force

# Install Playwright browsers manually
npx playwright install

# Check if Playwright can run
npx playwright --version
```

### Claude Can't See Browser Tools
- Ensure you've restarted Claude Desktop after configuration
- Check that `claude_desktop_config.json` has valid JSON syntax
- Look for the 🔨 hammer icon in Claude's input area
- Verify MCP server shows "Connected" in Developer settings

### Performance Issues
```bash
# Run with less resource usage
# Add to your configuration:
"env": {
  "PLAYWRIGHT_BROWSERS_PATH": "0",
  "PLAYWRIGHT_SKIP_BROWSER_DOWNLOAD": "1"
}
```

### Permission Errors (macOS/Linux)
```bash
# Fix npm permissions
sudo chown -R $(whoami) ~/.npm
sudo chown -R $(whoami) /usr/local/lib/node_modules
```

### Browser Crashes or Timeouts
```bash
# Increase timeout in configuration
"env": {
  "PLAYWRIGHT_TIMEOUT": "30000"
}
```

### Common Error Messages

| Error | Solution |
|-------|----------|
| "npx command not found" | Install Node.js from nodejs.org |
| "MCP server disconnected" | Restart Claude Desktop |
| "Browser launch failed" | Run `npx playwright install` |
| "Invalid JSON config" | Check JSON syntax in config file |
| "Permission denied" | Fix npm permissions (see above) |

## Cleanup After Demo

```bash
# Playwright MCP cleans up automatically
# No manual cleanup needed for the MCP server

# If you used the optional visual demo container:
docker stop visual-demo-browser
docker rm visual-demo-browser
docker rmi dorowu/ubuntu-desktop-lxde-vnc
```

## Workshop Follow-up

After the demo, participants can:
- Explore other MCP servers from the community
- Build their own simple MCP server
- Connect multiple MCP servers to Claude Desktop
- Try different tools like file system access, calculators, or API integrations

## Security Notes for Corporate Environments

- **Playwright MCP runs locally** - no external servers or cloud dependencies
- **Browser data stays on your machine** - all automation happens locally
- **Network access**: Playwright will access websites as configured
- **Consider corporate firewalls** - ensure npm and browser access is allowed
- **Review your company's Node.js and npm policies** before installation
- **Headless by default** - no visible browser windows unless configured otherwise

## Additional Resources

- [Microsoft Playwright MCP Documentation](https://github.com/microsoft/playwright-mcp)
- [MCP Official Documentation](https://modelcontextprotocol.io)
- [Anthropic MCP Repository](https://github.com/anthropics/mcp)
- [Community MCP Servers](https://github.com/modelcontextprotocol/servers)
- [Claude Desktop Configuration Guide](https://docs.claude.com)
- [Playwright Official Documentation](https://playwright.dev)
- [MCP Server Directory](https://www.pulsemcp.com)
