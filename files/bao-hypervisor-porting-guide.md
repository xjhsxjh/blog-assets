# Bao Hypervisor 平台移植入门指南

## 第一步：确认你的板子 SoC 是否已被支持

先看你的开发板是什么芯片。Bao 已支持的 arm64 平台包括：

| 平台 | SoC |
|------|-----|
| `rpi4` | Broadcom BCM2711 (Cortex-A72) |
| `imx8qm` | NXP i.MX8QM (Cortex-A72) |
| `imx8mn` | NXP i.MX8M Nano (Cortex-A53) |
| `imx8mp-verdin` | NXP i.MX8M Plus |
| `tx2` | NVIDIA Tegra X2 |
| `hikey960` | Kirin 960 |
| `zcu102/zcu104/ultra96` | Xilinx Zynq UltraScale+ |
| `qemu-aarch64-virt` | QEMU（用于学习） |
| `fvp-a` | ARM Fixed Virtual Platform（用于学习） |

如果你的 SoC 不在列表中，需要做平台移植。如果在列表中，直接用已有平台即可。

**建议先拿 `qemu-aarch64-virt` 跑通一遍，理解整个构建和部署流程，再移植到真实硬件。**

---

## 第二步：先在 QEMU 上体验（推荐新手路径）

```bash
# 交叉编译工具链
export CROSS_COMPILE=aarch64-none-elf-

# 构建
make PLATFORM=qemu-aarch64-virt CONFIG=example

# 产物在 bin/qemu-aarch64-virt/example/bao.bin
# 用 QEMU 运行（需要准备 guest VM 镜像，参考 bao-demos）
```

---

## 第三步：移植到新平台 — 需要创建的文件

每个平台在 `src/platform/<平台名>/` 下需要 **4 个文件 + 1 个目录**。以 rpi4 为例：

```
src/platform/<your_board>/
├── platform.mk              # 定义 ARCH, CPU, GIC版本, UART驱动
├── <board>_desc.c           # 描述物理平台：内存布局, UART地址, GIC地址
├── objects.mk               # 要编译的板级对象文件
└── inc/plat/
    └── platform.h           # UART驱动配置宏
```

### 3a. `platform.mk` — 平台编译配置

```makefile
ARCH:=armv8           # 固定为 armv8（arm64）
CPU:=cortex-a53       # 你的 SoC 大核型号，如 cortex-a72/cortex-a76
GIC_VERSION:=GICV3    # 或 GICV2，取决于你的 SoC
drivers = pl011_uart  # 或 8250_uart，取决于你的 UART 型号
platform_description:=<board>_desc.c
platform-cflags = -mtune=$(CPU)
```

这里的关键决策：

- **GIC_VERSION**：ARM GICv2 还是 GICv3（大多数现代 arm64 SoC 用 GICv3）
- **drivers**：你的 UART 控制器型号（查芯片手册），Bao 支持以下 UART 驱动：
  - `pl011_uart` — ARM PL011
  - `8250_uart` — 标准 8250 兼容
  - `imx_uart` — NXP i.MX
  - `zynq_uart` — Xilinx Zynq
  - `nxp_uart` — NXP S32
  - `cmsdk_uart` — ARM CMSDK

### 3b. `<board>_desc.c` — 物理平台描述

这是最核心的文件，描述你的开发板的物理资源。结构如下：

```c
#include <platform.h>

struct platform platform = {
    .cpu_num = 4,                    // 可用的 CPU 核数

    .region_num = 1,                 // 可用 DDR 内存区域数量
    .regions = (struct mem_region[]) {
        {
            .base = 0x40000000,      // 你的内存起始地址
            .size = 0x40000000,      // 内存大小（如 1GB）
        },
    },

    .console = {
        .base = 0xFE201000,          // UART 基地址（从芯片手册获取）
    },

    .arch = {
        .gic = {
            .gicd_addr = 0x08000000,       // GIC Distributor 基地址
            .gicc_addr = 0x08010000,       // GIC CPU Interface (GICv2)
            .gich_addr = 0x08030000,       // GIC Hypervisor Interface
            .gicv_addr = 0x08040000,       // GIC Virtual CPU Interface
            .gicr_addr = 0x080A0000,       // GIC Redistributor (GICv3)
            .maintenance_id = 25,          // GIC maintenance interrupt PPI ID
        },
    },
};
```

**关键信息来源**：你的开发板 SoC TRM（技术参考手册）的 "Memory Map" 和 "Interrupt Controller" 章节。最重要的是：

- 内存地址空间布局
- GIC 寄存器的物理基地址
- UART 控制器的物理基地址

### 3c. `inc/plat/platform.h` — 平台头文件

```c
#ifndef __PLAT_PLATFORM_H__
#define __PLAT_PLATFORM_H__

// 如果使用 8250 UART，需要定义寄存器宽度和偏移
// #define UART8250_REG_WIDTH   (4)
// #define UART8250_PAGE_OFFSET (0x40)

#include <drivers/pl011_uart.h>  // 根据你选的 UART 驱动换

#endif
```

### 3d. `objects.mk` — 编译对象

```makefile
boards-objs-y+=<board>_desc.o
```

---

## 第四步：创建 VM 配置

