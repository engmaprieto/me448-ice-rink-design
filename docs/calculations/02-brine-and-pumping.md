# Calculation 2 Brine tubing and pumping

[Guide](../engineering-guide.md) Â· [Previous: heat loads](01-heat-loads.md) Â· [Next: chiller and expansion tank](03-chiller-and-expansion-tank.md)

**Question:** What tubing arrangement and brine flow can transport the cooling load, and what pressure difference must the pump provide?

Source: [script 2](../../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt), Appendix C, report pp. 27-29. Main results appear on pp. 9-12, with output screenshots on p. 30. The equations below explain the intended model; visible discrepancies in the printed implementation remain identified.

## 1 Turn spacing into circuits

The script uses a 35-ft length along the header direction, 15-ft cross-rink passes, and 4-inch spacing:

$$
N_{passes}=\frac{35}{4/12}=105,\qquad
n_{passes/circuit}=\frac{105}{15}=7,
$$

$$
\ell_{circuit}=7\times15=105\ \mathrm{ft}.
$$

See [lines 15-22](../../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L15-L22) and [47-51](../../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L47-L51). `Num_Tubes` counts cross-rink passes, while `Num_Loops` counts parallel circuits. One circuit contains seven passes in series. Its six return bends are counted separately for local losses.

This layout gives 1,575 ft of modeled straight tubing across all circuits. It does not include bend arc lengths, end spacing, or connection allowances. The report's different 3-inch spacing cannot simply be substituted while retaining the same integer circuit layout.

## 2 Calculate flow and evaluate properties

For internal diameter $D_i$ and mean velocity $v$:

$$
A_{flow}=\frac{\pi D_i^2}{4},\qquad
\dot V_{loop}=vA_{flow},\qquad
\dot m_{loop}=\rho\dot V_{loop}.
$$

For equal flow in $N$ circuits, total mass and volume flow are $N$ times the per-circuit quantities. Equal flow is a modeling assumption requiring hydraulic symmetry or balancing; it is not guaranteed merely by connecting branches to common headers.

