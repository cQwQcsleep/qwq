# 04 · 刷机步骤

> 顺序很重要：**先刷 Recovery，再用 Recovery 刷内核**。原文把这两步穿插在一起讲，容易看错顺序。

---

## 第 1 步：刷入橙狐 Recovery

先把橙狐镜像刷进 `boot` 分区（MTK 机型 ramdisk 在 boot 里，所以恢复模式是刷 `boot`）：

```bash
# 手机开机状态下，先进 fastboot 模式
adb reboot fastboot

# 刷入橙狐（boot.img 换成你下载的橙狐镜像路径）
fastboot flash boot boot.img
```

如果你不想用命令行，也可以用手机端工具箱（如玩机助手、紫罗兰工具箱等）：
让手机进入 **fastboot 模式** → 选择"刷入 boot" → 选择橙狐镜像 → 刷入。

刷完后重启到 recovery，你会看到橙狐的界面。

---

## 第 2 步：在橙狐里刷入 HyperMoon 内核

1. 进入橙狐后，点下方的 **文件（Files）** 区域；
2. 找到你下载的 HyperMoon 压缩包（`HyperMoon-KSU-….zip`）；
3. 点击它 → **滑动确认刷入**；
4. 刷入完成后，**重启系统**。

重启进系统后打开 KernelSU 管理器，如果显示"工作中 / Working"，说明内核与管理器已正常对接，root 生效。

---

## 第 3 步：刷完后的建议

- 想要隐藏 root（过检测类 App）时，可再安装 SuSFS 用户态模块 `ksu_module_susfs`，配合内核里已集成的 SuSFS 补丁使用。
- 官方 release notes 提醒的"干净安装"流程（**升级 / 换内核前**）：
  1. 备份所有模块；
  2. 卸载所有模块 → 重启；
  3. 删除 `/data/adb/ksu` 和 `/data/adb/ksud` 目录；
  4. 重启进 recovery 刷入新内核。

---

## 快速校验

```bash
# 查看当前内核版本，确认刷入生效
adb shell uname -a

# 确认 KernelSU 目录已生成（root 生效后才有）
adb shell su -c "ls /data/adb/ksu"
```

---

- 上一章：[03 资源下载](03-downloads.md)
- 下一章：[05 常见问题](05-faq.md)
- 返回：[文档索引](README.md)
