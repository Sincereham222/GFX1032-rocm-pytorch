# GFX1032-rocm-pytorch
# Building PyTorch from Source with ROCm (gfx1032) on Kubuntu 24.10

This guide walks through how to build PyTorch with ROCm support from source, targeting `gfx1032` (e.g. RX 6600 XT, RX 6650 XT) on **Kubuntu 24.10**, using **kernel 6.11.0-21-generic**.

---

## System Details

- **OS**: Kubuntu 24.10 (Ubuntu base)
- **Kernel**: 6.11.0-21-generic
- **GPU**: AMD RX 6650 XT (gfx1032)
- **ROCm Version**: 6.3.0
- **Python**: 3.10
- **PyTorch Version**: 2.8.0a0 (latest nightly as of build)

---

## 1. Set Up the Environment

Install system dependencies:

```bash
sudo apt update && sudo apt install -y \
    build-essential git cmake python3-dev python3-pip \
    libopenblas-dev libomp-dev libprotobuf-dev protobuf-compiler \
    libgoogle-glog-dev libgflags-dev libyaml-dev libffi-dev \
    libnccl-dev libnuma-dev libsqlite3-dev zlib1g-dev \
    libssl-dev libcurl4-openssl-dev libbz2-dev
```

Install Python environment:

```bash
python3 -m venv venv
source venv/bin/activate
pip install --upgrade pip
```

---

## 2. Install ROCm 6.3

Follow the official AMD instructions to install ROCm 6.3 for Ubuntu 24.04 — it works fine for 24.10.

Set ROCm paths in your environment (append to `~/.bashrc` or `~/.zshrc`):

```bash
export PATH=/opt/rocm/bin:$PATH
export LD_LIBRARY_PATH=/opt/rocm/lib:/opt/rocm/lib64:$LD_LIBRARY_PATH
export ROCM_PATH=/opt/rocm
```

Reload:

```bash
source ~/.bashrc
```

Check GPU visibility:

```bash
rocminfo
```

---

## 3. Clone PyTorch

```bash
git clone --recursive https://github.com/pytorch/pytorch.git
cd pytorch
```

---

## 4. Configure Build Flags

```bash
export PYTORCH_ROCM_ARCH="gfx1032"
export USE_ROCM=1
export USE_MIOPEN=1
export USE_NCCL=0
export USE_XNNPACK=0
export BUILD_CAFFE2=0
export USE_FLASH_ATTENTION=0
export CMAKE_PREFIX_PATH=$(python -c 'import sysconfig; print(sysconfig.get_paths()["purelib"])')
```

---

## 5. Install Python Dependencies

```bash
pip install -r requirements.txt
pip install --upgrade numpy
```

---

## 6. Build PyTorch

```bash
python setup.py bdist_wheel
```

Once finished, install it:

```bash
pip install dist/torch-*.whl
```

Test your installation:

```python
python -c "import torch; print(torch.version.__version__, torch.version.hip, torch.cuda.is_available(), torch.version.git_version)"
```

---

## 7. Build torchvision (Optional but recommended)

```bash
git clone https://github.com/pytorch/vision.git
cd vision
python setup.py develop
```

Test:

```python
python -c "import torchvision; print(torchvision.__version__)"
```

---

## 8. (Optional) Test with ComfyUI

Set up ComfyUI and point it at your ROCm-enabled PyTorch. Test a flow to ensure model loads properly and renders on GPU.

---

## Notes

- This build excludes CUDA, XNNPACK, NCCL, and FlashAttention.
- gfx1032 support is limited upstream; you may need to recompile as patches are merged.

---

Let me know if anything needs correction or clarification!
