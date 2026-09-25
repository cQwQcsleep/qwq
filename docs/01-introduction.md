# 01 · 前言与结论

> 适用机型：红米 Note 12 Pro 5G / Note 12 Pro+ 5G（代号 `ruby`，天玑 1080）
> 内核版本：`4.19.325`（非 GKI 的 4.x 内核，即俗称的"四系内核"）
> 适用系统：MIUI / 澎湃 OS（HyperOS），**不适用于 AOSP 类原生系统**

---

## 四系内核能用 KernelSU 吗？

**能，但有一个前提。**

- KernelSU 官方从 **v1.0 起就放弃了非 GKI 设备（含 4.14 / 4.19）的官方支持**，最后一个支持非 GKI 的版本是 `v0.9.5`，官方也**不会**为非 GKI 设备提供现成的 boot 镜像。
- 官方给的路子是：**把 KernelSU 集成进设备内核源码，然后自己编译内核**。对普通用户来说，等价做法就是直接刷"别人已经集成好 KernelSU 的第三方内核"——本教程用的 HyperMoon 就是这种内核。
- 如果你的内核没集成 KernelSU，管理器装上去只会显示"不支持 / 未安装"。
- 补充：KernelSU 的分支 **KernelSU-Next 明确支持 4.4～6.6 内核**（4.x～5.4 走非 GKI 的 LTS 模式），SukiSU Ultra 等分支同样支持非 GKI，所以四系内核的可选方案并不少。

---

## 本教程的路线

```
解锁 Bootloader
      ↓
刷入橙狐 Recovery（写入 boot 分区）
      ↓
在 Recovery 里刷入 HyperMoon-KSU 内核 zip
      ↓
重启系统 → 安装 KernelSU 管理器 → 显示「Working」即成功
```

- 下一章：[02 准备工作](02-preparation.md)
- 返回：[文档索引](README.md)
