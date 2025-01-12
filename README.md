# sway-scratch [![version](https://img.shields.io/github/v/tag/b0o/sway-scratch?style=flat&color=yellow&label=version&sort=semver)](https://github.com/b0o/sway-scratch/releases) [![license: MIT](https://img.shields.io/github/license/b0o/sway-scratch?style=flat&color=green)](https://opensource.org/licenses/MIT)

> Floating scratchpads for Sway


## Usage

<!-- USAGE -->

```

sway-scratch v0.1.1 (github.com/b0o/sway-scratch)

(c) 2019-2025 Maddison Hellstrom <github.com/b0o>
MIT License (https://mit-license.org/)

Usage
  sway-scratch [-q query-type|-i|-x|-t|-f] [-c change-type] [-W width] [-H height] [-T timeout] [-p prefix] [-n num] [-r] [-R] [-P position] mark query command [arg ..]
  sway-scratch -h
  sway-scratch -v

Description
  sway-scratch creates and manages named scratchpads in Sway.

  Sway's native scratchpad only supports cycling through a collection of windows
  one-by-one.

  sway-scratch makes it possible to create any number of named scratchpads which can be
  toggled individually. Named scratchpads are assigned a unique mark which is
  used to identify and control them.

  Any program that spawns a window can be used as a named scratchpad.


  In order for sway-scratch to create the scratchpad, it subscribes to Sway window event
  notifications and then launches the program. Events are tested against the
  specified query, based on the query-type and change-type options.

  When a matching window is found, it's converted into a named scratchpad and
  sway-scratch exits with code 0. If no matching windows are found before the timeout
  expires, sway-scratch exits with a code of 2.

  If a window with the given mark already exists, its visibility is toggled and
  the command is not run.

Options
  General Options
    -h               Display usage information
    -v               Print sway-scratch version.
    -T               Number of seconds to wait for the target window to appear.
                     Default: 30

  Mark options
    -p               The string to prepend to the mark. This string is used to
                     indentify other scratchpads that have been created by sway-scratch
                     when using the -r option.
                     Default: "sc-"

  Query Options
    -q <query-type>  Set the type of query to use when looking for the
                     application window.

                     Supported query types are:
                       app_id   Match against the app_id property of the window.
                                Applicable to wayland windows only.

                       class    Match against the class property of the window.
                                Applicable to xwayland windows only.

                       title    Match against the window title.

                       focus    Match the focused window.

                     Default: "app_id".

    -i               Shorthand for '-q app_id'
    -x               Shorthand for '-q class'
    -t               Shorthand for '-q title'
    -f               Shorthand for '-q focus'

    -R               Treat the query as a regular expression

    -c <change-type> Window change type to listen for. See the WINDOW subsection
                     under EVENTS in sway-ipc(7) for all valid change types.
                     Default: "new".

    -n <num>         The event number to listen for. If 0, the first event
                     matching the query will be used. Otherwise, the nth event
                     matching the query will be used.
                     Default: 0.


  Scratchpad Options
    -W <width>       Width of the scratchpad window. Default: 75 ppt.
    -H <height>      Height of the scratchpad window. Default: 75 ppt.
                     Width and height can be followed by a unit type of "px" or "ppt".
                     If no unit is specified, "ppt" is assumed.

    -P <position>    Position of the scratchpad window. Valid values are:
                       center    Center the window on the screen.
                       mouse     Place the window under the mouse cursor.
                       <x>,<y>   Place the window at the specified coordinates.
                     Default: "center"

    -r               If another scratchpad is currently focused, it will be hidden
                     before showing the new scratchpad. Other scratchpads are
                     identified based on the presence of the prefix specified
                     with the -p option.

Arguments
  mark               The mark assigned to this scratchpad. The mark will be
                     prepended with the prefix specified by the -p option.
                     If a window with the mark is found, its visibility will be
                     toggled. Otherwise, the 'command' will be run to spawn the
                     target application. If this option is an empty string, an
                     unnamed scratchpad will be created and no mark will be
                     assigned.

  query              The query to test for when waiting for the target
                     application's window to appear. See Query Options above for
                     more information about query types.

  command [arg ..]   The command used to spawn the target application if a
                     matching mark is not found. This command will be executed
                     as 'swaymsg exec <command [arg ..]>'.


Example Sway Configuration

bindsym {
  #                         Options      Mark  Query      Command
  #--------------------------------------------------------------------------------
  Mod4+t exec sway-scratch               term  Alacritty  alacritty
  Mod4+b exec sway-scratch  -W 85 -H 85  ff-p  firefox    firefox -P personal
  Mod4+B exec sway-scratch  -W 85 -H 85  ff-w  firefox    firefox -P work
  Mod4+e exec sway-scratch  -x           eq    qpaeq      QT_QPA_PLATFORM=xcb qpaeq
  Mod4+s exec sway-scratch  -tc title    spot  Spotify    gtk-launch spotify
}

```

<!-- /USAGE -->

## License

<!-- LICENSE -->

&copy; 2019-2025 Maddison Hellstrom

Released under the MIT License.

<!-- /LICENSE -->
