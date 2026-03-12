# ACE-Step 1.5 Music Generation

![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)
![GPU: NVIDIA](https://img.shields.io/badge/GPU-NVIDIA-red)
![Model: Music Generation](https://img.shields.io/badge/Model-Music%20Generation-purple)

Self-hosted AI music generation powered by ACE-Step 1.5, deployed via Docker with NVIDIA GPU acceleration.

## Overview

ACE-Step is an advanced AI music generation model that creates high-quality audio from text prompts. This repository provides a ready-to-deploy Docker setup for running ACE-Step 1.5 locally with full GPU support.

**Live Demo:** [ace.liteit.se](https://ace.liteit.se)

## Features

- **High-Quality Music Generation** - State-of-the-art AI model for creating professional-grade music
- **Text-to-Audio** - Generate music from descriptive text prompts
- **API Support** - Full REST API for programmatic access
- **Web Interface** - Built-in Gradio server for easy interaction
- **GPU Accelerated** - Optimized for NVIDIA GPUs with CUDA 12.8
- **Persistent Storage** - Models and outputs stored in Docker volumes

## Quick Start

### Prerequisites

- Docker & Docker Compose
- NVIDIA GPU with CUDA support
- At least 16GB RAM (24GB shared memory recommended)

### Deployment

1. **Start the service:**
   ```bash
   docker compose up -d
   ```

2. **Access the web interface:**
   Open http://localhost:7861 in your browser

3. **API Access:**
   ```bash
   curl -X POST http://localhost:7861/api \
     -H "Content-Type: application/json" \
     -H "X-API-Key: aceace" \
     -d '{"prompt": "upbeat electronic dance music"}'
   ```

## Configuration

### Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `HF_HOME` | Path to Hugging Face models cache | `/app/models` |
| `HF_HUB_ENABLE_HF_TRANSFER` | Enable fast Hugging Face downloads | `1` |
| `ACESTEP_API_KEY` | API key for authentication | `aceace` |
| `UV_HTTP_TIMEOUT` | Package download timeout (seconds) | `600` |

### GPU Configuration

The container is configured to use GPU device ID `1`. Modify the `device_ids` in `docker-compose.yml` if your GPU has a different ID:

```yaml
device_ids: ["0"]  # Change to your GPU ID
```

To find your GPU ID:
```bash
nvidia-smi
```

## Volume Mounts

| Volume | Purpose |
|--------|---------|
| `ace-step-models` | Persistent storage for downloaded models (~10GB) |
| `ace-step-output` | Generated audio outputs |

## Technical Details

### Stack

- **Base Image:** NVIDIA CUDA 12.8.0 Runtime (Ubuntu 22.04)
- **Package Manager:** UV (Python package installer)
- **Python:** 3.11
- **Framework:** ACE-Step 1.5
- **Interface:** Gradio
- **API:** FastAPI

### Resources

- **Shared Memory:** 24GB (required for model loading)
- **GPU:** NVIDIA with CUDA support
- **Storage:** ~15GB for models and outputs

### Optimization Settings

- Concurrent downloads: 2
- HTTP timeout: 600 seconds (for large packages)
- Unsafe best-match index strategy for faster resolution

## Troubleshooting

### GPU Not Detected

Ensure NVIDIA Container Toolkit is installed:
```bash
sudo docker run --gpus all nvidia/cuda:12.8.0-base ubuntu nvidia-smi
```

### Out of Memory

Increase shared memory or reduce batch sizes:
```yaml
shm_size: '32gb'  # Increase if you have more RAM
```

### Slow Model Downloads

Enable HF transfer for faster downloads:
```yaml
environment:
  - HF_HUB_ENABLE_HF_TRANSFER=1
```

## License

MIT License - See LICENSE file for details.

## Credits

- **ACE-Step Team** - Original model development: [ace-step/ACE-Step-1.5](https://github.com/ace-step/ACE-Step-1.5)
- **Deployment Setup** - Magnus Froste

---

Built with ❤️ for the AI music generation community.
