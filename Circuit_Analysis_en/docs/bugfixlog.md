# Bug Fix Log - 2026-05-11

## Directory Structure Restructuring

### Directory Renaming
- Added `01_` ~ `09_` numeric prefixes to 9 subdirectories under `Circuit_Analysis/`
- Synchronously updated all 33 link paths in `index.html`

### Op-Amp Module Commenting Out
- The "Operational Amplifier" card section (Section 9) in `index.html` was entirely converted to HTML comments
- The `09_OpAmp/` subdirectory and its files are retained for future use in the Analog Electronics course
- Total page module count updated from 33 to 30

---

## Wheatstone_Bridge.html

### Bar Chart Width
- Bar chart width adjusted to 50% of original (`barPercentage: 0.45`)

### ASCII Circuit Diagram Replacement
- Removed the ASCII circuit diagram from the "Wheatstone Bridge Topology" window
- Replaced with the image `./img/Wheatstone_Bridge.jpg`

### Layout Adjustment
- Moved the "Wheatstone Bridge Balance Principle" and "Sensitivity and Galvanometer" explanation cards from the bottom of the page to the right sidebar
- Arranged in a single row with 2-column grid alongside "Bridge Arm Voltage and Galvanometer" and "Balance State", with aligned widths

---

## Ohms_Law.html

### Circuit Diagram Card
- Inserted a circuit diagram card between "Circuit Parameter Adjustment" and "Curve Interpretation"
- Inserted `./img/Ohms_Law.jpg`; all three cards have consistent widths

---

## Series_Parallel_Resistors.html

### Circuit Diagram Card
- Added a circuit diagram card below the bar chart, inserting `./img/Series_Parallel_Resistors.jpg`
- Image has `px-8` padding on both sides

### Refresh Mechanism Debouncing (Key Fix)
**Problem**: When changing parameters, the bar chart jitters severely and the equivalent resistance formula constantly redraws.

**Fix**:
1. Bar chart now uses `barChart.update('none')` for in-place refresh instead of destroy/recreate
2. Split `updateUI()` into two functions:
   - `updateFullUI()`: Called during mode switching, rebuilds all DOM and KaTeX
   - `updateValues()`: Called on slider changes, only updates values without touching formulas or description text
3. Voltage/current divider formulas and total current text restructured as "static formula + value span" pattern; slider changes only modify `innerText`
4. Bar chart consolidated into a single dataset; bars precisely aligned with R1/R2 labels
5. Bar width reduced to 60% (`barPercentage: 0.54`)

---

## RLC_Step_Response.html (RLC Second-Order Transient Response)

### Removed S-Plane Pole Plot
- Removed the s-plane pole position plot's HTML, JS functions, and all references

### Response Metrics Layout
- "Response Metrics" card moved from the left sidebar to below the right waveform plot
- Changed to a `grid-cols-3` single-row horizontal layout

### Y-Axis Range Extension
- Waveform plot y-axis negative half extended from -0.25 to -1.0 to prevent waveform clipping

### Critical Damping Button
- Added a "Critical Damping" button next to the reset button
- When clicked, calculates `R_crit = 2√(L/C)` based on current L and C values and sets it as the R value

---

## Three_Phase_Load_Connection.html (Three-Phase Load Star-Delta Connection)

### Waveform Plot Layout
- The Y-connection and Delta-connection waveform plots changed to vertical stacking (removed `lg:grid-cols-2`)
- Removed fixed canvas width `width="400"`; charts automatically fill card width
- Container height increased from 340px to 380px

### Refresh Mechanism Debouncing
- Waveform plots now use in-place update + `chart.update('none')` instead of destroy/recreate
- X/Y axis ranges dynamically adjust with parameters, eliminating blank space on the right

### Removed Power Factor Angle Slider
- The φ slider has no effect on voltage waveforms (only affects current phase, while the chart only displays voltage)
- Removed slider HTML, JS variables, and event listeners

### Reset Parameters Button
- Added a reset button that restores default values: Vp=220V, f=50Hz, Z=50Ω

---

## Three_Phase_Unbalanced.html (Three-Phase Unbalanced Load)

### Waveform Plot Dual Y-Axis
**Problem**: Current (a few A) and voltage (~311V peak) share the same y-axis, causing the current waveform to be compressed into a flat line.

