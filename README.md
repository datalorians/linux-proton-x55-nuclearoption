# X-55 Yaw Fix for Nuclear Option on Proton

Helper scripts for using a Saitek/Mad Catz X-55 Rhino stick with Nuclear
Option on Linux through Steam/Proton.

Some X-55 setups expose twist/yaw as `abs:rz`, while Nuclear Option expects the
rotation input on a different axis. This package uses `evsieve` to create a
temporary virtual joystick and remap:

```text
abs:rz -> abs:z
```

This repo is intentionally separate from the TrackIR package. It only handles
the X-55 joystick yaw workaround.

## Requirements

- X-55 Rhino stick
- `evsieve` installed at `~/.cargo/bin/evsieve`
- Read access to the X-55 event device
- Write access to `/dev/uinput`

Install `evsieve` with:

```bash
cargo install evsieve
```

## Install

```bash
git clone https://github.com/datalorians/linux-proton-x55-nuclearoption.git
cd linux-proton-x55-nuclearoption
./scripts/install-x55-yaw-fix.sh
```

Default input path:

```text
/dev/input/by-id/usb-Madcatz_Saitek_Pro_Flight_X-55_Rhino_Stick_G0013831-event-joystick
```

Override it when starting the adapter if your device path differs:

```bash
X55_INPUT=/dev/input/by-id/your-stick-event-joystick \
  ~/.local/bin/x55-nuclear-option-virtual-stick
```

## Steam Launch Option

Use this for Nuclear Option:

```bash
bash -lc '$HOME/.local/bin/x55-nuclear-option-virtual-stick; cleanup(){ $HOME/.local/bin/x55-nuclear-option-stop-virtual-stick; }; trap cleanup EXIT; "$@"; rc=$?; cleanup; exit $rc' -- %command%
```

The virtual device is stopped when the game exits.

## Stop Manually

```bash
~/.local/bin/x55-nuclear-option-stop-virtual-stick
```

## License

MIT.
