# Bao 移植核心工作详解 (ARMv8 AArch64)

## 概述

移植 Bao 到一个新的 ARMv8 AArch64 开发板，本质工作是**向 Bao 描述你的硬件平台**。Bao 已经完整实现了 ARMv8-A 虚拟化框架（EL2 页表、GIC 虚拟化、SMMU、PSCI 等），你只需要告诉它你的板子长什么样。

需要你动手的只有 **5 个文件**，其中真正的核心是 **1 个 C 结构体**。

---

## 一、平台移植 vs 配置 — 分清概念

| 概念 | 是什么 | 位置 | 影响范围 |
|------|--------|------|----------|
| **Platform（平台描述）** | 物理硬件资源声明 | `src/platform/<board>/` | 所有 VM 共享 |
| **Config（配置）** | VM 划分方案 | `configs/<name>/config.c` | 单个部署场景 |

Platform 只做一次（描述你的板子），Config 可以有多份（不同的 VM 划分方案）。

---

## 二、最核心的移植工作：物理平台描述

### 文件：`src/platform/<your_board>/<board>_desc.c`

这就是移植的核心。它是一个 `struct platform` 全局变量，向 Bao 声明你的物理硬件资源。

```c
#include <platform.h>

struct platform platform = {

    /* ---------- CPU ---------- */
    .cpu_num = 4,          // 分配给 Bao 管理的 CPU 核数

    /* ---------- 内存 ---------- */
    .region_num = 1,       // 可用 DDR 区域数目
    .regions = (struct mem_region[]) {
        {
            .base = 0x40000000,     // 这片内存的起始物理地址
            .size = 0x40000000,     // 这片内存的大小（如 1GB）
            .perms = MEM_RWX,       // 权限（通常 MEM_RWX）
        },
        // 如果 DDR 不连续（如 4GB 以上有洞），定义多个 region
    },

    /* ---------- 串口 ---------- */
    .console = {
        .base = 0xFE201000,   // UART 物理基地址（最重要！影响能否看到输出）
    },

    /* ---------- 中断控制器 ---------- */
    .arch = {
        .gic = {
            // GICv3（推荐，现代 SoC）：
            .gicd_addr = 0x08000000,   // Distributor
            .gicr_addr = 0x080A0000,   // Redistributor

            // GICv2 额外需要：
            .gicc_addr = 0x08010000,   // CPU Interface
            .gich_addr = 0x08030000,   // Hypervisor Control (VM 运行时用)
            .gicv_addr = 0x08040000,   // Virtual CPU Interface (VM 运行时用)

            .maintenance_id = 25,      // PPI 编号，GIC maintenance interrupt
        },
    },
};
```

### 这个结构体中每个字段的作用

#### `cpu_num`
分配给 Bao 的物理 CPU 数。Bao 采用 1:1 vCPU-pCPU 映射（无调度器），所以在 config 中为每个 VM 分配的 CPU 总数不能超过这个值。

#### `regions`（内存区域）
告诉 Bao 哪些 DDR 区域可用。注意事项：
- **给 ATF / 安全世界留空间**：低地址的前若干字节通常是 ATF 和 secure monitor 的领地，不要包含进来
- **给 Bao 自己留空间**：Bao 本身的代码、数据、页表需要内存，这部分 Bao 会自行管理，但你给的 regions 越大越好
- **给设备 MMIO 留空间**：从物理地址范围中排除设备 MMIO 区域
- 如果 DDR 地址空间不连续（比如部分内存在 4GB 以上），定义多个 region

#### `console.base`（最关键！）
UART 控制器的**物理基地址**。这个地址不对，你就看不到任何串口输出，等于盲调。

#### `arch.gic`（GIC 中断控制器）
ARM 中断控制器寄存器地址，从 SoC TRM 的 "Interrupt Controller" 或 "GIC" 章节获取。

**GICv2 vs GICv3 的区别对移植者的影响**：
- GICv2（如树莓派 4）：需要 `gicd_addr` + `gicc_addr` + `gich_addr` + `gicv_addr`
- GICv3（大多数现代 SoC）：需要 `gicd_addr` + `gicr_addr`。`gicc/gich/gicv` 可通过系统寄存器访问，不需要 MMIO 地址（留 0 即可）
- `maintenance_id`：GIC maintenance interrupt 的 PPI 编号，**永远填 25**（ARM 架构规定）

