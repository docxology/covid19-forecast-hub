# Data contract

The hub uses standardized task identifiers and model-output fields
[@hubverse]. A forecast row identifies the reference date, target,
location, horizon and target end date. A quantile row stores both
the quantile level and its predicted value.

Hospitalization forecasts use weekly admission counts. The emergency
department target uses a proportion of visits. Readers should retain
the target identifier and unit when combining models or dates. The
forecast hub describes the targets, submission schedule and data sources
[@forecast_hub].
