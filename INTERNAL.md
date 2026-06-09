# Internal Notes & Backlog

Not for public sharing. Lives on the `internal` branch.

---

## Backlog

| # | Task | Notes |
|---|---|---|
| 1 | Hostnaming | Configure hostnames across all active devices; update register |
| 2 | RAM Upgrade | 16GB → 32GB; 4× 8GB DDR3L, 1600 MT/s, Non-ECC UDIMM, 240-pin |
| 3 | Storage Upgrade | Replace HDD with SSD for Proxmox + VMs; repurpose HDD for backup storage |
| 4 | VLAN Setup | Separate homelab traffic from family/IoT traffic |
| 5 | Local AI Exploration | Current GPU (2GB VRAM) is the bottleneck; RTX 3060 12GB identified as target upgrade; Ollama + llama3.2:3b viable for CPU-only testing |

---

## Notes

- RDIMM ≠ UDIMM — the T1700 SFF requires **Non-ECC UDIMM** specifically. Confirmed via `dmidecode`. Don't buy the wrong RAM.
- Windows 11 is resource-heavy on this hardware. Chris Titus debloat tool is on the list for the Windows VM.
- The GPU (Quadro K620, 2GB VRAM) is the primary bottleneck for any AI workload. RAM upgrade is a prerequisite before going serious on local models.
