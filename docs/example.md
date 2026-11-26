# Check the archived baseline forecast

Run this Python standard-library example at the repository root. In a
sparse checkout Git can retrieve the named object on demand.

```bash
python3 - <<'PYTHON'
from collections import defaultdict
from csv import DictReader
from datetime import date, timedelta
from decimal import Decimal
from hashlib import sha256
import subprocess

source = (
    "ac0024aa85933de6ffdb83c625c82e6d471e54c4:"
    "model-output/CovidHub-baseline/2026-09-26-CovidHub-baseline.csv"
)
data = subprocess.check_output(["git", "show", source])
assert sha256(data).hexdigest() == (
    "6bf899f32e2888a5df8bd492061dcb64e8067d44c56c5f4efdebadf171e59e1a"
)
reader = DictReader(data.decode("utf-8").splitlines())
assert reader.fieldnames == [
    "reference_date", "horizon", "target", "target_end_date", "location",
    "output_type", "output_type_id", "value",
]
rows = list(reader)
keys = set()
selected = defaultdict(list)
for row in rows:
    reference = date.fromisoformat(row["reference_date"])
    horizon = int(row["horizon"])
    assert date.fromisoformat(row["target_end_date"]) == reference + timedelta(weeks=horizon)
    quantile = Decimal(row["output_type_id"])
    value = Decimal(row["value"])
    assert row["output_type"] == "quantile" and 0 < quantile < 1
    assert value.is_finite() and value >= 0
    if "prop" in row["target"]:
        assert value <= 1
    key = tuple(row[column] for column in reader.fieldnames[:-1])
    assert key not in keys
    keys.add(key)
    if row["location"] == "US" and horizon == 0:
        selected[row["target"]].append((quantile, value))

assert len(rows) == 12052
assert len({row["location"] for row in rows}) == 53
expected = {"wk inc covid hosp": Decimal("4368"), "wk inc covid prop ed visits": Decimal("0.0052")}
assert selected.keys() == expected.keys()
for target, points in selected.items():
    points.sort()
    assert len(points) == 23
    assert [value for _, value in points] == sorted(value for _, value in points)
    assert dict(points)[Decimal("0.5")] == expected[target]
    print(target, len(points), expected[target])
PYTHON
```

The assertions check the file's bytes, schema, date relationships and
units. The final checks read one national reference-week group for
each target. The [evidence record](../data/forecast-audit.json) stores
the verified counts and medians. Evaluating forecast skill requires
observations and an evaluation design across forecast rounds.
