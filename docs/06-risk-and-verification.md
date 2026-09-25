# 06 · 风险与核实

---

## 风险提示

- 解锁 Bootloader、刷内核均会**清空数据或影响保修**，请自行评估。
- 刷错机型的内核、跨 Android 大版本混刷 recovery / 内核，都可能导致**无法开机**。
- 本教程涉及的第三方网盘分享、社区维护的 recovery 版本，请自行核对来源与文件完整性。
- SuSFS 等隐藏方案属于"提高隐藏能力"，**不能保证** 100% 通过所有 App 的检测（如银行、Play Integrity 类校验）。

---

## 内容核实说明

### 已核实为准确的部分

- `ruby` = 红米 Note 12 Pro 5G / Pro+ 5G，天玑 1080，内核为 **4.19.325**（非 GKI 4.x 内核）。
- `Hypermoon_kernel_xiaomi_ruby` 仓库与 `1.0.2` 版本存在，发布页确有两个包：KSU 版与 Vanilla 版。
- 橙狐刷内核的正确入口是 **文件（Files）区 → 选中内核 zip → 滑动刷入**。
- 橙狐对 ruby **按 Android 13/14 与 Android 15 分版本**（与原文的版本区分说法一致）。
- 非 GKI 内核需自行集成 KernelSU；KernelSU-Next 支持 4.4～6.6。

### 需要修正或存疑的部分

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

---

- 上一章：[05 常见问题](05-faq.md)
- 返回：[文档索引](README.md)
