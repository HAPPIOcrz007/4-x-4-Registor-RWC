# 4×4 Register File PCB — Read / Write / Asynchronous Clear

A first-year B.Tech Digital Electronics project implementing a **4×4 register file** using standard 74-series TTL ICs, progressing from Logisim simulation through breadboard prototyping to a fabricated KiCad PCB — verified live at the **Circuit Conclave Hackathon 2026, MIT World Peace University**.

> 🏅 **Recommended by the Department Head and leading faculty of the Department of Electrical & Electronics Engineering, MIT-WPU, as a future laboratory teaching kit** — recognised for its compact 15×15 cm form factor, intuitive layout, and pedagogical value for FY B.Tech students.

<img width="300" height="300" alt="PCB top view — 4 Row Memory Storage" src="https://github.com/user-attachments/assets/7c3d3f21-c1f4-4416-a49a-ca6b9764e8e4" />

---

## ✨ Features

- **4 registers × 4 bits each** — 16 bits total volatile storage
- **2-bit address bus** for row selection via SN74LS139AN decoder
- **Synchronous write** — data latched on rising edge of NE555P astable clock (0.5 Hz)
- **Tri-state buffered read** — selected register drives shared LED output bus via SN74LS244N
- **Asynchronous clear** — active-low reset of addressed register, independent of clock
- **Single RW push-button** for mode control (default = read, pressed = write)
- **4 output LEDs** (CQY99) displaying bus values in real time
- **Compact 15×15 cm PCB** — all 31 components placed and routed on a 2-layer board
- **Single +5V supply** (see Hardware Notes for 6V field experience)

---

## 🏆 Presented At

**Circuit Conclave Hackathon 2026**
DoEEE & prismlabs Student Club | MIT World Peace University, Pune
FY B.Tech E&CE — Division 23 | Academic Year 2025–26

The PCB was demonstrated live before a jury comprising faculty from Basic Electrical Engineering, Digital Electronics, and an external industry examiner. The board powered up cleanly and passed all functional tests on the day — write, read, hold, and clear verified across all four register addresses.

**The project received special recognition from the Department Head and senior faculty of the DoEEE, who recommended it be adopted as a hands-on laboratory teaching kit for future first-year batches.**

### Team
| Member | Role |
|--------|-----|
| Harshvardhan M Rathod | Pcb Design, Logical iterations, connections, development, verifications and ordering |
| Alok Deep Verma | report and ppt writing and printing |

**Faculty Mentor:** Dr. Sarika Patil &nbsp;|&nbsp; **Digital Electronics:** Dr. Manisha Ingle

---

## 📂 Repository Structure

```
.
├── 4X4_MEMORY_READ_WRITE.circ           # Logisim Evolution simulation file
├── BOM.pdf                              # Bill of Materials (KiCad export)
├── Component box/                       # Custom component libraries
│   ├── <IC or switch folders>/          #   KiCad symbols, footprints, STEP models
│   └── License.txt
├── KiKad/                               # KiCad project files
│   ├── 4-x-4-Registor-RWC.kicad_pcb    #   PCB layout
│   ├── 4-x-4-Registor-RWC.kicad_sch    #   Schematic
│   ├── 4-x-4-Registor-RWC.kicad_pro    #   Project file
│   ├── 4-x-4-Registor-RWC.kicad_prl    #   Project local settings
│   └── 4-x-4-Registor-RWC.pretty/      #   Custom footprints
├── RAM-project-001.csv                  # Raw BOM CSV export
├── Report.pdf                           # Full project report
├── sm_black_top.png                     # PCB render — top view (KiCad)
├── output.pdf                           # schematic drawing sheet pdf
├── pcb_output.pdf                       # pcb order copy pdf
└── README.md                            # This file
```

---

## 📦 Bill of Materials

**Total: 31 components | 2-layer PCB | Single +5V supply**

| Category | Qty | Key Parts |
|----------|-----|-----------|
| ICs | 11 | SN74LS175N ×4, SN74LS139AN, SN74LS244N ×2, SN74LS08N, SN7402N, SN7432N, NE555P |
| Switches | 8 | SPST inputs (D0–D3, A0–A1), RW push-button, Clear push-button |
| LEDs | 4 | CQY99 — output bus indicators (Bits 0–3) |
| Resistors | 6 | 220 Ω ×4 (LED limiting), 10 kΩ + 68 kΩ (NE555P timing network) |
| Capacitors | 2 | 0.1 µF ceramic disc — VCC decoupling |
| **Total** | **31** | — |

