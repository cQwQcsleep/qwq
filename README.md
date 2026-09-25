# qwq

红米 Note 12 Pro 5G（`ruby`）刷机文档站。

## 概要

| 项目 | 内容 |
|---|---|
| 目标机型 | 红米 Note 12 Pro 5G / Note 12 Pro+ 5G（`ruby`，天玑 1080） |
| 内核 | HyperMoon（4.19.325，非 GKI「四系」内核） |
| 目标系统 | MIUI / 澎湃 OS（HyperOS） |
| 集成内容 | KernelSU（KSU 版内置）+ SuSFS |

## 文档目录

- [文档索引](docs/README.md)
- [01 · 前言与结论](docs/01-introduction.md)
- [02 · 准备工作](docs/02-preparation.md)
- [03 · 资源下载](docs/03-downloads.md)
- [04 · 刷机步骤](docs/04-flashing.md)
- [05 · 常见问题](docs/05-faq.md)
- [06 · 风险与核实](docs/06-risk-and-verification.md)

## 快速开始

```bash
# 1. 进 fastboot
adb reboot fastboot
# 2. 刷入橙狐 Recovery（MTK 机型 ramdisk 在 boot）
fastboot flash boot boot.img
# 3. 重启进橙狐 → 文件 → 选中 HyperMoon-KSU-*.zip → 滑动刷入 → 重启系统
```

> 详细步骤与排错见 [04 刷机步骤](docs/04-flashing.md)、[05 常见问题](docs/05-faq.md)。

## 免责声明

刷机有风险，解锁 Bootloader 与刷内核可能清空数据、影响保修，甚至导致无法开机。请先阅读 [06 风险与核实](docs/06-risk-and-verification.md) 并自行评估。
