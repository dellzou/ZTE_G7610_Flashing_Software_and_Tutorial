# 中兴 G7610 开 Telnet 与改码工具包

配套文章：[中兴 G7610 光猫开 Telnet：改地区、改 SN/MAC 与刷固件](https://xiaozou123.cn/zte-g7610-ont-telnet-region-sn-mac/)

仓库里只放**工具**。53 MB 的整包固件放在 Releases 里，不在仓库正文：

- Release 页面：https://github.com/dellzou/ZTE_G7610_Flashing_Software_and_Tutorial/releases/tag/v3.0.5p1n27
- 文件名：`G7610V2_V3.0.5P1N27_FULL.bin`

## 文件清单

| 文件 | 大小 | 用途 |
|---|---|---|
| `ZTelnet_v1.0.5.exe` | 9,729,169 | 新版开 Telnet 工具，带界面 |
| `zte_factroymode.exe` | 9,840,829 | 图形界面开工厂模式 |
| `factorymode_crack.exe` | 872,448 | 命令行开工厂模式，适合脚本化 |
| `ztecfg.exe` | 7,037,223 | 配置导出与解密 |
| `tftpd64.exe` | 343,040 | TFTP 服务端 |
| `telnetonu_1.5.0/TelnetONU_1.5.0.exe` | 81,920 | 老版 TelnetONU |
| `telnetonu_1.5.0/2022.lic` | 752 | 老版工具需要的授权文件 |
| `telnetonu_1.5.0/Cmd_Onu.txt` | 5,376 | ONU 一键信息收集脚本 |
| `telnetonu_1.5.0/指令.txt` | 255 | 改地区与改 SN 的简写命令 |

固件包（Release 附件）`G7610V2_V3.0.5P1N27_FULL.bin`：53,608,512 字节，包头为 `ZXHN G7610V2 UNI V3.0.5P1N27`，构建时间 2024-08-07。

## SHA256

```
c6b0fc3056864024ab0b153cc1f8aabd2d60ef4d61a3abde19adf1d122b5743f  ZTelnet_v1.0.5.exe
7fae6d52429eca484d75f51d9d3a2fcb03b790a8cd40c6885685b7d8e34926b8  zte_factroymode.exe
e5fa3ca8a74a768c58abe7a0e552bbd66276353804604cbaa6c2479b95b9d702  factorymode_crack.exe
a14be5d00516fc8ca950e97347e4f6e3f535f354254542f746739407aa9e3f46  ztecfg.exe
2f5de838cf3079723f1697e5fac2f75fad4b98cd81b165f87cdfa99ded22d801  tftpd64.exe
952c8be8bf878e87a795472945ec77a77658c5a55dce84951d60e2213237be68  telnetonu_1.5.0/TelnetONU_1.5.0.exe
906432d722ac04f3adef9208561dcb68f0b9cb36959e4d4dee01f5268fbac78f  telnetonu_1.5.0/2022.lic
e90ae2757686fb8837226209244e3579a44a2482ed1b8a83f8e7945e0f8c7160  telnetonu_1.5.0/Cmd_Onu.txt
385a3669421a4cf853677075b153faa4787ff7c8f273de340a1b460c627b0fb9  telnetonu_1.5.0/指令.txt
2feb03ca985431a3482e881cffd52e1eed80568f2edd2e5abc64ac800f8748d5  G7610V2_V3.0.5P1N27_FULL.bin
```

Windows 上核对：

```bat
certutil -hashfile ZTelnet_v1.0.5.exe SHA256
```

## 使用顺序

1. 电脑接光猫 LAN 口，网卡设 `192.168.1.x`，关防火墙，ping 通 `192.168.1.1`
2. `factorymode_crack.exe -i xxx open -i 192.168.1.1` 拿临时 telnet 账号（Web 端口不是 80 时加 `-p`）
3. telnet 登录后立刻固化：`sendcmd 1 DB set TelnetCfg ...`，账号变为 `root` / `Zte521`
4. `upgradetest sdefconf <地区码>` 切地区，自动重启
5. `setmac show2` 看清 ID，再按 2176/2177 改 SN、256–263 改 MAC、768+512 改设备标识
6. `sendcmd 1 DB save` 保存，重启后回读确认

## 已知问题

- `telnetonu_1.5.0/指令.txt` 里写「改 SN：`setmac 1 512 xxxxxxxx`」，与社区通行的 2176+2177 两段式不一致。以 `setmac show2` 的回读结果为准，`512` 通常存的是设备标识的后半段。
- 部分中兴通用命令文档里，改 MAC 那段把 256 写成了 17 位地址，262/263 顺序也乱。照抄会写入非法地址，请按 00–07 递增自行排列。
- 工厂模式返回的 `FactoryModeAuth.gch?user=...&pass=...` 是**当次会话**的临时凭据，不是固定账号，不要当通用密码用。

## 免责声明

本仓库中的工具与固件均来自厂商维修渠道与公开流传的维修资料，仅供**自有设备**的维修、恢复与学习使用。刷机有变砖风险，带 BOOTLDR 的整包固件写坏引导后通常只能上串口。请勿用于他人线路或未授权的设备，因使用造成的任何损失由使用者自行承担。
