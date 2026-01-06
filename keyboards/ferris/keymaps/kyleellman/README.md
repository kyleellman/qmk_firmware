## Pre-flashing sea-picros

This is output from a chat GPT session...

---

Right now you’re building the *default* Ferris Sweep target, which assumes an AVR Pro Micro (ATmega32u4) — that’s why you only get a `.hex`/`.elf`. To get a **`.uf2` for Sea-Picro (RP2040)** you need to use QMK’s **converter** when compiling.

### 1) Compile a UF2 for RP2040 (Sea-Picro)

From your `qmk_firmware` folder:

```bash
qmk compile -kb ferris/sweep -km kyleellman -e CONVERT_TO=rp2040_ce
```

This should produce something like:

* `ferris_sweep_kyleellman_rp2040_ce.uf2`

Usually it lands in the **root of `qmk_firmware`** (not necessarily inside `.build`). ([QMK Firmware][1])

> Note: older guides refer to `CONVERT_TO=promicro_rp2040`; the current name is `rp2040_ce`. ([42. Keebs][2])

### 2) Flash both halves (sets left/right)

For a split, you generally want **side-specific** flashing once so handedness is stored correctly. QMK supports this for UF2 via `uf2-split-left/right`. ([QMK Firmware][3])

Unplug the TRRS cable between halves while flashing.

Flash **left**:

```bash
qmk flash -kb ferris/sweep -km kyleellman -e CONVERT_TO=rp2040_ce -bl uf2-split-left
```

Flash **right**:

```bash
qmk flash -kb ferris/sweep -km kyleellman -e CONVERT_TO=rp2040_ce -bl uf2-split-right
```

Those `uf2-split-*` options generate a side-specific UF2 and set handedness in EEPROM. ([QMK Firmware][3])

### 3) If you prefer drag-and-drop instead of `qmk flash`

Put each Sea-Picro into UF2 boot mode (RPI-RP2 drive appears), then copy the generated `.uf2` to it. ([GitHub][4])

---

If you run `qmk compile ... CONVERT_TO=rp2040_ce` and **still** don’t see a `.uf2`, paste the last ~30 lines of your compile output — the converter step will usually say what it’s doing (or why it skipped).

[1]: https://docs.qmk.fm/feature_converters?utm_source=chatgpt.com "Converters | QMK Firmware"
[2]: https://42keebs.eu/shop/parts/controllers/frood-rp2040-pro-micro-controller/?utm_source=chatgpt.com "Frood RP2040 Pro Micro Controller - 42. Keebs"
[3]: https://docs.qmk.fm/flashing?utm_source=chatgpt.com "Flashing Instructions and Bootloader Information - QMK"
[4]: https://github.com/mechboardsguides/flashing-rp2040-promicro?utm_source=chatgpt.com "mechboardsguides/flashing-rp2040-promicro - GitHub"


