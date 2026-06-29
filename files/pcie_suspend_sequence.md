# PCIe 拓扑结构标准休眠流程 (ARM64 Linux)

本流程描述在 ARM64 Linux 平台上，系统执行休眠（如 Suspend to RAM）时，从主机端（RC）、下行 Switch 到各叶子终端设备（NVMe、网卡、USB Hub、Wi-Fi 等）的逐层休眠与断电交握全过程。

---

## 阶段一：Linux PM 层准备与驱动暂停 (深度优先)
1. **冻结用户态与内核线程**：Linux PM Core 挂起用户空间进程，冻结内核线程，确保不再有新的 I/O 请求下发。
2. **深度优先遍历设备树**：内核电源管理系统从 PCIe 拓扑的最底层（叶子设备）开始，向上追溯调用 `suspend()` 回调函数：
   * **NVMe/网卡/Wi-Fi**：停止 DMA 传输，关闭接收/发送队列，注销或关闭中断。若支持远程唤醒（WoL/WoWLAN），驱动会配置其 PME（Power Management Event）相关寄存器。
   * **USB Hub**：停止 USB 总统调度，下行端口进入挂起（Suspend）状态。

## 阶段二：设备配置与链路转换 (D0 -> D3hot -> L1)
3. **备份配置空间**：内核调用 `pci_save_state()`，将所有 PCIe 设备的配置空间（Configuration Space，包括 BAR 地址、中断配置、Command 寄存器等）备份到系统内存（RAM）中。
4. **切入 D3hot 状态**：
   * 驱动程序通过向终端设备的 `PMCSR`（电源管理控制与状态寄存器）写入 `0x3`，显式令设备进入 **D3hot** 状态。
5. **链路自动迁移至 L1**：
   * 当 Switch 的下行端口检测到相连的叶子设备进入 D3hot 且没有数据传输时，硬件物理层自动将该段 PCIe 链路切换到 **L1**（低功耗暂态）状态。
   * 当 Switch 下行的所有设备都进入 D3hot 后，Linux `pci_port` 驱动会将 Switch 自身的配置空间也设为 D3hot，Switch 到 RC 的上行链路同样迁移至 L1。

## 阶段三：RC 握手与物理层断电 (D3hot -> D3cold / L1 -> L2/L3)
6. **RC 广播 PME_Turn_Off**：
   * 整个 PCIe 树都进入 D3hot/L1 后，ARM64 RC (Root Complex) 驱动执行 `suspend`。
   * RC 控制器硬件向整个 PCIe 拓扑树广播发送 `PME_Turn_Off` 消息包。
7. **终端设备应答 Ack**：
   * Switch 负责向下转发该消息。
   * 所有叶子设备（NVMe、网卡等）收到 `PME_Turn_Off` 后，硬件自动回复 `PME_TO_Ack` 数据包进行确认。
8. **链路进入 L2/L3 Ready**：
   * 当所有的 `PME_TO_Ack` 经由 Switch 汇总并返回到 RC 后，PCIe 物理层确认拓扑空闲，链路正式切入 **L2/L3 Ready** 状态。
9. **切断主电源 (进入 D3cold)**：
   * Linux 内核通过通用电源域（GPD）框架或板级 Regulator 驱动，向 SoC 的 PMU 或外级 PMIC 发出断电指令。
   * **切断主电源 Vcc**（12V/3.3V）。此时，设备由于主电源消失，隐式转换为 **D3cold** 状态。
   * **唤醒电源保留 (Vaux)**：若设备支持带外/带内唤醒，板级电路需维持 **Vaux（辅助电源）** 供电，此时链路处于 **L2** 状态；若完全不需唤醒，则 Vaux 亦切断，链路降至 **L3**（彻底断电）。
