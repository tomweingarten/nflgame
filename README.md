[![Downloads](https://pepy.tech/badge/nflgame-redux)](https://pepy.tech/project/nflgame-redux)

A maintained fork of
[nflgame](https://github.com/BurntSushi/nflgame/)
================
**Currently releasing under [nflgame-redux](https://pypi.org/project/nflgame-redux/)** drop-in replacement for [nflgame](https://pypi.org/project/nflgame) so as to easily use with projects like [nfldb](https://github.com/derek-adair/nfldb).

### Purpose
nflgame is an API to retrieve and read NFL data feeds.
It can work with real-time data, which can be used for fantasy football.

### Installation
**this project is no longer python2 compatible.**.  The old python2 (<1.2.20) branch *should* work but... come on now... just [upgrade](https://docs.python.org/2/library/2to3.html).

1. Create a && activate a python3 venv
   ```
   python3 -m venv ~/python3
   source ~/python3/bin/activate
   ```


2. In your **python 3** virtualenv...

```
pip install nflgame-redux
```

3. Update players
```
nflgame-update-players
```

### Documentation and getting help
If you need help, please come visit us in discord on channel `https://discord.gg/3cHXbRp`.

If you aren't a programmer, then the
[tutorial for non
programmers](https://github.com/derek-adair/nflgame/wiki/Tutorial-for-non-programmers:-Installation-and-examples)
is for you!

Also, nflgame has decent (but not perfect)[API documentation](http://nflgame.derekadair.com/). If you're just looking around, make sure to look at the submodules as well.

Feel free to [open a new issue on the
tracker](https://github.com/derek-adair/nflgame/issues/new), which is currently the most expedient way to get support.


### How it works
nflgame works by parsing the same JSON data that powers NFL.com's live
GameCenter. Therefore, nflgame can be used to report game statistics while
a game is being played.

The package comes pre-loaded with game data from every pre- and regular
season game from 2009 up until the present (I try to update it every week).
Therefore, querying such data does not actually ping NFL.com.

However, if you try to search for data in a game that is being currently
played, the JSON data will be downloaded from NFL.com at each request (so be
careful not to inspect for data too many times while a game is being played).
If you ask for data for a particular game that hasn't been cached to disk
but is no longer being played, it will be automatically cached to disk
so that no further downloads are required.

Here's a quick teaser to find the top 5 running backs by rushing yards in the
first week of the 2013 season:

```python
import nflgame

games = nflgame.games(2013, week=1)
players = nflgame.combine_game_stats(games)
for p in players.rushing().sort('rushing_yds').limit(5):
    msg = '{} {} carries for {} yards and {} TDs'
    print( msg.format(p, p.rushing_att, p.rushing_yds, p.rushing_tds))
```

And the output is:

```
L.McCoy 31 carries for 184 yards and 1 TDs
T.Pryor 13 carries for 112 yards and 0 TDs
S.Vereen 14 carries for 101 yards and 0 TDs
A.Peterson 18 carries for 93 yards and 2 TDs
R.Bush 21 carries for 90 yards and 0 TDs
```

Or you could find the top 5 passing plays in the same time period:

```python
import nflgame

games = nflgame.games(2013, week=1)
plays = nflgame.combine_plays(games)
for p in plays.sort('passing_yds').limit(5):
    print p
```

And the output is:

```
(DEN, DEN 22, Q4, 3 and 8) (4:42) (Shotgun) P.Manning pass short left to D.Thomas for 78 yards, TOUCHDOWN. Penalty on BAL-E.Dumervil, Defensive Offside, declined.
(DET, DET 23, Q3, 3 and 7) (5:58) (Shotgun) M.Stafford pass short middle to R.Bush for 77 yards, TOUCHDOWN.
(NYG, NYG 30, Q2, 1 and 10) (2:01) (No Huddle, Shotgun) E.Manning pass deep left to V.Cruz for 70 yards, TOUCHDOWN. Pass complete on a fly pattern.
(NO, NO 24, Q2, 2 and 6) (5:11) (Shotgun) D.Brees pass deep left to K.Stills to ATL 9 for 67 yards (R.McClain; R.Alford). Pass 24, YAC 43
(NYG, NYG 20, Q1, 1 and 10) (13:04) E.Manning pass short middle to H.Nicks pushed ob at DAL 23 for 57 yards (M.Claiborne). Pass complete on a slant pattern.
```

### I want a database!

Great news! I've pulled nfldb into python3 and it comes with nflgame 2.0 already installed.  It even has an example docker image that should get everyone up and running pretty quick.

* [Project Page](https://github.com/derek-adair/nfldb)
* [PyPi listing](https://pypi.org/project/nfldb-redux/)


### Other related projects of [Andrew Gallant's](https://github.com/BurntSushi)...
[nflvid](https://github.com/BurntSushi/nflvid)
with nfldb to
[search and watch video of
plays](https://github.com/BurntSushi/nfldb/wiki/Watching-videos-of-plays-with-nflvid).

### Updating the player json (e.g., rosters)

Since player meta data (like a player's team, position or status) changes
throughout the season, the JSON database included with nflgame needs to be
updated occasionally. While I try to update it and push out new releases
weekly, you can also update the database by running the following command:

```
nflgame-update-players
```

It will send at least 32 requests (and usually not much more than that) to
NFL.com and update the JSON player database in place by default. I tend to run
it every 12 hours or so. This is the **only** piece of nflgame that relies on
web scraping.


### Loading data into Excel

Every sequence of players can be easily dumped into a file formatted
as comma-separated values (CSV). CSV files can then be opened directly
with programs like Excel, Google Docs, Open Office and Libre Office.

You could dump every statistic from a game like so:

```python
game.players.csv('player-stats.csv')
```

Or if you want to get crazy, you could dump the statistics of every player
from an entire season:

```python
nflgame.combine(nflgame.games(2010)).csv('season2010.csv')
```

### Contributing (WIP)
* All active development takes place on the "dev" branch.  This is where pull requests should be submitted against.
* Bug fixes for released versions should be submitted against "master" branch and will get merged accordingly.
* This project will stick to [Semantic Versioning](https://semver.org/)
* Tests are *greatly* encouraged but not required.

**Authored by [Andrew Gallant](https://burntsushi.net) maintained by [Derek Adair](https://derekadair.com)**
