# Clock Widget For Linux

A small translucent glass clock for your Linux desktop. It shows a big time with the day and date underneath, plus optional extras: seconds, AM/PM, week number, day of the year, a day-progress bar and a second time zone. Show or hide it with a keyboard shortcut.

<p align="center">
  <img src="docs/clock.png" alt="Clock Widget" width="578">
</p>

<p align="center">
  <a href="https://www.buymeacoffee.com/UmbraDomini" target="_blank" rel="noopener noreferrer"><img src="https://img.shields.io/badge/Buy%20Me%20A%20Coffee-support-FFDD00?logo=buymeacoffee&logoColor=black" alt="Buy Me A Coffee"></a>
</p>

## Features

* Glass card with a big centered time, the day and the date
* Everything optional and switchable: seconds (dimmed so the minutes stand out), AM/PM, week number, day of the year, day-progress bar, second time zone
* 12-hour or 24-hour clock, four date styles, full or short day name, day and date stacked or on one line
* Second time zone with type-ahead search (type "lond" to find Europe/London), a custom label, and a +1d / -1d marker when its date differs
* Settings window with live preview: colors, opacity, corner radius, font, sizes, position, monitor
* Show / hide with one keyboard shortcut that you pick during setup
* Pick a corner, or just drag the card anywhere: it remembers the spot
* Does no work while hidden, and wakes once a minute (once a second only if seconds are shown)

## Install

```bash
git clone https://github.com/Umbra-Domini/clock-widget.git
cd clock-widget
bash install/setup.sh
```

The setup script installs the dependencies (apt, dnf and pacman are supported), asks you which keys should show / hide the widget, and starts the widget hidden. Press your shortcut to see it.

At the prompt, type the keys like `Ctrl+Alt+C` and press Enter, press Enter alone for the default (**Ctrl+Alt+C**), or type `none` to skip. It needs at least two modifiers (or Super) so normal typing keeps working, and the last key can be a letter, a digit or an F-key.

Options:

| Option                  | What it does                                            |
| ----------------------- | ------------------------------------------------------- |
| `--shortcut "Ctrl+Alt+K"` | Choose the shortcut without being asked                |
| `--no-shortcut`         | Skip the shortcut                                       |
| `--no-deps`             | Skip the package install                                |

If you'd rather do it by hand, you need Python 3.9 or newer, PyGObject with GTK 3, and the system time zone database. For the built-in shortcut you also need keybinder:

```bash
sudo apt install python3-gi python3-gi-cairo gir1.2-gtk-3.0 gir1.2-keybinder-3.0 tzdata
```

## Usage

```bash
python3 clock-widget.py            # start (hidden until you press the shortcut)
python3 clock-widget.py toggle     # show / hide
python3 clock-widget.py settings   # open the settings window
python3 clock-widget.py quit       # stop the widget
```

You can also right-click the card, or hover it and click the gear icon, to open settings. The cross button on the card hides it (the widget keeps running so your shortcut can bring it back).

## Settings

Open the settings window with the gear icon on the card, the right-click menu, or `python3 clock-widget.py settings`. Every change shows on the card straight away and saves automatically to `~/.config/clock-widget/config.json`.

The buttons at the bottom of the window: **Reset to defaults** puts everything back (it asks first), **Quit widget** stops the widget, and **Close** closes the window.

### Clock

<p align="center">
  <img src="docs/settings-clock.png" alt="Clock settings" width="380">
</p>

| Setting                    | What it does                                                                               |
| -------------------------- | ------------------------------------------------------------------------------------------ |
| 24-hour clock              | Off is the 12-hour clock                                                                   |
| Show seconds               | Adds seconds to the time                                                                   |
| Dim the seconds            | Draws the seconds fainter so the minutes stand out. Only used when seconds are shown       |
| Show AM / PM               | The small AM / PM next to the time. Only used with the 12-hour clock                       |
| Show day of the week       | The day name under the time                                                                |
| Day style                  | Full (Tuesday) or short (Tue)                                                              |
| Show date                  | The date under the day                                                                     |
| Date style                 | October 13, 2026, Oct 13, 2026, 13 October 2026 or 2026-10-13                              |
| Day and date on one line   | `Tuesday · Oct 13, 2026` instead of two lines                                              |
| Show week number           | Adds "Week 42" (ISO week number)                                                           |
| Show day of the year       | Adds "Day 286". With the week number, both share one line                                  |
| Show day progress bar      | How far through today you are, midnight to midnight, with a percentage                     |
| Show a second time zone    | Adds a line with the time somewhere else                                                   |
| Time zone                  | Start typing a city or region (London, Kolkata, Toronto, Tokyo ...). It turns red if unknown |
| Label (optional)           | Name shown for the second time zone. Empty uses the city from the zone name                |

