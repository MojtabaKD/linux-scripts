# screenshots

A screenshot utility for Ubuntu/Xfce that captures the leftmost monitor,
selects a region, or captures after a delay.

## Requirements

Requires an X11 session.

```bash
sudo apt update
sudo apt install maim x11-xserver-utils
```

## Installation

Run from the repository directory:

```bash
install -Dm755 ./screenshots "$HOME/.local/bin/screenshots"
mkdir -p "$HOME/Pictures/screenshots"
```

Screenshots are saved to `~/Pictures/screenshots/`.

## Usage

Capture the entire leftmost monitor:

```bash
~/.local/bin/screenshots
```

Select a region:

```bash
~/.local/bin/screenshots --region
```

Capture after five seconds:

```bash
~/.local/bin/screenshots --delay 5
```

Select a region after a five-second delay:

```bash
~/.local/bin/screenshots --region --delay 5
```

## Xfce keyboard shortcuts

| Shortcut | Action |
| --- | --- |
| `Super + Shift + Print Screen` | Capture the entire leftmost monitor |
| `Super + Alt + F11` | Select a region |
| `Super + Alt + F12` | Capture after five seconds |

Configure the shortcuts from the terminal:

```bash
set_xfce_shortcut() {
    local path="$1"
    local command="$2"

    if xfconf-query -c xfce4-keyboard-shortcuts \
        -p "$path" >/dev/null 2>&1; then
        xfconf-query -c xfce4-keyboard-shortcuts \
            -p "$path" -s "$command"
    else
        xfconf-query -c xfce4-keyboard-shortcuts \
            -p "$path" --create -t string -s "$command"
    fi
}

set_xfce_shortcut \
    '/commands/custom/<Super><Shift>Print' \
    "$HOME/.local/bin/screenshots"

set_xfce_shortcut \
    '/commands/custom/<Super><Alt>F11' \
    "$HOME/.local/bin/screenshots --region"

set_xfce_shortcut \
    '/commands/custom/<Super><Alt>F12' \
    "$HOME/.local/bin/screenshots --delay 5"
```

## Verify shortcuts

```bash
xfconf-query -c xfce4-keyboard-shortcuts -lv |
    grep -E 'Super.*(Print|F11|F12)'
```
