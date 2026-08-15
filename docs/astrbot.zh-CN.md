# AstrBot 集成与插件选型

[返回首页](../README.md) | [English](astrbot.en.md)

AstrBot 只需要访问一个 ComfyUI 风格 HTTP 地址：单节点方案填 worker 地址，集群
方案填 Gateway 地址。不要让同一个插件实例在 worker 和 Gateway 地址之间随机切换。

## 推荐顺序

| 选择 | 适用场景 | 工作流与参数能力 | 建议 |
| --- | --- | --- | --- |
| [WsureDev 魔改版 `astrbot_plugin_comfyui_pro`](https://github.com/WsureDev/astrbot_plugin_comfyui_pro) + MCP | 需要聊天式、可控的生产使用 | workflow 切换、多图 `count`、正向/负向提示词、seed 与节点级参数注入 | **优先推荐** |
| 原版 `astrbot_plugin_comfyui_pro` | 只需要切换 workflow 和多图生成 | workflow 切换、多图生成；不能指定负向提示词 | 可用，但能力较少 |

魔改版与 MCP 是互补关系：插件负责聊天命令和 workflow 参数注入，MCP 负责 LoRA
生态检索、下载、库存管理和可选 Gateway 状态。MCP 不替代出图插件，也不直接生成图片。

## 地址配置

| 拓扑 | AstrBot `server_address` | MCP `COMFYUI_URL` | MCP `COMFYUI_GATEWAY_URL` |
| --- | --- | --- | --- |
| 单节点 | worker `http://worker.example:8188` | 不部署或同一 worker | 留空 |
| 单节点 + MCP | worker 地址 | 同一 worker 地址 | 留空 |
| Gateway 集群 + MCP | Gateway `http://gateway.example:19189` | primary worker 地址 | Gateway 地址 |

## 使用魔改版的工作流约定

1. 在 ComfyUI 导出 **API Format** JSON 并放入插件持久化 workflow 目录。
2. 在插件设置中选择 JSON，并配置正向输入节点和输出节点；需要时配置负向、seed、
   count 或其他参数对应节点。
3. 使用 `/comfy_ls` 查看 workflow，使用 `/comfy_use` 热切换，先在管理员环境验证。
4. 对负向提示词使用插件支持的命令或参数，不要通过字符串拼接破坏 workflow 的
   `CLIPTextEncode` 连接。
5. 多图 count 受 worker/GPU 显存、workflow latent 尺寸和 Gateway batch 上限共同约束。

## 权限和可观测性

- 保留插件的管理员、白名单、冷却和敏感词策略；它们与 Gateway token 不是同一层防护。
- 插件请求超时应允许排队时间，尤其是在 Gateway 有 active execution 时。
- Gateway 部署下，通过 `/gateway/v1/status` 或 MCP 的状态工具判断 worker readiness，
  不要仅凭 AstrBot 命令是否返回来判定集群健康。
- UI 导出的 workflow、私有 prompt、聊天记录和生成图像均可能是敏感数据，不应提交。
