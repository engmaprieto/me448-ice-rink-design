# Calculation 1 Heat loads

[Guide](../engineering-guide.md) · [Design basis](../design-basis.md) · [Next: brine and pumping](02-brine-and-pumping.md)

**Question:** How much heat must be removed to create the ice within the required time, and how much cooling is needed afterward?

Source: [script 1](../../reconstruction/report-transcriptions/01_heat_loads.txt), Appendix A, report pp. 23-25. The main reported outputs are in Table 1, p. 3; the independent historical output screenshots are on p. 26. They do not all agree.

## 1 Choose the system boundary and units

Take the rink's water/ice as the material being cooled. Use positive quantities for heat that must be removed. $Q$ denotes energy in BTU; $\dot Q$ denotes cooling rate in BTU/hr. The script's names beginning with `q_` generally mean rates even though they omit the dot.

For the following energy expressions, use mass in lbm, specific heat in BTU/(lbm F), latent heat in BTU/lbm, time in hours, and temperature differences in F. Radiation requires absolute temperatures and a Stefan-Boltzmann constant expressed in compatible units. Writing a unit next to an EES constant does not repair a missing numerical conversion elsewhere in an equation.

## 2 Convert geometry into ice mass

With rink area $A=LW$ and final ice thickness $\delta$:

$$
V_{ice}=A\delta,\qquad m_{ice}=\rho_{ice}A\delta.
$$

