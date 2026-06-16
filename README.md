# 📘 SSDT 从入门到精通

**ACPI 热补丁完全指南 · 硬件定制 · Hackintosh BIOS 修复 · 实战案例**

<div align="center">

![ACPI](https://img.shields.io/badge/Topic-ACPI_SSDT-00A3E0?style=flat-square)
![Platform](https://img.shields.io/badge/Platform-macOS_|_Hackintosh-0071BC?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)
![Status](https://img.shields.io/badge/Status-Maintained-success?style=flat-square)

**[📖 什么是 SSDT？](#什么是-ssdt) · [🛠️ 工具](#工具集) · [📋 常见表](#常见-ssdt-表) · [🎓 实战案例](#实战案例)**

</div>

---

## 📌 快速概览

| 项目 | 说明 |
|:---:|:-----|
| **SSDT** | Secondary System Description Table - ACPI 二级系统描述表 |
| **用途** | 扩展/修复 DSDT，定制硬件设备支持 |
| **难度** | ⭐⭐ 中等（自动工具简化流程） |
| **重要性** | ⭐⭐⭐ 必需（Hackintosh 的基础） |

---

## 🎯 目录导航

| 章节 | 内容 |
|:-----|:-----|
| [什么是 SSDT？](#什么是-ssdt) | ACPI 基础概念 |
| [SSDT 的作用](#ssdt-的作用) | 为什么需要定制 SSDT？ |
| [必需工具](#工具集) | 下载和安装工具 |
| [创建方式](#创建-ssdt-的方式) | 自动生成 vs 手工编写 |
| [常见表详解](#常见-ssdt-表) | EC、PLUG、USBX 等 |
| [实战案例](#实战案例) | 逐步指南 |
| [故障排除](#故障排除) | 常见问题解决 |

---

## 🔹 什么是 SSDT？

### 📖 基本概念

**SSDT（Secondary System Description Table）** 是 ACPI（高级配置和电源接口）规范的一部分，用于描述系统硬件配置。

在 Hackintosh 中：
- **DSDT** = 主系统描述表（由 BIOS 提供，通常不修改）
- **SSDT** = 次级表（我们创建，用于补充和修复）

### 🔄 BIOS 启动流程

```
启动 → BIOS 读取 DSDT → 加载所有 SSDT → OS 识别硬件
        ↑（UEFI）        ↑（我们注入）
```

---

## 🎯 SSDT 的作用

### ✅ 为什么 Hackintosh 需要 SSDT？

| 问题 | SSDT 解决方案 |
|:-----|:----------|
| **无法识别 EC（嵌入式控制器）** | SSDT-EC：仿冒或修复 EC 设备 |
| **CPU 无法动态频率调节** | SSDT-PLUG：启用 XCPM 电源管理 |
| **USB 设备断电** | SSDT-USBX：设置 USB 电源属性 |
| **笔记本屏幕无法调整亮度** | SSDT-PNLF：背光控制设备 |
| **睡眠/唤醒失败** | SSDT-AWAC 或 SSDT-RTC：修复时钟 |
| **显卡未被识别** | SSDT-GPU-SPOOF：伪装不兼容的 GPU |

### 💡 实际例子

```
原生 DSDT（来自 BIOS）：
├── Device (PCI0)
│   ├── Device (LPCB)  ← 这里没有 EC 设备
│   └── Device (HECI)
└── Device (EHC1)

补充后（+SSDT-EC）：
├── Device (PCI0)
│   ├── Device (LPCB)
│   │   └── Device (EC0)  ← ✅ 新增，macOS 可识别
│   ├── Device (HECI)
│   └── Device (MCHC)
└── ...
```

---

## 🛠️ 工具集

### 必需工具

| 工具 | 功能 | 平台 | 推荐度 |
|:-----|:-----|:----:|:-----:|
| **SSDTTime** | 自动生成常见 SSDT | Mac | ⭐⭐⭐ |
| **MaciASL** | 编辑/编译 SSDT | Mac | ⭐⭐⭐ |
| **iASL** | ACPI 编译器 | Windows/Mac/Linux | ⭐⭐ |
| **Hackintool** | 诊断和导出 ACPI | Mac | ⭐⭐ |
| **ProperTree** | Plist 编辑 | 跨平台 | ⭐⭐ |

### 下载链接

```bash
# SSDTTime（推荐新手）
git clone https://github.com/corpnewt/SSDTTime.git
cd SSDTTime && ./SSDTTime.command

# MaciASL（ACPI 编辑器）
# App Store 搜索 "MaciASL" 或 GitHub 下载

# iASL 编译器（跨平台）
# https://acpica.org/downloads/iasl-compiler
```

---

## 📋 创建 SSDT 的方式

### 方式 1️⃣ 自动生成（推荐新手）

**SSDTTime（最简单）**

```bash
1. 启动 SSDTTime.command
2. 选择菜单选项（通常是 "A"）
3. 选择要生成的 SSDT 类型
4. 输出文件在 Results 文件夹
5. 复制 .aml 文件到 EFI/OC/ACPI
6. 在 config.plist 中引用
```

### 方式 2️⃣ 半自动编辑

**适合定制化需求**

```bash
步骤 1: 从 BIOS 导出 ACPI 表
-------
启动 OpenCore（OC 菜单 → Tools → Reset NVRAM）
重启 → 运行 Hackintool → Utilities → Extract ACPI

步骤 2: 反编译
-------
iasl -d DSDT.aml SSDT*.aml

步骤 3: 编辑
-------
用 MaciASL 打开 .dsl 文件
修改设备定义或添加方法

步骤 4: 编译
-------
MaciASL 中按 Compile（Cmd+K）
或：iasl SSDT-EC.dsl

步骤 5: 测试
-------
复制 .aml 到 EFI → 重启测试
```

### 方式 3️⃣ 完全手写（高级）

适合深度定制（不推荐初学者）

---

## 📋 常见 SSDT 表详解

### 🔴 必需表

#### SSDT-EC.aml - 嵌入式控制器修复

**问题**：macOS 需要一个 EC（Embedded Controller）设备，但大多数 BIOS 中没有或命名不同

**解决**：
```asl
// 简化示例（实际复杂得多）
Device (EC0)
{
    Name (_HID, "APPLE0005")
    ...
}
```

**何时需要**：✅ 几乎总是

**生成方式**：
```bash
SSDTTime → Option 4 (EC Fix)
```

---

#### SSDT-PLUG.aml - CPU 电源管理

**问题**：macOS 无法识别 CPU 的动态频率调节（XCPM）

**解决**：注入 `plugin-type = 1` 属性

**何时需要**：✅ 所有平台（必需）

**适用 CPU**：Intel 6th-gen+，AMD Ryzen（需特殊配置）

**生成方式**：
```bash
SSDTTime → Option 1 (PLUG)
```

---

#### SSDT-USBX.aml - USB 电源属性

**问题**：USB 设备断电、无法充电、工作不稳定

**解决**：设置正确的 USB 电源限制

**何时需要**：⚠️ 推荐所有平台

**生成方式**：
```bash
SSDTTime → Option 5 (USB Reset)
```

---

### 🟡 条件表

#### SSDT-PNLF.aml - 笔记本背光

| 场景 | 需要 |
|:-----|:----:|
| 台式机 | ❌ |
| 笔记本（6-10 代 Intel） | ✅ |
| 笔记本（AMD Ryzen）| ⚠️ |

**生成方式**：
```bash
SSDTTime → Option 6 (PNLF)
```

---

#### SSDT-HPET.aml - HPET 中断修复

**问题**：系统启动卡顿或崩溃（IRQ 冲突）

**需要情况**：
- AMD B450/B550 主板
- 某些旧版 Intel 主板

**解决**：禁用冲突的 IRQ

---

### 🟢 设备仿冒表

#### SSDT-GPU-SPOOF（GPU 设备伪装）

**示例**：AMD RX 6950 XT 在旧版 macOS 中不原生支持

**解决方案**：伪装成支持的型号

```asl
Device (GFX0)
{
    Name (_HID, "ACPI0002")
    // Device-ID 仿冒
    Method (_DSM, 4, NotSerialized)
    {
        // 返回 macOS 支持的 Device-ID
    }
}
```

---

## 🎓 实战案例

### 📚 案例 1：基础 SSDT-EC 创建

**场景**：新安装 Hackintosh，EC 设备无法识别

**步骤**：

1. **启动 SSDTTime**
   ```bash
   cd ~/SSDTTime
   ./SSDTTime.command
   ```

2. **选择选项**
   ```
   A) Dump ACPI tables from Windows
   B) Dump ACPI tables from a Linux live USB
   C) Dump ACPI tables from a Mac
   
   → 选择 C（从 Mac/Hackintosh）
   ```

3. **选择 EC Fix**
   ```
   4) Dump EC (and fake EC)
   → 按 Enter
   ```

4. **获取结果**
   ```
   SSDT-EC.aml  ← 生成的文件
   SSDT-EC.dsl  ← 源代码（可选查看）
   ```

5. **部署**
   ```bash
   cp SSDT-EC.aml /EFI/OC/ACPI/
   
   # 在 config.plist 中添加：
   <dict>
       <key>Comment</key>
       <string>EC Fix</string>
       <key>Enabled</key>
       <true/>
       <key>Path</key>
       <string>SSDT-EC.aml</string>
   </dict>
   ```

6. **测试**
   ```bash
   重启系统 → 在 Hackintool 中验证 EC 设备
   ```

---

### 📚 案例 2：CPU 电源管理优化

**场景**：CPU 无法动态调频，性能和功耗都有问题

**解决步骤**：

1. **生成 PLUG 表**
   ```bash
   SSDTTime → Option 1 (PLUG)
   ```

2. **验证 SMBIOS**
   ```
   确保 config.plist 中的 SMBIOS 正确
   （不同 CPU 代数要用不同机型）
   ```

3. **启用 CPUFriend**（可选优化）
   ```bash
   # 下载 CPUFriend.kext
   # 生成 CPUFriendDataProvider.kext
   # 两个 Kext 都放入 EFI/OC/Kexts
   ```

4. **重启验证**
   ```bash
   关于本机 → 系统报告 → 处理器
   应显示动态频率（1.8 GHz - 4.2 GHz）
   ```

---

## 🔧 故障排除

### ❌ 常见问题

| 问题 | 症状 | 解决方�� |
|:-----|:-----|:------:|
| SSDT 编译错误 | `Invalid file` | 检查 ASL 语法，用 MaciASL 验证 |
| EC 设备仍未识别 | 系统报告无 EC | 尝试不同的 EC 设备名称（EC/EC0/H_EC） |
| 系统启动卡顿 | 黑屏 5+ 秒 | 移除 SSDT-HPET，逐个测试表 |
| USB 设备掉线 | 断连重连 | 创建 USB 端口映射而不是 SSDT-USBX |
| 文件不被加载 | config.plist 中未显示 | 检查 .aml 文件是否存在于正确路径 |

### 🐛 调试技巧

```bash
# 1. 启用详细日志
boot-args: -v debug=0x100 keepsyms=1

# 2. 收集启动日志
log show --last boot > ~/debug.log

# 3. 检查 ACPI 加载情况
ioreg -l | grep -i "ACPI" | head -20

# 4. 验证 SSDT 是否加载
ioreg -l | grep -i "SSDT-EC"
```

---

## 📖 参考资源

### 官方文档

| 资源 | 链接 |
|:-----|:----:|
| **ACPI 规范** | [uefi.org](https://uefi.org/specifications) |
| **OpenCore 指南** | [Dortania](https://dortania.github.io/OpenCore-Install-Guide/) |
| **ACPI 入门** | [Dortania ACPI Guide](https://dortania.github.io/Getting-Started-With-ACPI/) |
| **MaciASL 参考** | [GitHub](https://github.com/acidanthera/MaciASL) |

### 学习资源

- 📺 YouTube：搜索 "Hackintosh SSDT 教程"
- 📖 Reddit：[r/Hackintosh](https://reddit.com/r/hackintosh)
- 💬 论坛：[InsanelyMac](https://insanelymac.com)

### 工具项目

| 项目 | 功能 |
|:-----|:-----|
| [SSDTTime](https://github.com/corpnewt/SSDTTime) | 自动生成 SSDT |
| [MaciASL](https://github.com/acidanthera/MaciASL) | ACPI 编辑器 |
| [OpenCore](https://github.com/acidanthera/OpenCorePkg) | 官方文档和样本 |

---

## 📄 许可证

本指南采用 MIT License 发布，仅供学习和研究使用。

> ⚠️ **免责声明**：在非 Apple 硬件上安装 macOS 可能违反 Apple EULA

---

<p align="center">
  <b>🍎 为 Hackintosh 社区贡献</b><br/>
  <sub>最后更新：2026年 · ACPI 最佳实践 · OpenCore 兼容</sub>
</p>
