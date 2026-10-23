# Troubleshooting

Start every diagnosis with:

```bash
muse-ai doctor
systemctl status musegadget --no-pager
journalctl -u musegadget -f
```

## Token rejected: "that SDK token is not valid"

Make a fresh token at gadgets.muse.ai, Account, SDK tokens. Paste it
whole, on the machine itself. Common causes: a placeholder was pasted,
the token was copied with a trailing space or line break, or the value
was an API key from a different service. Tokens never go in chat, in
Git, or in an issue.

## Installer refuses to run

It refuses root by design. Leave the root shell, open a normal terminal
as your daily user, and rerun.

## "Untested, Arch-based" warning

Expected on Omarchy. Omarchy is Arch based, the upstream installer has
a pacman path, and the warning is informational. Continue.

## No Bluetooth controller

```bash
ls /sys/class/bluetooth
systemctl status bluetooth --no-pager
bluetoothctl power on
bluetoothctl show
```

If there is still no controller, check that the hardware is not blocked
(`rfkill list`) and that firmware loaded (`dmesg | grep -i bluetooth`).
A USB Bluetooth dongle is the practical fallback on old machines.
Bluetooth is only needed for pairing. Once paired, network is enough.

## Pairing window expired

Pairing opens a short window after install. If you miss it, restart the
service or rerun `muse-ai install-gadget`, keep the phone close to the
machine, and pair right away in the app: Developer mode, Settings,
Devices.

## Launcher missing from the menu

```bash
muse-ai launcher
ls ~/.local/share/applications/Muse.desktop
```

Search the launcher for "Muse AI", not "Muse". If the icon looks
black or invisible, rerun `muse-ai launcher`: it re-renders the blue
icon and clears the thumbnail cache, which was the fix on the test Air.

## Muse cannot see the machine after pairing

The machine must be awake and on the network. No terminal needs to stay
open. Check `muse-ai status` on the machine, confirm musegadget is
active, and confirm you are signed into the same Muse account in the
app and at muse.ai.

## Removing everything

```bash
muse-ai uninstall
# Gadget service, official path: re-download upstream install.sh, then
bash install.sh --uninstall
```
