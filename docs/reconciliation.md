# Results and unresolved source differences

[Guide](engineering-guide.md) · [Design basis](design-basis.md) · [Original issue record](../reconstruction/reconstruction-issues.json)

The original report contains main tables, printed scripts, and screenshots from runs that do not all use the same inputs. This page explains the differences and their consequences. It preserves the original results and identifies what would be needed for a consistent numerical model.

## Status of the evidence

| Evidence | Established | Not established |
| --- | --- | --- |
| Submitted PDFs | What the report and presentation state | That all stated values belong to one configuration |
| Reconstructed `.txt` source | Agreement of equation text with the printed appendices | EES compilation, property units, or numerical reproduction |
| Original output screenshots | Values shown for those historical runs | A complete run configuration or agreement with the displayed source |
| Abaqus images and setup narrative | Reported geometry, conditions, and displayed results | Recoverable model files, mesh independence, or transient freezing performance |
| New worked examples | Explicit arithmetic or derivations using declared inputs | A validated replacement design or new component selection |

## Differences recorded during source recovery

R identifiers correspond to the existing JSON record. Locations refer to PDF pages in the [report](source/ME448-Project-Report.pdf).

| ID | Source difference or missing information | Engineering consequence | Resolution needed |
| --- | --- | --- | --- |
| R01 | Saved EES unit system, variable information, solver settings, and unprinted revisions are missing; p. 2 describes lost settings | Property results and implicit unit assumptions cannot be recovered from names alone | Declare a reproducible unit/property configuration before solving |
| R02 | Script 1 uses 30-ft rink length; script 2 uses 35 ft, pp. 23 and 27 | Area, ice mass, cooling load, and circuit count change | Establish the intended geometry for each design case |
| R03 | Table 2 gives 3-inch spacing and 0.5-inch OD; script 2 gives 4-inch spacing, 0.5-inch ID, and 0.625-inch OD, pp. 8 and 27 | Thermal path, tubing count, flow, and pressure loss no longer describe the same layout | Resolve spacing, diameter definitions, and circuit topology together |
| R04 | Script 2 labels 39,594 BTU/hr steady-state; Table 1 identifies it as 24-hour startup, pp. 3 and 27 | The load handed into the hydraulic model is ambiguously labeled | Use explicitly named startup and maintenance cases |
| R05 | Printed script 1 sets 24 hours; p. 26 has two case screenshots | A saved two-case run procedure is absent | Define an explicit case table and all inputs for each run |
| R06 | A visible `F` follows the `rho_min` comment, p. 31 | Unexplained token outside the comment may prevent compilation | Correct it only in a separate working source version |
| R07 | Outlet-temperature and load-equality equations are quoted out, p. 28 | Printed code does not enforce both heat transport and required duty | Choose independent variables and restore a consistent equation set |
| R08 | Script 3 prints 25% concentration and 0.1 lbm/s per circuit; screenshots show 20% and 5.098 lbm/s; outlet temperatures also differ, pp. 27-33 | Properties, heat capacity rate, expansion, and NTU differ between records | Establish a single labeled set of inputs before comparison |
| R09 | SI/customary and hourly/per-second expressions coexist, pp. 24-25, 28-29, and 32 | Numerically plausible results can still be dimensionally wrong | Audit each expression and perform unit checks |
| R10 | Different absolute-temperature offsets, `B2` evaluated at the first film temperature, and a radiation sum that omits `q_surr_ice`, pp. 23-25 | Convection and radiation need both arithmetic and boundary review | Rebuild conversions and the relevant heat-balance network explicitly |
| R11 | Brine inlet set 5 F below its own freezing point, pp. 27 and 31 | Liquid-flow properties and the proposed operating state are not justified | Establish concentration and an appropriate liquid-temperature margin |
| R12 | Main heat-load and UA tables differ from appendix outputs, pp. 3, 17, 26, and 33 | Tables cannot be treated as reproduced outputs of the printed code | Compare independently checked calculations and fully specified runs |

## Additional observations from the walkthroughs

D identifiers are documentation-stage checks. They do not modify the historical R record or the equation files.

