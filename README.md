# Drain-Only Underlap in a Strained 15 nm Gate-All-Around Nanowire FET

**A 1 nm drain-side underlap cuts off-state leakage by 35% for a 2% on-current cost, raising I<sub>ON</sub>/I<sub>OFF</sub> from 1.72 × 10<sup>7</sup> to 2.59 × 10<sup>7</sup> at V<sub>DD</sub> = 0.7 V.**

> **Status.** This device was my individual entry to the **IEEE Electron Devices Society TCAD Hackathon 2026**, where it placed in the **top three** internationally ([IEEE EDS competitions](https://eds.ieee.org/education/competitions)). This repository documents the device, method and results. Simulation decks, meshes and raw data are **not released** (see [What is not released](#what-is-not-released-and-why)).
>
> This work may be submitted to the IEEE for possible publication. Copyright may be transferred without notice, after which this version may no longer be accessible.

<p align="center"><img src="figures/F1_architecture.png" width="760" alt="Device architecture, mesh and doping"></p>
<p align="center"><em>Figure 1. (a) Materials of the cylindrical n-channel nanowire FET. (b) Adaptive mesh with refinement at the drain-side gate edge. (c) Net donor concentration; the inset shows the 1 nm undoped strip that sets the drain junction back from the gate edge.</em></p>

---

## 1. Motivation

At a 15 nm gate length, off-state current in a gate-all-around (GAA) nanowire is no longer set only by subthreshold diffusion. Band-to-band tunneling at the drain-side gate edge, where the gate-to-drain field peaks, adds a leakage component that grows as junctions become more abrupt. Spacer underlap moves the junction away from the gate and lowers this field, but a symmetric underlap also adds series resistance on the source side, which costs drive current.

The question this project answers: **can the junction be set back only where the leakage is generated, the drain side, and keep the source side intact?**

## 2. Device

A 2D cylindrical cross-section is revolved about the nanowire axis, so the simulation represents a full 3D cylindrical nanowire.

| Parameter | Value |
|---|---|
| Channel | Silicon nanowire, radius 3 nm (diameter 6 nm) |
| Gate length L<sub>G</sub> | 15 nm |
| Inner spacer | 4 nm Si<sub>3</sub>N<sub>4</sub> |
| Source/drain epitaxy | 10 nm |
| Gate stack | 0.5 nm SiO<sub>2</sub> interfacial layer + 2.26 nm HfO<sub>2</sub> (k = 22), EOT 0.9 nm |
| Gate metal | TiN, work function 4.4 eV |
| Contacts / extensions | As 3 × 10<sup>20</sup> cm<sup>−3</sup> / As 2 × 10<sup>20</sup> cm<sup>−3</sup> |
| Channel | Undoped (B 1 × 10<sup>15</sup> cm<sup>−3</sup>) |
| Stress | 1 GPa uniaxial tensile along the transport direction |
| **Design change** | **1 nm undoped strip at the drain-side gate edge** (drain junction set back 1 nm); source doped up to the gate edge |

## 3. Method

Simulations were run in **Synopsys Sentaurus** (Structure Editor and Sentaurus Device) at 300 K, using drift-diffusion transport with:

- **Quantum confinement:** density-gradient quantum potential for electrons, applied in silicon only.
- **Mobility:** doping-dependent and transverse-field (E<sub>normal</sub>) degradation, with high-field velocity saturation driven by the quasi-Fermi gradient.
- **Strain:** piezoresistive mobility tensor (Kanda model) for the low-field mobility, plus deformation-potential shifts of the band edges, both driven by the 1 GPa stress. Velocity saturation is deliberately left stress-independent, so the reported peak velocity remains physical.
- **Leakage:** Hurkx band-to-band tunneling and doping-dependent Shockley-Read-Hall recombination.
- **Band structure:** Fermi statistics and Slotboom bandgap narrowing.

**Mesh.** The mesh is refined at the drain-side gate edge (Figure 1b), where the band-to-band generation rate peaks. Every doping profile was checked region by region in the meshed structure before any electrical run.

**Metrics** (all at V<sub>DS</sub> = 0.7 V). I<sub>ON</sub> is taken at V<sub>GS</sub> = 0.7 V and I<sub>OFF</sub> at V<sub>GS</sub> = 0. Peak transconductance is read from the transfer curve, and maximum electron velocity from the on-state solution. Currents are normalized to the nanowire circumference, W<sub>eff</sub> = πd = 18.85 nm.

## 4. Results

### 4.1 Submitted device

| Metric (V<sub>DS</sub> = 0.7 V) | Absolute | Normalized |
|---|---|---|
| I<sub>ON</sub> (V<sub>GS</sub> = 0.7 V) | 4.03 µA | 213.9 µA/µm |
| I<sub>OFF</sub> (V<sub>GS</sub> = 0) | 1.56 × 10<sup>−13</sup> A | 8.25 pA/µm |
| Peak g<sub>m</sub> | 30.6 µS | 1.62 mS/µm |
| Maximum electron velocity | 1.07 × 10<sup>7</sup> cm/s | |
| **I<sub>ON</sub>/I<sub>OFF</sub>** | **2.59 × 10<sup>7</sup>** | |

<p align="center"><img src="figures/F2_idvg_log.png" width="560" alt="Transfer characteristic"></p>
<p align="center"><em>Figure 2. Transfer characteristic of the submitted device at V<sub>DS</sub> = 0.7 V.</em></p>

### 4.2 Effect of the drain-only underlap

Relative to the same device without underlap (L<sub>u</sub> = 0):

| | Change |
|---|---|
| I<sub>ON</sub> | −2.0% |
| **I<sub>OFF</sub>** | **−35%** |
| **I<sub>ON</sub>/I<sub>OFF</sub>** | **+51%** (1.72 × 10<sup>7</sup> → 2.59 × 10<sup>7</sup>) |
| Peak g<sub>m</sub> | −0.5% |
| Peak electron velocity | ≈ 0 |

### 4.3 Design space

| Underlap | I<sub>ON</sub>/I<sub>OFF</sub> |
|---|---|
| None (L<sub>u</sub> = 0) | 1.72 × 10<sup>7</sup> |
| Symmetric, 1 nm | 2.54 × 10<sup>7</sup> |
| Symmetric, 2 nm | 2.46 × 10<sup>7</sup> |
| Symmetric, 3 nm | 1.91 × 10<sup>7</sup> |
| **Drain-only, 1 nm (submitted)** | **2.59 × 10<sup>7</sup>** |

<p align="center"><img src="figures/F5_designspace.png" width="620" alt="Underlap design-space trade-off"></p>
<p align="center"><em>Figure 3. Symmetric underlap trades on-current for leakage, and past 2 nm the leakage gain reverses. Drain-only underlap (star) keeps most of the on-current, because the source-side series resistance is unchanged.</em></p>

**Interpretation.** The leakage reduction comes from the drain side, where moving the junction 1 nm away from the gate edge lowers the peak lateral field and therefore band-to-band generation. The on-current penalty of a symmetric underlap comes mainly from the source side, where the undoped strip adds series resistance in front of the injection point. Underlapping only the drain separates the two effects.

### 4.4 Levers that did not help

These are reported because they bound the design space:

- **High-k spacer without underlap:** I<sub>OFF</sub> rose about 9×. A high-k spacer is only worth considering together with an underlap.
- **Graded junctions:** in a 15 nm channel, the gain in leakage was outweighed by worse drain-induced barrier lowering.

## 5. Limitations

- This is a drift-diffusion study with a density-gradient correction, not a full quantum-transport (NEGF) calculation, so ballistic and source-to-drain tunneling effects in a 15 nm channel are approximated.
- The device is an idealized single nanowire. Parasitic capacitance, contact resistance beyond the doped regions, and process variability are not modeled.
- Results are simulated only and have not been fabricated or measured.

## What is not released, and why

The Sentaurus structure and device decks, meshes and raw I-V data are not published. The work may be developed into a manuscript, and the input files depend on licensed Synopsys TCAD tools and material libraries. The device parameters, physical models and results above are enough to understand and assess the work. Further detail is available on request.

## Author

**Tushar Dudeja**, B.Tech. Electronics Engineering (VLSI Design), Vellore Institute of Technology
[LinkedIn](https://linkedin.com/in/tushardudeja) · [GitHub](https://github.com/thetushardudeja1)

© 2026 Tushar Dudeja. All rights reserved. Figures and text may not be reused without permission.
