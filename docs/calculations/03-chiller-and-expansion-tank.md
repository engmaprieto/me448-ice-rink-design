# Calculation 3 Chiller and expansion tank

[Guide](../engineering-guide.md) · [Previous: brine and pumping](02-brine-and-pumping.md) · [Open questions](../reconciliation.md)

**Questions:** What cooling duty must the chiller provide, and how much expansion volume must the fluid system accommodate?

Source: [script 3](../../reconstruction/report-transcriptions/03_expansion_tank_and_chiller.txt), Appendix E, report pp. 31-32. The equipment narrative is on p. 13, tank calculations appear as an image on p. 16, Table 5 is on p. 17, and output screenshots are on p. 33.

## 1 Define the chiller boundary

The chiller evaporator cools warm returning brine before that fluid goes back to the rink. Its cooling duty must cover the selected rink operating case plus applicable losses and heat added elsewhere in the fluid circuit. Selecting only the maintenance load does not demonstrate the ability to freeze the rink within the startup deadline.

[Lines 39-44](../../reconstruction/report-transcriptions/03_expansion_tank_and_chiller.txt#L39-L44) enter 24,602 BTU/hr, convert it to refrigeration tons, and add 10%:

$$
\dot Q_{tons}=\frac{24{,}602}{12{,}000}=2.05017,
\qquad C_{selected,printed}=1.1\dot Q_{tons}=2.25518\ \mathrm{tons}.
$$

This is arithmetic on the entered steady-state load. It is below both reported startup duties, so it is not a demonstrated startup capacity selection. For comparison, applying the same illustrative 10% multiplier to the report's startup duties would give approximately 6.378 and 3.629 tons. Those values are examples of the sizing calculation, not selections of actual equipment.

Actual capacity must be checked at the required brine concentration, leaving-fluid temperature, flow, and condenser conditions. The rating of an equipment family under other conditions does not supply that check. The report names Carrier 30XW-H and AquaSnap 30MPW, but the exact selection and chart identities remain unresolved.

## 2 Separate cooling duty from compressor input

For a refrigeration machine at a defined operating condition:

$$
COP=\frac{\dot Q_{evap}}{P_{compressor}},\qquad
P_{compressor}=\frac{\dot Q_{evap}}{COP}.
$$

The definition must specify which auxiliaries are included. Script 3 assumes `COP_chiller=3.0`, whereas the narrative gives 3.5 and 3.8 for the selected chillers. [Lines 49-52](../../reconstruction/report-transcriptions/03_expansion_tank_and_chiller.txt#L49-L52) give:

$$
\frac{24{,}602}{3}=8{,}200.67\ \mathrm{BTU/hr}
\approx2.403\ \mathrm{kW}.
$$

The conversion uses the international-table BTU, approximately 1,055.056 J. This numerical result explains the printed chiller input; it does not establish a manufacturer's performance at the specified cold-brine conditions. Total facility input would also account for pumping and other auxiliaries without double-counting them in COP.

At steady state, condenser rejection is approximately $\dot Q_{cond}=\dot Q_{evap}+P_{compressor}$ for the refrigeration boundary, with all quantities in compatible power units. The omitted condenser-water/heat-rejection installation therefore matters to a complete system design.

## 3 Interpret LMTD and conductance

For evaporation approximated at constant refrigerant temperature $T_{ref}$, define positive approach differences using the warm brine returning from the rink and cold brine supplied to it:

$$
\Delta T_{warm}=T_{rink,out}-T_{ref},\qquad
\Delta T_{cold}=T_{rink,in}-T_{ref},
$$

$$
\Delta T_{lm}=\frac{\Delta T_{warm}-\Delta T_{cold}}
{\ln(\Delta T_{warm}/\Delta T_{cold})},\qquad
UA=\frac{\dot Q_{evap}}{\Delta T_{lm}}.
$$

The source uses the rink-oriented names `T_mi` and `T_mo` in [lines 57-68](../../reconstruction/report-transcriptions/03_expansion_tank_and_chiller.txt#L57-L68). Swapping both end labels leaves the logarithmic mean unchanged. The differences must have compatible positive signs for this configuration; if equal, their common value is the limiting LMTD.

There is an important conditional check in the main tables: Table 3 on report p. 9 gives **-34.05 F** for cold brine entering the rink, and Table 5 on p. 17 also gives **-34.05 F** for refrigerant. If they describe the same constant-temperature evaporator, the cold-end approach is zero. With a finite warm-end difference, LMTD tends to zero as that approach tends to zero, so no finite $UA$ delivers a positive duty at the stated endpoints. The temperatures or their labels need reconciliation. The script's separately assumed refrigerant temperature, 10 F below cold brine, is a different input configuration.

$UA$ has units of heat rate per temperature difference. Dividing by an identified heat-transfer area gives an area-based coefficient $U$. The script uses the area of all rink tubing to form `A_total`, then combines it with a brine-to-refrigerant LMTD. Rink tubing and chiller evaporator are separate exchangers. A coefficient for the chiller requires its own heat-transfer area, or the result should remain a required $UA$ without assigning it to the rink area.

Table 5 labels its reported 1,318.8 value `BTU/hr*ft`, without a temperature denominator; that is not the expected conductance unit BTU/(hr F). The p. 33 screenshots give `UA=2905`. Neither the differing values nor the unit label establish one consistent exchanger result.

The [MIT heat-exchanger notes](https://web.mit.edu/16.unified/www/FALL/thermodynamics/notes/node131.html) explain the energy-balance and logarithmic-mean relationships. The [LBNL Buildings model library](https://simulationresearch.lbl.gov/modelica/releases/v7.0.2/help/Buildings_Fluid_HeatExchangers_BaseClasses.html) gives effectiveness/NTU relationships, including a constant-temperature phase-change side.

## 4 Read the effectiveness calculation against the correct boundary

For an idealized constant-temperature refrigerant side and a finite brine heat-capacity rate:

$$
C_{brine}=\dot m c_p,\qquad NTU=\frac{UA}{C_{brine}},\qquad
\varepsilon=1-e^{-NTU}.
$$

For brine cooling in that evaporator:

$$
\varepsilon=\frac{T_{rink,out}-T_{rink,in}}
{T_{rink,out}-T_{ref}},\qquad
T_{ref}=T_{rink,out}-\frac{T_{rink,out}-T_{rink,in}}{\varepsilon}.
$$

This explains the algebra of the last printed `T_ref_calculated` expression. It is not an independent validation when $UA$ has already been inferred from the same load and assumed temperatures. The load, flow, temperature rise, and conductance must first satisfy one consistent energy balance.

Script 3 hard-codes 0.1 lbm/s per loop; the result screenshots show 5.098 lbm/s instead. Its BTU/hr conductance and per-second mass flow also need an explicit time-base conversion before computing dimensionless NTU. The printed loop dimensions do not fix these discrepancies.

## 5 Convert fluid expansion into an acceptance volume

For a fixed fluid mass and a reference cold-state volume $V_c$:

$$
m=\rho_cV_c,\qquad
V_h=\frac{m}{\rho_h},\qquad
\Delta V=V_c\left(\frac{\rho_c}{\rho_h}-1\right).
$$

Here $\rho_c$ and $\rho_h$ are the densities at the selected cold and warm states; labels must follow the actual property trend. The source names them `rho_max` and `rho_min`. [Lines 15-31](../../reconstruction/report-transcriptions/03_expansion_tank_and_chiller.txt#L15-L31) assume 500 gal and calculate this density-based volume change.

The selected temperature range should cover filling, idle storage, startup, and operation as applicable. Using only two subzero operating temperatures does not establish the largest expansion the system could encounter. The 500-gal assumption requires a component inventory, and the printed `rho_min` line contains a stray `F` outside its comment.

## 6 Relate acceptance volume to gas compression

The following is a subsequent explanatory derivation under simplified assumptions: isothermal ideal-gas compression, an initially gas-filled tank at absolute pressure $P_c$, and no initial liquid reserve. Let $V_{g,c}$ be initial gas volume and $P_h$ the maximum absolute pressure:

$$
P_cV_{g,c}=P_h(V_{g,c}-\Delta V),
$$

$$
V_{g,c}=\frac{\Delta V}{1-P_c/P_h}.
$$

This illustrates why nominal vessel volume, liquid acceptance, initial gas volume, and compressed gas volume must be distinguished. Real sizing also depends on gas precharge, fill condition, liquid reserve, allowable pressure, and the manufacturer's acceptance data. [Caleffi's sizing method](https://www.caleffi.com/sites/default/files/media/external-file/01079_EN.pdf), PDF p. 3, uses absolute gas pressures and an acceptance-factor denominator. Its water/glycol product data do not establish calcium-chloride compatibility for this project.

The printed formula is instead:

```text
V_tank_min = DeltaV / ((P_max_abs / P_fill_abs) - 1)
```

Under the simplified assumptions above, that expression gives $V_{g,h}=P_c\Delta V/(P_h-P_c)$, the **compressed final gas volume**, rather than $V_{g,c}$, the initial gas-filled vessel volume. The source does not establish a different interpretation that would make it a complete nominal-tank sizing rule.

### Worked pressure and arithmetic check

Using the source's 15 psig fill pressure, 40 psig maximum pressure, and 14.7 psi atmospheric offset gives 29.7 and 54.7 psia. Using the image on report p. 16's approximately 0.225-ft3 expansion:

| Calculation | Illustrative result |
| --- | ---: |
| Printed formula: $0.225/(54.7/29.7-1)$ | **0.2673 ft3** |
| Initial gas-volume relation: $0.225/(1-29.7/54.7)$ | **0.4923 ft3** |
| Initial relation with the source's 20% multiplier | **0.5908 ft3** |

These calculations check algebra using a reported expansion value. They do not validate that expansion, establish the actual precharge, or select a tank. The p. 16 image prints about 0.321 ft3 for the first expression, which does not match direct evaluation. Its stated selected tank values therefore need both arithmetic and interpretation checks.

## 7 Hand the requirements to component selection

The output should be a clearly identified startup/steady case with required evaporator capacity, fluid and temperatures, total flow, allowable exchanger pressure loss, condenser conditions, and a separate expansion-tank volume/pressure specification. The report's component names and plotted charts are evidence of selection work; they do not complete those checks.

Review [the source discrepancies](../reconciliation.md) before using any of these reported values as design inputs.
