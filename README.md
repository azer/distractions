# distractions

A command-line tool to define distracting websites and turn them on/off when needed.

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
$ sudo distractions status
```
