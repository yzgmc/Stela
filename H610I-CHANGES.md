# H610I 非官方修改版 — 改动清单

> **这是非官方修改版，与上游作者无关。**
> 上游：https://github.com/tylertms/Stela
> 基线：`25f661c9bc5e046677e280f6cd2abdf5c0850cfe`（v0.3.1 之后、v0.4.0 之前）
> 修改期：2026-11-13 ~ 2026-11-14，共 v1 – v21 二十一个迭代
> 目标机器：JGINYUE H610I-GAMING / i5-12400F / RTX 4060 / 32 GB

本分支（`h610i-v21`）保存上述修改的完整源码树，逐文件对应已写入 U 盘的 v21 引导，
便于与上游对比和继续维护。**它是一个历史快照，不是上游的升级版本**：
上游已发布 v0.4.0 / v0.4.1（228 个提交、300 个文件改动），本修改版没有整体跟进，
只手工移植了其中与本机故障相关的个别修复。请勿把本分支与上游最新 main 混用。

## 许可与合规

- EFI 为 **GPL-2.0-only**，CLI 为 **GPL-3.0-or-later**；原项目的
  `LICENSE`、`NOTICE`、`LICENSES/` 及第三方许可材料均原样保留。
- 按许可证要求在此声明：本源码是修改版，修改者 yxp，修改日期 2026-11-13 ~ 11-14，
  修改内容见下文逐版清单与文件列表；作者与上游项目无关联，不提供担保。
- 发布任何编译产物时，必须同时提供**对应版本**的完整源码、依赖版本与构建脚本。
  本分支只含源码与说明，**不含** EFI/EXE 二进制、macOS 镜像、启动日志、
  硬件报告、设备固件、凭据或个人配置。
- 构建依赖与步骤：Rust 1.99.1（见 `rust-toolchain.toml`）、Clang/LLD 21–23、
  `x86_64-unknown-uefi` 与 `x86_64-pc-windows-gnu` targets；在 Linux 上交叉编译。
  校验和/补丁/重建脚本见私有仓库 `yzgmc/Stela-h610i` 的 `h610i/` 目录。

## 一、总览

| 项 | 值 |
| --- | --- |
| 上游 | https://github.com/tylertms/Stela |
| 本地基线 | `25f661c9bc5e046677e280f6cd2abdf5c0850cfe`（v0.3.1 附近） |
| 早期基线 v1–v3 | 用户附件 bundle 的 `668845a`，v4 起切到 `25f661c` |
| 版本数 | v1 – v21，共 21 版（2026-11-13 07:52 ~ 11-14 23:04，GMT+8） |
| 最终改动 | 78 个 git 条目 = 10 新增 + 68 修改（含 2 个子模块指针）→ **76 个源码文件** |
| 完整补丁 | `h610i/patches/v21-full.patch`（280 KB，相对基线） |
| 私有源码仓库 | https://github.com/yzgmc/Stela-h610i |
| 公开 Fork | https://github.com/yzgmc/Stela（保持上游原样） |

### 改动落在哪些模块

| 模块 | 改动文件数 |
| --- | ---: |
| `crates/stela-boot` | 21 |
| `crates/stela-runtime` | 11 |
| `crates/stela-driver-base` | 8 |
| `crates/stela-boot-core` | 7 |
| `crates/stela-driver-gpu` | 7 |
| `crates/stela-driver-storage` | 7 |
| `crates/stela-driver-net` | 6 |
| `crates/stela-cli` | 4 |
| `crates/stela-dev` | 2 |
| `crates/stela-drivers` | 2 |
| `(根目录/子模块)` | 2 |
| `crates/stela-linux` | 1 |

核心主线：**Realtek 网卡 / DNS → 离线镜像与 ASR 恢复 → NVMe 控制器接管 →
PCIe 桥 DMA → 安装进度显示 → RTX 4060 显卡识别/中断/显示/GPU 固件 DMA →
PCID 身份检查 → 键鼠输入 → 设置界面冻结的两处编译锁并发缺陷**。

## 二、逐版改动清单（21 版）

