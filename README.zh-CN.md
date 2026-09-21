[English](README.md) · **简体中文**

[![Stars](https://img.shields.io/github/stars/nobodywho-ooo/nobodywho?style=flat-square&logo=github&label=Star)](https://github.com/nobodywho-ooo/nobodywho/stargazers)
[![Licence](https://img.shields.io/badge/licence-EUPL--1.2-blue?style=flat-square)](LICENSE)
[![Docs](https://img.shields.io/badge/Docs-lightblue?style=flat-square)](https://docs.nobodywho.ooo)
[![Pub.dev Version](https://img.shields.io/pub/v/nobodywho?include_prereleases&style=flat-square&label=pub.dev)](https://pub.dev/packages/nobodywho)
[![PyPI Version](https://img.shields.io/pypi/v/nobodywho?style=flat-square&labelColor=%233775A9&color=%23FFD242)](https://pypi.org/project/nobodywho/)
[![npm Version](https://img.shields.io/npm/v/react-native-nobodywho?style=flat-square&logo=npm&label=npm)](https://www.npmjs.com/package/react-native-nobodywho)
[![Maven Central](https://img.shields.io/maven-central/v/ai.nobodywho/nobodywho?style=flat-square&label=Maven%20Central)](https://central.sonatype.com/artifact/ai.nobodywho/nobodywho)

<p align="center">
  <img src="assets/logo-nobodywho.png" alt="NobodyWho Logo" width="150"/>
</p>

# NobodyWho

**面向移动端与桌面端的端侧 AI：文本、视觉、嵌入、RAG、工具调用、语音转文字、文字转语音与语音活动检测。以 Metal / Vulkan 加速运行 GGUF 模型。可免费商用。**

一切都在设备上运行。无需 API 密钥，无需联网，没有按 token 计费，数据不会离开这台机器。

目前已用于 [Chat](https://example.com/nobodywho-chat)、[Eyes](https://example.com/nobodywho-eyes) 和 [Wrist](https://example.com/nobodywho-wrist)。

---

## 实际运行效果

NobodyWho 附带一个兼容 OpenAI 的服务器，你不用写一行代码，就能让现有的客户端接上本地模型：

```bash
uvx --from 'git+https://github.com/nobodywho-ooo/nobodywho.git#subdirectory=nobodywho/server' \
  nobodywho-server --model hf:NobodyWho/Qwen_Qwen3-0.6B-GGUF:Q4_K_M --name qwen
```

它默认监听 `http://127.0.0.1:8888`，提供 `/v1/models` 与 `/v1/chat/completions`。
把客户端的 base URL 设为 `http://127.0.0.1:8888/v1`，模型名设为 `qwen` 即可。
该服务器仍处于实验阶段 — 详见[服务器文档](https://docs.nobodywho.ooo/server/)。

| | |
|---|---|
| <img src="assets/screenshot-server.png" alt="现有的 OpenAI 客户端由本地模型作答" width="420"/> | <img src="assets/screenshot-app.png" alt="NobodyWho 在手机上离线运行" width="420"/> |
| 一个 OpenAI 客户端指向本机，离线作答。 | 同一个模型在飞行模式下的手机上运行。 |

⭐ **如果这正是你要找的东西，请给仓库点个 Star** — 这是对我们最大的帮助。

---

## 平台支持

| 绑定 | 安装方式 | 可运行于 | 文档 |
|------|----------|----------|------|
| **Kotlin** | [Maven Central](README.md#install) | 桌面端、Android | [docs.nobodywho.ooo/kotlin](https://docs.nobodywho.ooo/kotlin/) |
| **Swift** | [SPM](README.md#install) | 桌面端、iOS、visionOS、watchOS | [docs.nobodywho.ooo/swift](https://docs.nobodywho.ooo/swift/) |
| **React Native / Expo** | [npm](README.md#install) | 桌面端、Android、iOS | [docs.nobodywho.ooo/react-native](https://docs.nobodywho.ooo/react-native/) |
| **Flutter** | [pub.dev](README.md#install) | 桌面端、Android、iOS | [docs.nobodywho.ooo/flutter](https://docs.nobodywho.ooo/flutter/) |
| **Python** | [PyPI](README.md#install) | 桌面端 | [docs.nobodywho.ooo/python](https://docs.nobodywho.ooo/python/) |
| **Godot** | [AssetLib](README.md#install) | 桌面端、Android | [docs.nobodywho.ooo/godot](https://docs.nobodywho.ooo/godot/) |

文中的「桌面端」均指 Linux、macOS 和 Windows。动手之前有两个缺口值得先知道：

- **Godot 不支持导出到 iOS。** 在 iOS 上请改用 Flutter、React Native 或 Swift 绑定。
- **暂不支持 Web 导出。** 进展见 [issue #111](https://github.com/nobodywho-ooo/nobodywho/issues/111)。

## 硬件要求

推理会在可用时走 Vulkan 或 Metal，否则回退到 CPU。真正的瓶颈是内存，而不是算力：

- **经验法则** — 设备的**可用**内存需要约为模型文件大小的两倍，而可用内存远低于总内存：
  iOS 会占用 1–2 GB，Android 视厂商不同占用 2–4 GB。
- **iOS** — iPhone 11 或更新机型，4 GB 及以上内存。
- **Android** — 骁龙 855 / Adreno 640 / 6 GB 内存或更好。
- 在移动端，1 GB 以下的模型运行流畅。

## 模型

任何 [GGUF 格式](https://huggingface.co/models?library=gguf&sort=trending)的模型都能用，可选的有上千个。
在任何需要填模型路径的地方传入 `hf:owner/repo:QUANT`，首次使用时会自动下载并缓存；
也可以传 `"auto"`，由 NobodyWho 按可用内存挑一个合适的。

建议从 [Qwen3 0.6B](https://huggingface.co/NobodyWho/Qwen_Qwen3-0.6B-GGUF) 开始：约 330 MB，
小到手机跑得动，又足以验证集成是否正常。想往上选，可以看
[Hugging Face 上的 NobodyWho 组织](https://huggingface.co/NobodyWho)或
[模型选择页](https://www.nobodywho.ai/models/)。

---

## 安装与更多

六种绑定的安装步骤与上手代码、完整能力列表、架构说明、社区与许可证信息，
请见[英文 README](README.md)；完整文档见 [docs.nobodywho.ooo](https://docs.nobodywho.ooo)。

NobodyWho 以 [EUPL-1.2](LICENSE) 授权，**可免费用于专有及商业项目**。

---

> 译自 [`814b9919`](https://github.com/nobodywho-ooo/nobodywho/commit/814b9919)。
> 如与[英文版](README.md)有出入，以英文版为准。
