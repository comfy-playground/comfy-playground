# AGENTS.md

## Purpose

This repository documents the ComfyUI Playground deployment stack. Chinese is
the default language; every user-facing Chinese document must have a matching
English document with the same operational conclusions.

## Source of truth

- Gateway behavior: `comfy-playground/comfyui-cluster-gateway`.
- MCP behavior: `comfy-playground/comfyui-lora-manager-mcp`.
- Do not infer unsupported behavior from a workflow filename or a model name.
  Confirm route, configuration, and runtime assumptions against the owning
  repository before documenting them.

## Documentation rules

- Never publish API keys, local host paths, LAN addresses, GPU serials, model
  files, generated images, or `.env` contents.
- Deployment examples must use placeholders and loopback bindings by default.
- Keep the three supported shapes distinct: single node, single node plus MCP,
  and multiple workers behind Gateway plus MCP.
- State whether a setting is required, optional, or only needed for a feature.
- Prefer small Mermaid diagrams and copyable YAML/env examples over screenshots.
- Update both language variants and the roadmap when an architecture decision
  changes.

## Validation

- Check Markdown links and fenced code blocks after edits.
- Run `git diff --check` before committing.
- Examples are documentation inputs, not production secrets or a substitute
  for the Compose files owned by the component repositories.
