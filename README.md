# llamacpp-quantize

A streamlined tool for quantizing large language models using llama.cpp with automated GitHub Actions workflows. This repository provides a complete solution for converting Hugging Face models to quantized GGUF format and uploading them to Hugging Face Hub.

## Features

- **Automated Quantization**: Convert Hugging Face models to multiple GGUF quantization formats
- **GitHub Actions Integration**: Run quantization workflows on-demand via GitHub's infrastructure
- **Multiple Quantization Modes**: Support for 21 different quantization modes from IQ3_XS to Q8_0
- **Hugging Face Integration**: Direct upload to Hugging Face Hub after quantization
- **Docker-based**: Uses official llama.cpp Docker images for consistent environments

## Supported Quantization Formats

| Format | Description |
|--------|-------------|
| IQ3_M | Medium 3-bit integer quantization |
| IQ3_S | Small 3-bit integer quantization |
| IQ3_XS | Extra small 3-bit integer quantization |
| IQ4_NL | 4-bit integer quantization (no linear) |
| IQ4_XS | Extra small 4-bit integer quantization |
| Q2_K | 2-bit quantization (K-quants) |
| Q3_K | 3-bit quantization (K-quants) |
| Q3_K_L | Large 3-bit quantization (K-quants) |
| Q3_K_M | Medium 3-bit quantization (K-quants) |
| Q3_K_S | Small 3-bit quantization (K-quants) |
| Q4_0 | 4-bit quantization (type 0) |
| Q4_1 | 4-bit quantization (type 1) |
| Q4_K | 4-bit quantization (K-quants) |
| Q4_K_M | Medium 4-bit quantization (K-quants) |
| Q4_K_S | Small 4-bit quantization (K-quants) |
| Q5_0 | 5-bit quantization (type 0) |
| Q5_1 | 5-bit quantization (type 1) |
| Q5_K | 5-bit quantization (K-quants) |
| Q5_K_M | Medium 5-bit quantization (K-quants) |
| Q5_K_S | Small 5-bit quantization (K-quants) |
| Q6_K | 6-bit quantization (K-quants) |
| Q8_0 | 8-bit quantization (type 0) |

## Quick Start

### Prerequisites

- GitHub repository with this workflow
- Hugging Face account with write access to destination repositories
- Hugging Face API token configured as `HF_TOKEN` secret in GitHub repository

### Setup

1. **Fork or clone this repository**
   ```bash
   git clone https://github.com/yankeguo/llamacpp-quantize.git
   cd llamacpp-quantize
   ```

2. **Configure GitHub Secrets**
   - Go to your repository's Settings → Secrets and variables → Actions
   - Add `HF_TOKEN` with your Hugging Face API token

3. **Run Quantization**
   - Go to Actions tab in your repository
   - Select "Quantize Model" workflow
   - Click "Run workflow"
   - Fill in the required parameters:
     - **Model Name**: The name for your quantized models (e.g., "llama-3.1-8b")
     - **Source Repository**: Hugging Face repository URL (e.g., "meta-llama/Llama-3.1-8B")
     - **Destination Repository**: Your Hugging Face repository (e.g., "your-username/llama-3.1-8b-quantized")

## How It Works

1. **Model Download**: Downloads the source model from Hugging Face Hub
2. **Format Conversion**: Converts the model from Hugging Face format to GGUF F16 format
3. **Quantization**: Creates quantized versions in all supported formats
4. **Upload**: Uploads each quantized model to the specified destination repository
5. **Cleanup**: Removes temporary files to save space

## Manual Usage

You can also run the quantization process locally using Docker:

```bash
# Set environment variables
export MODEL_NAME="your-model-name"
export SRC_REPO="source/huggingface-repo"
export DST_REPO="destination/huggingface-repo"
export HF_TOKEN="your-hf-token"

# Run quantization
docker run --rm \
  -e MODEL_NAME="$MODEL_NAME" \
  -e SRC_REPO="$SRC_REPO" \
  -e DST_REPO="$DST_REPO" \
  -e HF_TOKEN="$HF_TOKEN" \
  -v "$(pwd)/run.sh:/app/run.sh" \
  ghcr.io/ggml-org/llama.cpp:full \
  ./run.sh
```

## File Structure

```
llamacpp-quantize/
├── .github/workflows/quantize.yml    # GitHub Actions workflow
├── run.sh                           # Main quantization script
├── README.md                        # This file
└── LICENSE                          # MIT License
```

## Environment Variables

| Variable | Description | Required |
|----------|-------------|----------|
| `MODEL_NAME` | Base name for output files | Yes |
| `SRC_REPO` | Source Hugging Face repository | Yes |
| `DST_REPO` | Destination Hugging Face repository | Yes |
| `HF_TOKEN` | Hugging Face API token | Yes |
| `HF_HUB_ENABLE_HF_TRANSFER` | Enable fast transfers (set to 1) | No |

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request. For major changes, please open an issue first to discuss what you would like to change.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- [llama.cpp](https://github.com/ggerganov/llama.cpp) for the quantization tools
- [Hugging Face](https://huggingface.co) for model hosting and hub services
