# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Build Command

```bash
# Build with a specific platform and config
make PLATFORM=<platform> CONFIG=<config> [CROSS_COMPILE=...] [GIC_VERSION=GICV2|GICV3]

# Examples:
make PLATFORM=fvp-a CONFIG=null GIC_VERSION=GICV3
make PLATFORM=qemu-riscv64-virt CONFIG=null CROSS_COMPILE=riscv64-unknown-elf-
make PLATFORM=fvp-r CONFIG=null CROSS_COMPILE=arm-none-eabi-

# Clean build artifacts
make clean PLATFORM=<platform> CONFIG=<config>
```

`PLATFORM` and `CONFIG` are mandatory. Platforms live in `src/platform/<name>/` and define an architecture via their `platform.mk`. Configs live in `configs/<name>/` as C files.

A `null` config exists at `configs/null/config.c` for build-only smoke testing. The `configs/example/config.c` shows a full VM setup.

## Linting / CI

```bash
# CI checks (run in the bao project Docker container):
make ci PLATFORM=<platform> CONFIG=<config>
# This runs: license-check (Apache-2.0 headers), format-check (clang-format)
```

CI workflows (`.github/workflows/`) build multiple platform+toolchain combinations. The official Docker image is `baoproject/bao:latest`.

## Architecture

Bao is a **static partitioning hypervisor** — resources are partitioned at VM instantiation time, with no scheduler, no overcommit, and pass-through IO only.

### Top-level layout

```
src/
 ├── arch/          Architecture-specific code (armv8, riscv, tricore, rh850)
 │   ├── armv8/armv8-a/   Armv8-A profile (AArch64/AArch32)
 │   ├── armv8/armv8-r/   Armv8-R profile (AArch64/AArch32)
 │   ├── riscv/            RISC-V (RV64/RV32), includes irqc/ (PLIC, AIA/APLIC)
 │   ├── tricore/          Infineon Tricore 1.8
 │   └── rh850/            Renesas RH850
 ├── core/          Hypervisor core (platform-agnostic)
 │   ├── mmu/       MMU-based memory protection (paging)
 │   └── mpu/       MPU-based memory protection
 ├── lib/           Utility library (bitmap, printk, string)
 ├── platform/      Board/platform descriptions (fvp-a, rpi4, imx8qm, tx2, etc.)
 │   └── drivers/   UART drivers (8250, pl011, imx, zynq, etc.)
 └── linker.ld      Linker script (preprocessed from macros)
configs/            VM configuration files (C structs, one per configuration)
scripts/            Host-side code generators for config/platform definitions
ci/                 CI rules (license/format checking)
```

### Boot flow

1. `_image_start` (linker.ld entry) → arch-specific boot code (`.boot` section)
2. Each CPU calls `init(cpu_id)` in `src/core/init.c`
3. `cpu_init()` → `mem_init()` → `platform_init()` → `console_init()` → `interrupts_init()` → `vmm_init()`
4. `vmm_init()` creates VMs from the static `config` struct and starts them

### Key abstractions

- **VM** (`struct vm`, `vm.h`): A virtual machine with statically assigned memory regions (`vm_mem_region`), device regions (`vm_dev_region`), IPC channels, and vCPUs. 1:1 vCPU-to-pCPU mapping.
- **VMM** (`vmm.h`): The virtual machine monitor — sets up stage-2 translation (MMU) or MPU regions, installs VMs, manages VM memory layout.
- **Config** (`config.h`): A global `struct config` populated by the config C file. Contains VM images (embedded via `.incbin`), memory/dev regions, CPU affinity, and colors. Config files are compiled into a host-side generator (`scripts/config_defs_gen.c`) that emits `config_defs_gen.h` with `#define` constants used throughout the hypervisor.
- **Platform** (`platform.h`): Each platform defines a description (e.g., `fvpa_desc.c`) compiled into a generator that emits `platform_defs_gen.h` with base addresses, IRQ counts, UART details, etc.
- **Memory protection**: Two modes — `MEM_PROT_MMU` (paging via `core/mmu/`) and `MEM_PROT_MPU` (via `core/mpu/`). The platform's architecture selects which one via `arch_mem_prot` in its `arch.mk`.
- **Colors**: A mechanism for partitioning cache ways (cache coloring) — each VM gets assigned colors via `colormap_t` in the config.

### Platform and config definitions

Each platform has:
- `platform.mk` — sets `ARCH`, drivers, flags, and `platform_description` (the C file describing the platform)
- A platform description C file that defines the platform's memory layout, devices, and constants
- Optional `inc/plat/` headers for platform-specific types

Each config has:
- A C file with a `struct config config` (the global variable), `VM_IMAGE()` declarations for embedded guest binaries, and the full VM layout
- Optional `config.mk` for out-of-tree config repos

### Cross-compilation

GCC is default. Clang/LLVM is supported via `CROSS_COMPILE=clang`. The Makefile auto-detects based on whether `clang` appears in `CROSS_COMPILE`.

### Commit style

Commits follow conventional commits: `feat(scope):`, `fix(scope):`, `ref(scope):` where scope typically names the subsystem (plat, riscv, core, drivers/8250, etc.).
