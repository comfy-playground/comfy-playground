# 路线图

[English](ROADMAP.en.md) | [返回首页](README.md)

## 已完成

- 独立 ComfyUI Cluster Gateway：持久化队列、worker 调度、输出归档、幂等恢复和
  ComfyUI 风格 HTTP API。
- 可选合批：SDXL、Anima Euler/SGM 与 Anima `er_sde` 独立随机状态支持。
- 独立 LoRA Manager MCP：单节点与 Gateway 模式共用一套工具和代码，状态工具可降级。
- Gateway 通过 MCP Git submodule 引用，避免维护两份 MCP 实现。
- Worker image、LoRA Manager、API workflow 样例和 AstrBot 魔改插件的可组合方案。

## 近期

- 为每种部署样例维护可执行的最小验收脚本和版本兼容矩阵。
- 扩展 workflow contract 测试，覆盖 AstrBot 的负向提示词、count、seed 和输出映射。
- 记录多 worker 的模型、LoRA、custom node 一致性检查流程。
- 发布固定 worker image tag 与 Gateway/MCP 兼容版本表。

## 后续方向

- 更丰富的 batch adapter，前提是能证明工作流语义和随机状态等价。
- 面向运营的只读指标、告警和容量建议，不把管理凭据暴露给聊天客户端。
- 工作流模板的元数据清单：生态、节点、模型、LoRA、显存和适配器能力。
- 可复用的 AstrBot 插件配置预设与权限策略示例。

任何路线图项目都必须先说明：影响的组件、兼容性策略、回滚路径、密钥边界和验证方式。
