# distractions

Command-line distraction management tool with builtin [pomodoro mode](#pomodoro).

```
SYNOPSIS
    distractions [on|off|toggle|status|clock|pomodoro]

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

# Usage

Create a `.distractions` file in your home directory:

```
gmail.com
news.ycombinator.com
twitter.com
quora.com
```

Turn off distractions:

```bash
$ sudo distractions off -f ~/.distractions
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
$ sudo distractions on
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
$ distractions pomodoro -f ~/.distractions
Starting Pomodoro.
Starting work-mode.
```

By default, work is 25 minutes and break is 5 minutes. You can customize that;

```bash
$ distractions pomodoro -f ~/.distractions --work-time 15m --break-time 3m
```

You can set custom scripts to be executed when work/break mode starts:

```bash
$ distractions pomodoro -f ~/.distractions --on-work ~/start-work.sh --on-break ~/start-break.sh 
```
