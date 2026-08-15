# 运维与安全

[返回首页](../README.md) | [English](operations.en.md)

## 密钥边界

| 密钥/配置 | 放置位置 | 用途 | 不应放置的位置 |
| --- | --- | --- | --- |
| Worker `CIVITAI_API_KEY` | worker 本机 `.env` | LoRA Manager 直接下载需认证内容 | Gateway、workflow JSON、Git |
| MCP `CIVITAI_API_KEY` | MCP 本机 `.env` | Civitai 搜索和元数据 API 认证 | worker 镜像、Git |
| Gateway generation token | Gateway `.env` + `config.yaml` 的变量名 | 保护生成路由 | AstrBot 日志、workflow |
| Gateway management token | Gateway `.env` + `config.yaml` 的变量名 | 保护 LoRA/catalog 管理路由 | 客户端公开配置 |

MCP token 不会自动转发到 worker 的下载请求，不能替代 worker token。默认绑定
`127.0.0.1`；公网或局域网暴露前必须增加 TLS、反向代理身份认证和网络策略。

## 升级顺序

1. 备份 Gateway SQLite、Gateway outputs、MCP SQLite、worker LoRA Manager state。
2. 在单个 worker 运行 API workflow 与 custom node 单测。
3. 发布 MCP 独立仓库版本；如 Gateway 使用其 submodule，显式更新 gitlink 并验证。
4. 构建 Gateway 镜像，运行 typecheck、测试和 Docker build。
5. 先滚动 optional worker，再验证 primary，最后切换 Gateway 或反向代理流量。
6. 对合批 workflow 验证 batch size 1/2、独立 history、输出数量和不同图片哈希。

## 常见故障

| 现象 | 首先检查 |
| --- | --- |
| Gateway `/readyz` 为 503 | required worker URL、精确设备名、LoRA Manager `/api/lm` 路由 |
| optional worker 不参与调度 | `/gateway/v1/status` 的 readiness、capability、catalog revision 与模型一致性 |
| MCP 状态显示 unavailable | 单节点模式是预期；集群模式检查 `COMFYUI_GATEWAY_URL`、token 和 Gateway health |
| LoRA 下载成功但 workflow 不可用 | primary catalog scan、其他 worker full rebuild、共同 LoRA 根目录 |
| AstrBot 生成失败 | API workflow 节点 ID、目标地址、排队超时、插件权限/冷却设置 |

## 数据保留

Gateway 输出归档与 worker output 是不同数据面；不要仅因 worker 输出仍在就忽略
Gateway `data/`。MCP download records 和 LoRA Manager download state 也是独立状态。
删除、迁移或缩容前先停止相关服务并备份 SQLite 的主文件、WAL 和 SHM 文件。
