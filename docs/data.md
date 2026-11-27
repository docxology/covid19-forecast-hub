# Data and repository layout

Target observations describe aggregated events for a week and reporting
location. Model outputs describe predicted quantiles for the corresponding
targets and horizons.

| Path | Contents |
| --- | --- |
| [`hub-config/`](../hub-config/) | Hub configuration, task identifiers and metadata schema. |
| [`model-metadata/`](../model-metadata/) | Model and contributor descriptions. |
| [`model-output/`](../model-output/) | Per-model forecast submissions grouped by reference date. |
| [`target-data/`](../target-data/) | Target observations and time-series artifacts. |
| [`auxiliary-data/`](../auxiliary-data/) | Additional source data and support artifacts. |

Hospital admission targets use weekly counts from the National Healthcare
Safety Network's hospital respiratory data. The emergency department
target is the fraction of emergency visits with the stated diagnosis.
Model output stores it as a proportion. Dashboards may display the
quantity as a percentage.

Use the Hubverse model-output schema and the upstream data dictionary
when preparing a downstream reader. Preserve location identifiers as
strings to retain leading zeros. For a reproducible analysis, record the
source revision and reference date. See the upstream hub for current
target releases and cloud mirror information. [Upstream repository](https://github.com/CDCgov/covid19-forecast-hub) · [Hubverse model output](https://docs.hubverse.io/en/latest/user-guide/model-output.html)
