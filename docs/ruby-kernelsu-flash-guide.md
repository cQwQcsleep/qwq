# 红米 Note 12 Pro 5G（ruby / 4.19 内核）刷 HyperMoon 内核 + KernelSU 教程

> 适用机型：红米 Note 12 Pro 5G / Note 12 Pro+ 5G（代号 `ruby`，天玑 1080）
> 内核版本：`4.19.325`（非 GKI 的 4.x 内核，即俗称的"四系内核"）
> 适用系统：MIUI / 澎湃 OS（HyperOS），**不适用于 AOSP 类原生系统**

---

## 一、结论：四系内核能用 KernelSU 吗？

**能，但有一个前提。**

- KernelSU 官方从 **v1.0 起就放弃了非 GKI 设备（含 4.14 / 4.19）的官方支持**，最后一个支持非 GKI 的版本是 `v0.9.5`，官方也**不会**为非 GKI 设备提供现成的 boot 镜像。
- 官方给的路子是：**把 KernelSU 集成进设备内核源码，然后自己编译内核**。对普通用户来说，等价做法就是直接刷"别人已经集成好 KernelSU 的第三方内核"——本教程用的 HyperMoon 就是这种内核。
- 如果你的内核没集成 KernelSU，管理器装上去只会显示"不支持/未安装"。
- 补充：KernelSU 的分支 **KernelSU-Next 明确支持 4.4～6.6 内核**（4.x～5.4 走非 GKI 的 LTS 模式），SukiSU Ultra 等分支同样支持非 GKI，所以四系内核的可选方案并不少。

---

## 二、开始前的准备（这一步别跳）

| 项目 | 说明 |
|---|---|
| 解锁 Bootloader | 必须。小米机型需要绑定账号并等待解锁期，解锁会清空数据 |
| 备份数据 | 刷机有风险，重要数据先备份 |
| **备份原厂 boot.img** | **最关键的一步**。刷失败/不开机时，用 fastboot 刷回原厂 boot 即可救回 |
| 原厂线刷包 | 建议提前下载好本机型的 Fastboot 线刷包，作为最后兜底 |
| 电脑环境 | 装好 fastboot 驱动（或用手机端工具箱，见下） |

**硬性警告（来自内核维护者的官方 wiki）**：

1. **必须等手机完成开机向导、系统设置全部走完，才能刷内核。** 如果在新机初始化之前刷，会卡在开机向导无法继续。
2. **HyperMoon 只用于 MIUI / HyperOS。** 如果你刷的是 AOSP / 类原生系统，要用作者的另一个内核 MoonWake。
3. 刷内核前请确认内核版本与你的 ROM 底包（Android 13 / 14 / 15）匹配，**不要跨 Android 大版本混刷**。

---

## 三、需要下载的三样东西

### 1. HyperMoon 内核

下载地址（选最新版）：
https://github.com/DXRN-MoonWake/hypermoon_kernel_xiaomi_ruby/releases/latest

以 `1.0.2` 版本为例，发布页里有两个包，**名字和用途必须分清**：

| 文件 | 说明 | 建议 |
|---|---|---|
| `HyperMoon-KSU-1.0.2-….zip` | **已内置 KernelSU**，并集成 SuSFS（隐藏 root 用的内核级补丁，1.0.2 已更新到 SuSFS 2.0.0） | **推荐用这个**，隐藏性好 |
| `HyperMoon-Vanilla-1.0.2-….zip` | Vanilla 纯净版，**不含任何额外 patch，也就没有 root** | 只想要省电/性能内核、不要 root 的人用 |

> 原文说"第一个是 susfs 版本、第二个是普通版"——方向对，但准确说法是**第一个是 KSU 版（含 SuSFS）、第二个是 Vanilla 纯净版**。注意：**刷 Vanilla 版管理器和 root 都不会出现**，别下错。

### 2. root 管理器（App）

任选其一（KSU 版内核可使用官方 KernelSU、KernelSU-Next、SukiSU Ultra、KowSU 等管理器）：

- KernelSU：https://github.com/tiann/KernelSU/releases
- KernelSU-Next：https://github.com/KernelSU-Next/KernelSU-Next/releases
- SukiSU Ultra：https://github.com/SukiSU-Ultra/SukiSU-Ultra/releases

