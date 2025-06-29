# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

WSLSnapIt MCP is a Model Context Protocol (MCP) server that provides advanced screenshot capture and clipboard reading capabilities for Windows applications from within WSL environments. The server enables AI assistants to capture screens, windows, and monitors with intelligent image processing and optimization, as well as read text and images from the Windows clipboard.

## Development Commands

### Package Management
```bash
# Install dependencies
bun install

# Start the MCP server
bun start
# or
node index.js
```

## Architecture

### Core Structure
- **index.js**: Main MCP server implementation (v2.1.0) - contains all screenshot logic, clipboard reading, image processing, and MCP protocol handling
- **Runtime**: Node.js with ES modules, designed for WSL environments
- **Communication**: stdio-based MCP server using JSON-RPC protocol

### Key Dependencies
- `@modelcontextprotocol/sdk@^0.6.0` - MCP framework for protocol implementation
- `sharp@^0.34.2` - Image processing and compression engine
- PowerShell integration for Windows API calls from WSL

### Screenshot Capabilities
The tool provides three main capture modes:
1. **Monitor capture**: all monitors, primary only, or specific monitor by number
2. **Window capture**: by title (partial matching) or process name
3. **Output modes**: direct return to Claude (optimized JPEG) or file system (PNG)

### Clipboard Capabilities
The clipboard tool provides:
1. **Auto-detection**: Automatically detects whether clipboard contains text or image
2. **Text reading**: Retrieves text content from Windows clipboard
3. **Image reading**: Captures images from clipboard with automatic compression
4. **Format selection**: Can force specific format (text/image) or use auto mode

### Image Processing Pipeline
- Automatic optimization using Sharp library with mozjpeg
- Dynamic quality adjustment to stay under 1MB limit
- Progressive resizing strategy: 1920px → 1280px → 800px if needed
- DPI-aware scaling for multi-monitor setups

### WSL/Windows Integration
- Cross-platform path handling between WSL and Windows
- PowerShell script execution from WSL environment
- Windows API integration for window enumeration and precise positioning
- Base64 encoding for binary data transfer over MCP protocol

## MCP Tool Interface

The server exposes two primary tools:
- `take_screenshot` - Comprehensive screenshot capture with parameters for monitor selection, window targeting, quality control, and output format
- `read_clipboard` - Read Windows clipboard content (text or images) with automatic format detection and image compression

## Development Notes

### Error Handling Patterns
- Custom error messages for window-not-found scenarios
- Multi-window disambiguation with user guidance
- Robust process enumeration with safety checks

### Performance Considerations
- Efficient image processing pipeline to handle large screenshots
- Memory-conscious handling of high-resolution captures
- Optimized PowerShell execution for Windows API calls

### Version History
- v2.1.0: Added clipboard reading functionality with text and image support
- v2.0.0: Enhanced image compression and window detection
- v1.6: Initial implementation with basic screenshot capabilities