#### `arch.clusters`（可选，多 cluster 异构）
如果 SoC 有大小核（big.LITTLE），需要描述 cluster 拓扑：

```c
.arch = {
    .clusters = {
        .num = 2,
        .core_num = (size_t[]) {4, 4},  // cluster0 有 4 核，cluster1 有 4 核
    },
    // ...
}
```

这会影响到 `cpuid → MPIDR` 的转换（`platform_arch_cpuid_to_mpidr`）。如果 `clusters.num == 0`，Bao 假定单 cluster，`MPIDR = cpuid`。

---

## 三、平台编译配置

### 文件：`src/platform/<your_board>/platform.mk`

```makefile
ARCH:=armv8                          # 固定值
CPU:=cortex-a53                      # 从 SoC TRM 获取大核型号
GIC_VERSION:=GICV3                   # GICV2 或 GICV3
drivers = pl011_uart                 # 或 8250_uart / imx_uart / nxp_uart / zynq_uart
platform_description:=<board>_desc.c # 上面那个描述文件的名字
platform-cflags = -mtune=$(CPU)
```

关键抉择：

**UART 驱动选择**：查看 SoC TRM，确认 UART 控制器 IP：
| 驱动名 | 对应硬件 IP |
|--------|------------|
| `pl011_uart` | ARM PrimeCell PL011 |
| `8250_uart` | 标准 8250/16550 兼容（需额外定义 `UART8250_REG_WIDTH` 和 `UART8250_PAGE_OFFSET`） |
| `imx_uart` | NXP i.MX UART |
| `nxp_uart` | NXP S32 UART |
| `zynq_uart` | Xilinx Zynq UART |

如果你的 UART IP 不在以上列表中，需要编写新的 UART 驱动（最坏的情况）。

---

## 四、平台头文件

### 文件：`src/platform/<your_board>/inc/plat/platform.h`

```c
#ifndef __PLAT_PLATFORM_H__
#define __PLAT_PLATFORM_H__

#include <drivers/pl011_uart.h>   // 换成你选的 UART 驱动

#endif
```

如果使用 8250 UART，还需要定义寄存器宽度和偏移：

```c
#define UART8250_REG_WIDTH   (4)     // 寄存器宽度（字节），通常 4（32-bit 访问）
#define UART8250_PAGE_OFFSET (0x40)  // 寄存器间偏移（stride），如 0x40=树莓派
```

---

## 五、编译对象文件列表

### 文件：`src/platform/<your_board>/objects.mk`

```makefile
boards-objs-y+=<board>_desc.o
```

---

## 六、VM 配置（逻辑划分）

### 文件：`configs/<your_config>/config.c`

平台移植好后，需要定义 VM 划分方案。核心是 `struct config` 全局变量：

```c
#include <config.h>

// 声明 guest OS 镜像（直接嵌入 Bao 二进制）
VM_IMAGE(linux, "path/to/Image")
VM_IMAGE(baremetal, "path/to/baremetal.bin")

struct config config = {
    CONFIG_HEADER

    .vmlist_size = 2,  // 2 个 VM
    .vmlist = (struct vm_config[]) {
        {
            .image = {
                .base_addr = 0x40000000,         // guest 物理内存起始地址
                .load_addr = VM_IMAGE_OFFSET(linux),
                .size = VM_IMAGE_SIZE(linux),
            },
            .entry = 0x40000000,                 // guest 入口地址
            .cpu_affinity = 0x3,                 // 分配 CPU 0,1（位图）
            .colors = 0xFFFF,                     // cache coloring 掩码

            .platform = {
                .cpu_num = 2,
                .region_num = 1,
                .regions = (struct vm_mem_region[]) {
                    { .base = 0x40000000, .size = 0x40000000 }
                },
                .dev_num = 1,
                .devs = (struct vm_dev_region[]) {
                    {   /* 直通 UART 给 Linux */
                        .pa = 0xFE201000,       // 物理地址
                        .va = 0xFE201000,       // VM 看到的地址
                        .size = 0x1000,
                        .interrupt_num = 1,
                        .interrupts = (irqid_t[]) {33}  // SPI 中断号
                    }
                },
                .arch = {
                    .gic = {
                        .gicd_addr = 0x08000000,
                        .gicc_addr = 0x08010000,
                    }
                }
            },
        },
        // ... 第二个 VM
    }
};
```