> 内核维护者的 release notes 里明确写了：**KSU variant 可以搭配 KSUN / SukiSU / KowSU / 官方 KSU 管理器使用**。所以"管理器与内核不匹配"时，换一个管理器分支往往是更省事的解法（详见第六节）。

### 3. 橙狐 Recovery（OrangeFox）

- 官方站点/文档：https://orangefox.download/ ｜ https://wiki.orangefox.tech/
- 本机型（ruby）的橙狐由社区维护者分发，官方 wiki 指向的是维护者的更新频道，且**分两个版本**：
  - **Android 13 / 14 用一版**
  - **Android 15 用另一版**

> 原文提到的 `R12.1.5 / R12.1.6` 以及第三方网盘（123pan）分享链接，我无法核实其来源、版本号与文件安全性，**建议优先从橙狐官方站点或内核维护者给出的渠道下载**，并核对文件名是否对应你的 Android 版本。混用不同 Android 版本的 recovery 容易出现解密失败、无法挂载分区等问题。

---

## 四、刷机步骤

### 第 1 步：刷入橙狐 Recovery

先把橙狐镜像刷进 `boot` 分区（MTK 机型 ramdisk 在 boot 里，所以恢复模式是刷 `boot`）：

```bash
# 手机开机状态下，先进 fastboot 模式
adb reboot fastboot

# 刷入橙狐（boot.img 换成你下载的橙狐镜像路径）
fastboot flash boot boot.img
```

如果你不想用命令行，也可以用手机端工具箱（如玩机助手、紫罗兰工具箱等）：让手机进入 **fastboot 模式** → 选择"刷入 boot" → 选择橙狐镜像 → 刷入。

刷完后重启到 recovery，你会看到橙狐的界面。

### 第 2 步：在橙狐里刷入 HyperMoon 内核

1. 进入橙狐后，点下方的 **文件（Files）** 区域；
2. 找到你下载的 HyperMoon 压缩包（`HyperMoon-KSU-….zip`）；
3. 点击它 → **滑动确认刷入**；
4. 刷入完成后，**重启系统**。

重启进系统后打开 KernelSU 管理器，如果显示"工作中 / Working"，说明内核与管理器已正常对接，root 生效。

---

## 五、刷完后的建议

- 想要隐藏 root（过检测类 App）时，可再安装 SuSFS 用户态模块 `ksu_module_susfs`，配合内核里已集成的 SuSFS 补丁使用。
- 官方 release notes 提醒的"干净安装"流程（**升级/换内核前**）：
  1. 备份所有模块；
  2. 卸载所有模块 → 重启；
  3. 删除 `/data/adb/ksu` 和 `/data/adb/ksud` 目录；
  4. 重启进 recovery 刷入新内核。

---

## 六、常见问题

### Q1：打开管理器提示"管理器与内核不匹配 / 内核版本过低，请升级到 xxx"

**这是原文第五条说的情况，处理方向正确：降级管理器（或换管理器分支）。**

原因是内核里内置的 KernelSU 版本较旧，而你装的管理器是最新版，管理器会报类似这样的提示：

> The Current KernelSU Version x is too low for the manager to work properly. Please upgrade to version y or higher!

三种可行解法（按推荐顺序）：

1. **换成 KernelSU-Next 管理器**：本内核的 KSU 版本兼容 KSUN / SukiSU / KowSU 等管理器，实测换 Next 后即可显示 Working；
2. **降级管理器**：卸载新版管理器，装与内核内置 KSU 版本匹配的旧版（社区经验是**非 GKI 内核可降到 1.0.1 左右**）；
3. **不要点管理器的"更新"**：降级后仍会提示有新版本，忽略即可，一旦升级会再次报错。

### Q2：换了管理器还是不对

release notes 给出的规则：**换管理器时，先卸载旧管理器 → 删除 `/data/adb/ksud` 文件 → 再安装新管理器。**

### Q3：刷完完全没有 root

先确认你刷的不是 `HyperMoon-Vanilla-….zip`。Vanilla 版不含 KernelSU，必须刷 **KSU 版**。

### Q4：刷完卡开机动画 / 无限重启

