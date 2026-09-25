# 02 · 准备工作

> 这一步别跳。刷机失败最常见的原因不是操作错了，而是没备份。

---

## 准备工作清单

| 项目 | 说明 |
|---|---|
| 解锁 Bootloader | 必须。小米机型需要绑定账号并等待解锁期，解锁会清空数据 |
| 备份数据 | 刷机有风险，重要数据先备份 |
| **备份原厂 boot.img** | **最关键的一步**。刷失败 / 不开机时，用 fastboot 刷回原厂 boot 即可救回 |
| 原厂线刷包 | 建议提前下载好本机型的 Fastboot 线刷包，作为最后兜底 |
| 电脑环境 | 装好 fastboot 驱动（或用手机端工具箱，见 [04 刷机步骤](04-flashing.md)） |

---

## 硬性警告（来自内核维护者的官方 wiki）

1. **必须等手机完成开机向导、系统设置全部走完，才能刷内核。**
   如果在新机初始化之前刷，会卡在开机向导无法继续。
2. **HyperMoon 只用于 MIUI / HyperOS。**
   如果你刷的是 AOSP / 类原生系统，要用作者的另一个内核 MoonWake。
3. **不要跨 Android 大版本混刷。**
   刷内核前请确认内核版本与你的 ROM 底包（Android 13 / 14 / 15）匹配。

---

## 如何备份原厂 boot.img

```bash
# 手机连电脑、开启 USB 调试后，用 adb 拉取当前 boot 分区
adb shell su -c "dd if=/dev/block/by-name/boot of=/sdcard/boot_backup.img"

# 或直接把设备上的 boot 分区镜像拉到电脑
adb pull /sdcard/boot_backup.img
```

> 不同底包的分区名可能略有差异（`boot` / `boot_a`），可用 `adb shell ls /dev/block/by-name/` 确认。
> 更稳妥的做法是直接下载机型的**官方 Fastboot 线刷包**，其中的 `boot.img` 就是原厂镜像。

---

- 上一章：[01 前言与结论](01-introduction.md)
- 下一章：[03 资源下载](03-downloads.md)
- 返回：[文档索引](README.md)
