# terminal-timer

A terminal countdown timer with a big ASCII hourglass, falling sand and a
seven-segment digital clock. Gruvbox-flavored, inspired by cliamp. One Python
file, standard library only.

## Requirements

- Python 3.8+ with `curses` (Linux and macOS)
- A 256-color terminal, at least about 30x18 (bigger shows the full hourglass)

## Install

    curl -O https://raw.githubusercontent.com/Icarus-312/terminal-timer/main/timer
    chmod +x timer && mv timer ~/.local/bin/

## Usage

    timer 5              start a 5 minute timer (decimals ok: timer 0.5)
    timer start 5        same thing
    timer pause          pause running timer(s)
    timer resume         resume them
    timer cancel         cancel all running timers
    timer stop           same as cancel

In the timer window: `Space` pauses and resumes, `q` or `Esc` cancels.
When time is up it sends a desktop notification and plays a sound
(`notify-send` and `paplay`/`pw-play` on Linux, `osascript` and `afplay` on
macOS), falling back to the terminal bell.

## How control works

Each running timer writes `~/.local/state/timer/<pid>.json`. `pause`,
`resume` and `cancel` find live timers there and signal them (`SIGUSR1`,
`SIGUSR2`, `SIGTERM`), so you can control a timer from any other terminal.

## Security

No network access, no credentials, no elevated privileges. The only files
touched are the state files above. Control commands signal only processes
listed there whose PID is alive.

## Your data

Nothing personal is stored. State files hold only the timer's PID, duration
and elapsed time, and are deleted when the timer exits.

## License

MIT
