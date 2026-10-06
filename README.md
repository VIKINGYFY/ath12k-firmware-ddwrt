# Ath12k-Firmware-DDWRT
源码地址：

https://github.com/mirror/dd-wrt/tree/master/src/router/firmwares/wireless/ath12k

## IPQ5332 WSI 配套固件

`IPQ5332/hw1.0` 中的 `q6_fw0`、`q6_fw1`、`iu_fw` MDT/分段文件采用
`WLAN.WBE.1.6-01270`，与本仓库 QCN9274 固件配套，用于 WSI 硬件组初始化。

文件来源：[openwrt-flint3 固定提交 2365932](https://github.com/perceival/openwrt-flint3/tree/2365932733ca8ec3b346621d9cec2eb3df3b2cf3/package/firmware/ath12k-firmware/src/IPQ5332/hw1.0)。
来源归档 SHA256：`8574a516dcbdf9ff9ca055b41951ae116627907d129f5833f9ca8a913660b287`。
同步替换 20 个载荷文件，并移除旧版本的三个 `.flist` 清单；板级 BDF、校准配置和其他芯片固件保持原有内容。

BE6500 的已归档实机对照中，AHB WBE 1.7 / PCI WBE 1.6 在启用 WSI 拓扑后
发生初始化崩溃，换用这套 1.6 配对后恢复双频 AP。此验证范围不包含其他机型的实机验收，
也不代表 MLO 双链路客户端、长时稳定性或吞吐问题已完整验证。
