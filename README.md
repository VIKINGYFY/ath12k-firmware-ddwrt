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

## DD-WRT 固件同步（2026-10-07）

来源为用户提供的 `dd-wrt-router_firmwares_wireless_ath12k.zip`，SHA256：
`da38f099db871181486f7c57984c69c59e9d2f94631d3deda2d4d14ef02b519d`。按目录逐文件核对提供的固件及配置；`mem_headroom_check.sh`、`mem_headroom.txt` 属于分析工具，保留在来源 ZIP 中。

| 芯片 | 固件版本 | 仓库目录 | 验证范围 |
|---|---|---|---|
| IPQ5424 | WLAN.WBE.1.7-01733 | `IPQ5424/hw1.0` | 文件完整性和源码归档；未实机验证 |
| QCN9589 | WLAN.WBO.1.0-01618 | `QCN9589/hw1.0` | 文件完整性和源码归档；未实机验证 |
| QCN9625 | WLAN.WBO.1.0-01618 | `QCN9625/hw1.0` | 文件完整性和源码归档；未实机验证 |
| QCN9625 V2 | WLAN.WBO.1.0-01618 | `QCN9625/hw2.0` | 文件完整性和源码归档；未实机验证 |

IPQ5332 的 `5332.wlanfw.eval`、`5332.wlan_fw2.mia_peb_eval` 和
`5332.wlan_fw2.mia_peb_peb_eval_cs` 三个 `WLAN.WBE.1.7-01733` 候选，
已在 BE6500 当前驱动、WSI 拓扑及 QCN9274 `WLAN.WBE.1.6-01270` 配套下分别进行启动阶段测试。
每次核对实际加载的 20 个 MDT/分段文件后，均观察到 Q6 `type fatal error` 和
`rootPD crashed`，未通过初始化，未开展吞吐验收。测试后恢复原固件和双频 AP，
因此本仓库 IPQ5332 保留原来的 1.6 配对。这一结果仅适用于上述硬件与软件组合，
不用于判断候选在其他板型或完整 1.7 配套中的兼容性。

## 硬件版本目录

| ath12k 硬件标识 | 仓库目录 | 固件载荷 |
|---|---|---|
| `ATH12K_HW_QCN9274_HW20` | `QCN9274/hw2.0` | 已有 |
| `ATH12K_HW_WCN7850_HW20` | `WCN7850/hw2.0` | 已有 |
| `ATH12K_HW_IPQ5332_HW10` | `IPQ5332/hw1.0` | 已有 |
| `ATH12K_HW_IPQ5424_HW10` | `IPQ5424/hw1.0` | 已有 |
| `ATH12K_HW_QCN6432_HW10` | `QCN6432/hw1.0` | 尚缺，仅保留目录说明 |
| `ATH12K_HW_QCN9625_HW10` | `QCN9625/hw1.0` | 已有 |
| `ATH12K_HW_QCN9625_HW20` | `QCN9625/hw2.0` | 已有 |
| `ATH12K_HW_QCN9589_HW10` | `QCN9589/hw1.0` | 已有 |

QCN9589 和 QCN9625 文件按硬件版本整理，原 `QCN9625_V2` 移至
`QCN9625/hw2.0`；固件内容及内部版本保持不变。QCN6432 尚无可核验的固件来源，
不使用其他芯片的载荷代替。
