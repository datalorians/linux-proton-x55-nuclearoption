# 🕹️ Saitek X-55 Rhino Yaw Fix for Nuclear Option on Linux / Proton

Fix missing or broken yaw/rotation input for the **Saitek X-55 Rhino** /
**Mad Catz X-55 Rhino** stick in **Nuclear Option** on Linux through
Steam/Proton.

This package creates a temporary virtual joystick with `evsieve` and remaps
the X-55 twist/yaw axis into the axis Nuclear Option reads correctly.

## 🔍 The Problem

On some Linux/Proton setups, the Saitek X-55 Rhino stick exposes twist/yaw as:

```text
abs:rz
```

Nuclear Option may ignore that axis for yaw/rotation. The stick can appear in
the game, and other axes can work, but yaw does nothing.

## ✨ What This Does

This package starts `evsieve`, grabs the real Saitek X-55 Rhino stick, and
creates a temporary virtual joystick with this remap:

```text
abs:rz -> abs:z
```

When Nuclear Option launches, it sees the virtual joystick with yaw on the axis
it expects. When the game exits, the helper stops the virtual device.

## 🚫 What This Does Not Do

- It does not change Nuclear Option files.
- It does not permanently modify your controller.
- It does not emulate a full Xbox controller.

## ✅ Tested Device

| Item | Value |
| --- | --- |
| Stick | Saitek X-55 Rhino / Mad Catz X-55 Rhino stick |
| Default event path | `/dev/input/by-id/usb-Madcatz_Saitek_Pro_Flight_X-55_Rhino_Stick_G0013831-event-joystick` |
| Tool | `evsieve` |
| Game | Nuclear Option on Steam/Proton |

People often search for `saitek x55`, `saitek x-55`, `x55 rhino`, or
`Mad Catz X-55 Rhino`; this is that device family.

## 📦 Requirements

- Saitek X-55 Rhino stick
- `evsieve` installed at `~/.cargo/bin/evsieve`
- Read access to the X-55 event device
- Write access to `/dev/uinput`

Install `evsieve`:

```bash
cargo install evsieve
```

If `evsieve` cannot read the joystick or create a virtual device, your user may
need udev permissions for the controller and `/dev/uinput`.

## 🚀 Install

```bash
git clone https://github.com/datalorians/linux-proton-x55-nuclearoption.git
cd linux-proton-x55-nuclearoption
./scripts/install-x55-yaw-fix.sh
```

This installs:

```text
~/.local/bin/x55-nuclear-option-virtual-stick
~/.local/bin/x55-nuclear-option-stop-virtual-stick
```

## 🎮 Steam Launch Option

Use this as the Nuclear Option launch option in Steam:

```bash
bash -lc '$HOME/.local/bin/x55-nuclear-option-virtual-stick; cleanup(){ $HOME/.local/bin/x55-nuclear-option-stop-virtual-stick; }; trap cleanup EXIT; "$@"; rc=$?; cleanup; exit $rc' -- %command%
```

That starts the virtual joystick, launches the game, and removes the virtual
joystick when the game exits.

## ⚙️ Custom Device Path

If your Saitek X-55 Rhino stick has a different event path, override it:

```bash
X55_INPUT=/dev/input/by-id/your-stick-event-joystick \
  ~/.local/bin/x55-nuclear-option-virtual-stick
```

To see likely device paths:

```bash
ls -l /dev/input/by-id/*X-55* /dev/input/by-id/*Rhino* 2>/dev/null
```

## 🧯 Troubleshooting

Start manually:

```bash
~/.local/bin/x55-nuclear-option-virtual-stick
```

Stop manually:

```bash
~/.local/bin/x55-nuclear-option-stop-virtual-stick
```

Log file:

```text
~/.local/state/x55-nuclear-option/evsieve.log
```

Virtual device link:

```text
~/.local/state/x55-nuclear-option/virtual-nuclear-option-x55-stick
```

If the game shows duplicate controllers, make sure the helper is using `grab`
successfully and that stale helper processes are stopped.

## 🤖 AI Disclosure

This package was developed with assistance from OpenAI's Codex/ChatGPT. The
scripts and documentation were reviewed and tested locally before publication,
but they are community-maintained and provided as-is.

AI disclosure is separate from licensing: the disclosure explains how the work
was produced, while the license explains what rights you have to use and modify
the code.

## 📄 License

Repository scripts and documentation are released under the [MIT License](LICENSE).

`evsieve` has its own license.
