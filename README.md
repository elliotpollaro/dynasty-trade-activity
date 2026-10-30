# Dynasty Trade Activity

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23822003.svg)](https://doi.org/10.5281/zenodo.23822003)

Weekly counts of how often each player and draft pick was traded in real dynasty fantasy football
leagues, published by [Dynasty Dealer](https://www.dynastydealer.com).

**Counts only:** no player values, no trade-by-trade details, no league or manager information.

**Coverage:** every week from March 2, 2026, with a new week added each Friday. Weeks run Monday to Sunday (UTC).

## Load it

R

```r
players <- read.csv("https://raw.githubusercontent.com/elliotpollaro/dynasty-trade-activity/main/data/players_weekly.csv")
```

Python

```python
import pandas as pd
players = pd.read_csv("https://raw.githubusercontent.com/elliotpollaro/dynasty-trade-activity/main/data/players_weekly.csv")
```

## Files

| File | One row per |
|---|---|
| `data/players_weekly.csv` | player, week and format |
| `data/picks_weekly.csv` | draft-pick season and round, week and format |
| `data/market_weekly.csv` | week and format |
| `latest/` | the same three files, newest week only |

`format` is `sf` (superflex leagues), `1qb` (every other league) or `all` (both).

### `players_weekly.csv`

| Column | Meaning |
|---|---|
| `week_start` | The Monday the week starts (UTC), `YYYY-MM-DD` |
| `format` | `all`, `sf` or `1qb` |
| `rank` | Rank by `trades` within the week and format (ties: lower `sleeper_id` first) |
| `sleeper_id` | Sleeper's public player ID. It joins to the `sleeper_id` column of DynastyProcess's player ID crosswalk (`db_playerids.csv`). |
| `player_name`, `position`, `nfl_team` | From Dynasty Dealer's player table when the week was exported |
| `trades` | Trades the player moved in |
| `leagues` | Distinct leagues those trades came from |
| `share_of_trades_pct` | `trades` ÷ all trades that week in that format × 100 |

### `picks_weekly.csv`

| Column | Meaning |
|---|---|
| `week_start`, `format` | As above |
| `pick_season` | The draft the pick is for |
| `pick_round` | The round. Rounds past 5 are mostly startup-draft picks. |
| `trades` | Trades that moved at least one pick of this season and round |
| `picks_moved` | Picks of this season and round that changed hands |
| `share_of_trades_pct` | `trades` ÷ all trades that week in that format × 100 |

### `market_weekly.csv`

| Column | Meaning |
|---|---|
| `week_start`, `format` | As above |
| `trades` | All trades that week in that format, pick-only trades included |
| `leagues_with_trades` | Leagues with at least one trade that week |
| `players_moved` | Players that changed hands (one per player per trade) |
| `picks_moved` | Draft picks that changed hands |
| `assets_per_trade` | (`players_moved` + `picks_moved`) ÷ `trades` |

## How it's made

- **Source:** completed trades in Sleeper leagues of the dynasty type, read from Sleeper's public API by
  Dynasty Dealer's collector. Keeper and redraft leagues are not included.
- A trade belongs to the week its completion time falls in (UTC).
- A player or pick row needs at least 5 trades in its week and format. Smaller counts are left out.
- Picks are counted by season and round. A season's picks are left out from September 1 of that year,
  when nearly every league has held that draft.
- Each week is exported once, on the Friday after it ends, and is not revised afterwards.

## Things to know

- **Coverage grows over time.** We keep finding new leagues, and trades a newly found league made in
  earlier weeks are not added back into weeks already published. To compare weeks, use
  `share_of_trades_pct`; `leagues_with_trades` shows how many leagues a week covers.
- The weeks from March 2 to October 25, 2026 were all exported on October 30, 2026.
- Player names, positions and teams are as of export, not as of the week.
- A 3-for-1 trade counts once for each of the 4 players in it.

## Not included

- Player or pick values, prices, or anything else from Dynasty Dealer's value model.
- Who was traded for whom.
- League IDs, transaction IDs, rosters, usernames or any other manager information.

## License

The data is licensed [CC BY-NC 4.0](LICENSE): free for non-commercial use, with credit. Please credit it as:

> Trade activity data: Dynasty Dealer (https://www.dynastydealer.com)

For commercial use, open an issue on this repository.

## Cite

Archived on Zenodo: DOI [10.5281/zenodo.23822003](https://doi.org/10.5281/zenodo.23822003). That DOI always points to
the newest version. A new version is archived on the 1st of each month, and each one has its own DOI. GitHub's
"Cite this repository" button (from [`CITATION.cff`](CITATION.cff)) gives the same citation.

Also from Dynasty Dealer: the code, league samples and results behind eight pre-registered fantasy
football case studies, DOI [10.5281/zenodo.23813186](https://doi.org/10.5281/zenodo.23813186).

Dynasty Dealer is not affiliated with or endorsed by Sleeper.
