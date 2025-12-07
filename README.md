# Wafer Headers

Pre-extracted C++/CUDA/CUTLASS headers for the Wafer VS Code extension's IntelliSense support.

## Contents

This repository hosts header archives that the Wafer extension downloads to provide IntelliSense (autocomplete, error checking, go-to-definition) for GPU kernel development.

## Available Packages

| ID | Name | Size | Contents |
|----|------|------|----------|
| `cutlass-3.6` | CUTLASS 3.6 + CUDA 12.8 | ~3.3 MB | CUDA 12.8 headers, CUTLASS 3.6 headers, CuTe headers |

## How It Works

1. User enables "IntelliSense Headers" in Wafer extension settings
2. Extension downloads `manifest.json` to discover available packages
3. Extension downloads and extracts the selected header archive (~3 MB)
4. Extension configures `.vscode/c_cpp_properties.json` with include paths
5. Full IntelliSense works locally without network requests

## Manifest Format

```json
{
  "version": 1,
  "images": {
    "cutlass-3.6": {
      "name": "CUTLASS 3.6 + CUDA 12.8",
      "archive_url": "https://github.com/wafer-ai/wafer-headers/releases/download/headers-v1/cutlass-3.6.tar.gz",
      "size_bytes": 3434070,
      "sha256": "...",
      "include_paths": ["cuda", "cpp/include", "cutlass/include", "cute/include"]
    }
  }
}
```

## Regenerating Headers

To regenerate headers from Docker images:

```bash
cd /path/to/wafer
python scripts/extract-headers.py \
  --image nvidia/cuda:12.8.0-cudnn-devel-ubuntu22.04 \
  --id cuda-12.8 \
  --output ./output
```

## License

Headers are extracted from:
- NVIDIA CUDA Toolkit (NVIDIA License)
- CUTLASS (BSD-3-Clause License)