在 `configs/<your_config>/config.c` 中定义你的虚拟机布局。核心内容：

```c
#include <config.h>

VM_IMAGE(linux_vm, "/path/to/linux-kernel-image.bin")

struct config config = {
    CONFIG_HEADER

    .vmlist_size = 1,
    .vmlist = (struct vm_config[]) {
        {
            .image = {
                .base_addr = 0x40000000,        // VM 看到的内存地址
                .load_addr = VM_IMAGE_OFFSET(linux_vm),
                .size = VM_IMAGE_SIZE(linux_vm),
            },
            .entry = 0x40000000,
            .cpu_affinity = 0x3,                // 分配给 VM 的 CPU 位图
            .colors = 0xFFFF,

            .platform = {
                .cpu_num = 2,
                .region_num = 1,
                .regions = (struct vm_mem_region[]) {
                    { .base = 0x40000000, .size = 0x40000000 }
                },
                .dev_num = 1,
                .devs = (struct vm_dev_region[]) {
                    {   /* 直通 UART 给 guest */
                        .pa = 0xFE201000,       // 物理地址
                        .va = 0xFE201000,       // guest 看到的地址
                        .size = 0x1000,
                        .interrupt_num = 1,
                        .interrupts = (irqid_t[]) {33}
                    }
                },
                .arch = {
                    .gic = {
                        .gicc_addr = 0x08010000,
                        .gicd_addr = 0x08000000,
                    }
                }
            },
        }
    }
};
```

---

## 第五步：构建和部署

```bash
# 构建
make PLATFORM=<your_board> CONFIG=<your_config> CROSS_COMPILE=aarch64-none-elf-

# 产物
#   bin/<your_board>/<your_config>/bao.bin     — 可执行二进制镜像
#   bin/<your_board>/<your_config>/bao.elf     — ELF 格式（带调试信息）
#   bin/<your_board>/<your_config>/bao.elf.txt — ELF 的 readelf 信息
#   bin/<your_board>/<your_config>/bao.elf.asm — 反汇编
```

部署通常需要配合 ATF（ARM Trusted Firmware）+ U-Boot / 其他 bootloader：

1. ATF 在 EL3 初始化，切换到 EL2 跳转到 Bao
2. Bao 初始化后启动 VM（Linux 或其他 guest OS）
3. 参考 [bao-demos](https://github.com/bao-project/bao-demos) 仓库的部署脚本

**Bao 必须运行在 EL2（hypervisor 特权级别）**，所以你的 bootloader 流程需要确保跳转到 Bao 时 CPU 处于 EL2 状态。典型启动流程：

```
BL1 (bootrom) → BL2 (ATF) → BL31 (ATF EL3) → Bao (EL2) → Guest VM (EL1/EL0)
```

---

## 第六步：调试技巧

1. **先让串口工作** — 这是你唯一能看到 Bao 输出的途径。确保 `console.base` 的 UART 地址正确，Bao 启动后第一件事就是初始化串口
2. **GIC 中断号**：ARM 上 SGI 是 0-15，PPI 是 16-31，SPI 从 32 开始。`maintenance_id` 必须是 PPI（16-31 范围内）
3. **内存布局**：注意给 ATF 和 Bao 自身保留内存，不要让 VM 覆盖。参考：
   - ATF 通常占用低地址空间
   - Bao 自身占用内存需要考虑 `.text`, `.data`, `.bss`, 以及每个 VM 的页表空间
4. **用 `null` config 先验证**：`CONFIG=null` 是空配置，只验证平台构建能通过，不涉及任何 VM
5. **GICv2 vs GICv3 地址差异**：
   - GICv2：需要 `gicd_addr`（Distributor）、`gicc_addr`（CPU Interface）、`gich_addr`（Hypervisor Control）
   - GICv3：需要 `gicd_addr`（Distributor）、`gicr_addr`（Redistributor），`gicc_addr`/`gich_addr` 可选
6. **如果 UART 没有输出**：检查 UART 控制器型号和基地址是否正确；可以用 JTAG 调试器看是否有 CPU 异常

---

## 总结：最小移植 Checklist

1. 获取 SoC TRM，找到：
   - 内存地址空间映射（DDR 起始地址和大小）
   - GIC 物理基地址（Distributor / CPU Interface / Redistributor / Hypervisor）
   - UART 控制器型号和物理基地址
2. 创建 `src/platform/<name>/` 下 4 个文件（`platform.mk`、`<board>_desc.c`、`objects.mk`、`inc/plat/platform.h`）
3. 创建 `configs/<name>/config.c`
4. `make PLATFORM=<name> CONFIG=<name> CROSS_COMPILE=aarch64-none-elf-`
5. 通过 JTAG/U-Boot 加载 `bao.bin` 到 EL2 运行
6. 看到串口输出 `Bao Hypervisor ...` 就成功了第一步

## 参考资源

| 资源 | 地址 |
|------|------|
| Bao 项目网站 | https://www.bao-project.org/ |
| 文档 | https://bao-project.readthedocs.io/ |
| Demo 仓库 | https://github.com/bao-project/bao-demos |
| "Hello Bao World" 教程 | https://www.youtube.com/watch?v=6c8_MG-OHYo |
