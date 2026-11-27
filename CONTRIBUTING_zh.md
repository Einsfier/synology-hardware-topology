# 贡献指南

[English](./CONTRIBUTING.md) | **简体中文**

## 1. 提交实机数据（GitHub Issue）

无需编写代码即可参与贡献。通过 SSH 以 `root` 身份登录群晖 NAS（若为非 `root` 账号或权限不足，请先执行 `sudo -i` 切换），运行以下 3 条只读命令：

```bash
lspci -PPvv
syno_slot_mapping
ls -al /sys/block/sata*
# 若安装了 M.2 NVMe 可选附带：
ls -al /sys/block/nvme*
```

以上命令均为只读且不含群晖主机序列号。通过 [实机校准 Issue 模板](https://github.com/Einsfier/synology-hardware-topology/issues/new?template=hardware-calibration.yml) 粘贴输出即可。

### 输出参考（`syno_slot_mapping` 与 `sysfs`）

```text
# syno_slot_mapping
Internal Disk
01: /dev/sata1
02: /dev/sata2
03: /dev/sata3
04: /dev/sata4
Esata port count: 1
Internal SSD Cache:
01: /dev/nvme0n1
02: /dev/nvme1n1

# ls -al /sys/block/sata*
lrwxrwxrwx 1 root root 0 ... /sys/block/sata1 -> ../devices/pci0000:00/0000:00:01.5/0000:08:00.0/ata1/host0/.../block/sata1
lrwxrwxrwx 1 root root 0 ... /sys/block/sata3 -> ../devices/pci0000:00/0000:00:08.2/0000:0a:00.0/ata6/host5/.../block/sata3
```

---

## 2. 贡献新机型页面（Pull Request）

### 产出清单

1. 新增独立页面 `models/<slug>.html`（文件名用 `plus` 替代 `+`，如 `models/ds1522plus.html`）。
2. 在 `index.html` 的 `MODELS` 数组中注册该机型。

### 页面规范

- **单文件与脱敏**：内联 CSS、Canvas 2D 与 Vanilla JS，零外部依赖。盘位统一使用通用名称（如 `SATA 盘位 1`、`M.2 2280 NVMe #1`），不含个人硬件标识；文案客观简洁，文档不使用 emoji。
- **数据准确性**：有实机输出时严格按实机绘制（`status: 'verified'`）；依据官方规格与拆解推算时标为 `status: 'inferred'`，并用 `*` 或 `(推算)` 明确区分推算项。有效单向带宽按 PCIe 1.x/2.x (8b/10b)、PCIe 3.0/4.0 (128b/130b) 计算。
- **视觉与卡片**：沿用 `models/ds923plus.html` 的深色主题（`#0a0f1a` / `#111827` / `#24324d` / `#38bdf8`）与 `1680 × 1025` HiDPI Canvas 布局；连线不压文字，端口标签在连线之后绘制。
  - **SoC 卡片与 Hover**：展示核心/线程、主频、TDP、内存配置、有无核显、DSM 平台与 PCIe 通道预算。
  - **设备卡片**：正面副标题写物理位置/角色（如 `专用网络升级槽`、`M.2 底部仓 · 槽位 1`）；片内外设仅列 NAS 有效模块；实机核验机型在设备 Hover 副标题（`meta.bdf`）中附带精确 PCI BDF / `ataX (hostY)`。
- **交互与导航**：
  - 中英双语，共享 `localStorage` 键名 `syno_topo_lang`。
  - 使用 Pointer Events（`pointermove` / `pointerdown` / `pointerleave`）支持桌面悬停、点击锁定、触屏点击与 Tooltip 防溢出定位。
  - 高清 PNG 导出前清除高亮态，并在画布右下角（`y = 1012`）绘制 `github.com/Einsfier/synology-hardware-topology`。
  - 顶部 `.nav-bar` 左侧为 `← 全部机型`（`../index.html`），右侧仅高亮当前完整型号名称（如 `Synology DS1522+`）。
  - 页脚仅保留内联 GitHub SVG 图标 + `Einsfier/synology-hardware-topology`。
  - 复用 `index.html` 中的内联 Base64 SVG `<link rel="icon">`，补齐中英双语 `<title>`、SEO / Open Graph / Twitter Meta 与 `<noscript>`。

### `index.html` `MODELS` 字段格式

```js
{
  id: 'ds1525plus',
  name: 'DS1525+',                       // 不带 Synology 前缀
  released: '2025-06',                   // 官方发布年月 YYYY-MM（列表自动按此字段倒序排列）
  line: 'ds-plus',                       // pas | fs | hd | sa | uc | rs-xs | rs-plus | rs-value | ds-xs | ds-plus | ds-value | ds-j
  href: './models/ds1525plus.html',
  status: 'inferred',                    // 'verified'（实机核验）| 'inferred'（拆解推算）
  searchText: 'synology ds1525+ ...',
  soc: ['AMD V1500B', 'x86_64 · 4C/8T'], // [处理器名称, 架构 · 核心规格]
  pcie: {
    zh: ['16× PCIe 3.0', '10/16 已分配 (x2 Mini 10G)'], // 仅填可分配 PCIe 通道；无外置 PCIe 统一填 ['—']（不写否定句、不重复架构）；有开放槽时末尾括号补充
    en: ['16× PCIe 3.0', '10/16 Allocated (x2 Mini 10G)']
  },
  bays: { zh: ['5 内置', '10 扩展'], en: ['5 Internal', '10 Expansion'] }, // 仅内置传单元素数组；不写扩展柜型号
  nvme: '2× PCIe 3.0 x2',                // 无 M.2 槽统一填 '—'
  nic: { zh: ['2× 2.5GbE', '10GbE 扩展槽'], en: ['2× 2.5GbE', '10GbE Slot'] }, // 单类别传单元素数组
  exp: { zh: ['2× USB-C (专用)', 'DX525'], en: ['2× USB-C (Proprietary)', 'DX525'] } // 不支持扩展柜统一填 ['—']；常规速率不加括号（如 eSATA 6Gbps）
}
```

### 提交前校验

- HTML/JS 语法无误，浏览器控制台 0 报错。
- 双语切换、桌面悬停/触屏点击防溢出、PNG 导出（含右下角仓库水印）均正常。
- 全页截图核验无文字重叠与连线压字。

---

## 3. AI Agent Prompt 模板

````markdown
请为本仓库新增一个 Synology NAS 硬件拓扑页面。

- 目标型号：<例如 Synology DS1522+>
- 目标文件：models/<slug>.html（例如 models/ds1522plus.html）
- 验证状态：<verified | inferred>

执行要求：
1. 先阅读 `CONTRIBUTING_zh.md`、`models/ds923plus.html` 与 `index.html`。
2. 严格按照 `CONTRIBUTING_zh.md` 中的页面、视觉、交互、SEO 与 `index.html` `MODELS` 字段规范生成 `models/<slug>.html` 并完成首页注册。
3. 完成语法、控制台、触屏/悬停交互、PNG 导出与全页截图校验。

实机原始输出（若为 `inferred` 拆解推算模式可省略）：

### `lspci -PPvv`
```text
<在此粘贴输出>
```

### `syno_slot_mapping`
```text
<在此粘贴输出>
```

### `ls -al /sys/block/sata*`
```text
<在此粘贴输出>
```
````