| ID | Observation | Interpretation and next check |
| --- | --- | --- |
| D01 | Table 1's 24-hour components sum to 39,603.5 BTU/hr rather than 39,594 | A 9.5-BTU/hr arithmetic discrepancy; separate from larger model/run differences |
| D02 | The report describes the lower 24-hour startup rate as energy savings | Rate, thermal energy, and electrical energy must be evaluated separately; duration and COP matter |
| D03 | Script 2 uses cross-sectional flow area in a convection resistance and adds a per-length wall resistance, p. 28 | Reconcile area/per-length definitions before interpreting `U` or the exponential outlet relationship |
| D04 | Script 2's 0.5-inch ID, 3 ft/s, and 15 circuits imply about 1.836 gpm per circuit and 27.54 gpm total; Table 3 gives 0.00053411 gpm per circuit | Geometry/velocity arithmetic alone conflicts with the reported volume flow; no density assumption is needed for this check |
| D05 | Table 4 gives 2.662 hp; the pump paragraph says approximately 2.403 kW, pp. 10 and 12 | 2.662 hp is approximately 1.985 kW. The 2.403-kW number also appears as script 3's load/COP result; reconcile power boundaries |
| D06 | Script 3's 10% capacity margin is applied to 24,602 BTU/hr, p. 31 | Approximately 2.255 tons is a maintenance-load calculation, not proof of either startup deadline |
| D07 | Script 3 uses rink tubing area with a brine/refrigerant LMTD, p. 32 | Identify which exchanger owns the inferred conductance and area; rink and evaporator boundaries differ |
| D08 | Printed tank denominator differs from the initial gas-volume relation, p. 31 | Under the walkthrough's stated ideal-gas assumptions, it calculates compressed final gas volume rather than initial vessel gas volume |
| D09 | Page 16 prints about 0.321 ft3 for $0.225/(54.7/29.7-1)$; direct evaluation gives 0.2673 ft3 | Resolve arithmetic independently from the tank-volume interpretation |
| D10 | The FEA setup prescribes surface temperature and also describes top heat flux, pp. 4-5 | Check the original boundary selections and reaction fluxes; prescribed temperature is not a predicted ability to meet startup time |
| D11 | Chart family labels and captions differ: the p. 14 chart names the 30XW family while its caption names 30MPW | Match exact equipment and data before accepting capacity, loss, COP, or compatibility claims |
| D12 | Presentation slide 6 says expansion valve while the image and report discuss expansion tanks | Correct explanatory terminology; a refrigerant expansion device and a brine expansion tank have different functions |
| D13 | Tables 3 and 5 both list -34.05 F for cold brine and refrigerant, pp. 9 and 17 | If they describe one constant-temperature evaporator, zero cold-end approach requires unbounded conductance for positive duty; reconcile temperature meanings |
| D14 | Table 3 uses lbm/hr and p. 33 uses lbm/s for the same 5.098 per-circuit value | Resolve the factor-of-3,600 ambiguity before calculating heat-capacity rate |
| D15 | Script 2's surface-to-tube expression gives about 8.76 F using its printed inputs, while FEA prescribes 20.66 F | Define and reconcile area-based surface resistance and outside wall boundary temperature before coupling the models |
| D16 | The source combines 15-psig fill, 40-psig maximum, and approximately 115.98-psi pump differential without a pressure profile | Distinguish tank-local pressure, running suction pressure, discharge pressure, and whole-loop component limits |

## A path to numerical validation

1. Establish one geometry and an explicit 8-hour, 24-hour, and maintenance case definition. Record which inputs are specified, assumed, or chosen.
2. Declare property models, concentration basis, units, and valid liquid operating temperatures.
3. Close the heat balances at the rink, brine loop, and chiller. Establish independent variables and which temperatures/flows are solved.
4. Check tubing thermal resistance, correlation regimes, pressure-loss paths, pump head conversion, and actual component operating points.
5. Recalculate expansion from an inventory and full temperature range, then use tank precharge/acceptance data.
6. Compare EES outputs with independent calculations. Preserve corrections, reasons, and resulting changes separately from recovered source.
7. Qualify the thermal simulation and physical equipment against defined acceptance criteria.

These steps are future validation work. Documentation can explain the submitted model in full while keeping unresolved values visible.
