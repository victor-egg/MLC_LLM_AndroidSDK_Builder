# MLC-LLM Android SDK Builder

Language: [`中文`](README.md) [`English`](README_EN.md)

## Overview

This repository features an automated GitHub Actions workflow designed to build the MLC-LLM Android SDK. By simply specifying the desired model parameters, the system automatically handles the compilation and build process.

### 🚀 Quick Start

1.  Navigate to the **Actions** tab in this GitHub repository.
2.  Select the **Builder** workflow from the sidebar.
3.  Click the **Run workflow** button.
4.  Fill in the required parameters:
    *   **Release Tag**: The tag for the release, e.g., `Qwen3-1.7B-q4f16_1`.
    *   **Release Name**: The title of the release, e.g., `Release`.
    *   **HuggingFace Repo**: The source repository path, e.g., `mlc-ai/Qwen3-1.7B-q4f16_1-MLC`.
    *   **Model ID**: The specific model folder name, e.g., `Qwen3-1.7B-q4f16_1-MLC`.
    *   **Required vRAM**: Estimated vRAM (in bytes) required to run the model on the device. Default is `3000000000` (approx. 3GB).
5.  Click **Run workflow** to start the process.
6.  Once the build finishes, go to the **Releases** page to download the generated Android SDK.

### Model Selection Guide

#### Recommended Model Sources
It is highly recommended to use official quantized models from MLC-LLM for the best compatibility and performance optimization:
[`https://huggingface.co/mlc-ai`](https://huggingface.co/mlc-ai)

#### Understanding Model Naming Conventions
Taking [`mlc-ai/Qwen3-1.7B-q4f16_1-MLC`](https://huggingface.co/mlc-ai/Qwen3-1.7B-q4f16_1-MLC) as an example:

##### 1. `Qwen3` (Model Family)
*   **Definition**: Refers to the 3rd generation of the **Qwen (Tongyi Qianwen)** series (or a specific user-defined version; while Qwen2.5 is current, Qwen3 denotes a newer or demo iteration).
*   **Source**: Developed by Alibaba Cloud.

##### 2. `1.7B` (Parameter Count)
*   **Definition**: **1.7 Billion** parameters.
*   **Significance**:
    *   This is categorized as a **Small Language Model (SLM)**.
    *   Compared to 7B or 72B models, 1.7B is extremely lightweight.
    *   **Use Case**: Ideal for local execution on **Android** or **iOS devices** due to low memory footprint and fast inference speeds.

##### 3. `q4f16_1` (Quantization Format - Core)
This is the specific quantization naming convention used by MLC-LLM, determining the model's size and execution efficiency on mobile GPUs.

*   **`q4` (Quantization 4-bit)**:
    *   **4-bit Quantization**. Original models are usually `f16` (16-bit).
    *   Compressing weights to 4-bit reduces the model size by approximately **75%**.
    *   *Example: A 1.7B model originally requiring ~3.4GB vRAM will only need just over 1GB, making it easy to run on mobile.*

*   **`f16` (Float 16)**:
    *   Indicates that computations (Activation/Scale) are performed using **Float16** precision.
    *   This ensures that while weights are compressed, the calculation process retains high precision to prevent quality degradation. This format is natively optimized for mobile GPUs (e.g., Adreno, Mali).

*   **`_1` (Variant/Group Size)**:
    *   Internal version identifier for MLC, typically referring to the **Group Quantization** strategy.
    *   **`_1`** usually represents a **Group Size of 32** (every 32 parameters share one scaling factor).
    *   **Comparison**:
        *   `q4f16_0`: Usually Group Size 128 or non-grouped. Smallest size, but slightly lower accuracy.
        *   `q4f16_1`: **Higher accuracy (Lower Perplexity)**. Although the file size is marginally larger than `_0`, **it is strongly recommended for small models (like 1.7B) to ensure response quality.**

### Integrating SDK into Your Project

For detailed integration instructions, please refer to the official documentation:
[`https://llm.mlc.ai/docs/deploy/android.html`](https://llm.mlc.ai/docs/deploy/android.html)

### Build Environment Details

| Component | Version | Description |
| :--- | :--- | :--- |
| **OS** | Ubuntu Linux (Latest LTS) | Stable build environment |
| **Python** | 3.13 | Model conversion and script execution |
| **Android NDK** | 27.3.13750724 | Native code compilation |
| **Java** | 21 (JetBrains Runtime) | Android build toolchain |
| **Rust** | Latest Stable | High-performance runtime components |

For more configuration details, check [`.github/workflows/Builder.yml`](.github/workflows/Builder.yml).