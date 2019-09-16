# Subreddit Moderator Actions Logger

Logs moderator actions into a database. Also posts moderator statistics to a discord webhook
and detailed statistics to a subreddit, with graphs, which look like this:

Graph example | Discord example
--------------|----------------
![Graph example](https://i.imgur.com/VTR2Fam.png)|![Discord example](https://i.imgur.com/8LCdW98.png)

There are two programs involved:

* `modLogStream.py` which streams moderator actions and puts them in the database

* `onceaday.py` which is executed once every 24h and calculates statistics and the 
graph, then posts them to reddit and discord

* `oldLogProg.py` is an older version of the program which didn't stream actions,
but worked them all out at once. This was horribly ineffieient, requiring disk I/O
for week old actions and sometimes taking ovre 40 minutes to run. The posting parts
of it have been commented out, so you can use it to put actions in the database if
the stream program goes down for a while.

## TODO

* [ ] Merge the code from `webhooks.py` and `login.py` into the main program,
since it is not nessicary to have them in separate files any more

* [ ] Delete references to the old program

* [x] Move the database name, subreddit to log, and subreddit to post to to `credentials.json`
to make it possible to easily run the program with other subreddits 

* [x] Automatically create the table when it doesn't exist

* [ ] Make graph look better

## Your `credentials.json` should be something like the following:

```
{
    "production": 1,
    "subredditstream": "subredditforlogging",
    "subredditpost": "subredditforstats",
    "database": "YourDatabasePath.db",
    "bots":
    [
        "AutoModerator",
        "RepostSentinel",
        "MAGIC_EYE_BOT"
    ],
    "redditapi":
    {
        "client_id": "xxxxxxxxxxxxxx",
        "client_secret": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
        "username": "xxxxxxxxxxxx",
        "password": "xxxxxxxxxxxxxxxxxxxxx",
        "user_agent": "xxxxxxxxxxxxxx"
    },
    "discordwebhooks":
    {
        "prodwebhook": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
        "testwebhook": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
    },
    "imgurapi":
    {
        "client_id": "xxxxxxxxxxxxxxx",
        "client_secret": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
    }
}
```

## Setup tutorial

* Download python and pip on your system

* Download the code, click 'Clone or download' above

* Download the dependencies: `pip install -r requirements.txt`

* Create a `credentials.json` file in the same directory as everything else and
populate it with the correct data

* Start a `tmux` instance and run `python3 runprog.py` to start logging actions. The program
will be run in a thread in case it errors and will reset every 15 minutes

* Start another `tmux` instance and run `onceaday.py`
