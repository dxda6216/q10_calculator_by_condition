# Q10 calculator by condition

A browser-based tool to fit the temperature dependence (Q10) of the circadian period separately for each experimental condition (cell type, drug treatment, medium, genotype and so on) and to compare the conditions.

**Open the app:** https://dxda6216.github.io/q10_calculator_by_condition/index.html

**User manual (PDF):** [Q10_calculator_by_condition_manual.pdf](Q10_calculator_by_condition_manual.pdf)

![Screenshot of the app with the example data loaded](screenshot.png)

## Features

- **Fit Q10 for each condition.** The app fits τ(T) = τb · Q10^((Tb − T)/10) by non-linear least squares on the period itself (Levenberg–Marquardt). It reports Q10 ± SE, the 95% CI, τb ± SE at a shared base temperature, R², and the activation energy (Ea) estimated from Q10 and from an Arrhenius regression.
- **Plot all conditions together.** Conditions are shown on period, frequency and Arrhenius plots, using colour-blind-safe colours and a distinct marker for each condition. The plotting area has a fixed 4:3 aspect ratio, and minor ticks (1, 3, 4 or 9 per interval) can be added as an option.
- **Restrict the fit range.** You can limit the temperature range used for the Q10 fit by typing the limits or by dragging across a plot. The fitted curves are still drawn over the whole temperature axis, darker inside the range and lighter outside it.
- **Compare conditions.**
  - An overall extra sum-of-squares F test checks whether Q10 is the same in all conditions.
  - Pairwise comparisons test the difference in ln Q10, with Holm-adjusted p values.
- **Optional polynomial fit (degree 1–6).** It fits period against temperature for each condition, with its own fit range.
- **Export your results.**
  - Figure as PNG or SVG
  - Results table as CSV
  - Full text report (TXT), optionally with the data and with the fitted Q10 and polynomial curves (200 points per condition, tab-delimited)
- **Private by design.** Everything runs in your browser and no data are uploaded. The app is a single HTML file with no dependencies, so it also works offline.

## Input format

Paste three columns, one measurement per row. Tab-delimited text (copied from Excel) is recommended; comma- or semicolon-delimited CSV also works.

```
Temperature	Period	Condition
42.2	20.7	Control
42.0		10 µM drug X
40.4	21.8	Control
40.0	26.6	10 µM drug X
```

- **Column 1:** temperature (°C).
- **Column 2:** period (h). If the period is blank or not a number (for example “arrhythmic”), the row is treated as having no data at that temperature.
- **Column 3:** condition label. Rows with identical labels are grouped together.
- **Skipped lines:** a header line and lines beginning with `#` are ignored.

## Quick start

1. Open the app. Example data with five simulated conditions (A–E) are loaded so you can try every feature.
2. Paste your own three columns into the **Data** box, or use **Load file…**.
3. Set the **Base temperature** (default 37 °C) and, if needed, the **Temperature range for the fit**.
4. Read the results in **Results by condition** and **Comparison between conditions**, then save the figure, table or report.

See the [user manual](Q10_calculator_by_condition_manual.pdf) for a full description of every setting and of the statistical methods.

## Running locally

No installation or build step is needed. Download `index.html` and open it in a current browser (Chrome, Edge, Firefox or Safari). The only external resource is a web font from Google Fonts; without a network connection the page falls back to system fonts.

## Repository contents

| File | Description |
|---|---|
| `index.html` | The complete app (HTML, CSS and JavaScript in one file) |
| `Q10_calculator_by_condition_manual.pdf` | User manual |
| `screenshot.png` | Screenshot used in this README |
| `LICENSE` | MIT License |

## Methods in brief

- **Q10 model.** Fitted on the period scale. Standard errors come from s²(JᵀJ)⁻¹ with s² = SSE/(n − 2). The 95% CI of Q10 is computed on the log scale and back-transformed, so it is asymmetric. SE and CI need at least 3 points.
- **Arrhenius analysis.**
  - Frequency is f = 24/τ, and ln f is regressed on 1000/T (T in kelvin).
  - Ea from the regression is −slope × 1000 × R.
  - Ea from Q10 is approximated as R·T₁·T₂·ln Q10/10.
- **Overall test.** F = [(SSE_common − SSE_separate)/(k − 1)] / [SSE_separate/(N − 2k)]. It compares a model with one shared Q10 (and a separate τb for each condition) against separate Q10 values.
- **Pairwise tests.** t = Δln Q10 / √(SE₁² + SE₂²), with n₁ + n₂ − 4 degrees of freedom and Holm correction. This is an approximate test; use it as a guide together with the confidence intervals.
- **Polynomial fit.** Ordinary least squares, computed internally on centred and scaled temperature. Coefficients and their SEs are reported for raw temperature (°C).

## Related project

[Q10 calculator for circadian period](https://dxda6216.github.io/q10/index.html): the single-condition version on which this app is based.

## License

This project is released under the [MIT License](LICENSE).