**Fix**:
- Left y-axis displays voltage (V), right y-axis displays current (A), each independently scaled
- Voltage axis in gray, current axis in purple, matching curve colors

### Waveform Plot Curve Simplification
- Commented out V_B and V_C, keeping only V_A, I_A, and I_N curves

### Phasor Diagram Current Scaling
**Problem**: Voltage (220V) and current (a few A) on the same scale causes current phasors to appear as colored dots near the origin.

**Fix**:
- Defined `I_SCALE = 50`; current phasors are multiplied by 50 before drawing
- Labels still display actual current values; axis title notes "Current×50(A)"

### Phasor Diagram Debouncing
- Arrow drawing and label logic extracted into a module-level plugin `phasorArrowPlugin`
- Chart created once on first load; subsequent updates only adjust axis ranges + `chart.update('none')`

### Current Color Modification
**Problem**: Current arrow colors are too similar to voltage arrow colors.

**Fix**:
| Phase | Old Color | New Color |
|-------|-----------|-----------|
| I_A | Orange #f97316 | Magenta #d946ef |
| I_B | Yellow-green #84cc16 | Pink #f472b6 |
| I_C | Cyan #06b6d4 | Amber #f59e0b |

---

## Mutual_Inductance.html (Mutual Inductance Coupled Coils)

### KaTeX Formulas Not Rendering
**Problem**: `setMode()` uses `innerText` to write text containing `\\(...\\)`. KaTeX never renders, and formulas display as raw LaTeX text.

**Fix**:
- Changed `modeDesc.innerText` to `modeDesc.innerHTML`
- Added `renderMathInElement(modeDesc, ...)` call after update for local rendering

### T-Equivalent Circuit Formula Degradation
**Problem**: `updateTEquiv()` rebuilds the DOM with `innerHTML` each time, destroying the initially rendered KaTeX nodes without re-rendering, causing formulas to degrade to raw text.

**Fix**:
- Added `renderMathInElement(tEquivDisplay, ...)` call at the end of `updateTEquiv()` for local rendering

### Bar Chart Label Garbled Text
**Problem**: Bar chart labels used broken LaTeX delimiters (starting with `\(`, ending with `$`), displaying as garbled text.

**Fix**:
- Changed to Unicode subscript plain text: `Aiding (L₁+L₂+2M)` / `Opposing (L₁+L₂-2M)`

### Chart Animation Jitter
- `waveformChart.update()` and `barChart.update()` changed to `update('none')`

---

## Ideal_Transformer.html

### Chart Animation Jitter
**Problem**: `voltageChart.update()` and `currentChart.update()` use default animations, causing visual jitter with transition animations when dragging sliders.

**Fix**:
- Changed to `update('none')` to disable animations

---

## Thevenin_Norton.html Circuit Topology Diagram Replacement

**File**: `02_Circuit_Theorems/Thevenin_Norton.html`

**Changes**:

1. **Original Circuit Topology Card** -- Removed ASCII circuit topology diagram, replaced with image `./img/Thevenin_Norton_Origin.jpg`
2. **Thevenin Equivalent Circuit Card** -- Removed ASCII circuit topology diagram, replaced with image `./img/Thevenin.jpg`
3. **Norton Equivalent Circuit Card** -- Removed ASCII circuit topology diagram, replaced with image `./img/Norton.jpg`
4. **JS `switchMode` function** -- Dynamically generated ASCII diagrams during Thevenin/Norton mode switching synchronously replaced with corresponding images
5. Removed formula text descriptions below the equivalent circuit diagrams (`equiv_diagram_formula`)

---

## Superposition.html (Superposition Theorem)

### ASCII Circuit Diagram Replacement
- Removed ASCII circuit topology diagrams from three circuit schematic cards
- "U_s acting alone" replaced with `./img/superposition-Us.jpg`
- "I_s acting alone" replaced with `./img/superposition-Is.jpg`
- "Complete circuit (superposition)" replaced with `./img/superposition-origin.jpg`
- Added a line of description text below each of the three cards: "Voltage across ab \(U_{out1}\)", "Voltage across ab \(U_{out2}\)", "Voltage across ab \(U_{out}\)"

### Removed R3-Related Content
- Removed R3 slider HTML (\(R_3\) slider in the parameter adjustment card)
- Removed JS variables `r3Slider`, `r3Display`
- Removed `par3()` helper function
- Deleted R3 parameters from `getParams()`, `computeAll()`, `updateDisplay()`
- Removed `r3Slider` event listener
- Updated theoretical formulas: removed R3 references from \(U_{out1}\) and \(U_{out2}\) formulas

