# Lab 06 notes

Name and surname: Radosław Liskiewicz

## Class

Command used to execute the script:
`python scene_report.py`

Two report rows I checked against the rules:
- `D,ELIGIBLE`: `D` has `cloud_percent = 20` and `size_mb = 600`. Since `size_mb > 0` and `cloud_percent` equals `MAX_CLOUD` (`20`, and only values strictly greater than `20` are `CLOUDY`), it is classified as `ELIGIBLE`.
- `F,INVALID`: `F` has `cloud_percent = 90` and `size_mb = 0`. Because `assess_scene` checks for invalid measurements (`size_mb <= 0`) before checking cloud cover, it returns `INVALID` rather than `CLOUDY`.

Observed counts at a limit of 20:
`{'ELIGIBLE': 5, 'CLOUDY': 1, 'INVALID': 2}` (5 `ELIGIBLE`, 1 `CLOUDY`, 2 `INVALID` across 8 rows).

My explanation of one loop, return, or repeated record decision:
`return results` belongs after the `for` loop in `review_scenes` because `return` exits the function immediately. Placing it inside the loop would terminate the function after processing only the first record (`A`) instead of collecting all eight records.

Agreed stopping point, if setup or time prevented completion:
None — completed all class tasks (A1, A2, and C1–C3).

## At home

Predicted selected labels and sizes at an individual limit of 250 MB, before running:
- `H1`: `250` MB (`cloud_percent = 20`, right at both limits)
- `H3`: `150` MB (`cloud_percent = 0`)
- `H1`: `100` MB (`cloud_percent = 10`, second `H1` record retained in order)

Observed saved rows, and any explanation of a mismatch:
`results/home_selection.csv` matched the prediction with 3 selected rows (`H1,250`, `H3,150`, and `H1,100`) and no mismatch. Row `H1` (`cloud_percent = 20`, `size_mb = 250`) passed right at both limits (`<= 20` and `<= 250`), while `H2` (`cloud_percent = 5`, `size_mb = 800`) was excluded because `800 > 250` even though its cloud cover was `ELIGIBLE`.

Prediction and observed result at 100 MB:
Predicted that only `H1` (`100` MB) would remain selected, dropping the first `H1` (`250` MB) and `H3` (`150` MB). After setting `MAX_SIZE_MB = 100` and running `python home_selection.py`, the output confirmed 1 selected record (`H1,100`), which passed at the exact 100 MB limit while `H3` (`150` MB) was excluded because `150 > 100`. Restored `MAX_SIZE_MB = 250` and reran afterward.

One suggestion I accepted or changed and the example that checked it, or my own check if I used no LLM:
Used an LLM to write-up and verify the boundary behavior of `choose_scenes`; checked the `<=` limit condition (`scene["size_mb"] <= max_size_mb`) using the first `H1` record (`size_mb = 250` at `MAX_SIZE_MB = 250`) and the second `H1` record (`size_mb = 100` at `MAX_SIZE_MB = 100`), confirming both exact-limit records were included while `H3` (`150` MB) was excluded at `100` MB.