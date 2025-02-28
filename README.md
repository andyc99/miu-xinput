# MIU XInput

Turn keyboard into a 360 Controller.

This is a fork of [XArcade XInput](https://github.com/mikew/xarcade-xinput) to add the ability to map a key to multiple left-stick axes, i.e. map a key to a non-cardinal angle.
It's specifically aimed at Marble It Up! where the optimal turning angle is not at 45 degrees.
NB Since version 1.2 key presses will only be captured when 'Marble It Up!' is the active application; other apps will function as normal (so you can type in other applications while MIU XInput is running.)

Credit to MikeW for doing all the hard work on XArcade Xinput.

## Installation

1. [Download the latest release](https://github.com/andyc99/miu-xinput/releases/latest)
1. Double-click `Install Driver.exe`.
1. Run `MIU XInput.exe`

## Manual Installation
1. [Download the latest release](https://github.com/andyc99/miu-xinput/releases/latest)
1. Run `Scp Driver Installer\ScpDriverInstaller.exe` and press `Install`
1. Run in Admin Command Prompt:
    ```dos
    netsh advfirewall firewall add rule name="MIU XInput" dir=in action=allow protocol=TCP localport=32123
    netsh http add urlacl url=http://+:32123/ user=Everyone
    ```
1. Run `MIU XInput.exe`

## Usage

Mappings are configurable. Some examples are below.

When you run "MIU XInput.exe" it will open [http://localhost:32123/](http://localhost:32123/) in a browser to access the Web UI configuration page.
From here you can turn it on or off, and change the mappings.
The Web UI can also be accessed on any phone, tablet, or other computer.

## Mappings

You can change any keyboard key to output any single 360 Controller Axis or a specified angle (relative to the up direction).
Or define a modifier key to swithch between normal movement and a specified angle.See the examples below.

The "angle" key mappings will take priority over standard axes, so you don't have to release other keys (e.g. "up") to get the desired angle.

The following "Axes" have been added in addition to those of XArcade Xinput:

LeftStickAngleLeft <angle>: Maps a key to the specified angle left
LeftStickAngleRight <angle>: Maps a key to the specified angle right
LeftStickAngleModifier <angle>: When held down, the mapped key modifies normal left and right axes to the above angle axes
LeftStickAngleToggle <angle>: When pressed, the mapped key toggles left and right keys between normal movement and the above angle axes

Example configs:

This uses a toggle key to dynamically switch the A and D keys, betwen the normal left and right controls and a 75 degree angle. This angle gives the fastest cornering speed for me:
This mapping is the default when the app is first run.

```json
{
    "W": [0, "LeftStickUp"],
    "S": [0, "LeftStickDown"],
    "A": [0, "LeftStickLeft"],
    "D": [0, "LeftStickRight"],
	"LShiftKey": [0, "LeftStickAngleToggle", 75]
}
```

This remaps the normal left and right controls, A and D, to a 75 degree angle:

```json
{
    "W": [0, "LeftStickUp"],
    "S": [0, "LeftStickDown"],
    "A": [0, "LeftStickAngleLeft", 75],
    "D": [0, "LeftStickAngleRight", 75]
}
```

This augments standard WASD controls with two extra ones at a 75 degree angle:

```json
{
    "W": [0, "LeftStickUp"],
    "S": [0, "LeftStickDown"],
    "A": [0, "LeftStickLeft"],
    "D": [0, "LeftStickRight"],
    "Q": [0, "LeftStickAngleLeft", 75],
    "E": [0, "LeftStickAngleRight", 75]
}
```

This uses a modifier key (which must be held down) to dynamically change the normal left and right controls, A and D, to a 75 degree angle:

```json
{
    "W": [0, "LeftStickUp"],
    "S": [0, "LeftStickDown"],
    "A": [0, "LeftStickLeft"],
    "D": [0, "LeftStickRight"],
	"LShiftKey": [0, "LeftStickAngleModifier", 75]
}
```

See the original XArcade Xinput project for further mappings which are possible for other controller sticks and buttons.

The syntax is JSON, where the key on the left is one of [System.Windows.Forms.Keys](https://msdn.microsoft.com/en-us/library/system.windows.forms.keys(v=vs.110).aspx#Anchor_1), and the value is an array of `[controllerIndex, controllerButtonOrAxis, ...parameters]`

## Command Line Arguments

You can pass arguments when running MIU XInput:

Argument | Purpose
---|---
`--debug` | Prints some debug information.
`--default` | Force using the default mapping. This can help if you get stuck when writing your own mappings. This takes precedence over other arguments.
`--skip-ui` | Will prevent your browser from opening.
`--start-disabled` | Won't listen for keyboard events when starting.
`--mapping` | Specify a mapping to use on startup.
