# Sources and engineering references

[Guide](engineering-guide.md) Â· [Design basis](design-basis.md) Â· [Reconciliation](reconciliation.md)

## Original project evidence

| Source | Where to look | What it supports |
| --- | --- | --- |
| [Project report](source/ME448-Project-Report.pdf) | Table 1 p. 3; thermal simulation pp. 4-8; piping and pumps pp. 9-12; chiller and tank pp. 13-17; controls and schematic p. 20; conclusions pp. 21-22 | Submitted design narrative, reported results, selected components, and stated limitations |
| Report Appendix A | pp. 23-25; [recovered script](../reconstruction/report-transcriptions/01_heat_loads.txt) | Printed heat-load equations |
| Report Appendix C | pp. 27-29; [recovered script](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt) | Printed tubing, flow, pressure, and pump equations |
| Report Appendix E | pp. 31-32; [recovered script](../reconstruction/report-transcriptions/03_expansion_tank_and_chiller.txt) | Printed expansion-tank and chiller equations |
| Report Appendices B, D, F | pp. 26, 30, 33; [evidence images](../reconstruction/evidence/) | Historical result screenshots with visible differences from the printed source |
| [Project presentation](source/ME448-Project-Presentation.pdf) | Slides 2-8 | Motivation, system diagram, methods, design considerations, and proposed future work |
| Marcus Prieto's role clarification | Owner clarification during documentation, November 2026 | EES scripting, system modeling, component selection, and thermofluids concepts; excludes Abaqus work |

PDF page numbers above refer to the supplied files, not the stale table-of-contents page references inside the report. Original authors are Marcus Prieto and Terak Hornik.

The four images in `docs/assets/` are extracted unchanged from the report: mesh from p. 5, the two temperature images from p. 6, and circulation schematic from p. 20. They are original project evidence, not newly generated simulation results.

## Heat transfer and hydraulic background

The following primary educational, software-author, and manufacturer references support the explanatory relationships and applicability checks. They do not validate the project's reported values or equipment selections.

| Reference | Use in these documents |
| --- | --- |
| [MIT Unified Engineering: Heat Exchangers](https://web.mit.edu/16.unified/www/FALL/thermodynamics/notes/node131.html) | Cylindrical convection/conduction resistance, uniform-wall-temperature behavior, energy balance, and LMTD |
| [Lienhard and Lienhard: A Heat Transfer Textbook](https://ahtt.mit.edu/wp-content/uploads/2024/04/AHTTv600.pdf) | General background for conduction, convection, phase change, and diffuse-gray radiation networks |
| [F-Chart: Brines](https://fchart.com/ees/eeshelp/hs612.htm) | Mass-percent concentration input, documented property units, and mass/molar basis |
| [F-Chart: PipeFlow Laminar](https://www.fchartsoftware.com/ees/heat_transfer_library/internal_flow/hs1024.htm) | Laminar-flow applicability, wall boundary conditions, and developing-flow treatment |
| [F-Chart: PipeFlow Turbulent](https://fchartsoftware.com/ees/heat_transfer_library/internal_flow/hs1022.htm) | Turbulent correlations, friction-factor convention, and applicability |
| [F-Chart: PipeFlow ND](https://www.fchartsoftware.com/ees/heat_transfer_library/internal_flow/hs1020.htm) | Handling laminar, transitional, and turbulent regimes within the documented library procedure |
| [Hydraulic Institute: Pump Curves](https://datatool.pumps.org/pump-fundamentals/pump-curves.html) | Head-flow operating points, efficiency, input power, and suction requirements |
| [LBNL Buildings: Heat Exchanger Base Classes](https://simulationresearch.lbl.gov/modelica/releases/v7.0.2/help/Buildings_Fluid_HeatExchangers_BaseClasses.html) | Effectiveness/NTU formulations and phase-change exchanger interpretation |
| [Caleffi: Expansion Vessels, technical brochure 01079](https://www.caleffi.com/sites/default/files/media/external-file/01079_EN.pdf) | PDF p. 3: acceptance-factor sizing using absolute gas pressures, precharge, and reserve volume |
| [NBS/NIST: Heating Values of Natural Gas and Its Components](https://nvlpubs.nist.gov/nistpubs/Legacy/IR/nbsir82-2401.pdf) | Appendix A3.2.2: international-table BTU conversion to joules |

The Caleffi brochure describes its own water/glycol applications. It supplies sizing background here, not approval of those products or materials for calcium-chloride brine. Likewise, current F-Chart library documentation explains correlation choices; it does not establish the EES version or libraries used during the original project.

## Recovery provenance

- [Manifest and hashes](../reconstruction/source-manifest.json) identify the supplied PDFs and recovered text files.
- [Source map](../reconstruction/source-map.json) maps recovered lines to printed appendix pages.
- [Verification record](../reconstruction/verification.json) records text-recovery checks; EES compilation and solution remain false.
- [Recovery notes](../reconstruction/RECONSTRUCTION-NOTES.txt) describe lost native state and normalization choices.
- [Reconciliation](reconciliation.md) separates original discrepancies from new explanatory arithmetic.
