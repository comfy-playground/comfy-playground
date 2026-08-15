# Workflows and Ecosystems

[Home](../README.en.md) | [中文](workflows.zh-CN.md)

The stack submits ComfyUI **API Format** workflow JSON. Stable node IDs, input
links and output nodes matter to AstrBot, Gateway and batching adapters.

| Example family | Ecosystem | Notable nodes |
| --- | --- | --- |
| `animagine_default_v4.0_API.json` | Animagine XL 4, ControlNet, IP-Adapter and LoRA Manager | checkpoint, ControlNet, IP-Adapter, KSampler |
| `REED_XXX_illustrious_SDXL_bot_API_v2_lora_manager_fast.json` | Illustrious / SDXL chat workflow | checkpoint, LoRA Manager, KSampler |
| `anima_base_v1.0_lora_manager.json` | Anima Base 1.0 with LoRA Manager | UNET, CLIP, VAE loaders and KSampler |
| `anima_2.9b_default_API.json` | Anima 2.9B base inference | UNET, CLIP, VAE loaders and KSampler |
| `anima_2.9b_lora_manager_fast.json` | Anima 2.9B with LoRA Manager | the same plus LoRA Manager |

Export through **Save (API Format)**. Configure the positive-prompt input and
final `SaveImage` output IDs in AstrBot. Model weights, LoRAs and user images
belong in persistent storage, never in the workflow repository.

For Gateway batch compatibility, preserve the documented graph contract. The
Anima Base LoRA Manager example uses IDs 5 positive prompt, 6 negative prompt,
13 sampler, 14 VAE decode and 15 SaveImage. Re-test direct, Gateway singleton
and batch execution after changing these nodes or their links.

Publish each workflow with its required ecosystem, custom nodes, prompt/seed/
count/output mapping, LoRA behavior and a no-secret validation result.
