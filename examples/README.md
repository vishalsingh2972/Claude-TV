# Claude TV: device examples

These grouped, commented package lists are inherited from the original project's device contributions. They are firmware-specific starting points, not universal safety guarantees. Review every package and comment out anything you use or have not identified.

Install and verify Claude TV before applying a list that disables stock HOME. Save a baseline and an exact list of newly disabled packages. See the [installation and restoration guide](../README.md).

| Folder | TV | OS | Notes |
|---|---|---|---|
| [xiaomi-mi-tv-4a](xiaomi-mi-tv-4a/debloat.txt) | Xiaomi Mi TV 4A (MiTV-MOOQ1) | Android TV 10 | PatchWall, Mi analytics, SmartShare, MiLink |
| [tcl-g10-google-tv](tcl-g10-google-tv/debloat.txt) | TCL 55" 4K Google TV, 2023 (G10, G10_4K_US_NF) | Google TV 12 | TCL TV+, TCL Live, store demo, smartlink installer |
| [xiaomi-mitv-axso0](xiaomi-mitv-axso0/debloat.txt) | Xiaomi MiTV-AXSO0 (dangal) | Android TV 9 | PatchWall, Mi analytics, MiLink; voice, Chromecast, unused streaming apps off by choice |
| [sanyo-atv-r2](sanyo-atv-r2/debloat.txt) | SANYO ATV R2 (SCBC, Realtek RTD285O) | Android TV 9 | S-Channels, E-SHOW demo, Vewd, TV Booster, SANYO OTA |
| [oneplus-tv-dubai](oneplus-tv-dubai/debloat.txt) | OnePlus TV (D00, Dubai) | Google TV 14 | HeyTap/OnePlus extras, OnePlus Connect, Smart Home |
| [mecool-km7plus](mecool-km7plus/debloat.txt) | Mecool KM7 Plus box (HP4623gb-Mecool, Amlogic S905Y4) | Google TV 14 | setupwraith Recovery HOME, Play Auto Installs, factory test; voice, Chromecast off by choice |

To add your TV, see [CONTRIBUTING.md](../CONTRIBUTING.md). The quickest way to get a list is to
run the prompt in [PROMPT.md](../PROMPT.md) and copy the packages your assistant disabled.
