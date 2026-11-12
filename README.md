# htop for Android 6.0+ (ARMv7)

A lightweight, precompiled build of **htop 2.0.2** for legacy **32-bit ARM Android devices**. This repository distributes ready-to-install ZIP packages for **Magisk** and **manual installation**.

The executable targets **Android API 23**, includes **ncurses 6.0 statically linked**, and incorporates compatibility fixes for Android 6's dynamic linker and older terminal databases.

## Features

- Native **ARMv7 (`armeabi-v7a`)** executable.
- **Android 6.0 (API 23)** compatibility.
- Statically linked **ncurses 6.0** — no separate ncurses shared library required.
- Legacy ELF linker compatibility while retaining a PIE executable.
- Included `xterm` and `xterm-256color` terminfo entries.
- Systemless installation and safe removal using **Magisk**.
- Separate ZIP for manual installation.

## Compatibility

| Component | Details |
| --- | --- |
| Android | 6.0+ (API 23 target; newer versions not extensively tested) |
| CPU architecture | ARMv7 / `armeabi-v7a` (32-bit) |
| htop | 2.0.2 |
| ncurses | 6.0 (statically linked) |
| Magisk | Tested with Magisk 29 |
| Supported terminals | `xterm`, `xterm-256color` |

**Tested device:** Samsung device running Android 6.0.1, ARMv7, and Magisk 29. Compatibility with other devices may vary.

## Downloads

Download the packages from the [Releases](https://github.com/KETCH502M/htop-for-Android-6.0-ARMv7-Magisk-Module/releases/) section:

| Package | Purpose |
| --- | --- |
| `htop-armv7-magisk-v2.0.2.zip` | Install as a Magisk module (recommended) |
| `htop-armv7-manual-v2.0.2.zip` | Install the executable and terminfo entries manually |

> These packages are intended for 32-bit ARM Android devices, not x86, x86_64, or ARM64-only environments.

## Installation

### Option 1: Magisk module (recommended)

1. Download `htop-armv7-magisk-v2.0.2.zip` from [Releases](https://github.com/KETCH502M/htop-for-Android-6.0-ARMv7-Magisk-Module/releases/).
2. Open **Magisk → Modules → Install from storage**.
3. Select the downloaded ZIP.
4. Reboot Android.
5. Open a terminal or SSH session and run:

   ```sh
   htop
   ```

The module makes `htop` and its terminal definitions available through Magisk's systemless mount, without writing directly to the system partition.

**Uninstall:** Open **Magisk → Modules**, remove the htop module, and reboot. The module does not intentionally delete files from unrelated installations.

### Option 2: Manual installation

Download and extract `htop-armv7-manual-v2.0.2.zip`.

On devices with root access and a writable `/system` partition, install the supplied files at:

```text
/system/bin/htop
/system/etc/terminfo/x/xterm
/system/etc/terminfo/x/xterm-256color
```

Use these permissions:

| File | Permissions |
| --- | --- |
| `/system/bin/htop` | `0755` |
| Terminfo files | `0644` |

Back up existing files before replacing them. **Manual installation modifies `/system` directly** and is not removed when the Magisk module is uninstalled.

Alternatively, the executable can be run from a writable location, such as `/data/local/tmp`, if `TERMINFO` points to the directory containing the supplied terminfo definitions.

## Usage over SSH

In most SSH terminal sessions, simply run:

```sh
htop
```

For a session using `xterm-256color`, the included terminfo entry should be available automatically when installed in `/system/etc/terminfo`.

If your SSH environment does not provide Android's standard environment variables, set them manually:

```sh
export ANDROID_ROOT=/system
export ANDROID_DATA=/data
export TERMINFO=/system/etc/terminfo
export TERM=xterm-256color
htop
```

## Troubleshooting

### `Error opening terminal: xterm-256color`

Check that the terminfo entry is present:

```sh
ls -l /system/etc/terminfo/x/xterm-256color
```

Then try:

```sh
export TERMINFO=/system/etc/terminfo
export TERM=xterm-256color
htop
```

If necessary, use `TERM=xterm` with the supplied basic xterm definition.

### `ANDROID_DATA not set` or `aNDROID_ROOT not set`

These messages can occur in minimal SSH or root shells. Set:

```sh
export ANDROID_ROOT=/system
export ANDROID_DATA=/data
```

## License and source code

- **htop 2.0.2:** GNU General Public License, version 2 or later (**GPL-2.0-or-later**).
- **ncurses 6.0:** its original permissive upstream license.

This repository distributes modified **binary builds** of GPL-licensed software. Redistributors must comply with the applicable source-code distribution obligations, including making the **corresponding source code and modifications** available as required by the GPL. Linking to the latest upstream source alone may not satisfy these obligations.

Upstream projects:

- [htop](https://github.com/htop-dev/htop)
- [ncurses](https://invisible-island.net/ncurses/)

## Disclaimer

This is an **unofficial community build**, not an official release from the htop, ncurses, or Magisk projects. Use caution when modifying system files. The Magisk package is recommended where supported.