| 版本 | 构建时间 | 该版主题（原始标题） | 源码差异 | 产物包 |
| --- | --- | --- | ---: | --- |
| v1 | 2026-11-13 07:52 | Stela 0.3.1 — Windows x64 / H610I 诊断修订构建（2026-11-13） | 3 个文件 | `stela-windows-h610i-20261113.zip` |
| v2 | 2026-11-13 15:49 | Stela 0.3.1 — H610I / RTL8168H 网络修订 v2 | 9 个文件 | `stela-windows-h610i-network-v2.zip` |
| v3 | 2026-11-13 17:49 | Stela H610I 恢复准备诊断 v3 | 10 个文件 | `stela-windows-h610i-restore-v3.zip` |
| v4 | 2026-11-13 18:23 | Stela H610I DNS 修订 v4（非官方定制编译） | 13 个文件 | `stela-windows-h610i-dns-v4.zip` |
| v5 | 2026-11-13 19:06 | Stela H610I RTL8168H 接收停滞恢复 v5（非官方定制编译） | 16 个文件 | `stela-windows-h610i-rx-v5.zip` |
| v6 | 2026-11-13 19:20 | Stela H610I ASR 镜像传输续读修订 v6（非官方定制编译） | 17 个文件 | `stela-windows-h610i-asr-v6.zip` |
| v7 | 2026-11-13 19:57 | Stela 本地安装与可见诊断修订 v7（非官方定制编译） | 26 个文件 | `stela-windows-h610i-local-v7.zip` |
| v8 | 2026-11-13 20:41 | Stela H610I 离线安装 USB 兼容修复 v8（非官方定制） | 28 个文件 | `stela-windows-h610i-local-v8.zip` |
| v9 | 2026-11-13 21:10 | Stela H610I NVMe 最小初始化兼容方案 v9（非官方定制） | 34 个文件 | `stela-windows-h610i-local-v9.zip` |
| v10 | 2026-11-13 21:34 | Stela H610I NVMe 接管兼容方案 v10（非官方定制） | 34 个文件 | `stela-windows-h610i-local-v10.zip` |
| v11 | 2026-11-13 21:57 | Stela H610I NVMe 分阶段启用与寄存器校验 v11（非官方定制） | 34 个文件 | `stela-windows-h610i-local-v11.zip` |
| v12 | 2026-11-14 09:12 | Stela H610I NVMe 接管与详细诊断 v12（非官方定制） | 36 个文件 | `stela-windows-h610i-local-v12.zip` |
| v13 | 2026-11-14 10:01 | Stela H610I NVMe 上游 DMA 修订 v13（非官方定制） | 37 个文件 | `stela-windows-h610i-local-v13.zip` |
| v14 | 2026-11-14 13:11 | Stela H610I 安装进度显示 v14（非官方定制） | 43 个文件 | `stela-windows-h610i-local-v14.zip` |
| v15 | 2026-11-14 15:44 | Stela H610I 显卡 PCI 设备识别修复 v15（非官方定制） | 45 个文件 | `stela-windows-h610i-local-v15.zip` |
| v16 | 2026-11-14 16:11 | Stela H610I / RTX 4060 中断接管修复 v16（非官方定制） | 56 个文件 | `stela-windows-h610i-local-v16.zip` |
| v17 | 2026-11-14 16:41 | Stela H610I / RTX 4060 显示与启动日志 v17（非官方定制） | 62 个文件 | `stela-windows-h610i-local-v17.zip` |
| v18 | 2026-11-14 17:48 | Stela H610I / RTX 4060 GPU 固件 DMA 修复 v18（非官方定制） | 63 个文件 | `stela-windows-h610i-local-v18.zip` |
| v19 | 2026-11-14 21:13 | Stela H610I / RTX4060 定制 v19：Linux 入口 PCID 身份检查修复 | 66 个文件 | `stela-windows-h610i-local-v19.zip` |
| v20 | 2026-11-14 21:55 | Stela 本地 v20：键鼠输入修复及 GPU 拒绝原因日志 | 2 个文件 | `stela-h610i-v20-efi-update.zip` |
| v21 | 2026-11-14 23:04 | Stela 本地 v21：设置界面停顿修复与诊断（2026-11-14） | 7 个文件 | `stela-h610i-v21-efi-update.zip` |

说明：v1–v19 的补丁是**相对上游基线的累积补丁**（文件数 3 → 66 递增），
任取一份即可从基线还原到该版；v20 / v21 是**相对上一版的小增量补丁**，
需叠加在 v19 之上。每版的完整中文说明、校验和与补丁已作为对应 Release 的附件上传。

### 关键节点

- **v1**：在 Linux 上交叉编译出 UEFI EFI 与 Windows EXE，加入启动阶段诊断日志；
  APIC 34 项测试通过，内嵌 EFI 与独立 EFI 逐字节一致。
