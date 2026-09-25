# 03 · 资源下载

需要下载三样东西：**内核**（刷进手机）、**Root 管理器**（装在系统里）、**橙狐 Recovery**（用来刷内核）。

---

## 1. HyperMoon 内核

下载地址（选最新版）：
https://github.com/DXRN-MoonWake/hypermoon_kernel_xiaomi_ruby/releases/latest

以 `1.0.2` 版本为例，发布页里有两个包，**名字和用途必须分清**：

| 文件 | 说明 | 建议 |
|---|---|---|
| `HyperMoon-KSU-1.0.2-….zip` | **已内置 KernelSU**，并集成 SuSFS（隐藏 root 用的内核级补丁，1.0.2 已更新到 SuSFS 2.0.0） | **推荐用这个**，隐藏性好 |
| `HyperMoon-Vanilla-1.0.2-….zip` | Vanilla 纯净版，**不含任何额外 patch，也就没有 root** | 只想要省电 / 性能内核、不要 root 的人用 |

> 原文说"第一个是 susfs 版本、第二个是普通版"——方向对，但准确说法是**第一个是 KSU 版（含 SuSFS）、第二个是 Vanilla 纯净版**。
> 注意：**刷 Vanilla 版管理器和 root 都不会出现**，别下错。

---

## 2. Root 管理器（App）

任选其一（KSU 版内核可使用官方 KernelSU、KernelSU-Next、SukiSU Ultra、KowSU 等管理器）：

- KernelSU：https://github.com/tiann/KernelSU/releases
- KernelSU-Next：https://github.com/KernelSU-Next/KernelSU-Next/releases
- SukiSU Ultra：https://github.com/SukiSU-Ultra/SukiSU-Ultra/releases

> 内核维护者的 release notes 里明确写了：**KSU variant 可以搭配 KSUN / SukiSU / KowSU / 官方 KSU 管理器使用**。
> 所以"管理器与内核不匹配"时，换一个管理器分支往往是更省事的解法（详见 [05 常见问题](05-faq.md)）。

---

## 3. 橙狐 Recovery（OrangeFox）

- 官方站点 / 文档：https://orangefox.download/ ｜ https://wiki.orangefox.tech/
- 本机型（ruby）的橙狐由社区维护者分发，官方 wiki 指向的是维护者的更新频道，且**分两个版本**：
  - **Android 13 / 14 用一版**
  - **Android 15 用另一版**

> 原文提到的 `R12.1.5 / R12.1.6` 以及第三方网盘（123pan）分享链接，我无法核实其来源、版本号与文件安全性，**建议优先从橙狐官方站点或内核维护者给出的渠道下载**，并核对文件名是否对应你的 Android 版本。
> 混用不同 Android 版本的 recovery 容易出现解密失败、无法挂载分区等问题。

---

## 下载前自检

- [ ] 内核 zip 是 `KSU` 版，不是 `Vanilla` 版
- [ ] 内核 / recovery 的版本对应我当前的 Android 大版本
- [ ] 管理器已下载到手机（离线安装包，避免刷完没网）
- [ ] 原厂 boot.img 已备份并放在电脑上

---

- 上一章：[02 准备工作](02-preparation.md)
- 下一章：[04 刷机步骤](04-flashing.md)
- 返回：[文档索引](README.md)
