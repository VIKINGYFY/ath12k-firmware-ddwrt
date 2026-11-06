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

## DD-WRT 固件同步（2026-11-07）

DD-WRT 固定来源：[c5b9d5d25e10](https://github.com/mirror/dd-wrt/tree/c5b9d5d25e10c170419729b19b2e1a79d58871d4/src/router/firmwares/wireless/ath12k)。
对应的最近固件更新提交为 `8fce77470c94`。固件及配置按 Git blob 核对；
`mem_headroom_check.sh`、`mem_headroom.txt` 属于分析工具，不作为固件载荷收录。

| 芯片 | 固件版本 | 仓库目录 | 验证范围 |
|---|---|---|---|
| IPQ5424 | WLAN.WBE.1.7-01831 | `IPQ5424/hw1.0` | 文件完整性；未实机验证 |
| QCN9589 | WLAN.WBO.1.0-01765 | `QCN9589/hw1.0` | 文件完整性；未实机验证 |
| QCN9625 | WLAN.WBO.1.0-01765 | `QCN9625/hw1.0` | 文件完整性；未实机验证 |
| QCN9625 V2 | WLAN.WBO.1.0-01765 | `QCN9625/hw2.0` | 文件完整性；未实机验证 |

IPQ5332 默认继续提供 `WLAN.WBE.1.6-01270`，用于当前 WSI 配套。
DD-WRT 的三种 `WLAN.WBE.1.7-01831` 评估镜像不作为该组合的默认载荷。
不同板型或完整 1.7 配套的兼容性须单独核验。

## QCN9274 HW1.0

`QCN9274/hw1.0` 的 AMSS/M3 更新至 `WLAN.WBE.1.0-02903-QCAHKSWPL_SILICONZ-1`。
来源为 Qualcomm 公开固件仓库的[固定提交 c6e9f96890bc](https://github.com/quic/upstream-wifi-fw/tree/c6e9f96890bc70efe97e536db23c97ee18460369/ath12k-firmware/QCN9274/hw1.0/testing/1.0/WLAN.WBE.1.0-02903-QCAHKSWPL_SILICONZ-1)。
原始 `Notice.txt` 一并保留，板级数据和校准文件由设备固件提供。
BE7000 ES 已验证基本连接、受控单链路流量和流量后的驱动重载；
此范围不代表长期稳定性、MLO 客户端或全部 RDDM 触发场景已验证。

所有硬件目录均提供 `fw_version.txt`，可识别版本与二进制内嵌版本核对。
QCN6432 尚缺完整载荷，其版本文件明确标为 `UNAVAILABLE`。

## 硬件版本目录

| ath12k 硬件标识 | 仓库目录 | 固件载荷 |
|---|---|---|
| `ATH12K_HW_QCN9274_HW10` | `QCN9274/hw1.0` | Qualcomm 公开 02903，原始许可见 `Notice.txt` |
| `ATH12K_HW_QCN9274_HW20` | `QCN9274/hw2.0` | 已有 |
| `ATH12K_HW_WCN7850_HW20` | `WCN7850/hw2.0` | 已有 |
| `ATH12K_HW_IPQ5332_HW10` | `IPQ5332/hw1.0` | 已有 |
| `ATH12K_HW_IPQ5424_HW10` | `IPQ5424/hw1.0` | 已有 |
| `ATH12K_HW_QCN6432_HW10` | `QCN6432/hw1.0` | 尚缺，仅保留目录说明 |
| `ATH12K_HW_QCN9625_HW10` | `QCN9625/hw1.0` | 已有 |
| `ATH12K_HW_QCN9625_HW20` | `QCN9625/hw2.0` | 已有 |
| `ATH12K_HW_QCN9589_HW10` | `QCN9589/hw1.0` | 已有 |

QCN9589 和 QCN9625 文件按硬件版本整理，原 `QCN9625_V2` 对应
`QCN9625/hw2.0`。QCN6432 尚缺完整载荷，不使用其他芯片的载荷代替。

## Windows 检出兼容

Windows 将 `AUX` 及带扩展名的 `aux.bin` 视为保留设备名，Git 无法检出该路径。
QCN9589、QCN9625 hw1.0/hw2.0 的辅助固件现以 `aux_ucode.bin` 保存，
与 Linux ath12k 的 `ATH12K_AUX_UC_FILE` 文件名一致；文件内容没有变化。
此前的失败克隆可在更新到本仓库新提交后正常检出，不需要关闭 `core.protectNTFS`。

如果旧版 DD-WRT 驱动明确请求 `aux.bin`，应在 Linux 的安装阶段将
`aux_ucode.bin` 安装为 `aux.bin`；不要在 Windows 工作树中创建这个保留文件名。
