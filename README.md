# OpenAvatarChat · 王承泽定制配置（wcz）

在 [HumanAIGC-Engineering/OpenAvatarChat](https://github.com/HumanAIGC-Engineering/OpenAvatarChat)
（Apache-2.0）基础上的个人化配置，目标是「屏幕里一个看起来像真人的数字人，能用人声对话」。

## 关键定制（config/wcz_gateway_musetalk.yaml）

- **LLM 走本机 key 池中转站** `http://127.0.0.1:8789/v1`（codex-free-gateway），自带多源聚合 / 流式透传 / failover / 多 key 轮询单 key 熔断
- **边缘 TTS**：`Edge_TTS` → 中文语音 `zh-CN-XiaoxiaoNeural`（免 GPU、实时、中文自然）
- **ASR**：`SenseVoiceSmall`（iic/SenseVoiceSmall）+ `SileroVad` 端点检测
- **Avatar**：`MuseTalk` 驱动的嘴型同步（需 `yongen.mp4` 参考视频与模型权重）
- 服务端口 8282，RTC 客户端接入

## 部署要点（东京服务器 / 本机）

1. Python 必须 **3.11**（OpenAvatarChat `requires-python = ">=3.11.7, <3.12"`，不能用 3.12）
2. torch 走 cu128 源：`pip install torch==2.8.0 torchvision==0.23.0 torchaudio==2.8.0 --index-url https://download.pytorch.org/whl/cu128`
3. `uv run python src/demo.py --config config/wcz_gateway_musetalk.yaml`
4. 需下载的权重：MuseTalk（SD-VAE / Whisper / DWPose / SyncNet / Face-Parse / s3fd）、SenseVoice、Silero-VAD
5. 麦克风采集 + 浏览器 RTC 拉流，即可人声对话

> 完整依赖由 OpenAvatarChat 的 `install.py` 按启用的 handler 合并安装（注入 PROTECTED_PACKAGES 保护 torch 等不被降级）。

## 关联仓库

- key 池：`codex-free-gateway`
- 多 AI 群聊：`ai-group-chat`
