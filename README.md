# aria2 Auto Build

Automatically build [aria2](https://github.com/aria2/aria2) with custom patches applied.

## What This Does

This repository contains a GitHub Actions workflow that:

1. **Checks daily** for new aria2 releases
2. **Downloads** the latest aria2 source code
3. **Applies** custom patches to modify default values
4. **Builds** static binaries for Linux (x86_64) and Windows (x86_64, i686)
5. **Releases** the patched binaries automatically

## Applied Patches

The following default values are modified from upstream aria2:

| Option | Default (Upstream) | Default (Patched) | Description |
|--------|-------------------|-------------------|-------------|
| `--continue` (`-c`) | `false` | `true` | Continue downloading partially downloaded files |
| `--max-concurrent-downloads` (`-j`) | `5` | `128` | Maximum concurrent downloads |
| `--max-connection-per-server` (`-x`) | `1` (max: 16) | `64` (no max) | Connections per server |
| `--min-split-size` (`-k`) | `20M` | `1M` | Minimum split size |
| `--connect-timeout` | `60` | `30` | Connection timeout in seconds |
| `--piece-length` | min: `1M` | min: `1K` | Minimum piece length reduced |
| `--retry-wait` | `0` | `2` | Seconds to wait between retries |
| `--split` (`-s`) | `5` | `128` | Number of connections for downloading |
| `--check-certificate` | `true` | `false` | Skip SSL certificate verification |

## Downloads

Pre-built binaries are available in the [Releases](../../releases) section.

### Available Builds

- **Linux x86_64**: `aria2c-linux-x86_64.tar.gz`
- **Linux arm64**: `aria2c-linux-arm64.tar.gz`
- **macOS arm64 (Apple Silicon)**: `aria2c-macos-arm64.tar.gz`
- **macOS x86_64 (Intel)**: `aria2c-macos-x86_64.tar.gz`
- **Windows x86_64**: `aria2c-windows-x86_64.zip`
- **Windows i686 (32-bit)**: `aria2c-windows-i686.zip`

## Manual Trigger

You can manually trigger a build from the Actions tab:

1. Go to **Actions** → **Build Patched aria2**
2. Click **Run workflow**
3. Optionally check "Force build even if version matches"
4. Click **Run workflow**

## Building Locally

To apply the patch and build aria2 locally:

```bash
# Clone aria2
git clone https://github.com/aria2/aria2.git
cd aria2

# Apply patch
patch -p1 < /path/to/patch.diff

# Build (Linux)
autoreconf -i
./configure
make -j$(nproc)
```

## License

The patches and workflow are provided as-is. aria2 is licensed under [GPL-2.0](https://github.com/aria2/aria2/blob/master/COPYING).

