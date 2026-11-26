# Methods

The sample is a dated submission from the hub's archived baseline
model [@baseline_snapshot]. The readback uses a pinned Git revision,
checks the CSV's digest and tests its fields. Date checks use the
reference date and the signed weekly horizon. The procedure rejects
duplicate identifiers, unbounded proportions and nonfinite values.

We selected the national location and reference week, compared the
ordering of the quantile values for each target and read each group's
median. The executable example and source digest are part of the
[verification guide](https://github.com/docxology/covid19-forecast-hub/blob/main/docs/example.md).
