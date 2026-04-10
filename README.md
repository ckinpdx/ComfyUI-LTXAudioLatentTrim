# ComfyUI-LTXAudioLatentTrim

Two small utility nodes for working with LTX2 AV audio latents in ComfyUI.

## Nodes

### LTX Audio Latent Trim
Trims an LTX audio latent `[B, C, T, F]` along the temporal dimension using start/end latent frame indices. Supports negative indexing (`end_index=-1` = last frame inclusive).

**Why it exists:** LTX2 audio latents are 4D tensors `[B, C, T, F]`, while video latents are 5D `[B, C, T, H, W]`. The existing trimming nodes in ComfyUI and KJNodes (`LTXVSelectLatents`, `GetLatentRangeFromBatch`) either expect 5D tensors or index the batch dimension rather than the temporal dimension — making them unusable for audio latent trimming in sliding window workflows. This node handles the 4D case correctly.

### Latent Strip Mask
Removes the `noise_mask` key from a latent dict.

**Why it exists:** `LTXVAddLatents` attempts to merge noise masks from both inputs. When a trimmed audio latent still carries its mask from a previous generation pass, the merge logic fails with a dimension mismatch. Stripping the mask before concatenation avoids this.

## Use case

These nodes are primarily useful in long-form LTX2 AV sliding window generation workflows, where audio and video latents must be trimmed and accumulated independently across multiple sampling passes.
