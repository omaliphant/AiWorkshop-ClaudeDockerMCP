# MCP Workshop Demo - Pre-Workshop Setup Guide (Playwright)

This guide helps you prepare and test Microsoft Playwright MCP server with Claude Desktop before your workshop to ensure a smooth demonstration.

## Pre-Workshop Checklist

### 1. Environment Requirements

**System Requirements:**
- [ ] **Node.js v18+** installed from [nodejs.org](https://nodejs.org/)
- [ ] **Claude Desktop** app installed and logged in
- [ ] **Stable internet connection** for package downloads
- [ ] **Modern web browser** (Chrome, Firefox, Safari, Edge)
- [ ] **Presentation setup** (dual monitors recommended)

**Node.js Verification:**
```bash
# Check Node.js version (must be 18 or higher)
node --version

# Check npm version
npm --version

# If versions are too old, download latest from nodejs.org
```

### 2. Playwright MCP Pre-Installation

**Pre-install Playwright MCP (Recommended):**
```bash
# Pre-install the Playwright MCP package
npx -y @playwright/mcp --help

# Install Playwright browsers (this can take 5-10 minutes)
npx playwright install

# Verify Playwright installation
npx playwright --version
```

**Test Playwright Functionality:**
```bash
# Quick test that Playwright can launch browsers
npx playwright open google.com

# This should open a browser window to Google
# Close the window after verifying it works
```

### 3. Test Run - Complete Setup

**Configure Claude Desktop:**
1. Open Claude Desktop
2. Go to **Settings** → **Developer**
3. Click **"Edit Config"** to open `claude_desktop_config.json`
4. Add the Playwright configuration:

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

**Verify Connection:**
```bash
# Check if the MCP server starts successfully
npx -y @playwright/mcp

# Should show MCP server starting without errors
# Press Ctrl+C to stop the test
```

### 4. Claude Desktop Integration Test

**Test Claude Desktop MCP Connection:**
1. Open Claude Desktop (after restart)
2. Go to **Settings** → **Developer**
3. Verify Playwright server shows as **"Connected"**
4. Look for 🔨 (hammer) icon in the chat input area

**Basic Functionality Test:**
Send this test message to Claude:
```
"Can you navigate to example.com and tell me what the page title is?"
```

**Expected Results:**
- Claude should recognize it has browser capabilities
- Claude should successfully navigate to example.com
- Claude should return information about the webpage

### 5. Advanced Configuration Testing

**Test Different Playwright Options:**
Try these variations in your `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["-y", "@playwright/mcp"],
      "env": {
        "PLAYWRIGHT_TIMEOUT": "30000"
      }
    }
  }
}
```

**Test Headed Mode (Optional for Demo):**
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

**Note:** Headed mode will show browser windows during automation, which can be great for demos but may require display configuration.

### 6. End-to-End Demo Test

**Test Comprehensive Automation:**

Send these progressive test messages to Claude:

1. **Basic Navigation:**
```
"Navigate to wikipedia.org and tell me what you see"
```

2. **Search Functionality:**
```
"Search for 'Model Context Protocol' on Wikipedia and summarize the first paragraph"
```

3. **Multi-step Workflow:**
```
"Go to github.com, search for 'MCP servers', and tell me about the top repository"
```

**Expected Results:**
- All commands should execute successfully
- Claude should provide detailed information about each webpage
- No error messages or timeouts
- Smooth progression between different websites

### 7. Performance Optimization

**For Smoother Demo Performance:**

**Option 1: Optimize Playwright Settings**
```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["-y", "@playwright/mcp"],
      "env": {
        "PLAYWRIGHT_TIMEOUT": "15000",
        "PLAYWRIGHT_BROWSER": "chromium"
      }
    }
  }
}
```

**Option 2: Pre-warm Browser Instances**
```bash
# Pre-launch browsers to improve startup time
npx playwright install chromium
npx playwright install firefox
npx playwright install webkit

# Test browser launches
npx playwright open --browser=chromium about:blank
```

**Resource Management:**
- Close unnecessary applications during demo
- Ensure stable internet connection
- Have backup demo scenarios ready

### 8. Demo Content Preparation

**Prepare Demo Scenarios:**
- [ ] Test basic navigation (Wikipedia, GitHub)
- [ ] Test search functionality (Google, documentation sites)
- [ ] Test form interactions
- [ ] Test screenshot capabilities
- [ ] Test multi-step workflows
- [ ] Prepare error handling examples

**Reliable Test Sites:**
- **wikipedia.org** - Good for text content and search
- **github.com** - Complex layouts and interactions
- **example.com** - Fast loading, simple structure
- **httpbin.org** - API testing and responses
- **playwright.dev** - Technical documentation

**Backup Demo Content:**
- Prepare local HTML files if internet fails
- Have screenshots of expected results
- Create simple demo scripts for common scenarios

### 9. Presentation Setup

**Screen Layout:**
- [ ] Claude Desktop on primary screen
- [ ] Browser window for showing Claude's work (if using headed mode)
- [ ] Terminal window ready for troubleshooting
- [ ] Workshop slides ready

**Demo Flow Practice:**
1. Show MCP server connection status in Claude Desktop
2. Send test command to Claude
3. If using headed mode, point out browser windows opening
4. Show Claude's response with extracted data
5. Demonstrate different types of web interactions
6. Practice smooth transitions between examples

**Presentation Tips:**
- Explain Playwright's advantages (speed, reliability, cross-browser)
- Highlight headless vs headed mode differences  
- Show the MCP configuration simplicity
- Demonstrate error handling gracefully

### 10. Backup Plans

**If Playwright Issues Occur:**
- Have alternative MCP servers ready (file system, calculator)
- Prepare screenshots of successful Playwright automation
- Demo the MCP configuration process itself
- Use manual browser demonstration with explanation

**If Network Issues Occur:**
- Use local HTML files for demonstration
- Focus on MCP concept explanation with local tools
- Have video recording of working demo as backup
- Demonstrate file system MCP server instead

**Emergency Demo Script:**
```bash
# Quick file system MCP demo as backup
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/path/to/safe/directory"]
    }
  }
}
```

### 11. Workshop Day Checklist

**30 Minutes Before Workshop:**
```bash
# Verify Node.js and npm are working
node --version
npm --version

# Test Playwright MCP server manually
npx -y @playwright/mcp --help

# Pre-launch browsers for faster demo
npx playwright install

# Restart Claude Desktop fresh
# (Close and reopen the application)
```

**Quick Function Test:**
- [ ] Claude Desktop opens without errors
- [ ] MCP server shows "Connected" in Developer settings
- [ ] 🔨 Hammer icon visible in chat input
- [ ] Test command works: "Navigate to example.com and tell me the page title"
- [ ] Claude responds with webpage information

### 12. Troubleshooting Quick Reference

**Common Issues & Fixes:**

| Issue | Quick Fix |
|-------|-----------|
| MCP server won't connect | Restart Claude Desktop |
| "npx command not found" | Install Node.js from nodejs.org |
| Browser launch failed | Run `npx playwright install` |
| Permission errors | Fix npm permissions: `sudo chown -R $(whoami) ~/.npm` |
| Slow performance | Close other applications, check internet |
| Invalid JSON config | Validate JSON syntax in config file |

**Quick Diagnostic Commands:**
```bash
# Check system readiness
node --version                    # Should be 18+
npx playwright --version          # Should show version
npx -y @playwright/mcp --help     # Should show help text

# Test browser installation
npx playwright install --dry-run  # Shows what would be installed
```

### 13. Clean Up After Testing

```bash
# Playwright MCP cleans up automatically - no manual cleanup needed

# Optional: Remove downloaded browser binaries to save space
npx playwright uninstall

# If you want to clear npm cache
npm cache clean --force

# MCP server configuration stays in Claude Desktop for future use
```

## Final Pre-Workshop Verification

- [ ] Node.js 18+ installed and working
- [ ] Playwright MCP server connects to Claude Desktop
- [ ] Demo scenarios tested and timed  
- [ ] Backup plans prepared and tested
- [ ] Screen sharing/projection tested with both Claude Desktop and browser windows
- [ ] Workshop slides integrated with live demo timing
- [ ] Troubleshooting commands bookmarked and tested

**Recommended Timeline:**
- **Initial setup:** 2-3 days before workshop  
- **Full demo rehearsal:** 1 day before workshop
- **Final verification:** 1 hour before workshop
- **System restart:** 15 minutes before workshop

**Day-of Success Indicators:**
- ✅ MCP server shows "Connected" in Claude Desktop
- ✅ 🔨 Hammer icon appears in chat input
- ✅ Test navigation command works flawlessly
- ✅ All demo scenarios execute within expected timeframes

Good luck with your MCP + Playwright workshop! This preparation should ensure a smooth, impressive demonstration of AI-powered browser automation.
