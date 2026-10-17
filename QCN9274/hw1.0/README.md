# QCN9274 hw1.0

`amss.bin` and `m3.bin` are Qualcomm's public
`WLAN.WBE.1.0.r4-00159-QCAHKSWPL_SILICONZ-1` firmware. This hardware
revision needs its own firmware; the existing hw2.0 payload is not a substitute.

Source: [quic/upstream-wifi-fw at c6e9f968](https://github.com/quic/upstream-wifi-fw/tree/c6e9f96890bc70efe97e536db23c97ee18460369/ath12k-firmware/QCN9274/hw1.0/testing/1.0.r4/WLAN.WBE.1.0.r4-00159-QCAHKSWPL_SILICONZ-1).
The original `Notice.txt` accompanies these unmodified files.

`regdb.bin` is the public Qualcomm ath12k regulatory database previously
distributed with the QCN9274 hw1.0 firmware. It is retained from the tested
BE7000 Pro firmware package, with SHA256
`10e0ab463e5a4e58951629173e70ccdf2ff3970115da131c1ac9f65bfcd8153d`.
All four imported files are covered by `SHA256SUMS`.

Xiaomi BE7000 Pro testing on 2026-10-17 confirmed dual-band AP operation
with a driver supporting hw1.0 full RX descriptors, RXDMA1 and disabled
REO queue lookup offload. The 5 GHz radio reported 5170–5835 MHz.
This does not establish 6 GHz, MLO, EHT-client or long-term stability support.
Board data and per-device ART calibration remain separate board-specific inputs.
