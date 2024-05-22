# The config file

Qtile is configured in Python. A script (`~/.config/qtile/config.py` by
default) is evaluated, and a small set of configuration variables are pulled
from its global namespace.

## Configuration lookup order

Qtile looks in the following places for a configuration file, in order:

* The location specified by the `-c` argument.
* `$XDG_CONFIG_HOME/qtile/config.py`, if it is set
* `~/.config/qtile/config.py`
* first `qtile/config.py` found in `$XDG_CONFIG_DIRS` (defaults to `/etc/xdg`)
* It reads the module `libqtile.resources.default_config`, included by
  default with every Qtile installation.

Qtile will try to create the configuration file as a copy of the default
config, if it doesn't exist yet, this one will be placed inside of 
`$XDG_CONFIG_HOME/qtile/config.py` (if set) or `~/.config/qtile/config.py`.

## Default Configuration

The [default configuration][default-config-file]
is invoked when qtile cannot find a configuration file. In addition, if qtile
is restarted or the config is reloaded, qtile will load the default
configuration if the config file it finds has some kind of error in it. The
documentation below describes the configuration lookup process, as well as what
the key bindings are in the default config.

The default config is not intended to be suitable for all users; it's mostly
just there so qtile does /something/ when fired up, and so that it doesn't
crash and cause you to lose all your work if you reload a bad config.

## Configuration variables

A Qtile configuration consists of a file with a bunch of variables in it, which
qtile imports and then runs as a Python file to derive its final configuration.
The documentation below describes the most common configuration variables; more
advanced configuration can be found in the [qtile-examples](https://github.com/qtile/qtile-examples)
repository, which includes a number
of real-world configurations that demonstrate how you can tune Qtile to your
liking. (Feel free to issue a pull request to add your own configuration to the
mix!)

* [Lazy objects](lazy.md)
* [Groups](groups.md)
* [Keys](keys.md)
* [Layouts](layouts.md)
* [Mouse](mouse.md)
* [Screens](screens.md)
* [Hooks](hooks.md)

In addition to the above variables, there are several other boolean
configuration variables that control specific aspects of Qtile's behavior:

### `auto_fullscreen`

**Default**: `True`.
If a window requests to be fullscreen, it is automatically
fullscreened. Set this to false if you only want windows to be
fullscreen if you ask them to be.

### `bring_front_click`

**Default**: `False`.
When clicked, should the window be brought to the front or not.
If this is set to "floating_only", only floating windows will get affected
(this sets the X Stack Mode to Above). This will ignore the layering rules and
will therefore bring windows above other windows, even if they have been set
as "kept_above". This may cause issues with docks and other similar apps as these
may end up hidden behind other windows. Setting this to `False` or `"floating_only"`
may therefore be required when using these apps.

### `cursor_warp`

**Default**: `False`.
If true, the cursor follows the focus as directed by the keyboard,
warping to the center of the focused window. When switching focus between
screens, If there are no windows in the screen, the cursor will warp to
the center of the screen.

### `dgroups_key_binder`

**Default**: `None`.
A function which generates group binding hotkeys. It takes a single
argument, the DGroups object, and can use that to set up dynamic key bindings.

A sample implementation is available in [libqtile/dgroups.py][]
called simple_key_binder(), which will bind groups to mod+shift+0-10 by default.

[libqtile/dgroups.py]: https://github.com/qtile/qtile/blob/master/libqtile/dgroups.py

### `dgroups_app_rules`

**Default**: `[]`.
A list of Rule objects which can send windows to various groups based on matching criteria.

### `extension_defaults`

**Default**: `dict(font="sans", fontsize=12, padding=3)`.
Default settings for extensions.

### `floating_layout`

**Default**: `layout.Floating(float_rules=[...])`.
The default floating layout to use. This allows you to set
custom floating rules among other things if you wish.

See the configuration file for the default `float_rules`.

### `floats_kept_above`

**Default**: `True`.
Floating windows are kept above tiled windows (Currently x11 only. Wayland support coming soon.)

### `focus_on_window_activation`

**Default**: `'smart'`.
Behavior of the _NET_ACTIVATE_WINDOW message sent by applications:

- urgent: urgent flag is set for the window
- focus: automatically focus the window
- smart: automatically focus if the window is in the current group
- never: never automatically focus any window that requests it

### `follow_mouse_focus`

**Default**: `True`.
Controls whether or not focus follows the mouse around as it moves
across windows in a layout.

### `widget_defaults`

**Default**: `dict(font='sans', fontsize=12, padding=3)`.
Default settings for bar widgets.

### `reconfigure_screens`

**Default**: `True`.
Controls whether or not to automatically reconfigure screens when there
are changes in randr output configuration.

### `wmname`

**Default**: `'LG3D'`.
Gasp! We're lying here. In fact, nobody really uses or cares
about this string besides java UI toolkits; you can see several
discussions on the mailing lists, GitHub issues, and other WM
documentation that suggest setting this string if your java app
doesn't work correctly. We may as well just lie and say that
we're a working one by default. We choose LG3D to maximize irony:
it is a 3D non-reparenting WM written in java that happens to be
on java's whitelist.

### `auto_minimize`

**Default**: `True`.
If things like steam games want to auto-minimize themselves when losing
focus, should we respect this or not?

## Testing your configuration

The best way to test changes to your configuration is with the provided scripts
at [./scripts/xephyr][] (X11) or [./scripts/wephyr][] (Wayland). This will run
Qtile with your `config.py` inside a nested window and prevent your running
instance of Qtile from crashing if something goes wrong.

[./scripts/xephyr]: https://github.com/qtile/qtile/blob/master/scripts/xephyr
[./scripts/wephyr]: https://github.com/qtile/qtile/blob/master/scripts/wephyr

See [Hacking Qtile][hacking-on-qtile] for more information on using Xephyr.
