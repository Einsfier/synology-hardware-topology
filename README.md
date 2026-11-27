# Synology Hardware Topology

**English** | [简体中文](./README_zh.md)

Interactive hardware interconnect diagrams for Synology NAS models — visually mapping SoC PCIe lane allocation, SATA/NVMe controllers, networking links, and expansion unit bottlenecks.

## Supported Models

Browse the full list of supported models (filterable by verification status, official Synology product line, or keyword) on the live site:

**[https://einsfier.github.io/synology-hardware-topology/](https://einsfier.github.io/synology-hardware-topology/)**

## Highlights

- **Interactive Path Inspection**: Hover (desktop) or tap (touchscreen) any controller, NIC, or drive bay to highlight its full upstream/downstream path and inspect link speeds, drivers, and bottlenecks (hardware-verified models additionally include PCI BDF and MPS/MRRS register details).
- **Clear Bottleneck Analysis**: Highlights real-world bandwidth ceilings across PCIe lane downgrades, SATA controllers, M.2 slots, and shared expansion uplinks.
- **Bilingual (EN / 中文)**: Seamless English and Simplified Chinese switching across all pages.
- **High-Res PNG Export**: One-click export of crisp HiDPI topology diagrams.

## Help Calibrate & Contribute

Own a Synology NAS listed as **Teardown + Spec** or a model not yet covered? You can help in two ways:

1. **Submit Real-Hardware Data (No Coding Needed)**: Run three read-only SSH commands (`lspci -PPvv`, `syno_slot_mapping`, `ls -al /sys/block/sata*`) as `root` (none output your Synology host serial number) and paste them into a [Hardware Calibration Issue](https://github.com/Einsfier/synology-hardware-topology/issues/new?template=hardware-calibration.yml).
2. **Submit a Pull Request**: See [CONTRIBUTING.md](./CONTRIBUTING.md) for guidelines and a ready-to-use AI Agent prompt template to generate new model diagrams.

## License

This project is licensed under the [Creative Commons Attribution 4.0 International License (CC BY 4.0)](./LICENSE). You are free to share and adapt the diagrams and data with appropriate attribution.