---

## ⚙️ Operating Modes

| Mode | RW (SWW1) | Address | Clock | CLR\ | Result |
|------|-----------|---------|-------|------|--------|
| Write | 1 — pressed | Row n | ↑ rising edge | 1 | Data latched into Row n |
| Read | 0 — default | Row n | — | 1 | Row n displayed on OL0–OL3 |
| Hold | 0 | Any | — | 1 | No change |
| Clear | X | Row n | — | 0 — pressed | Selected row reset to 0000 |

**Clock frequency:** `f = 1.44 / [(R1 + 2×R2) × C] = 1.44 / [146 kΩ × 20 µF] ≈ 0.5 Hz`

---

## 🔧 Hardware Notes & Lessons Learned

### Power Supply — 6V Field Operation
The board was designed for a regulated **+5V** supply. During the live demonstration it was powered from a **6V battery**, with a **toggle switch inserted between the battery and the VCC patch point** for clean power-on/off control. The 74LS-series ICs functioned correctly at 6V for the duration of the demo. Note that 6V is outside the 74LS absolute maximum rating (5V ±5%), so a regulated 5V supply (e.g. 7805 linear regulator or USB power module) is recommended for sustained operation.

### Noise on SN74LS175N — Decoupling Lesson
During live testing, switching noise was observed being picked up on the **SN74LS175N (Quad D Flip-Flop) VCC pins**, occasionally causing spurious state changes on the output. The examining faculty identified the root cause immediately:

**Fix recommended by faculty:**
> Add a **0.1 µF ceramic capacitor between VCC and GND at every IC's power pins**, placed as physically close to each DIP package as possible.

The existing BOM includes two 0.1 µF decoupling caps (CC1, CC2). The lesson: **every IC in a TTL design should have its own dedicated decoupling capacitor** — not just selected ICs. Switching transients from multiple ICs sharing the same power rail accumulate and cause exactly this noise behaviour.

**Updated decoupling plan for next PCB revision:**

| IC | KiCad Ref | Decoupling Cap |
|----|-----------|----------------|
| SN74LS175N ×4 | D074175–D374175 | 0.1 µF per IC — **×4 to add** |
| SN74LS139AN | D74139 | 0.1 µF — **to add** |
| SN74LS08N | A7408 | 0.1 µF — **to add** |
| SN7402N | N7402 | 0.1 µF — **to add** |
| SN7432N | O7432 | 0.1 µF — **to add** |
| NE555P | CNE555 | 0.1 µF — **to add** |
| SN74LS244N ×2 | B074244, B174244 | 0.1 µF — already placed ✓ |

**Total additional caps for Rev 2: +7 × 0.1 µF ceramic disc**

---

## 🖥️ Simulation

The full register file was first verified in **Logisim Evolution** (`4X4_MEMORY_READ_WRITE.circ`) before any hardware was built. All four operating modes — write, read, hold, and clear — were confirmed across all four register addresses in simulation before committing to PCB layout.

---

## 📚 References

- Texas Instruments datasheets: [SN74LS175](https://www.ti.com/lit/gpn/sn74ls175) · [SN74LS139A](https://www.ti.com/lit/gpn/sn74ls139a) · [SN74LS244](https://www.ti.com/lit/gpn/sn74ls244) · [NE555](https://www.ti.com/lit/gpn/ne555)
- M. M. Mano & M. D. Ciletti, *Digital Design*, 5th ed., Pearson
- D. A. Patterson & J. L. Hennessy, *Computer Organisation and Design*, 5th ed., Morgan Kaufmann

---

## 🚀 Future Work

- Add 0.1 µF decoupling capacitor at each IC VCC pin (see Hardware Notes above)
- On-board 7805 / LDO regulator footprint for regulated 5V from a 9V battery
- Expansion connector for 16-LED monitor matrix (as in Logisim simulation)
- FPGA / HDL implementation for direct speed and area comparison
- Formal adoption as DoEEE laboratory teaching kit for future FY B.Tech batches

---

## 📜 License

- **Hardware design files:** CERN OHL v2 (Strongly Reciprocal)
- **Code & documentation:** MIT License

See `LICENSE` for full terms.
