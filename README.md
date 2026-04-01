# 4×4 Register File PCB (Read/Write + Asynchronous Clear)

A first-year B.Tech Digital Electronics project implementing a **4×4 register file** using standard 74-series TTL ICs.  
The design supports synchronous clock-triggered write, tri-state bus-buffered read, and asynchronous active-low clear.  
Fabricated as a **KiCad PCB (RAM-project-001)** and verified with discrete logic components.

<img width="300" height="300" alt="sm_black_top" src="https://github.com/user-attachments/assets/7c3d3f21-c1f4-4416-a49a-ca6b9764e8e4" />

---

## ✨ Features
- **4 registers × 4 bits each** (total 16 bits storage).
- **2-bit address bus** for row selection.
- **Synchronous write**: data latched on rising clock edge.
- **Tri-state buffered read**: selected register drives shared bus.
- **Asynchronous clear**: active-low reset of selected register.
- **NE555P astable clock** at 0.5 Hz for step-by-step observation.
- **Single RW push-button** for mode control (read/write).
- **Output LEDs** (4) to display bus values.

---

## 📂 Repository Structure
.  
├── 4X4_MEMORY_READ_WRITE.circ      # Logisim evolution simulation file  
├── BOM.pdf                         # Bill of Materials (KiCad export)  
├── Component box/                  # Custom component libraries  
│   ├── <IC or switch folders>      # Each contains KiCad symbols, footprints, STEP models  
│   └── License.txt                 # Component library license  
├── KiKad/                          # KiCad project files  
│   ├── 4-x-4-Registor-RWC.kicad_pcb   # PCB layout  
│   ├── 4-x-4-Registor-RWC.kicad_sch   # Schematic  
│   ├── 4-x-4-Registor-RWC.kicad_pro   # Project file  
│   ├── 4-x-4-Registor-RWC.kicad_prl   # Project local settings  
│   └── 4-x-4-Registor-RWC.pretty/     # Custom footprints  
├── RAM-project-001.csv             # Raw BOM CSV export  
├── Report.pdf                      # Full project report  
├── sm_black_top.png                # PCB render (top view)  
├── README.md                       # This file  

---

## 📦 Bill of Materials (Summary)
Total components: **31**

| Category | Qty | Key Parts |
|----------|-----|-----------|
| ICs      | 11  | SN74LS175N ×4, SN74LS139AN, SN74LS244N ×2, SN74LS08N, SN7402N, SN7432N, NE555P |
| Switches | 8   | SPST inputs (D0–D3, A0–A1), RW, Clear |
| LEDs     | 4   | CQY99 (Output bus indicators) |
| Resistors| 6   | 220Ω ×4 (LED), 10kΩ, 68kΩ (555 timing) |
| Capacitors | 2 | 0.1µF decoupling |
| **Total** | 31 | — |

---

## ⚙️ Operating Modes
| Mode   | RW (SWW1) | Address | Clock | Clear | Result |
|--------|-----------|---------|-------|-------|--------|
| Write  | 1 (pressed) | Row n | Rising edge | — | Data latched |
| Read   | 0 (default) | Row n | — | — | Row n outputs on LEDs |
| Hold   | 0 | Any | — | — | No change |
| Clear  | X | Row n | — | 0 (pressed) | Row reset to 0000 |

---

## 🖥️ Project Files
- **Simulation**: `4X4_MEMORY_READ_WRITE.circ` (Logisim)
- **PCB Design**: KiCad project in `/KiKad`
- **Component Libraries**: `/Component box`
- **BOM**: `BOM.pdf` and `RAM-project-001.csv`
- **Documentation**: `Report.pdf`
- **PCB Render**: `sm_black_top.png`

---

## 📚 References
- Texas Instruments datasheets: [SN74LS175](https://www.ti.com/lit/gpn/sn74ls175), [SN74LS139A](https://www.ti.com/lit/gpn/sn74ls139a), [SN74LS244](https://www.ti.com/lit/gpn/sn74ls244), [NE555](https://www.ti.com/lit/gpn/ne555)
- M. M. Mano & M. D. Ciletti, *Digital Design*, 5th ed.
- D. A. Patterson & J. L. Hennessy, *Computer Organisation and Design*, 5th ed.

---

## 📜 License
- **Hardware design files**: CERN OHL v2 (Strongly Reciprocal)  
- **Code & documentation**: MIT License  
See the `LICENSE` file for details.

---

## 🚀 Future Work
- Expansion connector for 16-LED monitor matrix (as in Logisim simulation).
- Possible FPGA/HDL implementation for comparison.