- **v2–v6**：RTL8168H 收包轮询与 DHCP 状态、恢复准备诊断、DNS over TCP 回退、
  网卡接收停滞恢复、ASR 镜像传输续读。
- **v9–v13**：NVMe 从最小初始化到完整控制器接管，分阶段启用、队列寄存器回读校验、
  标准复位优先 + PCIe FLR 兜底，开启上游 PCIe 桥 DMA 转发；解决 Identify 后
  `CSTS=0x3` 致命状态与 PCI Master Abort。
- **v14**：安装文件传输进度（组件名、已发送/总量、速度、ETA、无进展时长），
  区分「发送完成」与「安装器处理中」。
- **v15–v19**：显卡 PCI 检查误读 Intel SMBus 的修正、IOAPIC 中断接管、
  显示缓冲地址与显示器延迟检测、补齐 Nouveau DMA 内存分配接口、
  PCID 标签误判导致的 `CorruptContext` 修复。
- **v20**：三个 `--system` 启动入口补 `--input` 启用键鼠；GPU PASS REFUSED
  增加 reason/tile/目标尺寸等具体原因日志。
- **v21**：设置界面冻结修复——`AirCompiler::llvm` 延后重试不再持服务锁等 LLVM；
  `SharedService::run_job` 持编译锁发布完成状态后再释放；正常编译延后不再误报
  GPU PASS REFUSED；新增后台 `BOOT STALL v21` 阶段诊断。
  GPU 550 项测试通过、0 失败、1 忽略；UEFI/Windows 构建成功；U 盘哈希核对通过。
  **设置界面冻结是否彻底消除仍未实机验证。**

只读测速：SSD ≈ 1424 MiB/s、U 盘 ≈ 147 MiB/s，没有证据表明磁盘读速是冻结主因。
另外新增了只读安装状态检查工具 `stela-inspect-install.rs` / `install_region.rs`；
**断点续装未实现**，不能仅凭 ASR 完成状态跳过后续步骤。

## 三、76 个改动文件完整清单

`A` = 新增，`M` = 修改。路径相对仓库根目录，与私有仓库 `h610i/modified/` 一一对应。

### `(根目录/子模块)`（2 个）

```
M  third_party/linux
M  third_party/mesa
```

### `crates/stela-boot`（21 个）

```
M  crates/stela-boot/src/engine.rs
M  crates/stela-boot/src/engine/display.rs
M  crates/stela-boot/src/engine/gpu.rs
M  crates/stela-boot/src/engine/gpu/client.rs
M  crates/stela-boot/src/engine/gpu/display.rs
M  crates/stela-boot/src/engine/host/local/context.rs
M  crates/stela-boot/src/engine/kernel_work.rs
M  crates/stela-boot/src/engine/network.rs
M  crates/stela-boot/src/engine/remote.rs
M  crates/stela-boot/src/engine/restore.rs
M  crates/stela-boot/src/engine/run.rs
M  crates/stela-boot/src/engine/service.rs
M  crates/stela-boot/src/engine/start.rs
M  crates/stela-boot/src/engine/storage.rs
M  crates/stela-boot/src/native_irq.rs
M  crates/stela-boot/src/native_irq/madt.rs
M  crates/stela-boot/src/native_mmio/shared.rs
M  crates/stela-boot/src/native_smp/executor.rs
M  crates/stela-boot/src/output.rs
M  crates/stela-boot/src/status.rs
M  crates/stela-boot/src/system.rs
```

### `crates/stela-boot-core`（7 个）

```
M  crates/stela-boot-core/src/ioapic.rs
M  crates/stela-boot-core/src/ioapic/tests.rs
M  crates/stela-boot-core/src/lib.rs
M  crates/stela-boot-core/src/madt.rs
M  crates/stela-boot-core/src/page_window.rs
M  crates/stela-boot-core/src/root_transition.rs
A  crates/stela-boot-core/src/stall_watch.rs
```

### `crates/stela-cli`（4 个）

```
M  crates/stela-cli/src/boot_log.rs
M  crates/stela-cli/src/lib.rs
M  crates/stela-cli/src/setup/command.rs
M  crates/stela-cli/src/setup/tests.rs
```

### `crates/stela-dev`（2 个）

```
A  crates/stela-dev/src/bin/stela-inspect-install.rs
A  crates/stela-dev/src/install_region.rs
```