The script obtains density, viscosity, conductivity, heat capacity, and Prandtl number at its average brine temperature. See [lines 33-45](../../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L33-L45) and [61-66](../../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L61-L66). The [documented EES brine functions](https://fchart.com/ees/eeshelp/hs612.htm) take `C` in mass percent. The selected concentration, unit system, and mass/molar property basis must be established before combining outputs with the geometry. In the documented English outputs, viscosity is lbm/(ft hr), so `mu_brine/rho_brine` uses an hourly time base while the entered velocity is ft/s. An explicit conversion is needed before using that kinematic viscosity in the Reynolds number; changing a displayed unit label does not change the numerical value.

### Worked geometry and flow check

Using only the printed $D_i=0.5$ inch, $v=3$ ft/s, and 15 circuits:

| Quantity | Illustrative calculation |
| --- | --- |
| Flow area per tube | $\pi(0.5/12)^2/4=0.00136354$ ft2 |
| Per-circuit volume flow | $3\times0.00136354=0.00409062$ ft3/s |
| Per-circuit flow in US gpm | Approximately **1.836 gpm** |
| Total flow in US gpm | Approximately **27.54 gpm** |
| Fluid volume in 1,575 ft of straight tubing | Approximately **16.06 US gal** |

The conversion uses 231 in3 per US gallon. These results follow from geometry and assumed velocity; no brine density or thermal performance is inferred. Table 3's 0.00053411 gpm per loop does not match this geometry/velocity calculation. The 500-gal system-volume assumption in script 3 needs an inventory of the other components; the tube-only volume does not establish total system volume.

Mass-flow labels also differ: Table 3 on report p. 9 gives 5.098 **lbm/hr** per circuit, while the third-script screenshot on p. 33 labels 5.098 **lbm/s**. The identical digits do not represent the same flow; a silent transfer changes the heat-capacity rate by a factor of 3,600.

## 3 Close the heat-transport balance

At the rink boundary, define brine inlet as cold supply and outlet as warm return. For constant heat capacity:

$$
\dot Q_{rink}=\dot m_{total}c_p(T_{out}-T_{in}).
$$

If the required load and allowable temperature rise are specified, this sets a required flow. If flow is selected first, the relationship predicts the rise. For a heat rate in BTU/hr, use lbm/hr and BTU/(lbm F); a lbm/s flow needs an explicit conversion.

Script 2 chooses a velocity, sets `T_mo=-20 F`, and calculates `q_totalcalculated` in [lines 68-74](../../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L68-L74). However, the equality tying that calculated heat rate to the required `q_total` is quoted out. The printed source therefore does not enforce agreement between transported heat and the specified load.

Both brine scripts also place `T_mi` below the calculated brine freezing point. The liquid operating range must be resolved before selecting property values or treating this as a feasible transport state.

## 4 Connect the surface load to the outside tube wall

Before calculating internal convection, the script estimates an outside tube temperature from the ice-surface temperature, plan-area heat flux, and an entered area-based resistance:

$$
T_{pipe}=T_s-\left(\frac{\dot Q_{total}}{A_{rink}}\right)R''_{surface\rightarrow pipe}.
$$

[Lines 25-31](../../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L25-L31) enter `R_ins=0.2021 hr ft2 F/BTU`, set `R_total_surface=R_ins`, and apply this expression. With the printed values:

$$
T_{pipe,illustrative}=24-\frac{39{,}594}{525}(0.2021)
\approx8.76\ \mathrm{F}.
$$

This checks the source expression, not the validity of its resistance. The FEA prescribes an outside tube temperature of **20.66 F**; those are different boundary values. Neither is the bulk brine temperature. A consistent handoff would establish the material path and rink area used for $R''$, then connect surface, outside wall, inside wall, and fluid temperatures through the relevant resistances.

$R''$ here is based on rink plan area. It has units hr ft2 F/BTU and multiplies a plan-area heat flux. The per-unit-length cylindrical tube resistance below has different dimensions and a different reference geometry; the two cannot be substituted without defining the area/length transformation and physical path.

## 5 Determine internal heat transfer

The dimensionless groups are:

$$
Re_D=\frac{vD_i}{\nu},\qquad Pr=\frac{\mu c_p}{k},\qquad
h_i=\frac{Nu_D k}{D_i}.
$$

[Lines 53-57](../../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L53-L57) use a Darcy friction factor and a Gnielinski-form expression for $Nu_D$. Correlations must match the Reynolds/Prandtl range, wall condition, roughness, and entrance development. A formula that returns a number outside its range is not thereby applicable. EES documents separate [laminar](https://www.fchartsoftware.com/ees/heat_transfer_library/internal_flow/hs1024.htm) and [turbulent](https://fchartsoftware.com/ees/heat_transfer_library/internal_flow/hs1022.htm) treatments; its [combined procedure](https://www.fchartsoftware.com/ees/heat_transfer_library/internal_flow/hs1020.htm) interpolates between regimes.

### Keep flow area separate from heat-transfer area

For a tube of length $\ell$, the wetted heat-transfer area is $\pi D_i\ell$, whereas flow area is $\pi D_i^2/4$. With an outside tube-wall boundary, consistent per-unit-length resistances are:

$$
R'_{conv}=\frac{1}{h_i\pi D_i},\qquad
R'_{wall}=\frac{\ln(D_o/D_i)}{2\pi k_{tube}},\qquad
G'=\frac{1}{R'_{conv}+R'_{wall}}.
$$

$G'$ has units of heat rate per length per temperature difference. An external ice/contact resistance would also be needed if the boundary were the ice surface rather than a specified outside tube wall.

The printed `R_total` instead adds `1/(h*A_tube)` using cross-sectional flow area to a wall resistance expressed per unit length. Those terms have different dimensions. This prevents interpreting its `U` directly as a verified conductance. This per-length explanation is a subsequent dimensional reconstruction, not an edit to the original source. See [MIT's heat-exchanger development](https://web.mit.edu/16.unified/www/FALL/thermodynamics/notes/node131.html) for the cylindrical-resistance and uniform-wall-temperature relationships.

For a constant outer boundary temperature $T_b$, constant properties, and consistent $G'$:

$$
T_{out}=T_b-(T_b-T_{in})
\exp\left(-\frac{G'\ell}{\dot m_{loop}c_p}\right).
$$

The exponent must be dimensionless, with flow and conductance using the same time base. The source has a related outlet expression but comments it out. Matching heat transport, wall-to-fluid transfer, and the imposed load requires solving a consistent set of independent equations.

## 6 Add pressure losses along a flow path

In coherent SI units, the Darcy-Weisbach and local-loss expressions are:

$$
\Delta p_{straight}=f_D\frac{\ell}{D}\frac{\rho v^2}{2},\qquad
\Delta p_{local}=\sum K\frac{\rho v^2}{2}.
$$

Here $\rho$ is kg/m3, velocity m/s, and pressure Pa. In customary units, using slug/ft3 with ft/s gives lbf/ft2; using lbm/ft3 requires the corresponding $g_c$ conversion. The source converts density to slugs for its loss expressions in [lines 77-104](../../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L77-L104). Whether the property-function result was actually in lbm/ft3 still needs to be established.

The source adds circuit straight friction, return bends, header friction, hose friction, and an assumed 5-psi heat-exchanger loss. It uses full total flow in lumped header and hose sections.

```mermaid
flowchart LR
    H["Shared supply piping"] --> C["One complete rink circuit"]
    C --> R["Shared return piping"]
    R --> X["Chiller brine-side loss"]
    X --> P["Required differential pressure"]
```

Parallel circuit losses are not added as though the fluid passes through all 15 circuits in sequence. The pump must support total flow at the loss of the controlling path through the network. A refined model would account for header segments, branch connections, valves, flow distribution, and actual component curves.

## 7 Convert pressure loss into pump head and input power

In coherent units:

$$
H=\frac{\Delta p}{\rho g},\qquad
P_{hyd}=\Delta p\dot V,\qquad
P_{shaft}=\frac{P_{hyd}}{\eta_p}.
$$

For customary mass density in lbm/ft3 and pressure in lbf/ft2, the head expression is $H=\Delta p\,g_c/(\rho_{lbm}g)$. The printed expression `deltaP_total/(rho_brine*g_c)` needs correction or a different explicitly demonstrated unit interpretation. Head is not obtained by treating the mass-force conversion factor as gravitational acceleration.

The source divides hydraulic power by an assumed 0.70 pump efficiency. A separate motor efficiency would be needed to obtain electrical input if 0.70 represents pump efficiency alone. The [Hydraulic Institute explanation](https://datatool.pumps.org/pump-fundamentals/pump-curves.html) distinguishes pump head, input power, efficiency, and required suction head.

Table 4 reports 267.5928618 ft, 115.979167 psi, and 2.662 hp for both cases. Their interpretation requires the brine density, exact unit conversions, and actual operating point. The 2.662-hp value corresponds to approximately 1.985 kW; it is not the approximately 2.403 kW stated in the pump paragraph. The latter also occurs in script 3 as a chiller-input calculation.

## 8 Use a pump curve to select an operating point

The report names Grundfos CR 15-7 and CR 15-8 for the respective cases. A defensible selection would overlay the system curve and the exact pump head-flow curve, then check efficiency, driver power, suction requirements, brine viscosity/density, temperature limits, and wetted-material compatibility. The pump produces the operating point where its curve and the system requirement meet; a single quoted head or efficiency does not establish that point.

Fill pressure is a static pressure condition; it is not the pump differential pressure. In a closed circulating loop, elevation gained and lost around the circuit balances, while friction and component losses still require pump head. A pressure profile is needed to check local limits and pump suction conditions.

For example, **if** the tank connection is at 15 psig during operation and the pump adds the reported 115.98 psi at the same elevation, pressure immediately after the pump would be about 131 psig before intervening losses. This is a conditional pressure-profile illustration, not a measured operating condition. The script's 40-psig maximum must be defined as a tank-local pressure or a whole-loop limit; it cannot be assumed to mean both under this interpretation.

The outputs handed to the next stage should be a labeled operating case with brine temperatures, total flow, fluid properties, and pressure losses. Continue to [Calculation 3](03-chiller-and-expansion-tank.md).
