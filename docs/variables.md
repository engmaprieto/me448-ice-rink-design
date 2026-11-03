# EES variable dictionary

[Guide](engineering-guide.md) Â· [Design basis](design-basis.md) Â· [Calculation chapters](calculations/01-heat-loads.md)

Names are scoped to each script. A reused name can have a different meaning: `A_tube` is flow area, `A_total` is tubing surface area, script 2 `U` is the inverse of an inconsistent resistance, and script 3 `U` is area based. `C` means concentration; `C_min` means heat-capacity rate.

Units below describe printed labels or the dimensions needed for interpretation. They do not recover the missing EES unit system. Property-function output units must be established separately. [EES brine functions](https://fchart.com/ees/eeshelp/hs612.htm) define `C` as mass percent; the historical choice of 20 versus 25 remains unresolved. The dictionary covers every uncommented left-hand-side variable in the three recovered files.

## Script 1 Heat loads

| EES names | Units or dimensions | Meaning and source |
| --- | --- | --- |
| `L`, `W` | ft | Rink length and width; script 1 uses 30 by 15 ft. [Source](../reconstruction/report-transcriptions/01_heat_loads.txt#L15-L16). |
| `A` | ft2 | Rink plan area L times W. [Source](../reconstruction/report-transcriptions/01_heat_loads.txt#L17). |
| `Height` | ft | Tent height input; not used in the printed radiation expressions. [Source](../reconstruction/report-transcriptions/01_heat_loads.txt#L18). |
| `thickness` | ft after conversion | Final ice thickness, entered as 1 inch. [Source](../reconstruction/report-transcriptions/01_heat_loads.txt#L19). |
| `T_ice`, `T_initial`, `T_final` | F | Target ice temperature, initial water temperature, and freezing-stage temperature. [Source](../reconstruction/report-transcriptions/01_heat_loads.txt#L20-L25). |
| `T_ice_R`, `T_initial_R`, `T_final_R` | Rankine intended | Absolute forms of those temperatures; the suffix does not itself perform a conversion. [Source](../reconstruction/report-transcriptions/01_heat_loads.txt#L21-L23). |
| `T_air`, `T_tent`, `T_sky` | F | Prescribed air, tent, and effective sky temperatures. [Source](../reconstruction/report-transcriptions/01_heat_loads.txt#L27-L32). |
| `T_air_R`, `T_tent_R`, `T_sky_R` | Rankine intended | Absolute temperatures for radiation and related differences; T_air_R contains the differing 459.76 offset. [Source](../reconstruction/report-transcriptions/01_heat_loads.txt#L26-L33). |
| `T_film`, `T_film2` | F | Mean ice/air and tent/air temperatures for property evaluation. [Source](../reconstruction/report-transcriptions/01_heat_loads.txt#L28-L31). |
| `rho_ice` | mass/volume | Ice density from Ice_inc property call; output units depend on EES configuration. [Source](../reconstruction/report-transcriptions/01_heat_loads.txt#L34). |
| `m_ice` | mass | Ice mass formed from density and rink volume; intended energy equations require compatible mass units. [Source](../reconstruction/report-transcriptions/01_heat_loads.txt#L35). |
| `t` | hr | Startup duration; printed as 24 hours. [Source](../reconstruction/report-transcriptions/01_heat_loads.txt#L36). |
| `g` | ft/hr2 after printed conversion | Gravitational acceleration used in buoyancy calculation. [Source](../reconstruction/report-transcriptions/01_heat_loads.txt#L37). |
| `P_atm` | psia | Pressure supplied to air and water property calls. [Source](../reconstruction/report-transcriptions/01_heat_loads.txt#L38). |
| `B`, `B2` | 1/absolute temperature | Air expansion-coefficient approximation; both use T_film in the printed source. [Source](../reconstruction/report-transcriptions/01_heat_loads.txt#L39-L40). |
| `nu`, `nu2` | length2/time | Kinematic viscosity at the two film temperatures; time base must match the Rayleigh expression. [Source](../reconstruction/report-transcriptions/01_heat_loads.txt#L41-L42). |
| `alpha`, `alpha2` | length2/time | Thermal diffusivity at the two film temperatures; not surface absorptivity. [Source](../reconstruction/report-transcriptions/01_heat_loads.txt#L43-L44). |
| `k`, `k2` | heat rate/(length temperature) | Air conductivity at the two film temperatures; property-output units require configuration. [Source](../reconstruction/report-transcriptions/01_heat_loads.txt#L45-L46). |
| `cp_water`, `cp_ice` | energy/(mass temperature) | Water and ice specific heat capacities for sensible-energy calculations. [Source](../reconstruction/report-transcriptions/01_heat_loads.txt#L47-L68). |
| `Gs` | BTU/(hr ft2) printed | Solar irradiance input, not included in the printed final load expressions. [Source](../reconstruction/report-transcriptions/01_heat_loads.txt#L48). |
| `sigma` | W/(m2 K4) printed | Stefan-Boltzmann constant; numerical units must agree with radiation areas and temperatures. [Source](../reconstruction/report-transcriptions/01_heat_loads.txt#L49). |
| `epsilon_tent`, `epsilon_ice`, `epsilon` | dimensionless | Tent, ice, and a separate generic emissivity; generic epsilon is 0.87. [Source](../reconstruction/report-transcriptions/01_heat_loads.txt#L50-L52). |
| `L_c` | ft | Plan-area/perimeter characteristic length. [Source](../reconstruction/report-transcriptions/01_heat_loads.txt#L53). |
| `R_a_L_c`, `R_a_L_c_2` | dimensionless if units agree | Ice/air and tent/air Rayleigh numbers. [Source](../reconstruction/report-transcriptions/01_heat_loads.txt#L54-L57). |
| `Nu_L_c`, `Nu_L_c_2` | dimensionless | Nusselt numbers from the printed correlations; applicability needs review. [Source](../reconstruction/report-transcriptions/01_heat_loads.txt#L55-L58). |
| `h`, `h_tent` | heat rate/(area temperature) | Ice/air and tent/air convection coefficients. [Source](../reconstruction/report-transcriptions/01_heat_loads.txt#L56-L59). |
| `HF` | energy/mass | Latent heat of fusion returned by enthalpy_fusion. [Source](../reconstruction/report-transcriptions/01_heat_loads.txt#L63). |
| `q_freeze` | BTU/hr intended | Average rate to remove latent freezing energy. [Source](../reconstruction/report-transcriptions/01_heat_loads.txt#L64). |
| `q_ice_cool`, `q_cool_water` | BTU/hr intended | Average sensible-cooling rates for ice and liquid water. [Source](../reconstruction/report-transcriptions/01_heat_loads.txt#L69-L73). |
| `q_conv` | BTU/hr intended | Surface convection heat gain used in final totals. [Source](../reconstruction/report-transcriptions/01_heat_loads.txt#L77). |
| `q_tent` | heat rate | Tent-to-air convection result; not directly used in final rink totals. [Source](../reconstruction/report-transcriptions/01_heat_loads.txt#L78). |
| `alpha_abs` | dimensionless | Absorptivity input; separate from thermal diffusivity; not used in final totals. [Source](../reconstruction/report-transcriptions/01_heat_loads.txt#L82). |
| `q_rad_exch` | heat rate | Diagnostic tent/air fourth-power expression; not used in final totals. [Source](../reconstruction/report-transcriptions/01_heat_loads.txt#L83). |
| `q_rad_ice`, `q_rad_surr`, `q_surr_ice` | heat rate | Printed tent/ice, tent/sky, and sky/ice expressions. Only the first two enter q_rad. [Source](../reconstruction/report-transcriptions/01_heat_loads.txt#L84-L86). |
| `q_rad`, `q_net` | heat rate | Both sum q_rad_ice and q_rad_surr; the boundary interpretation requires review. [Source](../reconstruction/report-transcriptions/01_heat_loads.txt#L87-L88). |
| `F_tent_ice`, `F_tent_surr`, `F_ice_surr` | dimensionless | Assumed radiation view factors; geometric validity and reciprocity are not demonstrated. [Source](../reconstruction/report-transcriptions/01_heat_loads.txt#L90-L92). |
| `q_startup`, `q_steady_state` | BTU/hr intended | Printed startup and steady totals; neither explicitly adds ground heat gain. [Source](../reconstruction/report-transcriptions/01_heat_loads.txt#L96-L100). |

## Script 2 Brine and pumping

| EES names | Units or dimensions | Meaning and source |
| --- | --- | --- |
| `L`, `W`, `A` | ft; ft2 | Rink length, width, and plan area; script 2 uses 35 by 15 ft. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L15-L17). |
| `S` | ft after conversion | Tube-pass spacing, entered as 4 inches. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L18). |
| `Num_Tubes` | count | Cross-rink passes computed from L/S; distinct from parallel circuit count. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L19). |
| `D_i`, `D_o` | ft after conversion | Tube internal and outside diameters, entered as 0.5 and 0.625 inches. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L21-L22). |
| `k_tube` | BTU/(hr ft F) printed | PEX wall conductivity. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L23). |
| `R_ins`, `R_total_surface` | hr ft2 F/BTU printed | Entered surface-to-tube area-based resistance and its alias. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L25-L30). |
| `T_s`, `T_pipe` | F intended | Ice surface temperature and calculated outer tube-boundary temperature. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L27-L31). |
| `q_total` | BTU/hr | Entered required load, 39,594; source comment calls it steady state although Table 1 calls it startup. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L29). |
| `C` | mass percent (EES definition) | Brine concentration, entered as 25 mass percent; screenshots use a different input. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L33). |
| `T_freeze`, `T_mi`, `T_mo`, `T_m` | F intended | Brine freezing point, rink inlet, rink outlet, and mean temperature; inlet is defined below freezing point, outlet fixed at -20 F. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L34-L38). |
| `rho_brine` | mass/volume | Brine density at mean temperature and concentration. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L40). |
| `mu_brine` | mass/(length time) | Dynamic viscosity; verify returned mass/force and time units. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L41). |
| `nu_brine` | length2/time | Kinematic viscosity mu/rho with consistent units. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L42). |
| `k_brine` | heat rate/(length temperature) | Brine thermal conductivity. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L43). |
| `cp_brine` | energy/(mass temperature) | Brine specific heat capacity. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L44). |
| `Pr_brine` | dimensionless | Brine Prandtl number from property function. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L45). |
| `Num_Loops`, `Tubes_per_Loop` | count | Parallel circuits, and passes in series within each circuit. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L47-L48). |
| `x` | ft | Modeled straight tube length per circuit, 105 ft. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L49). |
| `v_brine` | ft/s | Assumed tube mean velocity, 3 ft/s. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L50). |
| `A_tube` | ft2 | Cross-sectional flow area; not the wetted heat-transfer area. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L51). |
| `Re_D` | dimensionless | Tube Reynolds number; requires matching velocity, diameter, and viscosity units. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L53). |
| `f`, `f8` | dimensionless | Tube friction-factor result and f/8 used in the Nusselt expression; Darcy convention must be consistent. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L54-L55). |
| `Nu_D` | dimensionless | Nusselt number calculated with a Gnielinski-form equation. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L56). |
| `h` | heat rate/(area temperature) | Internal brine convection coefficient. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L57). |
| `R_total`, `U` | ambiguous in printed model | Printed combined resistance and its inverse mix flow-area and per-length forms; see Calculation 2. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L58-L59). |
| `m_dot_loop`, `m_dot_total` | lbm/s intended | Per-circuit and total mass flow; this label assumes compatible property units. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L61-L65). |
| `m_dot_loop_hr`, `m_dot_total_hr` | lbm/hr intended | Mass flow variables multiplied by 3600 s/hr. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L63-L66). |
| `Theta` | dimensionless intended | Exponential temperature ratio; conductance and flow time bases need reconciliation. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L68). |
| `DeltaT_loop` | F difference | Brine warming across rink: T_mo minus T_mi. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L70). |
| `q_totalcalculated` | heat rate, time base unresolved | Transported heat from mass flow, heat capacity, and rise; equality to q_total is disabled. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L72). |
| `rho_brine_slug` | slug/ft3 intended | Density divided by 32.174 lbm/slug; assumes original density was lbm/ft3. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L77). |
| `deltaP_friction`, `deltaP_bends`, `deltaP_loop` | lbf/ft2 intended | Circuit straight-tube, bend, and combined pressure losses. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L78-L82). |
| `Num_Bends`, `K_bend` | count; dimensionless | Return bends per circuit and assumed loss coefficient per bend. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L79-L80). |
| `D_header`, `A_header` | ft; ft2 | Header diameter and cross-sectional flow area. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L84-L85). |
| `m_dot_total_s` | lbm/s intended | Total flow converted back from the per-hour variable. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L87). |
| `v_header`, `Re_header`, `f_header` | ft/s; dimensionless | Lumped full-flow header velocity, Reynolds number, and friction factor. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L88-L90). |
| `L_header`, `deltaP_header` | ft; lbf/ft2 intended | Modeled header length and friction loss. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L91-L92). |
| `D_hose`, `A_hose` | ft; ft2 | Hose diameter and cross-sectional flow area. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L94-L95). |
| `v_hose`, `Re_hose`, `f_hose` | ft/s; dimensionless | Hose velocity, Reynolds number, and friction factor. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L96-L98). |
| `L_hose`, `deltaP_hose` | ft; lbf/ft2 intended | Modeled hose length and friction loss. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L99-L100). |
| `deltaP_HX`, `deltaP_total` | lbf/ft2 intended | Assigned 5-psi exchanger loss after conversion, and sum of modeled system losses. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L102-L104). |
| `g_c` | lbm ft/(lbf s2) | Mass-force conversion factor; not gravitational acceleration. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L106). |
| `Pump_head` | ft intended | Printed head expression requires a dimensional correction or demonstrated alternate interpretation. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L107). |
| `Qv_total` | ft3/s intended | Total brine volumetric flow. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L110). |
| `Pump_power`, `Pump_power_hp` | ft lbf/s; hp intended | Hydraulic power divided by assumed pump efficiency, then converted to horsepower. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L112-L114). |
| `Pump_efficiency` | dimensionless | Assumed 0.70; no separate motor efficiency is defined. [Source](../reconstruction/report-transcriptions/02_piping_pressure_and_pump.txt#L116). |

## Script 3 Chiller and expansion tank

| EES names | Units or dimensions | Meaning and source |
| --- | --- | --- |
| `V_system` | ft3 after conversion | System brine inventory, entered as 500 US gal; cold reference state must be specified. [Source](../reconstruction/report-transcriptions/03_expansion_tank_and_chiller.txt#L15). |
| `C` | mass percent (EES definition) | Printed CaCl2 concentration is 25 mass percent; screenshots show 20 mass percent. [Source](../reconstruction/report-transcriptions/03_expansion_tank_and_chiller.txt#L17). |
| `T_freeze`, `T_mi`, `T_mo`, `T_m` | F intended | Freezing point, rink cold supply, rink warm return, and mean. T_mo is fixed at -15 F in this script. [Source](../reconstruction/report-transcriptions/03_expansion_tank_and_chiller.txt#L18-L22). |
| `T_min`, `T_max` | F intended | Cold and warm states used for density-based expansion; scope excludes any unmodeled filling/storage extremes. [Source](../reconstruction/report-transcriptions/03_expansion_tank_and_chiller.txt#L23-L24). |
| `P_fill`, `P_max` | psig | Entered fill pressure and maximum pressure for the tank calculation. [Source](../reconstruction/report-transcriptions/03_expansion_tank_and_chiller.txt#L25-L26). |
| `rho_min`, `rho_max` | mass/volume | Densities evaluated at T_max and T_min. A stray F follows the rho_min line outside its comment. [Source](../reconstruction/report-transcriptions/03_expansion_tank_and_chiller.txt#L28-L29). |
| `DeltaV` | ft3 if density units agree | Fluid volume increase relative to the reference density and volume. [Source](../reconstruction/report-transcriptions/03_expansion_tank_and_chiller.txt#L31). |
| `P_fill_abs`, `P_max_abs` | psia | Gauge pressures plus the entered 14.7-psi atmospheric offset. [Source](../reconstruction/report-transcriptions/03_expansion_tank_and_chiller.txt#L33-L34). |
| `V_tank_min`, `V_tank_selected` | ft3 intended | Printed pressure-ratio result and 20-percent multiplier; initial versus final gas-volume interpretation needs correction. [Source](../reconstruction/report-transcriptions/03_expansion_tank_and_chiller.txt#L35-L37). |
| `q_total`, `q_cooling` | BTU/hr | Entered steady cooling load 24,602 and its alias; not a demonstrated startup load. [Source](../reconstruction/report-transcriptions/03_expansion_tank_and_chiller.txt#L39-L40). |
| `q_cooling_tons`, `Chiller_capacity` | refrigeration tons | Cooling load divided by 12,000, then increased by 10 percent. [Source](../reconstruction/report-transcriptions/03_expansion_tank_and_chiller.txt#L42-L44). |
| `COP_chiller` | dimensionless | Assumed refrigeration coefficient of performance, 3.0. [Source](../reconstruction/report-transcriptions/03_expansion_tank_and_chiller.txt#L49). |
| `q_input`, `P_input_kW` | BTU/hr; kW | Cooling load divided by COP and converted to input power; not pump power. [Source](../reconstruction/report-transcriptions/03_expansion_tank_and_chiller.txt#L50-L52). |
| `T_ref` | F intended | Assumed refrigerant temperature, 10 F below the entered cold-brine temperature. [Source](../reconstruction/report-transcriptions/03_expansion_tank_and_chiller.txt#L57). |
| `del_T1`, `del_T2`, `delta_T_lm` | temperature difference | Cold-end and warm-end approach differences and their logarithmic mean. [Source](../reconstruction/report-transcriptions/03_expansion_tank_and_chiller.txt#L58-L60). |
| `Num_Loops`, `x`, `D_i` | count; ft; ft | Copied circuit count, per-circuit straight length, and tube internal diameter. [Source](../reconstruction/report-transcriptions/03_expansion_tank_and_chiller.txt#L62-L64). |
| `A_total` | ft2 | Total inner surface area of rink tubing; not established as chiller evaporator area. [Source](../reconstruction/report-transcriptions/03_expansion_tank_and_chiller.txt#L66). |
| `UA` | BTU/(hr F) intended | Required exchanger conductance from cooling load and LMTD. [Source](../reconstruction/report-transcriptions/03_expansion_tank_and_chiller.txt#L68). |
| `U` | BTU/(hr ft2 F) intended | UA divided by rink tube area; physical exchanger boundary is unresolved. [Source](../reconstruction/report-transcriptions/03_expansion_tank_and_chiller.txt#L70). |
| `m_dot_loop`, `m_dot_total` | lbm/s printed | Fixed 0.1 per circuit and its 15-circuit total; not linked to script 2 output. [Source](../reconstruction/report-transcriptions/03_expansion_tank_and_chiller.txt#L72-L73). |
| `cp_brine` | energy/(mass temperature) | Brine heat capacity from property call. [Source](../reconstruction/report-transcriptions/03_expansion_tank_and_chiller.txt#L75). |
| `C_min` | heat rate/temperature intended | Finite brine heat-capacity rate for a constant-temperature refrigerant-side interpretation; reconcile seconds and hours. [Source](../reconstruction/report-transcriptions/03_expansion_tank_and_chiller.txt#L77). |
| `NTU`, `E` | dimensionless intended | Number of transfer units and effectiveness 1-exp(-NTU). [Source](../reconstruction/report-transcriptions/03_expansion_tank_and_chiller.txt#L78-L80). |
| `T_ref_calculated` | F intended | Refrigerant temperature rearranged from effectiveness; not an independent validation of assumed inputs. [Source](../reconstruction/report-transcriptions/03_expansion_tank_and_chiller.txt#L82). |