[Lines 15-36](../../reconstruction/report-transcriptions/01_heat_loads.txt#L15-L36) define the dimensions, temperatures, density call, mass, and startup time. Script 1 uses 30 ft by 15 ft with 1-inch ice thickness, giving 450 ft2 and 37.5 ft3 of final ice. Its density call must return units compatible with that volume.

Using the mass of the specified final ice layer for all three energy stages assumes no water loss during freezing. If instead the specified inch described the initial water layer, the mass basis would need to be defined differently. The piping script's 35-ft length is a separate unresolved geometry.

## 3 Remove sensible and latent energy

The startup path contains three stages:

| Stage | Energy expression | Script variable |
| --- | --- | --- |
| Cool water from 52 F to 32 F | $Q_w=m c_{p,w}(52-32)$ | `q_cool_water = Q_w/t` |
| Freeze water at 32 F | $Q_f=m h_{fus}$ | `q_freeze = Q_f/t` |
| Cool ice from 32 F to 24 F | $Q_i=m c_{p,i}(32-24)$ | `q_ice_cool = Q_i/t` |

The average rate for these stored-energy changes is:

$$
\dot Q_{stored}=\frac{Q_w+Q_f+Q_i}{t_{startup}}.
$$

The code implements these relationships in [lines 63-73](../../reconstruction/report-transcriptions/01_heat_loads.txt#L63-L73). Property values are treated as constants for each stage. A more detailed transient model would account for changing properties, changing boundary conditions, and a moving freezing front; dividing the total energy by time does not produce that time history.

**Design consequence:** shortening the startup deadline increases the average capacity required to remove the same stored energy. It does not by itself change the latent heat needed to freeze a fixed mass.

## 4 Add convection from the air

The heat-transfer relationship is:

$$
\dot Q_{conv}=hA(T_{air}-T_{ice}).
$$

The unknown convection coefficient $h$ is related to geometry, air properties, and buoyancy. The printed script calculates a film temperature, characteristic length $L_c=A/[2(L+W)]$, Rayleigh number, and Nusselt number before using $h=Nu\,k/L_c$. Its ice-surface correlation is printed as $Nu=0.52 Ra^{1/5}$.

See [lines 28-59](../../reconstruction/report-transcriptions/01_heat_loads.txt#L28-L59) and [77-78](../../reconstruction/report-transcriptions/01_heat_loads.txt#L77-L78). The constants, exponent, orientation, Rayleigh-number range, and property evaluation need to be checked against an appropriate natural-convection correlation. This is a cold upward-facing surface, so a correlation for a differently oriented heated plate should not be adopted without checking its applicability.

The script separately calculates tent-to-air convection as `q_tent`, but that value is not added directly to its final rink load. Treating the tent temperature as prescribed is different from solving a complete tent energy balance.

## 5 Model radiation with absolute temperatures

For two diffuse-gray surfaces in a suitable enclosure model, the exchange relationship can be written:

$$
\dot Q_{1\rightarrow2}=
\frac{\sigma(T_1^4-T_2^4)}
{\frac{1-\varepsilon_1}{A_1\varepsilon_1}+
\frac{1}{A_1F_{12}}+
\frac{1-\varepsilon_2}{A_2\varepsilon_2}}.
$$

This expression explains the resistance-network structure visible in [lines 82-92](../../reconstruction/report-transcriptions/01_heat_loads.txt#L82-L92). Multiple visible surfaces require an appropriate enclosure/radiosity treatment and geometric view factors; independently adding two-surface formulas is not automatically a consistent multi-surface solution. See the [heat-transfer reference](../references.md#heat-transfer-and-hydraulic-background) for the governing background.

The printed radiation section needs particular care:

- `sigma` is entered in SI units while temperatures and areas are formed in customary units. The numerical values must be converted to one compatible system before taking fourth powers.
- `q_rad_ice` represents a tent-to-ice expression, while `q_rad_surr` represents tent-to-sky exchange.
- `q_surr_ice` is calculated, but `q_rad` adds `q_rad_ice + q_rad_surr` instead. The ice load should account for net exchange at the ice boundary; a tent-to-sky exchange belongs to a tent balance.
- `epsilon` and `epsilon_ice` are different in the script, including in the last denominator term of `q_rad_ice`.
- View factors are assumed to be 0.5. Their geometry, completeness, and area reciprocity require verification.

The defined solar irradiance `Gs` and absorptivity `alpha_abs` do not enter the printed final totals. Their presence as variables is not evidence that solar gain was included.

## 6 Include the base and other operating loads explicitly

For a simple one-dimensional layered path, an area-based resistance would be:

$$
R''=\sum_j\frac{\delta_j}{k_j},\qquad
\dot Q_{ground}=\frac{A(T_{ground}-T_{cold})}{R''}.
$$

Embedded tubes create two-dimensional conduction, so the team used Abaqus to examine the actual material region. This simplified expression explains resistance units and direction; it is not a replacement model for the reported geometry.

With a 62 F ground boundary and colder rink, the expected ground contribution is a heat gain to the cooled system. The report labels its Table 2 values "heat loss to the ground"; the boundary-normal sign convention must be established. The first script's final equations do not include an explicit ground term.

A full operating-load inventory would also decide how to account for pump heat entering the fluid, piping gains, people, resurfacing, moisture, and any solar transmission through the tent. These are completeness questions, not additional loads measured by this project. Avoid double-counting a load already included at another boundary.

## 7 Assemble startup and steady loads

The printed totals in [lines 96-100](../../reconstruction/report-transcriptions/01_heat_loads.txt#L96-L100) are:

$$
\dot Q_{startup,printed}=\dot Q_{freeze}+\dot Q_{ice,cool}
+\dot Q_{water,cool}+\dot Q_{conv}+\dot Q_{rad},
$$

$$
\dot Q_{steady,printed}=\dot Q_{conv}+\dot Q_{rad}.
$$

These equations state the scope of the printed load model. A reconciled design would explicitly add any other applicable loads rather than interpreting the word "total" as proof of completeness.

### Worked arithmetic check from Table 1

The following uses the table's rates directly; it does not supply property values or rerun EES.

| Contribution, BTU/hr | 8-hour case | 24-hour case |
| --- | ---: | ---: |
| Water cooling | 5,411 | 1,804 |
| Freezing | 38,501 | 12,843 |
| Ice cooling | 1,063 | 354.5 |
| Convection | 3,941 | 3,941 |
| Radiation | 20,661 | 20,661 |
| Sum of listed contributions | **69,577** | **39,603.5** |
| Startup total printed in Table 1 | **69,577** | **39,594** |

The 8-hour sum agrees. The listed 24-hour components sum to 9.5 BTU/hr more than the printed total. That small arithmetic difference is separate from the much larger differences between Table 1 and the Appendix B screenshots.

## Startup rate and total energy

If the reported startup rates were constant over the entire stated duration, multiplying them by time would give:

$$
E_{8,illustrative}=69{,}577\times8=556{,}616\ \mathrm{BTU},
$$

$$
E_{24,illustrative}=39{,}594\times24=950{,}256\ \mathrm{BTU}.
$$

These are illustrations of rate versus energy, not validated startup energy predictions. Environmental gains change as water and ice temperatures change; compressor input additionally depends on COP and operating conditions. The lower 24-hour cooling rate does not establish lower electrical energy use.

## What the result drives

Cooling load is an input to brine heat transport and chiller capacity. It must carry a case label: startup or steady operation, together with the chosen geometry and assumptions. Script 2 enters 39,594 BTU/hr but calls it a steady-state load; script 3 enters 24,602 BTU/hr. Those are not an established consistent transfer of one case through the system.

Continue to [Calculation 2: brine, tubing, and pumping](02-brine-and-pumping.md).
