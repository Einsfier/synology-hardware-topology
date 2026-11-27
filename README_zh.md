# Synology Hardware Topology（群晖 NAS 硬件拓扑合集）

[English](./README.md) | **简体中文**

面向群晖 NAS 用户的交互式硬件互联拓扑图合集，直观呈现各机型的 SoC PCIe 通道分配、SATA/NVMe 控制器走线、网络接口与外置扩展柜瓶颈。

## 已收录机型

完整已收录机型列表（支持按验证状态、群晖官方产品线与关键词筛选）请直接访问在线站点：

**[https://einsfier.github.io/synology-hardware-topology/](https://einsfier.github.io/synology-hardware-topology/)**

## 主要亮点

- **交互式链路高亮**：桌面悬停或触屏点击任意控制器、网卡与盘位，即可高亮完整上下游通路并查看速率、驱动与瓶颈说明（实机核验机型额外提供 PCI BDF 与 MPS/MRRS 等寄存器参数）。
- **清晰瓶颈标注**：直观标出 PCIe 通道降速/降宽、SATA 控制器共享上行、M.2 槽位限速及扩展柜共享带宽瓶颈。
- **中英双语切换**：全站支持中英文一键切换。
- **高清 PNG 导出**：支持一键导出当前语言版本的高清拓扑图。

## 欢迎协助校准与贡献

如果你拥有标注为**拆解推算**的机型，或其他尚未收录的群晖 NAS，欢迎通过以下任一方式参与贡献：

1. **提交实机输出协助校准（无需写代码）**：通过 SSH 以 `root` 身份运行 3 条只读命令（`lspci -PPvv`、`syno_slot_mapping`、`ls -al /sys/block/sata*`，均不含群晖主机序列号），通过 [实机校准 Issue 模板](https://github.com/Einsfier/synology-hardware-topology/issues/new?template=hardware-calibration.yml) 直接粘贴提交即可。
2. **提交 Pull Request**：请参阅 [CONTRIBUTING_zh.md](./CONTRIBUTING_zh.md)（内含完整规范与可直接使用的 AI Agent Prompt 模板）。

## 开源协议

本项目基于 [署名 4.0 国际许可协议（CC BY 4.0）](./LICENSE) 发布。在注明出处的前提下，欢迎自由分享、引用与二次创作。
