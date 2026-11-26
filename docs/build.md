# Build the data manuscript

Use a dedicated [Docxology Template](https://github.com/docxology/template)
checkout with Pandoc, pandoc-crossref and XeLaTeX installed. Run the
following from this fork:

```bash
template_dir=/absolute/path/to/template
git -C "$template_dir" switch --detach 87704feae0a4490ef7dad9c931d268e8c30273c2
mkdir -p "$template_dir/projects/working"
git worktree add --no-checkout --detach "$template_dir/projects/working/covid19-forecast-hub" HEAD
git -C "$template_dir/projects/working/covid19-forecast-hub" sparse-checkout set --cone docs data
git -C "$template_dir/projects/working/covid19-forecast-hub" checkout --detach HEAD
cd "$template_dir"
uv sync --locked --python 3.14 --no-dev
uv run --no-sync python scripts/pipeline/stage_03_render.py --project working/covid19-forecast-hub
uv run --no-sync python scripts/pipeline/stage_04_validate.py --project working/covid19-forecast-hub
```

The PDF, web pages and validation reports are written beneath
`projects/working/covid19-forecast-hub/output/`.
