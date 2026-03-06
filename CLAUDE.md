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

| Index | Name      | Purpose                                                            |
| ----- | --------- | ------------------------------------------------------------------ |
| 0     | BASE      | Default QWERTY                                                     |
| 1     | NUMBER    | Numpad + nav                                                       |
| 2     | FN        | Function keys + BT                                                 |
| 3     | MOUSELESS | Vimium-style nav (toggled)                                         |
| 4     | SYMBOLS   | Symbol layer                                                       |
| 5     | IPAD      | Mouse/scroll for iPad (effective 1200 cursor speed, default)       |
| 6     | IPAD_600  | Speed overlay — empty layer, activates 600 effective cursor speed  |
| 7     | IPAD_2400 | Speed overlay — empty layer, activates 2400 effective cursor speed |
| 8     | IPAD_4800 | Speed overlay — empty layer, activates 4800 effective cursor speed |

## IPAD Speed Layer Architecture

Cursor speed is controlled via **per-layer input processor overrides** on `&mmv_input_listener`, NOT by duplicating cursor key bindings. The speed overlay layers (6-8) are completely empty (all `&trans`) — they exist solely as "tags" for the input processor system.

**How it works:**

- `ZMK_POINTING_DEFAULT_MOVE_VAL = 1200`
- `&mmv_input_listener` has per-layer `zip_xy_scaler` overrides
- Speed layers are checked first (higher priority), IPAD default last
- Entering IPAD via double-tap on BASE key 47 → `&to 5` → only layer 5 active → 1200 effective
- Keys 14-17 on IPAD layer switch speeds via macros (`&to 5` + `&tog N`)

**Scaler math:**
| Layer | Scaler | Calculation | Effective |
|-------|--------|-------------|-----------|
| IPAD (5) | `1 1` | 1200 × 1 | 1200 |
| IPAD_600 (6) | `1 2` | 1200 × 0.5 | 600 |
| IPAD_2400 (7) | `2 1` | 1200 × 2 | 2400 |
| IPAD_4800 (8) | `4 1` | 1200 × 4 | 4800 |

**When modifying the IPAD layer layout, you only need to edit layer 5.** Speed layers 6-8 are all `&trans` and don't need changes unless adding/removing speed tiers.

## BASE Key 47 Behavior

`td_mouse_ipad` (tap-dance wrapping hold-tap):

- Single tap → MOUSELESS (via `mouse_lt_enter` → tap → `mouse_tog_enter`)
- Double tap → IPAD (`&to 5`, effective 1200 cursor speed)
- Hold → FN (250ms td + 200ms ht = ~450ms total delay)

## Encoder

EC11 encoder on left half. `steps = <15>`, board default `triggers-per-rotation = <60>`.
Scroll encoder uses `&msc` with `tap-ms = <30>`.
