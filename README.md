# distractions

A command-line tool (and background service) for managing distractions with builtin [pomodoro mode](#pomodoro).

```
SYNOPSIS
    distractions [on|off|toggle|status|background|clock|pomodoro]

OPTIONS
    -f --file
        Path to the distractions file. Needed for 'off' command
       --work-time
        Work time for pomodoro mode. Example values: '20m' '1h'. Default: 25m.
       --break-time
        Break time for pomodoro mode. Example values: 20m, 3m. Default: 5m
       --on-work
        Execute given script on starting to work
       --on-break
        Execute given script on break
       --json
        Format output as JSON. (Only implemented for clock command)
       --relative
        Format output as relative time. (Only implemented for clock command)

    -h --help
        Show help
```

# Install

Clone and add it to `$PATH`

# Setting up

Create a `.distractions` file in your home directory:

```
gmail.com
news.ycombinator.com
twitter.com
quora.com
```

Start a background service as `root`, it needs write access to `/etc/hosts`;

```bash
$ sudo distractions background -f ~/.distractions
```

If preferred, you can define a system service;

```
Unit]
Description=Distractions

[Service]
Type=simple
ExecStart=distractions background -f /home/azer/.distractions
User=root

[Install]
WantedBy=multi-user.target
```

# Usage

Once distractions is running on the background, you can call it by sending signals via the same distraction script;

```bash
$ distractions off
```

It'll add these lines to `/etc/hosts`:

```hosts
# <distractions>
127.0.0.1 gmail.com
127.0.0.1 news.ycombinator.com
127.0.0.1 twitter.com
127.0.0.1 quora.com
# </distractions>
```

You can turn them on if needed, so they'll be removed from `/etc/hosts`

```bash
$ distractions on
```

Check if it's on/off:

```bash
$ distractions status
```

# Pomodoro

Run `clock` command to see how many minutes has passed since you've turned off distractions;

```bash
$ distractions clock
00:08 Work
```

Run `pomodoro` command if you prefer switching work/break mode automatically:

```bash
$ distractions pomodoro
Starting Pomodoro.
Starting work-mode.
```

By default, work is 25 minutes and break is 5 minutes. You can customize that;

```bash
$ distractions pomodoro --work-time 15m --break-time 3m
```

You can set custom scripts to be executed when work/break mode starts:

```bash
$ distractions pomodoro --on-work ~/start-work.sh --on-break ~/start-break.sh
```

If desired, you can define a system service for pomodoro mode so you can control it easily;

```
Description=Pomodoro

[Service]
Type=simple
ExecStart=distractions pomodoro --on-work /home/azer/.pomodoro/start-work.sh --on-break /home/azer/.pomodoro/start-break.sh
Environment=DISPLAY=:0
Environment=XAUTHORITY=%h/.Xauthority
Environment=DBUS_SESSION_BUS_ADDRESS=unix:path=/run/user/1000/bus
User=azer

[Install]
WantedBy=multi-user.target
```

Note that this service runs as non-root user, in contrast to the background service.