### 关键配置项的说明

| 字段 | 含义 | 注意事项 |
|------|------|----------|
| `image.base_addr` | Guest 镜像在 VM 地址空间中的加载地址 | 必须在一个 `region` 范围内 |
| `entry` | Guest 启动入口地址 | 通常等于 `base_addr`（Linux） |
| `cpu_affinity` | 分配给该 VM 的物理 CPU 位图 | 各 VM 的 affinity 必须互斥（不能共享 CPU） |
| `colors` | Cache coloring 位图 | 各 VM 的 colors 必须互斥 |
| `regions` | 分配给该 VM 的内存区域 | 必须是 platform 声明的物理内存的子集，各 VM 不能重叠 |
| `devs` | 直通给该 VM 的设备 | 包含 MMIO 地址范围 + 中断号 |

---

## 七、Boot 流程与前置条件

理解 Bao 对启动环境的要求对移植很重要。Bao 启动时**要求**：

1. **CPU 必须已处于 EL2**（hypervisor 异常级别）
   - 通常由 ATF BL31 在 EL3 初始化完成后，降级到 EL2 再跳转到 Bao
   - 典型启动链：`bootrom → BL2 → BL31(EL3) → Bao(EL2) → VM(EL1/EL0)`

2. **所有 CPU 都从同一个入口进入** Bao（`_image_start`）
   - CPU0（BSP/boot CPU）先执行，设置全局页表
   - 其他 CPU（AP/secondary）等待 BSP 完成后设置各自的 CPU 私有页表

3. **缓存一致性已在 monitor（ATF）中启用**
   - boot.S 中明确注释："We are assuming monitor code already enabled SMP coherency"

4. **设备树 / ACPI 不需要**
   - Bao 本身不解析设备树，所有硬件信息来自 `struct platform` 和 `struct config`

5. **物理地址**
   - Bao 的虚拟地址空间从 `0xfd8000000000` 开始（见 `arch/bao.h`），这是一段特意选在高地址区域的范围

---

## 八、移植核心工作优先级

按重要性从高到低排列：

| 优先级 | 工作内容 | 不正确的后果 |
|--------|----------|-------------|
| **P0** | UART 基地址 (`console.base`) | 看不到任何输出，无法调试 |
| **P0** | Boot 流程 + CPU 在 EL2 | 系统无法启动 |
| **P1** | GIC 寄存器地址 | 中断不工作，VM 无法响应外设 |
| **P1** | 内存区域定义 (`regions`) | 内存分配失败或覆盖 ATF/Bao 自身 |
| **P2** | CPU 数量和 cluster 拓扑 | 多核无法使用 |
| **P2** | VM 设备直通和中断号 | Guest 外设不可用 |
| **P3** | Cache coloring | 性能/隔离性不完全，系统仍可运行 |

---

## 九、调试方法

1. **先让串口有输出**：`console.base` 正确 → `bao.bin` 第一条指令就会初始化 UART → 看到 `Bao Hypervisor ...` 就是成功了
2. **用 JTAG 调 boot.S**：如果串口没有输出，问题大概率在 `boot.S` 的页表设置阶段。用 JTAG 看 `_image_start` 时 CPU 状态
3. **`CONFIG=null` 验证平台**：null 配置不启动任何 VM，只验证平台初始化能否走完
4. **利用 `bao.elf.asm`**：每次构建产物包含反汇编，可用于 JTAG 调试时对照

---

## 十、移植文件清单

```
# 你必须创建的文件：
src/platform/<your_board>/
├── platform.mk                   # 约 10 行
├── <board>_desc.c                # 约 30-40 行（核心！）
├── objects.mk                    # 1 行
└── inc/plat/
    └── platform.h                # 约 5 行

configs/<your_config>/
└── config.c                      # VM 配置，长度取决于 VM 数量

# 总计：5 个文件，约 100-200 行代码
```
