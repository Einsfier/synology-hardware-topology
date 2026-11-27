# Contributing Guide

**English** | [简体中文](./CONTRIBUTING_zh.md)

## 1. Submit Real-Hardware Data (GitHub Issue)

You do not need to write HTML to contribute. SSH into your Synology NAS as `root` (if using a non-root account or hitting permission errors, switch via `sudo -i`) and run:

```bash
lspci -PPvv
syno_slot_mapping
ls -al /sys/block/sata*
# Optional if NVMe drives are installed:
ls -al /sys/block/nvme*
```

These commands are read-only and do not output your Synology host serial number. Paste the outputs into the [Hardware Calibration Issue Template](https://github.com/Einsfier/synology-hardware-topology/issues/new?template=hardware-calibration.yml).

### Sample Output (`syno_slot_mapping` & `sysfs`)

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

## 2. Contribute a Model Page (Pull Request)

### Deliverables

1. Create a standalone page at `models/<slug>.html` (replace `+` with `plus`, e.g., `models/ds1522plus.html`).
2. Register the model in the `MODELS` array inside `index.html`.

### Page Requirements

- **Self-Contained & Sanitized**: Inline CSS, Canvas 2D, and Vanilla JS with zero external dependencies. Use generic drive labels (`SATA Bay 1`, `M.2 2280 NVMe #1`) and exclude personal hardware identifiers. Copy must stay objective and free of emojis.
- **Data Provenance**: Derive verified models strictly from SSH outputs (`status: 'verified'`). For models compiled from official specs and teardowns (`status: 'inferred'`), clearly mark inferred links/mappings with `*` or `(Inferred)`. Calculate effective unidirectional bandwidth using 8b/10b (PCIe 1.x/2.x) and 128b/130b (PCIe 3.0/4.0).
- **Canvas Visuals & Cards**: Match the dark theme (`#0a0f1a` / `#111827` / `#24324d` / `#38bdf8`) and `1680 × 1025` HiDPI Canvas layout of `models/ds923plus.html`. Keep routing lines clear of all text and draw port badges after links.
  - **SoC card & hover**: Show core/thread count, clock, TDP, RAM config, iGPU presence, DSM platform, and PCIe lane budget.
  - **Device cards**: Use physical roles/locations as card headers (e.g., `Dedicated 10GbE Slot`, `M.2 Bottom Bay · Slot 1`); list only active NAS blocks in on-die peripherals; place exact PCI BDF / `ataX (hostY)` in hover subtitles (`meta.bdf`) for verified models.
- **Interaction & Chrome**:
  - Bilingual (`zh` / `en`) sharing `localStorage` key `syno_topo_lang`.
  - Pointer Events (`pointermove`, `pointerdown`, `pointerleave`) supporting desktop hover, click-to-pin, touchscreen tap-to-inspect, and viewport-clamped tooltips.
  - HiDPI PNG export that clears active highlights and renders `github.com/Einsfier/synology-hardware-topology` at bottom-right (`y = 1012`).
  - Top `.nav-bar`: `← All Models` (`../index.html`) on the left, and only the current model's full name highlighted on the right (e.g., `Synology DS1522+`).
  - Minimal footer with the inline GitHub SVG icon + `Einsfier/synology-hardware-topology`.
  - Copy the inline Base64 SVG `<link rel="icon">` from `index.html` and include SEO/Open Graph/Twitter meta tags and `<noscript>`.

### `index.html` `MODELS` Entry Schema

```js
{
  id: 'ds1525plus',
  name: 'DS1525+',                       // Omit "Synology" prefix
  line: 'ds-plus',                       // pas | fs | hd | sa | uc | rs-xs | rs-plus | rs-value | ds-xs | ds-plus | ds-value | ds-j
  href: './models/ds1525plus.html',
  status: 'inferred',                    // 'verified' | 'inferred'
  searchText: 'synology ds1525+ ...',
  soc: ['AMD V1500B', 'x86_64 · 4C/8T'], // [Model, Arch · Cores/Threads]
  pcie: {
    zh: ['16× PCIe 3.0', '10/16 已分配 (x2 Mini 10G)'], // Parenthetical slot note only if an open slot exists
    en: ['16× PCIe 3.0', '10/16 Allocated (x2 Mini 10G)']
  },
  bays: { zh: ['5 内置', '10 扩展'], en: ['5 Internal', '10 Expansion'] }, // 1-element array if internal-only; omit expansion unit names
  nvme: '2× PCIe 3.0 x2',
  nic: { zh: ['2× 2.5GbE', '10GbE 扩展槽'], en: ['2× 2.5GbE', '10GbE Slot'] }, // 1-element array if single tier
  exp: { zh: ['2× USB-C (专用)', 'DX525'], en: ['2× USB-C (Proprietary)', 'DX525'] } // No parentheses on normal speeds (e.g., eSATA 6Gbps)
}
```

### Verification Checklist

- Valid HTML/JS syntax and zero browser console errors.
- Working bilingual switch, hover/touch tooltip clamping, and PNG export (with bottom-right attribution visible).
- Visual screenshot check confirming zero text overlap or line-over-text collisions.

---

## 3. AI Agent Prompt Template

````markdown
Add a new Synology NAS hardware topology page to this repository.

- Target Model: <e.g., Synology DS1522+>
- Target File: models/<slug>.html (e.g., models/ds1522plus.html)
- Verification Status: <verified | inferred>

Instructions:
1. Read `CONTRIBUTING.md`, `models/ds923plus.html`, and `index.html` first.
2. Follow every page, visual, interaction, SEO, and `index.html` `MODELS` schema requirement in `CONTRIBUTING.md`.
3. Generate `models/<slug>.html`, register the model in `index.html`, and complete all verification checks (syntax, console, touch/hover, PNG export, and visual screenshot review).

Hardware Data (omit if inferred from official specs & teardowns):

### `lspci -PPvv`
```text
<PASTE_OUTPUT_HERE>
```

### `syno_slot_mapping`
```text
<PASTE_OUTPUT_HERE>
```

### `ls -al /sys/block/sata*`
```text
<PASTE_OUTPUT_HERE>
```
````
