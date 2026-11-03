# Design basis and assumptions

[Guide](engineering-guide.md) Â· [Variables](variables.md) Â· [Reconciliation](reconciliation.md)

The source contains several versions of the design. These tables identify where each input comes from without choosing a single corrected configuration. "Printed input" means that a value appears in the recovered equations; it does not establish whether the assignment specified it or the team chose it.

## Evidence and calculation conventions

| Label | Meaning |
| --- | --- |
| Reported result | Value stated in a report table or screenshot |
| Printed input | Constant or formula in the recovered scripts |
| Stated assumption | Described as assumed or estimated in the source |
| Illustrative check | Subsequent calculation using declared inputs, with no claim to reproduce an EES run |
| Unresolved | Sources disagree or the information needed to check the result is absent |

Use positive cooling load for heat that refrigeration must remove from the rink. In the rink, brine normally warms from supply to return; define its rise as $T_{out}-T_{in}>0$. In the chiller, the same brine is cooled. Every temperature labeled "inlet" or "outlet" must identify the component boundary.

The walkthroughs state the units needed for their equations. They do not restore original EES settings. In particular, BTU/hr heat rates must use mass flow in lbm/hr with BTU/(lbm F) heat capacity; mass flow in lbm/s requires a seconds-to-hours conversion. Coherent SI formulas use kg, m, s, Pa, J, and K. Temperature differences in F and Rankine have the same increment; radiation and ideal-gas calculations require absolute temperatures.

## Requirements and temperatures

| Quantity | Value in source | Status and source |
| --- | --- | --- |
| Startup duration | 8 or 24 hr | Design cases, report Table 1, p. 3; script 1 prints only `t=24` |
| Initial water temperature | 52 F | Script 1, Appendix A, p. 23 |
| Freezing temperature used for water | 32 F | Script 1, p. 23 |
| Target ice temperature | 24 F | Script 1 and FEA boundary list, pp. 4 and 23 |
| Air temperature | 80 F | Script 1, p. 23 |
| Tent temperature | 90 F | Script 1, p. 23; treated as prescribed |
| Effective sky temperature | 55 F | Script 1, p. 23 |
| Ground temperature | 62 F | FEA boundary list, p. 4 |
| Tube outside surface temperature | 20.66 F | FEA boundary and Table 2, pp. 4 and 8 |
| Ice thickness | 1 inch | Script 1, p. 23 |
| Tent height variable | 8 ft | Script 1, p. 23; not used in its printed radiation geometry |

The brine inlet is defined in scripts 2 and 3 as 5 F below the calculated freezing point of the brine. That is unresolved: a liquid-brine operating temperature needs a margin above its freeze boundary. It must not be confused with a colder refrigerant temperature. No replacement supply temperature is selected here.

## Geometry and piping

| Quantity | Script value | Other source or interpretation |
| --- | --- | --- |
| Rink length | 30 ft in script 1; 35 ft in script 2 | Unresolved geometry difference |
| Rink width | 15 ft in both | Printed input |
| Rink area | 450 or 525 ft2 from the respective dimensions | Derived arithmetic; not one agreed area |
| Tube spacing | 4 inches in script 2 | Table 2, p. 8 gives 3 inches |
| Tube inside diameter | 0.5 inch in scripts 2 and 3 | Table 2 instead labels 0.5 inch as outside diameter |
| Tube outside diameter | 0.625 inch in script 2 | Conflicts with Table 2's diameter description |
| Parallel circuits | 15 | Printed input in scripts 2 and 3 |
| Cross-rink tube passes | 105 | Script 2: rink length divided by spacing |
| Passes per circuit | 7 | 105 divided by 15 circuits |
| Circuit straight length | 105 ft | Seven 15-ft passes; bend/connection lengths omitted by this expression |
| Header diameter and modeled length | 1.5 inches; 30 ft | Script 2, pp. 28-29 |
| Hose diameter and modeled length | 2 inches; 70 ft | Script 2, p. 29 |
| Number and coefficient of return bends | 6 per circuit; $K=0.2$ each | Script 2; coefficient is explicitly assumed |

The straight-length calculation describes serpentine paths within parallel circuits. The total 15-circuit tubing length is not the serial hydraulic path traversed by one fluid parcel. Shared header flow also changes along its length as branches take off or return; the printed header model uses a lumped full-flow treatment.

