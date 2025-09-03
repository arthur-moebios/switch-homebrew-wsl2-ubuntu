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
