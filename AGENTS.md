# ZMK Eyelash Corne Keymap

## Key Position Reference

- **Always** reference `KEY_POSITIONS.svg` when the user gives key position numbers. This is the canonical position map (48 keys: 0-47).
- **Always** reference `keymap-drawer/eyelash_corne.svg` to see current keymap bindings visually.

## Keymap Drawer (MANDATORY)

**Before every commit that touches the keymap, you MUST regenerate the keymap drawings locally:**

```sh
keymap -c keymap_drawer.config.yaml parse -z config/eyelash_corne.keymap > keymap-drawer/eyelash_corne.yaml
keymap -c keymap_drawer.config.yaml draw keymap-drawer/eyelash_corne.yaml > keymap-drawer/eyelash_corne.svg
```

Add any new custom bindings to `keymap_drawer.config.yaml` → `raw_binding_map` so they render with proper labels/icons.

**Never commit and push keymap changes without rerendering first.**

## Layer Architecture

| Index | Name      | Purpose                    |
| ----- | --------- | -------------------------- |
| 0     | BASE      | Default QWERTY             |
| 1     | NUMBER    | Numpad + nav               |
| 2     | FN        | Function keys + BT         |
| 3     | MOUSELESS | Vimium-style nav (toggled) |
| 4     | SYMBOLS   | Symbol layer               |

## BASE Key 47 Behavior

`&mouse_lt_enter 0 0`:

- Single tap → MOUSELESS (via `mouse_lt_enter` → tap → `mouse_tog_enter`)
- Hold → FN

## Encoder

EC11 encoder on left half. `steps = <15>`, board default `triggers-per-rotation = <60>`.
Scroll encoder uses `&msc` with `tap-ms = <30>`.
