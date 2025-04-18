# GFX1032-rocm-pytorch
# Building PyTorch from Source with ROCm (gfx1032) on Ubuntu-based Distros

This guide walks through how to build PyTorch with ROCm support from source, targeting `gfx1032` (e.g. RX 6600 XT, RX 6650 XT) on **Kubuntu 24.10**, using **kernel 6.11.0-21-generic**.

> 🟢 This method **SHOULD** work for any Ubuntu **24.04+** base (including Kubuntu, Xubuntu, Pop!_OS, etc.)

---

## System Details

- **OS**: Kubuntu 24.10 (Ubuntu base)
- **Kernel**: 6.11.0-21-generic
- **GPU**: AMD RX 6650 XT (gfx1032)
- **ROCm Version**: 6.3.2
- **Python**: 3.12
- **PyTorch Version**: 2.8.0a0 (latest nightly as of build)

---

Just use the prebuilt binaries.
