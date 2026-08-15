# AstrBot Integration and Plugin Choice

[Home](../README.en.md) | [中文](astrbot.zh-CN.md)

AstrBot needs one ComfyUI-style HTTP endpoint: a worker for single-node mode or
Gateway for clustered mode. Do not randomly switch one plugin instance between
both addresses.

| Choice | Capabilities | Recommendation |
| --- | --- | --- |
| [WsureDev modified `astrbot_plugin_comfyui_pro`](https://github.com/WsureDev/astrbot_plugin_comfyui_pro) plus MCP | Workflow switching, multi-image `count`, positive/negative prompts, seed and node-level parameter injection | **Preferred** |
| Upstream `astrbot_plugin_comfyui_pro` | Workflow switching and multi-image generation, but no explicit negative-prompt parameter | Suitable for simpler use |

The plugin handles chat commands and workflow injection. MCP handles LoRA
discovery, downloads, inventory and optional Gateway status; it does not render
images and does not replace the plugin.

| Topology | AstrBot endpoint | MCP `COMFYUI_URL` | MCP `COMFYUI_GATEWAY_URL` |
| --- | --- | --- | --- |
| Single node | worker | same worker or no MCP | empty |
| Single node plus MCP | worker | same worker | empty |
| Gateway cluster plus MCP | Gateway | primary worker | Gateway |

Use API-format workflow JSON, configure input/output node IDs, and validate
negative prompt, count, seed and output behavior in an administrator-only
environment before exposing a workflow. Plugin access control, cooldown and
content filtering complement rather than replace Gateway authentication.
