# Operations and Security

[Home](../README.en.md) | [中文](operations.zh-CN.md)

Keep worker and MCP Civitai keys separate. A worker key authenticates LoRA
Manager downloads; an MCP key authenticates Civitai metadata requests and is
not forwarded to worker download calls. Keep Gateway generation and management
tokens in its local environment and configure only their variable names in
`config.yaml`.

Default bindings are loopback-only. Add TLS, reverse-proxy authentication and
network controls before LAN or Internet exposure.

Upgrade in this order: back up Gateway/MCP SQLite and worker state; validate
workflows on one worker; publish MCP separately; explicitly update the Gateway
submodule pointer; build/test Gateway; roll optional workers first; validate
primary; then switch traffic. For batching, validate batch sizes one and two,
independent history, output count and distinct image hashes.

Key diagnostics: required-worker issues surface on Gateway `/readyz`; optional
worker readiness, capability and catalog revision appear in
`/gateway/v1/status`; MCP status unavailable is expected in single-node mode;
and a downloaded LoRA still needs catalog synchronization before workflows can
use it.
