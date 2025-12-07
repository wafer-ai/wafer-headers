# Wafer Headers

Pre-extracted C++/CUDA/CUTLASS headers for the Wafer VS Code extension's IntelliSense support.

## Contents

This repository hosts header archives that the Wafer extension downloads to provide IntelliSense (autocomplete, error checking, go-to-definition) for GPU kernel development.

## Available Packages

| ID                    | Name                        | Size   | Contents                                                                               |
| --------------------- | --------------------------- | ------ | -------------------------------------------------------------------------------------- |
| `cuda-13-cutlass-4.3` | CUDA 13.0.2 + CUTLASS 4.3.2 | ~30 MB | CUDA 13.0.2 headers, CUTLASS 4.3.2 headers (from GitHub), C++ standard library headers |

## How It Works

1. User enables "IntelliSense Headers" in Wafer extension settings (requires Beta Mode)
2. Extension downloads `manifest.json` to discover available packages
3. Extension downloads and extracts the selected header archive (~30 MB compressed, ~100 MB extracted)
4. Extension configures `.vscode/c_cpp_properties.json` with include paths
5. Full IntelliSense works locally without network requests

## Manifest Format

```json
{
	"version": 1,
	"images": {
		"cuda-13-cutlass-4.3": {
			"name": "CUDA 13.0.2 + CUTLASS 4.3.2",
			"docker_image": "wafer/nvidia-mega-dev:latest",
			"archive_url": "https://github.com/wafer-ai/wafer-headers/releases/download/headers-v2/cuda-13-cutlass-4.3.tar.gz",
			"size_bytes": 31457280,
			"sha256": "...",
			"include_paths": ["cuda/include", "cutlass/include", "cpp/include"]
		}
	}
}
```

## Regenerating Headers

To regenerate headers from the mega-dev Docker image:

```bash
cd /path/to/wafer/services/wafer-api

# Build the mega-dev image first
docker build -f Dockerfile.mega-dev -t wafer/nvidia-mega-dev:latest .

# Extract headers
cd /path/to/wafer
python scripts/extract-headers.py \
  --image wafer/nvidia-mega-dev:latest \
  --id cuda-13-cutlass-4.3 \
  --type mega \
  --name "CUDA 13.0.2 + CUTLASS 4.3.2" \
  --cdn-base-url "https://github.com/wafer-ai/wafer-headers/releases/download/headers-v2" \
  --output ./output
```

## License

Headers are extracted from:

-   NVIDIA CUDA Toolkit 13.0.2 (NVIDIA License)
-   CUTLASS 4.3.2 from GitHub (BSD-3-Clause License)
-   C++ Standard Library (varies by distribution)
