# WSLSnapit-MCP 📸

WSLSnapit-MCP is a powerful Model Context Protocol (MCP) server that provides advanced screenshot capture and clipboard reading capabilities for Windows applications from within WSL environments. It enables AI assistants to capture screens, windows, and monitors with intelligent image processing and optimization, as well as read text and images from the Windows clipboard.

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![MCP](https://img.shields.io/badge/MCP-2.0-green.svg)
![Node](https://img.shields.io/badge/node-%3E%3D18.0.0-brightgreen.svg)

## ✨ Features

### 📸 Screenshot Capture
- **Monitor Selection**: Capture all monitors, primary monitor, or specific monitor by number
- **Window Targeting**: Capture specific windows by title (partial match) or process name
- **Smart Compression**: Automatic image optimization to stay under 1MB with progressive quality adjustment
- **Direct Return**: Images can be returned directly to AI assistants or saved to disk
- **DPI-Aware**: Handles multi-monitor setups with different scaling factors correctly

### 📋 Clipboard Reading
- **Auto-Detection**: Automatically detects whether clipboard contains text or image
- **Text Support**: Read text content from Windows clipboard
- **Image Support**: Capture images from clipboard with automatic compression
- **Format Control**: Force specific format (text/image) or use auto mode

### 🚀 Performance
- **Efficient Processing**: Memory-conscious handling of high-resolution captures
- **Progressive Compression**: Dynamic quality adjustment (1920px → 1280px → 800px if needed)
- **Optimized Communication**: Base64 encoding for reliable binary data transfer

## 📦 Installation

### Prerequisites
- Windows 10/11 with WSL2 installed
- Node.js 18.0.0 or higher
- npm, yarn, or bun package manager
- PowerShell (comes with Windows)

### Quick Install

```bash
# Clone the repository
git clone https://github.com/yourusername/WSLSnapit-MCP.git
cd WSLSnapit-MCP

# Install dependencies
npm install
# or
yarn install
# or
bun install
```

### Claude Desktop Configuration

Add the following to your Claude Desktop configuration file:

**Windows**: `%APPDATA%\Claude\claude_desktop_config.json`
**macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`
**Linux**: `~/.config/Claude/claude_desktop_config.json`

```json
{
  "mcpServers": {
    "wslsnapit": {
      "command": "wsl",
      "args": [
        "--cd",
        "/mnt/h/dev/mcp/wslsnapit-mcp",
        "node",
        "index.js"
      ]
    }
  }
}
```

**Note**: Adjust the path `/mnt/h/dev/mcp/wslsnapit-mcp` to match your installation location.

## 🎯 Usage

### Screenshot Capture

The `take_screenshot` tool supports various capture modes:

#### Basic Usage
```javascript
// Capture all monitors (default)
take_screenshot()

// Capture primary monitor only
take_screenshot({ monitor: "primary" })

// Capture specific monitor (1, 2, 3, etc.)
take_screenshot({ monitor: 2 })
```

#### Window Capture
```javascript
// Capture window by title (partial match)
take_screenshot({ windowTitle: "Visual Studio" })

// Capture window by process name
take_screenshot({ processName: "notepad" })

// Handle multiple matching windows
take_screenshot({ windowTitle: "Chrome", windowIndex: 2 })
```

#### Output Options
```javascript
// Return image directly (default)
take_screenshot({ returnDirect: true })

// Save to file
take_screenshot({ 
  returnDirect: false, 
  filename: "myshot.png",
  folder: "/home/user/screenshots" 
})

// Control JPEG quality (for direct return)
take_screenshot({ 
  returnDirect: true, 
  quality: 90 
})
```

### Clipboard Reading

The `read_clipboard` tool supports both text and image content:

```javascript
// Auto-detect clipboard content (default)
read_clipboard()

// Force text reading
read_clipboard({ format: "text" })

// Force image reading
read_clipboard({ format: "image" })
```

## 🔧 Parameters Reference

### take_screenshot

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `filename` | string | "screenshot.png" | Filename when saving to disk |
| `monitor` | string\|number | "all" | Monitor selection: "all", "primary", or monitor number |
| `windowTitle` | string | - | Capture window by title (partial match) |
| `windowIndex` | number | 1 | Which window to capture when multiple match |
| `processName` | string | - | Capture window by process name |
| `folder` | string | - | Custom folder path (supports WSL and Windows paths) |
| `returnDirect` | boolean | true | Return image to AI or save to disk |
| `quality` | number | 80 | JPEG quality (1-100) for direct return |

### read_clipboard

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `format` | string | "auto" | Format: "auto", "text", or "image" |

## 🛡️ Security Considerations

WSLSnapit-MCP is designed with security in mind:

- **Minimal Permissions**: Windows executable runs with minimal permissions, only accessing screen capture and clipboard APIs
- **Local Processing**: All clipboard data is processed locally without any network transmission
- **Process Isolation**: PowerShell runs as a separate Windows process, preventing access to sensitive WSL environment
- **Input Validation**: All user inputs are properly escaped to prevent injection attacks
- **Resource Limits**: Memory limits and progressive compression prevent resource exhaustion

## 🏗️ Architecture

### Core Components
- **MCP Server**: Implements the Model Context Protocol for AI assistant integration
- **PowerShell Bridge**: Executes Windows-specific operations from WSL
- **Image Processor**: Uses Sharp library for efficient image compression
- **Path Handler**: Seamlessly converts between WSL and Windows paths

### Technology Stack
- Node.js with ES modules
- @modelcontextprotocol/sdk for MCP implementation
- Sharp for image processing
- PowerShell for Windows API integration

## 🐛 Troubleshooting

### Common Issues

1. **"No windows found" error**
   - Ensure the target window is visible and not minimized
   - Try using a shorter or different part of the window title
   - Use process name instead of window title

2. **Large screenshots fail to return**
   - The tool automatically compresses images to stay under 1MB
   - If issues persist, try lowering the quality parameter

3. **Clipboard reading returns empty**
   - Ensure content is properly copied to clipboard
   - Try specifying the format explicitly instead of using auto-detect

4. **Path not found errors**
   - Verify the folder path exists
   - Remember WSL paths start with `/mnt/c/` for C: drive

### Debug Mode

To run with debug output:
```bash
DEBUG=* node index.js
```

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request. For major changes, please open an issue first to discuss what you would like to change.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- Built for use with [Claude Desktop](https://claude.ai/download) and the [Model Context Protocol](https://modelcontextprotocol.io)
- Image processing powered by [Sharp](https://sharp.pixelplumbing.com/)
- Thanks to the MCP community for inspiration and support

## 📞 Support

- 🐛 Report bugs by [opening an issue](https://github.com/yourusername/WSLSnapit-MCP/issues)
- 💡 Request features through [GitHub discussions](https://github.com/yourusername/WSLSnapit-MCP/discussions)
- 📧 Contact: your-email@example.com

---

Made with ❤️ for the WSL and MCP community