# MCP Workshop Demo - Pre-Workshop Setup Guide

This guide helps you prepare and test everything before your workshop to ensure a smooth demonstration.

## Pre-Workshop Checklist

### 1. Environment Requirements

**System Requirements:**
- [ ] Docker Desktop installed with at least 4GB RAM allocated
- [ ] Claude Desktop app installed and logged in
- [ ] Stable internet connection
- [ ] Modern web browser (Chrome, Firefox, Safari, Edge)
- [ ] Presentation setup (dual monitors recommended)

**Port Availability Check:**
```bash
# Verify ports 6080 and 8080 are available
netstat -an | grep :6080
netstat -an | grep :8080

# If ports are in use, you'll need to stop those services or use different ports
```

### 2. Docker Image Pre-Download

**Download Required Images (Do this in advance - can take 10+ minutes):**
```bash
# Pre-pull the browser container image
docker pull dorowu/ubuntu-desktop-lxde-vnc

# Verify image is downloaded
docker images | grep ubuntu-desktop-lxde-vnc
```

### 3. Test Run - Complete Setup

**Run Full Test Setup:**
```bash
# Start the container
docker run -d \
  --name mcp-browser-test \
  -p 6080:6080 \
  -p 8080:8080 \
  -e VNC_PASSWORD=demo123 \
  --memory=2g \
  --cpus=2 \
  dorowu/ubuntu-desktop-lxde-vnc

# Wait 30-60 seconds for container to fully start
sleep 60

# Check container is running
docker ps | grep mcp-browser-test
```

### 4. Browser Interface Test

**Test NoVNC Access:**
1. Open browser to: `http://localhost:6080/vnc.html`
2. Enter password: `demo123`
3. You should see Ubuntu desktop
4. Open Firefox browser inside the desktop
5. Navigate to a test website (e.g., google.com)
6. Confirm browser works properly

### 5. Claude Desktop Configuration

**Create MCP Configuration:**
1. Open Claude Desktop
2. Go to Settings → Developer/MCP Servers
3. Add this configuration:

```json
{
  "mcpServers": {
    "browser": {
      "command": "docker",
      "args": ["exec", "-i", "mcp-browser-test", "mcp-server"],
      "env": {
        "DISPLAY": ":1"
      }
    }
  }
}
```

4. Restart Claude Desktop
5. Verify MCP server shows as "Connected" in settings

### 6. End-to-End Test

**Test Claude Browser Control:**

Send this message to Claude:
```
"Can you open a web browser and navigate to example.com, then tell me what you see?"
```

**Expected Results:**
- Claude should acknowledge it can use browser tools
- Browser in NoVNC should open and navigate to example.com
- Claude should describe the webpage content

### 7. Performance Optimization

**For Smoother Demo Performance:**
```bash
# Stop test container and recreate with optimal settings
docker stop mcp-browser-test
docker rm mcp-browser-test

# Run with performance optimizations
docker run -d \
  --name mcp-browser-demo \
  -p 6080:6080 \
  -p 8080:8080 \
  -e VNC_PASSWORD=demo123 \
  -e RESOLUTION=1280x720 \
  -e DEPTH=16 \
  --memory=3g \
  --cpus=2 \
  --shm-size=1g \
  dorowu/ubuntu-desktop-lxde-vnc
```

### 8. Demo Content Preparation

**Prepare Demo Scenarios:**
- [ ] Test basic navigation (Wikipedia)
- [ ] Test search functionality (Google)
- [ ] Test form interactions
- [ ] Test error handling (ask for non-existent elements)
- [ ] Prepare backup scenarios if network is slow

**Bookmark Useful Test Sites:**
- wikipedia.org (good for text content)
- github.com (shows complex layouts)
- google.com (tests search forms)
- example.com (simple, loads fast)

### 9. Presentation Setup

**Screen Layout:**
- [ ] Claude Desktop on primary screen
- [ ] NoVNC browser window on secondary screen (or side-by-side)
- [ ] Terminal window ready for troubleshooting commands
- [ ] Workshop slides ready

**Test Presentation Flow:**
1. Show MCP server connection in Claude Desktop
2. Send test command to Claude
3. Switch to browser view to show action
4. Return to Claude to see response
5. Practice smooth transitions between screens

### 10. Backup Plans

**If Docker Issues Occur:**
- Have alternative MCP server examples ready (file system, calculator)
- Prepare screenshots of working demo
- Have manual browser automation as backup explanation

**If Network Issues Occur:**
- Use offline websites or local HTML files
- Focus on MCP concept explanation rather than live demo
- Have video recording of working demo as backup

### 11. Workshop Day Checklist

**30 Minutes Before Workshop:**
```bash
# Start containers fresh
docker stop mcp-browser-demo 2>/dev/null || true
docker rm mcp-browser-demo 2>/dev/null || true

docker run -d \
  --name mcp-browser-demo \
  -p 6080:6080 \
  -p 8080:8080 \
  -e VNC_PASSWORD=demo123 \
  -e RESOLUTION=1280x720 \
  --memory=3g \
  --cpus=2 \
  dorowu/ubuntu-desktop-lxde-vnv

# Wait for startup
sleep 90

# Test browser access
curl -s http://localhost:6080 > /dev/null && echo "NoVNC accessible" || echo "NoVNC failed"
```

**Quick Function Test:**
- [ ] NoVNC loads at localhost:6080
- [ ] Claude Desktop shows MCP server connected
- [ ] Test command works: "Navigate to example.com"
- [ ] Browser view shows the navigation happening

### 12. Troubleshooting Quick Reference

**Common Issues & Fixes:**

| Issue | Quick Fix |
|-------|-----------|
| NoVNC won't load | `docker restart mcp-browser-demo` |
| Claude can't connect | Restart Claude Desktop |
| Browser is slow | Reduce resolution: `RESOLUTION=1024x768` |
| Port conflicts | Use different ports: `-p 6081:6080` |
| Container won't start | Check Docker has enough memory allocated |

### 13. Clean Up After Testing

```bash
# Stop and remove test containers
docker stop mcp-browser-demo mcp-browser-test 2>/dev/null || true
docker rm mcp-browser-demo mcp-browser-test 2>/dev/null || true

# Keep image for workshop day
# docker rmi dorowu/ubuntu-desktop-lxde-vnc  # Only if you want to re-download
```

## Final Pre-Workshop Verification

- [ ] All components work together smoothly
- [ ] Demo scenarios tested and timed
- [ ] Backup plans prepared
- [ ] Screen sharing/projection tested
- [ ] Workshop slides integrated with demo timing
- [ ] Troubleshooting commands bookmarked

**Recommended Timeline:**
- Complete setup: 2-3 days before workshop
- Final test run: Day before workshop
- Fresh container start: 30 minutes before workshop

Good luck with your MCP workshop! This preparation should ensure a smooth, impressive demonstration.
