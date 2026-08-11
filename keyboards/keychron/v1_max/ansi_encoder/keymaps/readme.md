# Custom Keymaps for Keychron V1 Max (ANSI Encoder)

## Keymaps

### `glob`

Replaces the Fn key position (between Right Cmd and Right Ctrl on the Mac layer) with a **dual-function Globe/Fn key** (`LT_GLOB`):

- **Tap** — sends `AC_NEXT_KEYBOARD_LAYOUT_SELECT` (Globe key), which cycles the input source/language on macOS.
- **Hold** — activates the `MAC_FN` layer for the duration of the hold, giving access to F-keys, RGB, Bluetooth, etc.

The tap-or-hold decision is based on `TAPPING_TERM` (default 200ms). The Mac base layer also moves media/brightness controls to the F-row (F1=Brightness Down, F2=Brightness Up, etc.) and puts F1–F12 on the FN layer.

VIA is **not** enabled. Only `KEYBOARD_SHARED_EP` is set in `rules.mk`.

### `glob_ctrl`

Uses a **dedicated Globe key** (`KC_GLOB`) that replaces Right Ctrl on the Mac layer:

- **Press** — always sends `AC_NEXT_KEYBOARD_LAYOUT_SELECT` (Globe key). No hold behavior.

The Fn key remains a separate, standard `MO(MAC_FN)` layer toggle. On the Windows layer, the same physical key is mapped to `KC_RCTL` (Right Control) — hence the name "glob_ctrl" (Globe on Mac, Ctrl on Windows).

The Mac base layer keeps F1–F12 on the F-row (with media/brightness on the FN layer), matching the standard Keychron layout more closely.

VIA **is** enabled via `rules.mk`.

### Key Differences

| Feature | `glob` | `glob_ctrl` |
|---|---|---|
| Globe key position | Fn key (tap/hold) | Dedicated key (replaces Right Ctrl on Mac) |
| Fn layer access | Hold the Globe key | Separate Fn key |
| Mac F-row default | Media/Brightness | F1–F12 |
| Right Ctrl on Mac | Separate key | Not available (replaced by Globe) |
| VIA support | No | Yes |

## rules.mk

Each keymap has a `rules.mk` that enables additional QMK features at compile time.

### `glob/rules.mk`

```makefile
KEYBOARD_SHARED_EP = yes
```

- **`KEYBOARD_SHARED_EP = yes`** — Combines the keyboard HID interface with other HID interfaces (mouse, extra keys) into a single shared USB endpoint. Required by Keychron wireless keyboards to work correctly over Bluetooth/2.4GHz, since wireless has a limited number of endpoints.

### `glob_ctrl/rules.mk`

```makefile
VIA_ENABLE = yes
KEYBOARD_SHARED_EP = yes
```

- **`VIA_ENABLE = yes`** — Enables [VIA](https://www.caniusevia.com/) support, allowing you to remap keys, configure RGB, and change layers in real time through the VIA app without recompiling firmware.
- **`KEYBOARD_SHARED_EP = yes`** — Same as above.

## How to Compile and Flash

1. Switch to USB mode, make sure the keyboard is disconnected from the computer.

2. Hold down the `Esc` key and plug the keyboard into the computer with the USB cable.

```bash
# glob keymap
qmk flash -kb keychron/v1_max/ansi_encoder -km glob

# glob_ctrl keymap
qmk flash -kb keychron/v1_max/ansi_encoder -km glob_ctrl
```

To compile without flashing:

```bash
qmk compile -kb keychron/v1_max/ansi_encoder -km glob
```