### Appearance

<p align="center">
  <img src="docs/settings-appearance.png" alt="Appearance settings" width="380">
</p>

| Setting                        | What it does                                                          |
| ------------------------------ | --------------------------------------------------------------------- |
| Card color                     | Background color of the card                                          |
| Card opacity                   | 0% is clear glass, 100% is solid                                      |
| Border color / opacity / width | The thin outline around the card. Set the width to 0 for no border    |
| Card corner radius             | How round the card corners are                                        |
| Text color                     | Color for all the text                                                |
| Accent color (start / end)     | The gradient used by the progress bar and the settings window         |
| Font                           | Pick a font, or click Default for the system font                     |
| Time / Day / Date size         | Text sizes in pixels                                                  |
| Small text size                | Week / day of the year, second time zone and the progress percentage  |

### Layout

<p align="center">
  <img src="docs/settings-layout.png" alt="Layout settings" width="380">
</p>

| Setting                         | What it does                                                                                         |
| ------------------------------- | ---------------------------------------------------------------------------------------------------- |
| Card width / height             | Size of the card in pixels. Height is a minimum: the card grows if the content needs more room       |
| Position                        | Fixed coordinates, one of the four corners (the default is top right), or Center                     |
| Monitor                         | Primary monitor, or pick a specific one                                                              |
| Distance from screen edge       | Gap to the screen edge. Only used by the corner positions                                            |
| X position / Y position         | Only used by Fixed                                                                                   |
| Use the card's current position | Switches to Fixed and saves where the card is right now                                              |

Dragging the card moves it and switches Position to Fixed, so it stays where you drop it.

### Behavior

<p align="center">
  <img src="docs/settings-behavior.png" alt="Behavior settings" width="380">
</p>

| Setting                        | What it does                                                                                                                                             |
| ------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Global shortcut                | Click it, then press a key combo (use Ctrl, Alt or Super, or an F-key). Esc cancels, Backspace clears. See [Shortcuts on Wayland](#shortcuts-on-wayland) |
| Window layer                   | Normal, Always on top, or Below other windows. Below can block clicks if your desktop has an icon layer                                                  |
| Start visible at login         | On shows the card as soon as the widget starts. Off keeps it hidden until you use the shortcut                                                           |
| Take keyboard focus when shown | Off means the card never steals focus from what you're typing in                                                                                         |

To start the widget when you log in, add `python3 /full/path/to/clock-widget.py` to your desktop's startup applications.

## Shortcuts on Wayland

On X11 the built-in shortcut (Settings > Behavior > Global shortcut) works from anywhere.

On Wayland it only works while an X11 window has focus, so you need a shortcut from your desktop instead. On GNOME, `bash install/setup.sh` sets this up for you. On other desktops, add a custom shortcut in your keyboard settings with this command (use the real path to the file):

```bash
python3 /full/path/to/clock-widget.py toggle
```

* **KDE Plasma:** System Settings > Shortcuts > Add Command
* **XFCE:** Settings > Keyboard > Application Shortcuts
* **Others:** look for "custom shortcuts" in your keyboard settings

The command also starts the widget if it isn't running.

## Troubleshooting

* **"Choose a time zone in Settings" on the card:** the second time zone is switched on but no zone is set. Type one in Settings > Clock > Time zone.
* **"Unknown time zone" on the card, or the Time zone box is red:** use a name from the list that appears as you type, like `Europe/London`. If the list is empty, install the time zone database: `sudo apt install tzdata`.
* **Shortcut does nothing:** see [Shortcuts on Wayland](#shortcuts-on-wayland). If the settings window says the shortcut couldn't be registered, another app already uses that combination.
* **Run it in a terminal** (`python3 clock-widget.py`) to see any errors.

## Files

| File                | What it does                                      |
| ------------------- | ------------------------------------------------- |
| `clock-widget.py`   | The widget itself                                 |
| `cw_config.py`      | Defaults, validation, loading and saving settings |
| `cw_style.py`       | Builds the CSS from your settings                 |
| `cw_settings.py`    | The settings window                               |
| `install/setup.sh`  | Installs dependencies and sets the shortcut       |

Keep the four `.py` files in the same folder.

## More widgets

Looking for music controls too? See the [Spotify Widget](https://github.com/Umbra-Domini/spotify-widget): the same glass look with album art, a seek bar and volume. The two are separate programs, so you can use either one or both.

## Support

If you like the widget and want to say thanks, you can <a href="https://www.buymeacoffee.com/UmbraDomini" target="_blank" rel="noopener noreferrer">buy me a coffee</a>. It's never expected, and it's appreciated.

## License

MIT, see [LICENSE](LICENSE).
