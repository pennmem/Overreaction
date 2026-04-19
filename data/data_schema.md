# Data JSON Structure

Each file itself is a different trial. To be able to download data, add `?debug=1` in the URL and use the Download data button after finishing the task.

## Schema

- **`completion`** — `code`, `ended_wall_iso` (time when they finished).
- **`participant`** — `participant_id`
- **`design`** — Parameters held fixed for the subject's session:
  - `rho`: the AR(1) persistence.
  - `typing_mode`: `forced` or `passive`, determining whether the subject undergoes active or passive recall. `forced`: after each observation value and after each revealed forecast outcome, they must type that number exactly. `passive`: those screens only need Enter (no typing the values).
  - `distractor_task` — randomly chosen between an `animacy` or `size` task.
  - `time_series` — Parameters for the underlying time series: `mu`, `sigma_e`, `bounds.min`, `bounds.max`. Most cases we use 500, 20, 1, 1000 respectively. 
  - `trial_counts` — Number of `observation` trials and `forecast` trials. This is generally 20 and 40 respectively. 
  - `distractor_block_duration_sec` — `min`, `max` in seconds between periods.
- **`payment`** — `total_score`, `base_pay_usd`, `bonus_per_score`, `bonus_usd`, `total_pay_usd`.
- **`series`** — `realized_values` (full path for this run) and `index_note` (how indices map to trials).
- **`trials`** — `observations` and `forecasts` (arrays; see below).
- **`distractor`** — `events` (array) and `event_note` (short string).
- **`diagnostics`** — `session_log`, `screen_timings`, `client` (`user_agent`, `viewport.width`, `viewport.height`).

### `trials.observations`

Each item has a `phase` (which is always observation here), `obs_index`, `x_t`, `obs_encoding` (same as `design.typing_mode`), `started_ms`, `ended_ms`, `timestamp`.

### `trials.forecasts`

Each item has a `phase` (always forecast here), `forecast_index`, `t_index`, `forecast_t1`, `realized_t1`, `score_t1`, `rho`, `mu`, `sigma_e`, `encoding_condition` (same as `design.typing_mode`), `distractor_task`, forecast/feedback timing fields (`forecast_started_ms`, `forecast_submitted_ms`, `feedback_started_ms`, `feedback_ended_ms`), `timestamp`.

### `distractor.events`

- Normal row: word judgment (`word`, `choice`, `rt_ms`, `prompt`, plus tags such as `phase`, `obs_index` or `forecast_index`).
- End of a distractor block: `interval_event` is `true`, plus `dt_sec`, `started_ms`, `ended_ms`, `timestamp`.

