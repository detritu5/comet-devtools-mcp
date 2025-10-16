# Comet DevTools MCP

[![npm comet-devtools-mcp package](https://img.shields.io/npm/v/comet-devtools-mcp.svg)](https://npmjs.org/package/comet-devtools-mcp)

`comet-devtools-mcp` lets your coding agent (such as Gemini, Claude, Cursor or Copilot) control and inspect a live Comet browser. It acts as a Model-Context-Protocol (MCP) server, giving your AI coding assistant access to the full power of Chrome DevTools for reliable automation, in-depth debugging, and performance analysis.

**Primary Compatibility**: This MCP server is primarily designed for **Comet Browser** (Perplexity's browser based on Chromium), but maintains compatibility with Chrome and other Chromium-based browsers through the Chrome DevTools Protocol.

## [Tool reference](./docs/tool-reference.md) | [Changelog](./CHANGELOG.md) | [Contributing](./CONTRIBUTING.md) | [Troubleshooting](./docs/troubleshooting.md)

## Key features

- **Get performance insights**: Uses [Chrome DevTools](https://github.com/ChromeDevTools/devtools-frontend) to record traces and extract actionable performance insights.
- **Advanced browser debugging**: Analyze network requests, take screenshots and check the browser console.
- **Reliable automation**: Uses [puppeteer](https://github.com/puppeteer/puppeteer) to automate actions in Comet/Chrome and automatically wait for action results.
- **Comet-optimized**: Specifically configured for optimal performance with Comet Browser.

## Disclaimers

`comet-devtools-mcp` exposes content of the browser instance to the MCP clients allowing them to inspect, debug, and modify any data in the browser or DevTools. Avoid sharing sensitive or personal information that you don't want to share with MCP clients.

## Requirements

- [Node.js](https://nodejs.org/) v20.19 or a newer [latest maintenance LTS](https://github.com/nodejs/Release#release-schedule) version.
- [Comet Browser](https://www.perplexity.ai/) (recommended) or [Chrome](https://www.google.com/chrome/) current stable version or newer.
- [npm](https://www.npmjs.com/).

## Getting started

Add the following config to your MCP client:

```json
{
  "mcpServers": {
    "comet-devtools": {
      "command": "npx",
      "args": ["-y", "comet-devtools-mcp@latest"]
    }
  }
}
```

> [!NOTE]
> If you are using [Claude Desktop](https://claude.ai/download), the config file is located at:
> - macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
> - Windows: `%APPDATA%\Claude\claude_desktop_config.json`
> - Linux: `~/.config/Claude/claude_desktop_config.json`

**Step 2: Launch Comet Browser with remote debugging enabled**

Start Comet Browser (or Chrome) with the remote debugging port enabled. Make sure to close any running browser instances before starting a new one with the debugging port enabled. The port number you choose must be the same as the one you specified in the `--browser-url` option in your MCP client configuration.

For security reasons, [Chrome/Comet requires you to use a non-default user data directory](https://developer.chrome.com/blog/remote-debugging-port) when enabling the remote debugging port. You can specify a custom directory using the `--user-data-dir` flag. This ensures that your regular browsing profile and data are not exposed to the debugging session.

### Launching Comet Browser

**macOS**
```bash
# Adjust the path if Comet is installed in a different location
/Applications/Comet.app/Contents/MacOS/Comet --remote-debugging-port=9222 --user-data-dir=/tmp/comet-profile-debug
```

**Linux**
```bash
# Adjust the path based on your Comet installation
/usr/bin/comet --remote-debugging-port=9222 --user-data-dir=/tmp/comet-profile-debug
```

**Windows**
```bash
# Adjust the path based on your Comet installation
"C:\Program Files\Comet\Application\comet.exe" --remote-debugging-port=9222 --user-data-dir="%TEMP%\comet-profile-debug"
```

### Alternative: Launching Chrome Browser

If you don't have Comet Browser installed, you can use Chrome:

**macOS**
```bash
/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --remote-debugging-port=9222 --user-data-dir=/tmp/chrome-profile-debug
```

**Linux**
```bash
/usr/bin/google-chrome --remote-debugging-port=9222 --user-data-dir=/tmp/chrome-profile-debug
```

**Windows**
```bash
"C:\Program Files\Google\Chrome\Application\chrome.exe" --remote-debugging-port=9222 --user-data-dir="%TEMP%\chrome-profile-debug"
```

**Step 3: Configure MCP to connect to Comet**

By default, `comet-devtools-mcp` will attempt to connect to `http://localhost:9222`. If you need to use a different port or host, you can specify it in your MCP client configuration:

```json
{
  "mcpServers": {
    "comet-devtools": {
      "command": "npx",
      "args": ["-y", "comet-devtools-mcp@latest", "--browser-url", "http://localhost:9222"]
    }
  }
}
```

**Step 4: Test your setup**

After configuring the MCP client and starting Comet Browser, you can test your setup by running a simple prompt in your MCP client:

```
Check the performance of https://developers.chrome.com
```

Your MCP client should connect to the running Comet instance and receive a performance report.

For more details on remote debugging, see the [Chrome DevTools documentation](https://developer.chrome.com/docs/devtools/remote-debugging/).

## Known limitations

### Operating system sandboxes

Some MCP clients allow sandboxing the MCP server using macOS Seatbelt or Linux containers. If sandboxes are enabled, `comet-devtools-mcp` is not able to start Comet/Chrome that requires permissions to create its own sandboxes. As a workaround, either disable sandboxing for `comet-devtools-mcp` in your MCP client or use `--browser-url` to connect to a Comet/Chrome instance that you start manually outside of the MCP client sandbox.

### Browser compatibility

While this MCP server is optimized for Comet Browser, it uses the standard Chrome DevTools Protocol and should work with any Chromium-based browser (Chrome, Edge, Brave, etc.) launched with the `--remote-debugging-port` flag.

## About this project

This project was originally created as `chrome-devtools-mcp` and has been rebranded to `comet-devtools-mcp` to reflect its primary focus on Comet Browser compatibility while maintaining full Chrome DevTools Protocol support.
