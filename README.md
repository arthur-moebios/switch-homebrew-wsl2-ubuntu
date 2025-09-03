# devkitPro snapshot for **Ubuntu on WSL2**

> **TL;DR:** This repo hosts a **snapshot** of `/opt/devkitpro` intended **only** for **Ubuntu running on WSL2 (Windows 10/11)**.  
> Install by extracting the Release tarball at the filesystem root (`/`), then add the environment variables to your `~/.bashrc`.

---

## ⚠️ Scope / Support

- ✅ **Supported:** Ubuntu on **WSL2** (running inside Windows).  
- ⚠️ **Not tested / unsupported:** WSL1, other WSL distros, bare-metal Linux, macOS, native Windows.

If you need a cross-platform setup, use the official devkitPro installer (pacman). This snapshot is a pragmatic shortcut for **WSL2 + Ubuntu**.

---

## 📦 What’s in this snapshot?

A prebuilt tree of **devkitPro** under `/opt/devkitpro` for homebrew development (e.g., Nintendo Switch).

Typical contents (may vary per release):

- Toolchains: `devkitA64/` (AArch64) and `devkitARM/` (ARM EABI)  
- `libnx/` and `tools/` (e.g., `elf2nro`)  
- Common portlibs: `switch-glfw`, `switch-glad`, `switch-mesa` (EGL/GLES), `switch-mbedtls`, `switch-glm`, `switch-libdrm_nouveau`, `switch-zlib`, `switch-libpng`, `switch-freetype`, `switch-bzip2`, `switch-pkg-config`, `switch-curl`, etc.

See the Release notes for the exact list.

---

## ✅ Requirements

- Windows 10/11 with **WSL2** enabled  
- **Ubuntu** installed on WSL2  
- Disk space (the tarball is ~**166 MB**; installed size is larger)

---

## 🚀 Installation (Ubuntu on WSL2)

> **Run these inside the Ubuntu (WSL2) terminal**, not in PowerShell.

1) Download the file from the **Releases** tab (e.g., `devkitpro-wsl-YYYY-MM-DD.tar.zst`).  
   If you downloaded via Windows browser, it’s likely under:
/mnt/c/Users/YOUR_USER/Downloads/devkitpro-wsl-YYYY-MM-DD.tar.zst

2) **Extract at the filesystem root `/`** (the tarball already contains `opt/devkitpro/...`):

```bash
# example path — adjust to your filename/location
TAR="/mnt/c/Users/YOUR_USER/Downloads/devkitpro-wsl-YYYY-MM-DD.tar.zst"

sudo mkdir -p /opt/devkitpro
# For .tar.zst:
sudo tar -I zstd -xvf "$TAR" -C /
# For .tar.xz (alternative):
# sudo tar -xvf "$TAR" -C /

3) Add environment variables to your ~/.bashrc:
grep -q "DEVKITPRO=/opt/devkitpro" ~/.bashrc || cat >>~/.bashrc <<'EOF'
# devkitPro (WSL2)
export DEVKITPRO=/opt/devkitpro
export DEVKITARM=${DEVKITPRO}/devkitARM
export DEVKITA64=${DEVKITPRO}/devkitA64
export PATH=$PATH:${DEVKITPRO}/tools/bin:${DEVKITARM}/bin
EOF

# apply to current shell
source ~/.bashrc
4) Sanity checks:
which aarch64-none-elf-gcc && aarch64-none-elf-gcc --version | head -n1
which arm-none-eabi-gcc     && arm-none-eabi-gcc --version | head -n1
which elf2nro
test -f /opt/devkitpro/libnx/switch_rules && echo "libnx OK"

## 🧪 Usage tips

Build without sudo (fix project perms if needed):
sudo chown -R "$USER":"$USER" /path/to/your/project
cd /path/to/your/project
make -j"$(nproc)"

🧹 Uninstall
sudo rm -rf /opt/devkitpro
# (optional) remove the lines from ~/.bashrc

🔐 Verify (optional)

If a checksum is provided in the Release:

sha256sum devkitpro-wsl-YYYY-MM-DD.tar.zst
# Compare with the hash on the Release page

🙏 Credits

devkitPro and portlib maintainers

Homebrew community

This repo only ships a snapshot to speed up setup on WSL2 + Ubuntu