# Bug Fix Log

## 2026-05-14


### Max_Power_Transfer.html -- Chart Curve Not Updating

**Problem**: After adjusting V_th, R_th, R_L sliders, the power curve in the upper right corner shows no change.

**Root Cause**:
1. In the update branch of `updateChart()`, `chartInstance.options.plugins = chartInstance.options.plugins || {};` overwrites Chart.js internal plugin configuration, preventing proper redrawing during `update()` calls
2. The `rthLine` plugin captures `Rth` via closure; after the chart is first created, the closure variable never changes, so the reference line does not move with the slider

**Fix**:
- Removed `chartInstance.options.plugins = ...` to preserve complete plugin configuration
- Changed `update()` to `update('none')` to skip animation for instant redraw during slider dragging
- Added `_rth` property to `rthLine` plugin to store the current Rth value
- Update branch synchronously updates `chartInstance.options.plugins.rthLine._rth = Rth`
- `afterDraw` callback changed to read `rthVal = chart.config.options.plugins.rthLine._rth`

### Max_Power_Transfer.html -- Missing KaTeX Formula Delimiters

**Problem**: Multiple formula texts are missing `\(` and `\)` delimiters, so KaTeX does not render them, displaying as plain text.

**Affected Locations**:
- Static HTML in the "Matching Characteristics" card (containing `R_L`, `R_th`, etc.)
- `theory_text` and `character_text` for DC/AC modes in the `switchMode()` function
- Matching status display in `matchDisplay` within `updateDisplay()` (both DC and AC modes)
- `rlPeakDisplay` within `updateDisplay()` (AC mode complex conjugate matching formula)

**Fix**:
- Added `\(` and `\)` delimiters to all formula texts in the above locations
- Added `renderMathInElement()` calls at the end of `switchMode()` and `updateDisplay()` to ensure dynamically inserted formulas are rendered by KaTeX


### Max_Power_Transfer.html -- Chart R_th Reference Line Not Updating

**Problem**: After modifying V_th, R_th, R_L in the slider panel, the curve in the upper right corner shows no change.

**Root Cause**: The custom Chart.js plugin `rthLine`'s `afterDraw()` callback captures the `Rth` variable from the `updateChart()` function via closure. When the chart is first created, the plugin is registered. Subsequent slider changes only update the data array without rebuilding the plugin, so the `Rth` in the closure always retains its initial value, causing the green dashed reference line position and label to never update.

**Fix**:
- Stored the Rth value on the chart instance's custom property `chart.$rth`
- Plugin `afterDraw()` changed to read `chart.$rth` instead of closure variable
- Synchronize `chartInstance.$rth = Rth` on each update
- Changed `chart.update()` to `chart.update('none')` to avoid animation interference during fast dragging
- Removed ineffective code `chartInstance.options.plugins = ...`

---

### Max_Power_Transfer.html -- Added "Match Maximum Power" Button

**Requirement**: Add a one-click button for matching maximum power, applicable to both DC and AC modes.

**Implementation**:
- Inserted a green button between the \(R_L\) slider and AC controls
- DC mode: Click sets \(R_L = R_{th}\)
- AC mode: Click sets \(R_L = R_{th}\), \(X_L = -X_{th}\) (complex conjugate matching)
- Button text switches with mode (DC: \(R_L = R_{th}\) / AC: \(Z_L = Z_{th}^*\))

---

### Max_Power_Transfer.html -- Added Circuit Schematic

**Requirement**: Add a circuit diagram window below the power curve window, with the same width as the power curve.

**Implementation**: Inserted an image card between the power curve card and the theory card grid, referencing `./img/Max_Power_Transfer.jpg`, using the same rounded shadow styling as the chart area.

---

### Ideal_Transformer.html -- Relationship Verification Card Layout Adjustment

**Requirement**: Move the relationship verification card below the current waveform chart, with the same width as the current waveform chart; arrange the three verification formulas in a single row.

**Implementation**:
- Removed the verification card from the left panel and moved it to the right panel, between the current waveform chart and theoretical characteristics
- Card uses the same `rounded-2xl shadow-xl` styling as the waveform chart
- Three formulas changed to `flex justify-between gap-2` horizontal single-row layout, each taking equal width