### `crates/stela-driver-base`（8 个）

```
M  crates/stela-driver-base/src/bus/dma/amd/unit/registers/capability.rs
M  crates/stela-driver-base/src/bus/pci/aspm/native.rs
M  crates/stela-driver-base/src/bus/pci/capability.rs
M  crates/stela-driver-base/src/bus/pci/capability/tests.rs
M  crates/stela-driver-base/src/bus/pci/requester.rs
M  crates/stela-driver-base/src/bus/pci/requester/roles.rs
M  crates/stela-driver-base/src/bus/pci/requester/tests.rs
M  crates/stela-driver-base/src/bus/vmd/endpoint/capabilities.rs
```

### `crates/stela-driver-gpu`（7 个）

```
M  crates/stela-driver-gpu/src/metal/compiler.rs
M  crates/stela-driver-gpu/src/metal/compiler/cache_tests.rs
M  crates/stela-driver-gpu/src/metal/device/tests.rs
M  crates/stela-driver-gpu/src/metal/kms.rs
M  crates/stela-driver-gpu/src/metal/kms/tests.rs
M  crates/stela-driver-gpu/src/metal/service/commands/render.rs
M  crates/stela-driver-gpu/src/metal/service/shared.rs
```

### `crates/stela-driver-net`（6 个）

```
M  crates/stela-driver-net/src/realtek.rs
A  crates/stela-driver-net/src/realtek/recovery.rs
M  crates/stela-driver-net/src/realtek/ring.rs
M  crates/stela-driver-net/src/realtek/tests.rs
M  crates/stela-driver-net/src/realtek/tests/packet_io.rs
A  crates/stela-driver-net/src/realtek/tests/receive_recovery.rs
```

### `crates/stela-driver-storage`（7 个）

```
M  crates/stela-driver-storage/src/nvme.rs
M  crates/stela-driver-storage/src/nvme/admin.rs
M  crates/stela-driver-storage/src/nvme/features.rs
M  crates/stela-driver-storage/src/nvme/owned.rs
M  crates/stela-driver-storage/src/nvme/recovery.rs
A  crates/stela-driver-storage/src/nvme/takeover.rs
M  crates/stela-driver-storage/src/nvme/tests.rs
```

### `crates/stela-drivers`（2 个）

```
M  crates/stela-drivers/src/probe.rs
A  crates/stela-drivers/src/probe/nvme_path.rs
```

### `crates/stela-linux`（1 个）

```
M  crates/stela-linux/src/compat/kernel/pci_host.c
```

### `crates/stela-runtime`（11 个）

```
M  crates/stela-runtime/src/platform/net/stack.rs
A  crates/stela-runtime/src/platform/net/stack/dns_tcp.rs
M  crates/stela-runtime/src/platform/net/stack/tests.rs
A  crates/stela-runtime/src/platform/net/stack/tests/dns_fallback.rs
M  crates/stela-runtime/src/restore.rs
M  crates/stela-runtime/src/restore/asr.rs
M  crates/stela-runtime/src/restore/driver.rs
M  crates/stela-runtime/src/restore/driver/tests.rs
A  crates/stela-runtime/src/restore/progress.rs
M  crates/stela-runtime/src/restore/session.rs
M  crates/stela-runtime/src/restore/session/tests.rs
```

## 四、状态与提醒

- v21 已通过 550 项 GPU 测试、2 项停顿监测测试，UEFI/Windows 构建成功，
  U 盘 EFI 与离线镜像哈希核对通过；**设置界面整机冻结是否彻底消除仍未实机验证**。
- 上游 v0.4.1 包含了本修改版手工移植的多数修复，另有大量 boot/PCI/IOMMU/GPU/NVMe
  改进与 CLI 破坏性重构（`stela setup` 改为 `create/update/repair/status/settings/log`）。
  若要升级，应基于 v0.4.1 新建分支逐个移植本分支独有的功能，而不是把两者直接合并。
- 只读测速：SSD ≈ 1424 MiB/s、U 盘 ≈ 147 MiB/s，没有证据表明磁盘读速是冻结主因。
  Stela 把 Apple Silicon 版 macOS 的 ARM64 指令翻译到 x86-64 并模拟苹果硬件，
  明显慢于传统黑苹果属预期。
- **断点续装未实现**：不能仅凭 ASR 完成状态跳过后续安装步骤。
