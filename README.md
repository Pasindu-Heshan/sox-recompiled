
# SoX for M3/M4 Macs: Portable ARM64 Binary

This repository provides a portable, ARM64-compatible SoX 14.4.2 binary tailored for macOS M3 and M4 systems. It’s designed to work seamlessly with applications like `node-record-lpcm16` for audio processing, resolving compatibility issues (e.g., "spawn Unknown system error -86") on Apple Silicon.

## Purpose

The standard SoX binary from SourceForge is compiled for Intel (x86_64), which fails on M3/M4 Macs. This prebuilt binary is optimized for ARM64 and packaged in a portable `sox-dist` directory, making it easy to bundle with your application for distribution across macOS systems.

## Prerequisites

- **macOS**: Ventura or later (tested on M3/M4 Macs).
- **Node.js**: Required if using with `node-record-lpcm16`.

## Installation

The binary is provided in the `sox-dist` directory. No compilation is needed—just use it directly.

1. **Clone the Repository**:
   ```bash
   git clone <your-repo-url>
   cd <your-repo-name>
   ```

2. **Verify the Binary**:
   ```bash
   ./sox-dist/bin/sox --version  # Should output "sox: SoX v14.4.2"
   file ./sox-dist/bin/sox       # Should show "Mach-O 64-bit executable arm64"
   ```
   The `sox-dist/bin` directory contains `sox`, `play`, and `rec` (symlinks to `sox`).

## Usage

Bundle the `sox-dist` directory with your application. Update your app to point to the binary. Example for a Node.js app using `node-record-lpcm16`:

```javascript
const path = require("path");
const fs = require("fs");

const getSoxPath = () => {
  const possiblePaths = [
    path.resolve(__dirname, "../../sox-dist/bin/sox"), // Bundled
    "/opt/homebrew/bin/sox",                          // Homebrew
    "/usr/local/bin/sox",                             // System
  ];
  for (const fullPath of possiblePaths) {
    if (fs.existsSync(fullPath)) {
      console.log(`Using SoX at: ${fullPath}`);
      return fullPath;
    }
  }
  throw new Error("SoX not found");
};
```

This ensures your app uses the portable binary when available, falling back to system-installed versions if needed.

## Why This Binary?

- **M3/M4 Compatibility**: Built specifically for ARM64, ensuring no "Bad CPU type" errors.
- **Portability**: The `sox-dist` directory is self-contained, with relative library paths (no external dependencies), making it ideal for app distribution.
- **Tested**: Works with `node-record-lpcm16` for audio recording in transcription apps.

## Alternative: Homebrew

If you don’t need a portable binary, install SoX via Homebrew:
```bash
brew install sox
```
This provides an ARM64 binary at `/opt/homebrew/bin/sox` but isn’t portable.

## Contributing

Found issues or improvements? Open an issue or PR! Contributions are welcome.

## License

This repository distributes a custom-built SoX 14.4.2 binary, licensed under the [GNU GPL](https://www.gnu.org/licenses/gpl-3.0.html) and [LGPL](https://www.gnu.org/licenses/lgpl-2.1.html) per SoX’s terms. See `LICENSE.GPL.txt` and `LICENSE.LGPL.txt` in the SoX source for details.
