# 05 · 常见问题

---

## Q1：打开管理器提示"管理器与内核不匹配 / 内核版本过低，请升级到 xxx"

**这是原文第五条说的情况，处理方向正确：降级管理器（或换管理器分支）。**

原因是内核里内置的 KernelSU 版本较旧，而你装的管理器是最新版，管理器会报类似这样的提示：

> The Current KernelSU Version x is too low for the manager to work properly. Please upgrade to version y or higher!

三种可行解法（按推荐顺序）：

1. **换成 KernelSU-Next 管理器**：本内核的 KSU 版本兼容 KSUN / SukiSU / KowSU 等管理器，实测换 Next 后即可显示 Working；
2. **降级管理器**：卸载新版管理器，装与内核内置 KSU 版本匹配的旧版（社区经验是**非 GKI 内核可降到 1.0.1 左右**）；
3. **不要点管理器的"更新"**：降级后仍会提示有新版本，忽略即可，一旦升级会再次报错。

---

## Q2：换了管理器还是不对

release notes 给出的规则：**换管理器时，先卸载旧管理器 → 删除 `/data/adb/ksud` 文件 → 再安装新管理器。**

```bash
# 删除残留的 ksud，避免新旧管理器状态冲突
adb shell su -c "rm -f /data/adb/ksud"
```

---

## Q3：刷完完全没有 root

先确认你刷的不是 `HyperMoon-Vanilla-….zip`。Vanilla 版不含 KernelSU，必须刷 **KSU 版**。

排查顺序：

1. 核对刷入的 zip 文件名里有没有 `KSU`；
2. `adb shell uname -a` 确认内核确实换掉了；
3. 确认管理器 App 有正常安装（不是被系统拦截的旧包）。

---

## Q4：刷完卡开机动画 / 无限重启

用 fastboot 刷回你备份的原厂 `boot.img`：

```bash
fastboot flash boot 原厂boot.img
```

必要时用原厂 Fastboot 线刷包完整回厂。

---

## Q5：recovery 里看不到内部存储 / 解密失败

大概率是 **recovery 版本与你的 Android 大版本不匹配**（Android 15 的机器刷了 Android 13/14 版橙狐）。
换用对应版本的橙狐镜像重新刷入 `boot`。

---

- 上一章：[04 刷机步骤](04-flashing.md)
- 下一章：[06 风险与核实](06-risk-and-verification.md)
- 返回：[文档索引](README.md)
