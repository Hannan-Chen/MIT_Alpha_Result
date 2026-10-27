# MIT–Alpha cooling simulation results

**Four rescale factors (50, 100, 150, 200) × fixed/dynamic scheduling: eight 24-hour simulated trajectories.**
All use the provided weather for 2021-08-31 in America/New_York. No extra artificial scenarios are added.

## Downloads

- [Packaged release](../../releases/latest)
- [English data description (PDF)](docs/MIT_Alpha_Data_Description_EN.pdf)
- [English LaTeX source](docs/MIT_Alpha_Data_Description_EN_LaTeX.zip)
- [Generated data](generated_data/) and [field dictionary](generated_data/data_dictionary.csv)
- [Fixed policy](generated_data/fixed/) / [dynamic policy](generated_data/dynamic/)
- [Component power and energy](generated_data/component_energy_summary.csv)
- [All eight full simulation output folders](raw_simulation_outputs/)

**Original source data are not uploaded.** This release excludes original GPU/weather packages, the source-only and request-only tables, and original GPU-power columns. See [source access and attribution](SOURCE_REFERENCES.md). Rescaled cooling-load requests and implemented model boundaries remain as experimental context.

## Results

| Rescale | Mean requested cooling (MW) | Fixed electricity (MWh) | Dynamic electricity (MWh) |
|---:|---:|---:|---:|
| 50 | 1.517 | 13.369 | 7.885 |
| 100 | 3.035 | 14.572 | 14.335 |
| 150 | 4.552 | 16.746 | 16.746 |
| 200 | 6.070 | 21.296 | 21.296 |

Rescale defines equivalent cooling demand, not measured total IT power, measured site cooling demand, or PUE.
The English description presents fixed and dynamic results separately, with category mean powers, electricity and time-series plots.

## Data contents

- 2,304 five-minute summaries and 69,120 ten-second records.
- Chiller, chilled-water pump, condenser-water pump and cooling-tower fan category powers.
- Six individual chillers and twelve tower fans as endpoint powers. Individual pump electrical powers are not exposed by this FMU and are not invented.
- Full implemented actions, controller decisions, active counts/identities, event records and quality flags.
- Raw output folders preserve warmup; principal delivered tables exclude it.

Power fields are W unless named kW; temperatures are K. Category means use saved start/end integration. Individual device endpoints are not interval means.

## Warmup

Every run first holds its own initial cooling request and wet bulb for 7,200 s (2 h), with supply feedback and its assigned policy active. All eight pass the final-30-minute supply-range criterion (at most 0.1 K) and adjacent-15-minute mean-power criterion (at most 1%). The same FMU instance/state continues into the target day. Warmup is excluded from principal records, energy and COP. See the English description and verification/warmup_audit.json.

Load and supervisory decisions use 300 s intervals; true-supply feedback is 30 s, FMI communication 10 s. These are distinct from internal integration.

## Quality and scope

All eight simulations complete; five pass every inherited normal-data quality gate. Preserve rejected flags:

- R50_fixed / R50_dynamic: head-MLP input extrapolation.
- R100_dynamic: two ten-second pump-efficiency outliers near the natural 01:15 staging event.

Differences in rejected runs are diagnostic rather than validated achievable savings. All eight trajectories share one source day and are not eight independent measured days.

Raw-result audits pass 568 checks for the original six and 264 for factor 50; a further 648 original-export checks cover component integrals, individual sums, policy splits and unchanged original trajectories. Additional public-release checks verify retained fields while removing original source-power columns.

## Provenance and rights

[Source references](SOURCE_REFERENCES.md), metadata.json, verification/, and MANIFEST.sha256 retain provenance, temporal semantics, quality findings and file integrity.
No third-party FMU binary, serialized model weights, management credentials or private workstation paths are distributed.