## Fluid and thermal model inputs

| Quantity | Printed value | Status and interpretation |
| --- | --- | --- |
| Secondary fluid | Calcium-chloride solution | `CaCl2` property calls |
| Concentration | 25 mass % in scripts 2 and 3 | Third-script screenshots show 20 mass %; the selected input differs |
| Tube velocity | 3 ft/s | Explicit assumption in script 2 |
| Brine outlet at rink boundary | -20 F in script 2 | Assigned constant, despite comment "to be determined" |
| Brine outlet carried into script 3 | -15 F | Different assigned constant; screenshots also differ |
| Total brine volume | 500 US gal | Assigned in script 3; component-by-component inventory not supplied |
| PEX conductivity | 0.24 BTU/(hr ft F) | Printed input in script 2 |
| Surface-to-tube resistance used in script 2 | 0.2021 hr ft2 F/BTU | Entered directly; differs from reported FEA resistance values |
| Tent and ice emissivity | 0.87 and 0.95 | Explicit assumptions in script 1 |
| Tent-to-ice view factor | 0.5 | Assumed; the code comment calls it a fraction of heat transfer |
| Solar input and absorptivity | `Gs=291.6`, `alpha_abs=0.39` | Defined but not used in the printed startup/steady totals |
| Chiller COP | 3.0 in script 3 | Narrative gives 3.5 and 3.8 for the selected chillers |
| Chiller capacity multiplier | 1.1 | Stated 10% margin on the entered steady load |
| Tank size multiplier | 1.2 | Stated 20% margin on the printed tank formula |

A view factor is a geometric radiation-exchange factor, not a prescribed fraction of net heat transfer. The [documented EES brine functions](https://fchart.com/ees/eeshelp/hs612.htm) require concentration in mass percent, even with molar property settings. Ice, water, air, and brine properties require a declared property model, temperature, pressure where applicable, concentration, and unit/mass-or-molar settings. The report's screenshots show why those choices cannot be inferred solely from variable names.

## Pressure and hydraulic inputs

| Quantity | Value | Source |
| --- | --- | --- |
| Pump efficiency | 0.70 | Assumed in script 2 |
| Heat-exchanger pressure loss | 5 psi | Assigned in script 2, p. 29 |
| System fill pressure | 15 psig | Script 3, p. 31 |
| Maximum pressure used in tank calculation | 40 psig | Script 3, p. 31 |
| Atmospheric pressure used for conversion | 14.7 psi | Script 3, p. 31 |
| Converted fill / maximum pressure | 29.7 / 54.7 psia | Arithmetic, not a documented gas precharge setting |

The tank pressure limit, pump differential pressure, and local operating pressure are different quantities. Comparing them requires a pressure profile and a defined tank connection. The 5-psi exchanger loss is not a substitute for a verified pressure-loss curve at the actual brine flow.

## Team FEA material inputs

These are printed on report page 4. Values are retained as reported, including rough or placeholder inputs. Conductivity is stated per inch of length, so it must be converted before combining with per-foot expressions.

| Material | Conductivity, BTU/(hr in F) | Specific heat, as listed | Density, as listed |
| --- | ---: | --- | --- |
| Ice | 0.108 | 0.4952 BTU/(lb F) | 0.0329 lb/in3 |
| Insulation | 0.00173 | 10, described as a rough estimate; no unit printed | 1, no unit printed |
| Concrete | 0.0674 | 0.239 BTU/(lb F) | 1, no unit printed |

The reported analysis step is steady state. Heat capacity and density do not determine a no-storage steady conduction solution, even if the software input workflow required entries. These placeholder values do not support a transient freezing simulation.

## Source locations

- [Report](source/ME448-Project-Report.pdf): Table 1 p. 3; FEA setup pp. 4-8; piping/pumps pp. 9-12; chillers/tank pp. 13-17; controls p. 20; EES source pp. 23-25, 27-29, and 31-32.
- [Presentation](source/ME448-Project-Presentation.pdf): system overview slide 4; methods slide 5; selection considerations slide 6; future work slide 8.
- [Reconciliation](reconciliation.md): conflicts and what must be established before numerical validation.
