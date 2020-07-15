# waybind

I wrote this to fill my need of doing simple rebindings to match the behaviour of my 60% keyboard on my laptop's keyboard. I'm using sway and I couldn't find suitable software for my needs. [This blog post](https://www.codedbearder.com/posts/writing-keyboard-remapper-wayland/) explains its existence.

It simply creates a virtual keyboard and repeats keystrokes from your keyboard, except when it detects the configured rebindings, then it replaces those keystrokes.

If you want to do complex macros take a look at [Hawck](https://github.com/snyball/Hawck). Waybind will never support those use cases.

There's nothing wayland specific about this remapper, there's just no shortage of keyboard remappers for X11, hence the name.

## Install
### Archlinux
There is the AUR package [waybind-git](https://aur.archlinux.org/packages/waybind-git/) available.
The package adds a systemd unit `waybind.service` which runs waybind as the user waybind.
The configuration file should be located at `/etc/waybind/config.yml`.

## Configuration
waybind searches in 3 paths for it configuration file: `./config.yml`, `$HOME/.config/waybind/config.yml` and `/etc/waybind/config.yml`.

To find out which input device you have to put in you can look at `/proc/bus/input/devices` for your keyboard and under "Handlers" you should see which event number it has.

Following shows an example:
```yaml
device: /dev/input/event0
rebinds:
  # Binds KEY_GRAVE to KEY_ESC
  # If modifier KEY_CAPSLOCK is also pressed then it's still KEY_GRAVE but KEY_CAPSLOCK is removed
  # If modifier is KEY_LEFTSHIFT then it's KEY_LEFTSHIFT + KEY_GRAVE
  - from: KEY_GRAVE
    to: KEY_ESC
    with_modifiers:
      - modifier: KEY_CAPSLOCK
        to: KEY_GRAVE
      - modifier: KEY_LEFTSHIFT
        to: SKIP

  # Binds KEY_CAPSLOCK + KEY_BACKSPACE to KEY_DELETE
  - from: KEY_BACKSPACE
    with_modifiers:
      - modifier: KEY_CAPSLOCK
        to: KEY_DELETE

  # KEY_CAPSLOCK + KEY_F12 will make waybind exit
  - from: KEY_F12
    with_modifiers:
      - modifier: KEY_CAPSLOCK
        to: EXIT

  # Completely unbind KEY_CAPSLOCK
  - from: KEY_CAPSLOCK
    unbind: true
``` 
