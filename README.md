# MCP Workshop Demo: Claude Desktop + Docker Browser Toolkit

This guide shows how to set up a live demonstration of Model Context Protocol (MCP) using Claude Desktop connected to a Docker-based browser that you can view in real-time through your web browser.

## Prerequisites

- Docker Desktop installed and running
- Claude Desktop app installed  
- Web browser for viewing the demo
- Basic familiarity with terminal/command line

## Quick Setup

### Step 1: Run the MCP Browser Container

```bash
# Pull and run the browser container with NoVNC web interface
docker run -d \
  --name mcp-browser-demo \
  -p 6080:6080 \
  -p 8080:8080 \
  -e VNC_PASSWORD=demo123 \
  dorowu/ubuntu-desktop-lxde-vnc
```

### Step 2: Verify Container is Running

```bash
# Check if container is running
docker ps

# Should show mcp-browser-demo container with ports 6080 and 8080
```

### Step 3: Access Browser View

Open your web browser and navigate to:
```
http://localhost:6080/vnc.html
```

- **Password:** `demo123`
- You should see a desktop environment with a web browser
- This is what Claude will be controlling during the demo

### Step 4: Configure Claude Desktop

1. Open Claude Desktop
2. Click the gear icon (Settings)
3. Navigate to "Developer" or "MCP Servers" section
4. Add the following configuration:

```json
{
  "mcpServers": {
    "browser": {
      "command": "docker",
      "args": ["exec", "-i", "mcp-browser-demo", "mcp-server"],
      "env": {
        "DISPLAY": ":1"
      }
    }
  }
}
```

### Step 5: Restart Claude Desktop

Close and reopen Claude Desktop to load the new MCP server configuration.

## Demo Script Ideas

### Basic Navigation Test
```
"Can you open a web browser and navigate to wikipedia.org?"
```

### Search and Summarize
```
"Please go to wikipedia.org, search for 'Model Context Protocol', and summarize what you find"
```

### Multi-step Workflow
```
"Navigate to github.com, search for 'MCP servers', and tell me about the most popular repository"
```

### Form Interaction
```
"Go to google.com, search for 'Anthropic Claude', and describe the top 3 search results"
```

## Presentation Tips

1. **Dual Screen Setup:** Show both Claude Desktop and the browser view side-by-side
2. **Narrate Actions:** Explain what Claude is doing as it happens
3. **Show Tool Discovery:** Demonstrate how Claude automatically discovers available browser tools
4. **Error Handling:** Try asking Claude to interact with non-existent elements to show graceful error handling
5. **Real-time Visual:** The audience can see exactly what Claude sees and does

## Troubleshooting

### Container Won't Start
```bash
# Check if ports are already in use
netstat -an | grep 6080
netstat -an | grep 8080

# Stop and remove existing container if needed
docker stop mcp-browser-demo
docker rm mcp-browser-demo
```

### Can't Access Browser View
- Ensure Docker container is running: `docker ps`
- Try accessing `http://127.0.0.1:6080/vnc.html` instead
- Check firewall settings for localhost connections
- Verify port 6080 isn't blocked

### Claude Can't Connect to MCP Server
```bash
# Check container logs
docker logs mcp-browser-demo

# Restart Claude Desktop after configuration changes
# Verify MCP server status in Claude Desktop settings (should show green/connected)
```

### Browser Inside Container is Slow
```bash
# Allocate more resources to Docker container
docker stop mcp-browser-demo
docker rm mcp-browser-demo

# Run with more memory and CPU
docker run -d \
  --name mcp-browser-demo \
  -p 6080:6080 \
  -p 8080:8080 \
  -e VNC_PASSWORD=demo123 \
  --memory=2g \
  --cpus=2 \
  dorowu/ubuntu-desktop-lxde-vnc
```

## Cleanup After Demo

```bash
# Stop and remove the container
docker stop mcp-browser-demo
docker rm mcp-browser-demo

# Remove the image if no longer needed
docker rmi dorowu/ubuntu-desktop-lxde-vnv
```

## Workshop Follow-up

After the demo, participants can:
- Explore other MCP servers from the community
- Build their own simple MCP server
- Connect multiple MCP servers to Claude Desktop
- Try different tools like file system access, calculators, or API integrations

## Security Notes for Corporate Environments

- This setup is for **demonstration purposes only**
- Container has internet access - consider network restrictions for production
- Use stronger passwords in real implementations
- Consider using HTTPS for production deployments
- Review your company's Docker and container policies

## Additional Resources

- [MCP Official Documentation](https://modelcontextprotocol.io)
- [Anthropic MCP Repository](https://github.com/anthropics/mcp)
- [Community MCP Servers](https://github.com/modelcontextprotocol/servers)
- [Claude Desktop Configuration Guide](https://docs.claude.com)
