# JSON File Structure

Save with **Download data** after finishing the task (`?debug=1` in the URL). One JSON object per file.

## Top level

- **`completion`** — `code`, `ended_wall_iso` (UTC when they finished).
- **`participant`** — `participant_id` always. If the link included MTurk-style parameters, you may also see `mturk_assignment_id` and/or `mturk_hit_id` (worker id is already `participant_id`).
- **`design`** — Everything held fixed for the session:
  - `rho` — AR(1) persistence.
  - `typing_mode` — `forced` or `passive`, same everywhere below. **`forced`:** after each observation value and after each revealed forecast outcome, they must type that number exactly. **`passive`:** those screens only need Enter (no typing the values).
  - `distractor_task` — e.g. `animacy` or `size`.
  - `time_series` — `mu`, `sigma_e`, `bounds.min`, `bounds.max` for the simulated series.
  - `trial_counts` — `observation`, `forecast` (how many of each).
  - `distractor_block_duration_sec` — `min`, `max` in seconds between periods.
- **`payment`** — `total_score`, `base_pay_usd`, `bonus_per_score`, `bonus_usd`, `total_pay_usd`.
- **`series`** — `realized_values` (full path for this run) and `index_note` (how indices map to trials).
- **`trials`** — `observations` and `forecasts` (arrays; see below).
- **`distractor`** — `events` (array) and `event_note` (short string).
- **`diagnostics`** — `session_log`, `screen_timings`, `client` (`user_agent`, `viewport.width`, `viewport.height`).

## `trials.observations[]`

Each item: `phase` (`"observation"`), `obs_index`, `x_t`, `obs_encoding` (same as `design.typing_mode`), `started_ms`, `ended_ms`, `timestamp`.

## `trials.forecasts[]`

Each item: `phase` (`"forecast"`), `forecast_index`, `t_index`, `forecast_t1`, `realized_t1`, `score_t1`, `rho`, `mu`, `sigma_e`, `encoding_condition` (same as `design.typing_mode`), `distractor_task`, forecast/feedback timing fields (`forecast_started_ms`, `forecast_submitted_ms`, `feedback_started_ms`, `feedback_ended_ms`), `timestamp`.

## `distractor.events[]`

- Normal row: word judgment (`word`, `choice`, `rt_ms`, `prompt`, plus tags such as `phase`, `obs_index` or `forecast_index`).
- End of a distractor block: `interval_event` is `true`, plus `dt_sec`, `started_ms`, `ended_ms`, `timestamp`.

