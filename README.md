# MLC-LLM Android SDK 构建

## 概述

本仓库已配置自动化的 GitHub Actions 工作流，填写需要构建的模型，自动构建Android SDK

### 快速开始

1. 在 GitHub 仓库中进入 **Actions** 标签页
2. 选择 **Builder** 工作流
3. 点击 **Run workflow** 按钮
4. 填写参数：
   - **发布标签** : 模型标签，例如 `Qwen3-1.7B-q4f16_1`
   - **发布名称** : 发布的名称，例如 `Release`
   - **HuggingFace 模型地址** : HuggingFace 仓库，例如 `mlc-ai/Qwen3-1.7B-q4f16_1-MLC`
   - **模型id** : HuggingFace 仓库名，例如 `Qwen3-1.7B-q4f16_1-MLC`
   - **运行模型所需的vRAM** : 端侧运行模型预估需要的vRAM，默认 `3000000000`(3GB)
5. 点击 **Run workflow** 开始执行
6. 构建完成后到 **Releases** 中下载 Android SDK

### MLC-LLM (Machine Learning Compilation) 模型命名格式

优先选择 MLC-LLM 官方量化模型,拥有更好的兼容性与性能优化 [`https://huggingface.co/mlc-ai`](https://huggingface.co/mlc-ai)
此次使用[`mlc-ai/Qwen3-1.7B-q4f16_1-MLC`](https://huggingface.co/mlc-ai/Qwen3-1.7B-q4f16_1-MLC)举例

#### 1. `Qwen3` (模型家族)
*   **含义**：指的是 **Qwen (通义千问)** 系列模型的第 3 代（或者用户自定义的某个版本，目前主流是 Qwen2.5，Qwen3 可能是最新发布或为了演示）。
*   **来源**：阿里云 (Alibaba Cloud) 开发的开源大语言模型。

#### 2. `1.7B` (参数量)
*   **含义**：**1.7 Billion (17亿)** 参数。
*   **意义**：
    *   这是一个 **小语言模型 (SLM)**。
    *   相比于 7B 或 72B 的模型，1.7B 非常轻量。
    *   **适用场景**：非常适合 **Android 手机** 或 **iOS 设备** 本地运行，因为它的内存占用低，推理速度快。

#### 3. `q4f16_1` (量化编码格式 - 核心部分)
这是 MLC-LLM 特有的量化命名方式，决定了模型的大小和在手机 GPU 上的运行效率。

*   **`q4` (Quantization 4-bit)**：
    *   **4比特量化**。原始模型通常是 `f16` (16比特)。
    *   将权重压缩到 4比特，意味着模型体积缩小了约 **75%**。
    *   *例如：1.7B 模型原本需要约 3.4GB 显存，压缩后只需要约 1GB 多一点，手机轻松跑。*

*   **`f16` (Float 16)**：
    *   指模型在计算时（Activation/Scale）使用 **Float16** 精度。
    *   这保证了虽然权重被压缩了，但计算过程依然保持较高的精度，避免模型变“傻”。这也是移动端 GPU（如 Adreno, Mali）最擅长的计算格式。

*   **`_1` (Variant/Group Size)**：
    *   这是 MLC 的内部版本标识，通常代表 **分组量化 (Group Quantization)** 的策略。
    *   **`_1`** 通常代表 **Group Size 32**（即每32个参数共享一个缩放因子）。
    *   **对比**：
        *   `q4f16_0`: 通常是 Group Size 128 或不分组。体积最小，但精度稍差。
        *   `q4f16_1`: **精度更高 (Perplexity 更低)**，虽然体积比 `_0` 微微大一点点，但在小模型（如 1.7B）上，**为了保证回答质量，强烈推荐使用 `_1`**。

### 将 MLC-LLM Android SDK 添加到你的项目

详细信息见 [`https://llm.mlc.ai/docs/deploy/android.html`](https://llm.mlc.ai/docs/deploy/android.html)

### 构建环境

该工作流会自动配置以下环境:
- **操作系统**: Ubuntu Linux (最新版本)
- **Python**: 3.13
- **Android NDK**: 27.3.13750724
- **Java**: 21 (Jetbrains 发行版)
- **Rust**: 最新稳定版

详细信息见 [`.github/workflows/Builder.yml`](.github/workflows/Builder.yml)