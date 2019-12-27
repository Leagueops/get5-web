# get5-web

**Status: Supported only for LEAGUEOPS events**


This is a web panel meant to be used in conjunction with the [get5](https://github.com/splewis/get5) CS:GO server plugin. It provides a more convenient way of managing matches and match servers. **This webpanel is intended for competitive 5v5 ONLY**

This fork of get5 introduces a few new features that are used within get5 already, but were not originally ported over to the experimental web-panel. I've tried to make the web panel a little bit more secure (storing passwords, etc), and more handy for people to organize matches (Seasons and Leaderboards). Right now it's considered complete as it fits my needs and small-scale application. If you have any feature suggestions, *please create an issue*. It's extremely unlikely I'll put any more work into it, but pull requests are always welcome with features, provided they've been tested and pass CI!

**WARNING: THIS BUILD OF THE WEB-PANEL USES MODIFIED GET5-APISTATS MATERIAL. I DO NOT TAKE RESPONSIBILITY FOR ANY DATA LOSS, MISUSE, ETC. THAT THIS WEBAPP MAY INCUR.**


## How to use it:

1a. Download the new get5_apistats.smx from the [releases](https://github.com/Leagueops/get5-web/releases) page.
1b. Create your game servers on the "Add a server" page by giving their ip, port, and rcon password
2. Create teams on the "Create a Team" page by listing the steamids for each of the players
3. Go to the "Create a Match" page, selecting the teams, server, and rules for the match
4. Optional - Create a season with a given date range to keep track for a subset of matches.

Once you do this, the site will send an rcon command to the game server `get5_loadmatch_url <webserver>/match/<matchid>/config`, which will load the match config onto the gameserver automatically for you. Stats and game status will automatically be updated on the webpage.

As the match owner, you will be able to cancel the match. Additionally, on its matchpage there is a dropdown to run admin commands: add players to the teams if a ringer is needed, pause the match, load a match backup, list match backups, and run any rcon command.

Note: when using this web panel, the CS:GO game servers **must** be have both the core get5 plugin and the get5_apistats plugin. This means the server must also be running the [Steamworks](https://forums.alliedmods.net/showthread.php?t=229556) and [SMJansson](https://forums.alliedmods.net/showthread.php?t=184604) extensions, as well as [System2](https://github.com/dordnung/System2/releases) (for automated demo uploads, if you so choose). If you are using the public webpanel, please do not do FTP uploads, as they will not link properly on the web-panel itself, but will still upload to your personal FTP server.

## Screenshots

![Match Creation Page](/screenshots/create_match.png?raw=true "Match Creation Page")

![Match Stats Page](/screenshots/match_stats.png?raw=true "Match Stats Page")

![Teams Page](/screenshots/teams.png?raw=true "Teams Page")

![Team Creation Page](/screenshots/team_edit.png?raw=true "Team Creation Page")

![Season Creation Page](/screenshots/season_create.png?raw=true "Season creation Page")

![Season View Page](/screenshots/season_standings.png?raw=true "Season creation Page")

## Requirements:

- python2.7
- MySQL (other databases will likely work, but aren't guaranteed to, [example for MariaDB](https://github.com/splewis/get5-web/issues/146#issuecomment-480908372))
- a linux web server capable of running Flask applications ([see deployment options](http://flask.pocoo.org/docs/0.11/deploying/))

## Installation

Please see the [installation instructions](https://github.com/Leagueops/get5-web/wiki/Installation) for Ubuntu 16.04 with apache2. You can use other distributions or web servers, but you will likely have to figure out how to install a python flask app yourself. Raspberry Pis do work, but require a few other packages to be installed for apache.

## How do the game server and web panel communicate?

1. When a server is added the web server will send `get5_web_avaliable` command through rcon that will check for the appropriate get5 plugins to be installed on the server
2. When a match is assigned to a server, the `get5_loadmatch_url` command is used through rcon to tell the websever a file to download the get5 match config from
3. When stats begin to update (map start, round end, map end, series end), the game server plugins will send HTTP requests to the web server, using a per-match API token set in the `get5_web_api_key` cvar when the match was assigned to the server

## Other useful commands:

Autoformatting:

```sh
autopep8 -r get5 --in-place
autopep8 -r get5 --diff # should have no output
```

Linting errors:

```sh
cd get5
pyflakes *.py
```

Testing:
You must also setup a `test_config.py` file in the `instance` directory.

```sh
./test.sh
```

Manually running a test instance: (for development purposes)

```sh
python2.7 main.py
```
