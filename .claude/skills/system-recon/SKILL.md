---
name: system-recon
description: Gather a minimal system snapshot before starting ML work. Use this at the start of a new paper implementation, when the user asks what hardware or Python setup is available, or when you need a quick check of whether the machine is ready. Keep it short and practical.
---

# System Recon

Goal: collect only the information needed to decide whether work can start now or what must be installed first.

## What to check

Run a small set of checks. Prefer short, direct commands. Run all commands inside the sandbox first — only retry with `dangerouslyDisableSandbox: true` if a command fails with evidence of sandbox restrictions (e.g., "Operation not permitted", device access denied).

### Hardware

- **GPU**: `nvidia-smi` — model, VRAM, driver, CUDA version
- **CPU**: `lscpu | grep -E "Model name|CPU\(s\)|Thread\(s\) per core|Core\(s\) per socket|Socket\(s\)"`
- **RAM**: `free -h`
- **Disk**: `df -h /`

### Software

- **Python**: `python3 --version`
- **uv**: `uv --version`
- **Primary ML framework**: first determine which framework matters for this task from the user request, the paper, or the existing repo. Default to PyTorch only if nothing points elsewhere.
  - **PyTorch**: `python3 -c "import torch; print(torch.__version__); print(torch.cuda.is_available())"`
  - **JAX**: `python3 -c "import jax; print(jax.__version__); print(jax.devices())"`
  - **TensorFlow**: `python3 -c "import tensorflow as tf; print(tf.__version__); print(tf.config.list_physical_devices('GPU'))"`

Note: `nvidia-smi` and framework GPU probes may fail inside the sandbox. GPU device access (`/dev/nvidia*`) requires read/write ioctls that the sandbox blocks — only `/dev/stdout`, `/dev/stderr`, `/dev/null`, `/dev/tty`, `/dev/dtracehelper`, and `/dev/autofs_nowait` are in the write allowlist. If `nvidia-smi` fails with "Operation not permitted" or similar, retry with `dangerouslyDisableSandbox: true`. If `nvidia-smi` succeeds but the selected framework's GPU probe reports no GPU, retry that framework check with `dangerouslyDisableSandbox: true`.

If the selected framework is missing, report that plainly and stop there. If the user explicitly asks about multiple frameworks, check only those frameworks and report missing ones individually. Do not inventory every ML library unless the user asks.

## Output format

Keep the report compact.

### Environment Summary
- GPU
- CPU
- RAM
- Disk
- Python
- uv
- Framework check(s)

### Readiness
A short paragraph covering:
- what is ready now
- what is missing or blocked
- the next concrete step
- any framework assumption you made if the user did not specify one

## Rules

- Keep it under 10 lines if possible.
- Do not run broad package scans.
- Do not probe network access unless the user asks or the next step fails because of networking.
- Always try commands inside the sandbox first. Only use `dangerouslyDisableSandbox: true` if a command fails with evidence of sandbox restrictions (e.g., "Operation not permitted", device access denied). Do not preemptively bypass the sandbox.
- If `nvidia-smi` fails even after disabling the sandbox, report that no GPU was detected and continue with the remaining checks.
- Recommend `uv`, not `pip`.