用 fastboot 刷回你备份的原厂 `boot.img`：

```bash
fastboot flash boot 原厂boot.img
```

必要时用原厂 Fastboot 线刷包完整回厂。

---

## 七、风险提示

- 解锁 Bootloader、刷内核均会**清空数据或影响保修**，请自行评估。
- 刷错机型的内核、跨 Android 大版本混刷 recovery/内核，都可能导致**无法开机**。
- 本教程涉及的第三方网盘分享、社区维护的 recovery 版本，请自行核对来源与文件完整性。
- SuSFS 等隐藏方案属于"提高隐藏能力"，**不能保证** 100% 通过所有 App 的检测（如银行、Play Integrity 类校验）。

---

## 八、内容核实说明

已核实为准确的部分：

- `ruby` = 红米 Note 12 Pro 5G / Pro+ 5G，天玑 1080，内核为 **4.19.325**（非 GKI 4.x 内核）。
- `Hypermoon_kernel_xiaomi_ruby` 仓库与 `1.0.2` 版本存在，发布页确有两个包：KSU 版与 Vanilla 版。
- 橙狐刷内核的正确入口是 **文件（Files）区 → 选中内核 zip → 滑动刷入**。
- 橙狐对 ruby **按 Android 13/14 与 Android 15 分版本**（与原文的版本区分说法一致）。
- 非 GKI 内核需自行集成 KernelSU；KernelSU-Next 支持 4.4～6.6。

需要修正或存疑的部分：

- "第一个是 susfs 版本、第二个是普通版" → 应为 **KSU 版（含 SuSFS）** 与 **Vanilla 纯净版（无 root）**。
- 原文步骤顺序互相穿插（先讲用橙狐刷 kernel，第三步才讲装橙狐），本教程已按正确顺序重排。
- `R12.1.5 / R12.1.6` 这两个具体版本号、以及 123pan 网盘链接，**无法从官方渠道核实**，请以实际下载到的文件与维护者说明为准。
- "管理器与内核不匹配就降级管理器"属实，但属**社区经验**而非官方文档结论；官方更推荐的做法是让管理器与内核内置 KSU 版本匹配，或改用 KernelSU-Next。

---

## 参考来源

- [HyperMoon 1.0.2 Release（DXRN-MoonWake/hypermoon_kernel_xiaomi_ruby）](https://github.com/DXRN-MoonWake/hypermoon_kernel_xiaomi_ruby/releases/tag/1.0.2)
- [Flash MoonWake / HyperMoon MIUI HyperOS（维护者官方 wiki）](https://github.com/DXRN-MoonWake/moonwake_kernel_xiaomi_ruby/wiki/Flash-MoonWake---HyperMoon-MIUI-HyperOS)
- [如何为非 GKI 内核集成 KernelSU（官方文档，存档）](https://kernelsu.org/zh_CN/guide/how-to-integrate-for-non-gki.html)
- [KernelSU 常见问题（非 GKI 支持、内核版本与 Android 版本的关系）](https://kernelsu.org/zh_CN/guide/faq.html)
- [KernelSU 安装文档（KMI、备份 boot.img）](https://kernelsu.org/zh_CN/guide/installation.html)
- [KernelSU Next 官网（支持 4.4～6.6，非 GKI LTS 模式）](https://kernelsu-next.github.io/webpage/zh_CN/)
- [The Current KernelSU Version is too low [Fixed]（管理器版本过低与降级解法）](https://droidwin.com/the-current-kernelsu-version-1-is-too-low-fixed/)
- [KernelSU 升级问题，如何正确更新管理器（ZOL 问答，非 GKI 降级经验）](https://ask.zol.com.cn/x/29314041.html)
- [REDMI Note 12 Pro 5G / Pro Plus ROM（codename ruby）](https://miuirom.org/phones/redmi-note-12-pro)
- [OrangeFox Recovery 官网](https://orangefox.tech/) ｜ [下载页](https://orangefox.download/) ｜ [Wiki](https://wiki.orangefox.tech/)
- [What Is SUSFS?（SuSFS 作用与配合 KernelSU 使用）](https://www.privacyportal.co.uk/blogs/free-rooting-tips-and-tricks/what-is-susfs)
