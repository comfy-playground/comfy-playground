# 工作流与生态

[返回首页](../README.md) | [English](workflows.en.md)

平台提交的是 ComfyUI **API Format** workflow JSON，而不是 UI 导出包。AstrBot、
Gateway 和批处理 adapter 都依赖节点 ID、输入连接和输出节点的稳定性。

## 已验证的样例类别

| 样例文件 | 生态/用途 | 关键节点 |
| --- | --- | --- |
| `animagine_default_v4.0_API.json` | Animagine XL 4、ControlNet、IP-Adapter、LoRA Manager | `CheckpointLoaderSimple`、`ControlNetLoader`、`IPAdapterModelLoader`、`KSampler` |
| `REED_XXX_illustrious_SDXL_bot_API_v2_lora_manager_fast.json` | Illustrious / SDXL、聊天机器人快速出图 | `CheckpointLoaderSimple`、`Lora Loader (LoraManager)`、`KSampler` |
| `anima_base_v1.0_lora_manager.json` | Anima Base 1.0 + LoRA Manager | `UNETLoader`、`CLIPLoader`、`VAELoader`、`KSampler` |
| `anima_2.9b_default_API.json` | Anima 2.9B 基础推理 | `UNETLoader`、`CLIPLoader`、`VAELoader`、`KSampler` |
| `anima_2.9b_lora_manager_fast.json` | Anima 2.9B + LoRA Manager | 同上，加 LoRA Manager |
| `test_deepseek_maid_api.json` | 特定角色/提示词回归样例 | 仅用于验证，不是通用产品 workflow |

与 `.lora.json` 同名的文件保存对应 LoRA 选择或说明。它们不是运行时模型文件，模型
权重、LoRA 和用户图片必须留在持久化存储中。

## AstrBot 工作流适配

导出步骤：在 ComfyUI 使用 **Save (API Format)**，再记录正向提示词输入和最终
`SaveImage` 输出节点 ID。插件将提示词、负向提示词、seed、count 等能力注入到已
配置的节点；不能假定任意 UI workflow 直接兼容 API 调用。

使用 Gateway 的 batch adapter 时，不要任意重排契约节点。Anima Base LoRA Manager
样例遵循以下稳定约定：

| 节点 ID | 角色 |
| --- | --- |
| `5` | 正向提示词编码 |
| `6` | 负向提示词编码 |
| `13` | sampler；合批时由 Gateway 转换 |
| `14` | VAE decode |
| `15` | `SaveImage` 输出 |

修改这些 ID、正负提示词连接或输出连接后，必须在单节点和 Gateway singleton/batch
路径都重新验证。

## 生态选择

- **SDXL / Illustrious / Animagine XL**：适合 checkpoint 型工作流；可与 ControlNet、
  IP-Adapter 和 LoRA Manager 组合。
- **Anima Base 1.0 / Anima 2.9B**：使用 UNET、CLIP 和 VAE 分离加载器；选择与
  worker image 固定兼容版本匹配的节点。
- **LoRA**：由 LoRA Manager 下载、扫描和 catalog 管理。需要跨 worker 路由时，
  先保证每台 worker 都能看到一致的目录和 catalog。
- **批处理**：不是模型生态本身。仅在 workflow 和 sampler 与 adapter 匹配时启用。

## 工作流发布清单

1. 附 API JSON、所需模型生态和最低 custom node 列表。
2. 标注正向、负向、seed、count、输出节点的配置方式。
3. 用空 LoRA、单 LoRA 和缺失 LoRA 分别测试。
4. 如支持 Gateway，记录 singleton 与合批后的输出数量、history 与图片哈希结果。
5. 不要把 token、绝对模型路径、用户图片或私有 prompt 放进样例 JSON。
