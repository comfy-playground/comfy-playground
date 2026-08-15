# Deployment Examples

[Home](../README.en.md) | [中文](deployments.zh-CN.md)

All addresses below are placeholders. Component-owned repositories remain the
source of truth for their Dockerfiles, Compose files, and storage mounts.

## A. Single node

`AstrBot or client -> ComfyUI worker :8188`

Use this for one GPU or initial development. Point AstrBot `server_address` at
the worker. Gateway and MCP are not required. Keep a Civitai token in the
worker environment only when its LoRA Manager needs authenticated downloads.

## B. Single node plus MCP

`AstrBot or client -> worker`, with `MCP -> worker -> Civitai`.

```dotenv
COMFYUI_URL=http://worker.example:8188
COMFYUI_GATEWAY_URL=
LORA_ROOT=/app/ComfyUI/models/loras
CIVITAI_API_KEY=
```

Leave `COMFYUI_GATEWAY_URL` empty. MCP LoRA tools work directly against the
worker; `get_generation_cluster_status` intentionally returns an unavailable
single-node capability response.

## C. Multiple workers plus Gateway and MCP

`AstrBot or client -> Gateway :19189 -> primary and optional workers`, with
`MCP -> primary worker` and `MCP -> Gateway`.

Gateway needs exactly one required primary worker. Optional workers can leave
the cluster without blocking readiness. All eligible workers need compatible
models, custom nodes, LoRA catalog and one visible GPU.

```dotenv
COMFYUI_URL=http://primary-worker.example:8188
COMFYUI_GATEWAY_URL=http://gateway.example:19189
COMFYUI_GATEWAY_GENERATION_TOKEN=
LORA_ROOT=/app/ComfyUI/models/loras
```

Point AstrBot to the Gateway endpoint. Confirm required-worker `/readyz` before
clients submit work, then inspect `/gateway/v1/status` and MCP `/healthz`.

## Acceptance criteria

- Execute each API workflow directly on a worker first.
- Copy the exact `/system_stats` device name into Gateway configuration.
- Keep all routable workers compatible with their workflows and LoRA catalog.
- Put public endpoints behind TLS, authentication and network controls.
