# Session JSON

One object per completed run. Open the task with `?debug=1`, finish it, then use **Download data** to save the file.

**Top level**

- `completion` — `code` (completion code), `ended_wall_iso` (when they finished, UTC).
- `participant` — `participant_id` (Prolific PID, MTurk worker id)
- `design` — Assigned condition: `rho`, `forecast_feedback_encoding`, optional `observation_encoding_url_override`, `distractor_task`, `time_series` (μ, σ, min/max bounds), `trial_counts`, `distractor_block_duration_sec`.
- `payment` — Total score and the dollar amounts shown on the debrief screen.
- `series` — `realized_values` (the full simulated path for that session) and `index_note` (how list indices relate to trials).
- `trials` — `observations` and `forecasts`: arrays with one object per trial (details below).
- `distractor` — `events` (word-task rows and block summaries) plus a short `event_note`.
- `diagnostics` — `session_log`, `screen_timings`, and `client` (browser string and viewport size).

**`trials.observations` (each item)**

- Which observation trial, the value shown (`x_t`), whether typing was forced or passive (`obs_encoding`), monotonic times (`started_ms`, `ended_ms`), and wall-clock `timestamp`.

**`trials.forecasts` (each item)**

- Forecast round index, internal series index, their forecast vs outcome (`forecast_t1`, `realized_t1`), round score, forecast/feedback timing fields, and copies of main design parameters on that row.

**`distractor.events` (each item)**

- Most rows: a word trial (`word`, `choice`, `rt_ms`, `prompt`, plus tags like which phase it followed).
- Rows that end a distractor block: `interval_event` is true, with `dt_sec` and timing fields for the whole block.
