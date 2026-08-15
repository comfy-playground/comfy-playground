# Roadmap

[中文](ROADMAP.md) | [Home](README.en.md)

## Delivered

- Durable ComfyUI Cluster Gateway with scheduling, output archival and recovery.
- Conservative SDXL and Anima batching, including independent stochastic seeds.
- One MCP implementation for direct single-node and Gateway deployments.
- Gateway consuming MCP as a Git submodule instead of duplicating its code.
- Composable worker, workflow and AstrBot integration guidance.

## Next

- Executable smoke checks and a compatibility matrix for every deployment shape.
- Workflow contract tests for negative prompt, count, seed and output mapping.
- Worker model/LoRA/custom-node consistency checks and release tag tables.

## Later

- Additional proven-equivalent batch adapters, read-only operational metrics,
  workflow metadata catalogs, and reusable AstrBot policy presets.

Every roadmap item must state its affected components, compatibility strategy,
rollback path, secret boundary and validation method.
