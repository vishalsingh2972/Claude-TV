<div align="center">

# Claude TV

### Your TV. Your Rules. No Clutter.

A quiet home screen for Android TV and Google TV.<br>
Your favourite apps, thoughtful remote navigation, and tools to disable unwanted packages.

[![Platform](https://img.shields.io/badge/Platform-Android_TV_%2F_Google_TV-3DDC84?style=flat-square)](#supported-devices)
[![Minimum SDK](https://img.shields.io/badge/Minimum_SDK-26-CC785C?style=flat-square)](launcher/app/build.gradle)
[![Java](https://img.shields.io/badge/Java-17-555555?style=flat-square)](launcher/app/build.gradle)
[![License](https://img.shields.io/badge/License-MIT-blue?style=flat-square)](LICENSE)

[Get started](#quick-start) · [Customize](#customization) · [Restore](#debloating-and-restoration) · [Contribute](CONTRIBUTING.md)

![Stock Google TV home beside the clean launcher retained by Claude TV](docs/stock-vs-new-side.jpg)

**Less noise. More of what you came to watch.**

Customized and maintained by **Vishal**.<br>
Not affiliated with, endorsed by, or sponsored by Anthropic.

</div>

---

## A home screen that gets out of the way

Claude TV combines a native Java launcher with Python ADB tools and editable, model-specific debloat lists. The launcher has no advertising feeds or Internet permission. Debloating disables selected packages rather than uninstalling them; it does not remove ads inside streaming apps.

| Feature | What you get |
|---|---|
| Favourite apps first | A configurable dock with four slots by default; other TV apps follow alphabetically. |
| A focused interface | Black background, app-coloured ambient glow, banner artwork and an animated glass-style focus highlight. |
| Remote-first navigation | Row-by-row D-pad movement, a grid revealed with Down, and focus restored on return from an app while the process survives. |
| Everyday controls | Inputs, Wi-Fi status and system Settings; long-press an app tile for its Android app settings. |
| TV inputs | HDMI and other passthrough inputs exposed by the device, plus a configurable Live TV app. Availability depends on firmware. |
| Clock and greeting | Time, date, time-of-day greeting and the bundled Outfit typeface. |
| Optional screensaver | A drifting clock and slowly changing ambient glow using Android's dream service. |
| Local tooling | Shell, batch operations, file transfers and package disable/restore scripts for Bash and PowerShell. |

### A closer look

| Before · stock home | After · the retained launcher interface |
|:---:|:---:|
| ![Stock home with recommendations](docs/stock-home.jpg) | ![Clean dock, clock and controls](docs/new-home.jpg) |

<details>
<summary><strong>Explore navigation, the screensaver and another comparison</strong></summary>

| App grid focus | Back to the dock |
|:---:|:---:|
| ![FanCode tile focused](docs/focus-1-fancode.png) | ![Netflix focused after moving up](docs/focus-2-up-netflix.png) |

![Focus restored after returning from an app](docs/focus-3-back-from-app.png)

![Clock screensaver](docs/clock-screensaver.png)

![Additional before-and-after comparison](docs/stock-vs-new.jpg)

</details>

These are the original project's included screenshots, illustrating the interface preserved by Claude TV. They are not newly captured device tests. App availability and input labels vary by TV.

## Contents

[How it works](#how-it-works) · [Devices](#supported-devices) · [Requirements](#requirements) · [Quick start](#quick-start) · [Installation](#installation) · [Debloat and restore](#debloating-and-restoration) · [Customization](#customization) · [Architecture](#architecture) · [Troubleshooting](#troubleshooting) · [FAQ](#faq) · [Contributing](#contributing) · [Credits](#credits-and-license)

## How it works

1. **Inspect and record.** Connect through authenticated ADB over TCP; save package lists, firmware details, settings and the original HOME component.
2. **Configure and build.** Set dock preferences and the Live TV package in XML, then build the existing application.
3. **Install and verify.** Select Claude TV as HOME and confirm navigation and inputs before disabling the original launcher.
4. **Debloat selectively.** Review a device-specific list and disable unwanted packages with `pm disable-user --user 0`.
5. **Restore when needed.** Re-enable packages using that same list and select the original launcher again.

The scripts do not create backups automatically or decide whether a package is safe to disable. Restoration is not a full device snapshot rollback. Keep your baseline and the exact list you applied.

## Supported devices

The launcher requires **Android 8.0 / API 26 or newer**. This project targets **Android TV and Google TV** devices that permit ADB access, sideloading and replacement of the HOME activity. Meeting the minimum SDK alone does not guarantee compatibility.

The included community examples record these devices; they are starting points, not certification of every model or firmware:

| Device | Recorded OS | Package list |
|---|---|---|
| Xiaomi Mi TV 4A · MiTV-MOOQ1 | Android TV 10 | [View](examples/xiaomi-mi-tv-4a/debloat.txt) |
| Xiaomi MiTV-AXSO0 · dangal | Android TV 9 | [View](examples/xiaomi-mitv-axso0/debloat.txt) |
| TCL 55-inch G10 · 2023 | Google TV / Android 12 | [View](examples/tcl-g10-google-tv/debloat.txt) |
| SANYO ATV R2 | Android TV 9 | [View](examples/sanyo-atv-r2/debloat.txt) |
| OnePlus TV D00 · Dubai | Google TV / Android 14 | [View](examples/oneplus-tv-dubai/debloat.txt) |
| Mecool KM7 Plus · HP4623gb-Mecool | Google TV / Android 14 | [View](examples/mecool-km7plus/debloat.txt) |

**Not supported by this project:** LG webOS, Samsung Tizen and Amazon Fire TV. Other Android TV models need their own package review and device testing. Firmware can restrict HOME replacement, input switching or debugging.

## Requirements

| Requirement | Details |
|---|---|
| TV | Compatible Android TV / Google TV, remote, developer options and an accessible ADB TCP endpoint. |
| Computer | Windows with PowerShell, or macOS/Linux with Bash; network access to the TV. |
| Python | Python 3 with `venv` and `pip`; [dependencies](tools/requirements.txt): `adb-shell`, `rsa`, `cryptography`. |
| Java | JDK 17 for the project's Java 17 source configuration. |
| Android SDK | Platform 34 and build-tools 34.0.0; set `ANDROID_HOME` or `launcher/local.properties`. |
| Build tooling | Included Gradle wrapper: Gradle 8.9, Android Gradle Plugin 8.5.2. Initial builds need dependency repository access unless cached. |

No root or Anthropic account is required. A coding assistant is optional. The Python tools do not require the stock `adb` executable. They read `TV_HOST`, with `TV_PORT` defaulting to `5555`; they do not implement pairing-code Wireless debugging.

## Quick start

Start in the **existing project root**. No second checkout or new application is needed.

1. On the TV, enable developer options by selecting **Build** seven times, then enable **USB debugging** and Network/ADB debugging if offered. Menu names vary. Confirm your firmware exposes ADB over TCP.
2. Follow [Installation](#installation) to prepare Python, connect, save a baseline and build.
3. Configure your [dock and Live TV package](#customization), install, and verify Home, apps, inputs and Settings.
4. Optionally apply a reviewed [debloat list](#debloating-and-restoration). Keep its restore command handy.

**Prefer assistant-guided setup?** Open this local folder in a coding assistant with shell access and use [PROMPT.md](PROMPT.md). It directs the assistant to reuse this project, inspect your model, ask about preferences and record changes.

## Installation

These commands are for a device installation you choose to perform. Replace `192.168.1.50` with your TV's address. Run project-relative commands from the repository root unless explicitly changing directory.

### 1. Prepare Python and connect

<details open>
<summary><strong>Windows · PowerShell</strong></summary>

```powershell
python -m venv tools/venv
./tools/venv/Scripts/python.exe -m pip install -r tools/requirements.txt
$env:TV_HOST = '192.168.1.50'
./tools/venv/Scripts/python.exe -I tools/tv.py --probe
```

</details>

<details>
<summary><strong>macOS / Linux · Bash</strong></summary>

```bash
python3 -m venv tools/venv
tools/venv/bin/python -m pip install -r tools/requirements.txt
export TV_HOST=192.168.1.50
tools/venv/bin/python -I tools/tv.py --probe
```

</details>

Accept the debugging authorization prompt on the TV. The client creates its private key pair in `tools/adbkey/`, which is ignored by Git. Keep it local.

### 2. Save a baseline

Use a fresh backup directory for each installation. `backup/` is ignored by Git. These dumps are reference records, not backups of app data.

<details>
<summary><strong>Windows backup commands</strong></summary>

```powershell
New-Item -ItemType Directory -Force backup | Out-Null
$py = './tools/venv/Scripts/python.exe'
& $py -I tools/tv.py 'pm list packages' > backup/packages.txt
& $py -I tools/tv.py 'pm list packages -d' > backup/disabled-before.txt
& $py -I tools/tv.py 'getprop' > backup/getprop.txt
& $py -I tools/tv.py 'settings list system' > backup/settings-system.txt
& $py -I tools/tv.py 'settings list secure' > backup/settings-secure.txt
& $py -I tools/tv.py 'settings list global' > backup/settings-global.txt
& $py -I tools/tv.py 'cmd package resolve-activity --brief -a android.intent.action.MAIN -c android.intent.category.HOME' > backup/home-before.txt
```

</details>

<details>
<summary><strong>macOS / Linux backup commands</strong></summary>

```bash
mkdir -p backup
tools/venv/bin/python -I tools/tv.py 'pm list packages' > backup/packages.txt
tools/venv/bin/python -I tools/tv.py 'pm list packages -d' > backup/disabled-before.txt
tools/venv/bin/python -I tools/tv.py 'getprop' > backup/getprop.txt
tools/venv/bin/python -I tools/tv.py 'settings list system' > backup/settings-system.txt
tools/venv/bin/python -I tools/tv.py 'settings list secure' > backup/settings-secure.txt
tools/venv/bin/python -I tools/tv.py 'settings list global' > backup/settings-global.txt
tools/venv/bin/python -I tools/tv.py 'cmd package resolve-activity --brief -a android.intent.action.MAIN -c android.intent.category.HOME' > backup/home-before.txt
```

</details>

Read `home-before.txt` and record the stock launcher package and full `package/activity` component. The scripts default to `com.google.android.tvlauncher`; the included TCL example instead uses `com.google.android.apps.tv.launcherx`.

### 3. Configure and build locally

Edit [config.xml](launcher/app/src/main/res/values/config.xml) using [Customization](#customization). Set `JAVA_HOME` to your JDK and `ANDROID_HOME` to your SDK. If needed, install components through your existing SDK manager:

```text
sdkmanager "platforms;android-34" "build-tools;34.0.0"
```

These build-only commands do not contact a TV. Configure the SDK through `ANDROID_HOME` or Android Studio's `local.properties` first.

```powershell
# Windows
Push-Location launcher
./gradlew.bat assembleDebug lint --console=plain
Pop-Location
```

```bash
# macOS / Linux
(cd launcher && ./gradlew assembleDebug lint --console=plain)
```

The APK is `launcher/app/build/outputs/apk/debug/app-debug.apk`. `assembleRelease` is also available, but release signing and minification are not configured.

### 4. Install and select Home

For a first installation, use this explicit sequence to inspect each response. Keep `TV_HOST` set. First read the existing application ID from the build configuration in your current shell:

```powershell
# PowerShell
$appId = ./tools/venv/Scripts/python.exe -c "from pathlib import Path; print(Path('launcher/app/build.gradle').read_text().split('applicationId ')[1].split(chr(39))[1])"
```

```bash
# Bash
appId=$(tools/venv/bin/python -c "from pathlib import Path; print(Path('launcher/app/build.gradle').read_text().split('applicationId ')[1].split(chr(39))[1])")
```

For the following Python commands, **in Bash substitute `tools/venv/bin/python` for `./tools/venv/Scripts/python.exe`**; the remaining arguments are identical. Keep the double quotes shown so your shell expands `$appId`.

```powershell
./tools/venv/Scripts/python.exe -I tools/b64push.py launcher/app/build/outputs/apk/debug/app-debug.apk /data/local/tmp/tvhome.apk
./tools/venv/Scripts/python.exe -I tools/tv.py 'pm install -r /data/local/tmp/tvhome.apk'
```

Continue only after upload reports **MATCH** and installation reports **Success**:

```powershell
./tools/venv/Scripts/python.exe -I tools/tv.py "pm grant $appId android.permission.ACCESS_FINE_LOCATION"
./tools/venv/Scripts/python.exe -I tools/tv.py "cmd package set-home-activity --user 0 $appId/.MainActivity"
```

Location permission supports Wi-Fi network-name display; firmware and location settings can still affect it. Press Home on the remote and verify the dock, grid, Settings, inputs and return-from-app focus. Only after the new HOME works, disable stock HOME using the package you recorded. This example uses the default stock package:

```powershell
./tools/venv/Scripts/python.exe -I tools/tv.py 'pm disable-user --user 0 com.google.android.tvlauncher'
./tools/venv/Scripts/python.exe -I tools/tv.py 'rm -f /data/local/tmp/tvhome.apk'
```

<details>
<summary><strong>Convenience build-and-install scripts</strong></summary>

These scripts build, upload, install, select HOME and disable the supplied stock launcher. They modify the TV; they are not build-only checks.

```powershell
powershell -ExecutionPolicy Bypass -File launcher/build-and-install.ps1 -TvHost 192.168.1.50 -StockLauncher com.google.android.tvlauncher
```

```bash
TV_HOST=192.168.1.50 STOCK_LAUNCHER=com.google.android.tvlauncher bash launcher/build-and-install.sh
```

For the included TCL model, substitute `com.google.android.apps.tv.launcherx`.

The Windows script checks upload/install failure and HOME output containing `Error` before disabling stock HOME. The Bash script batches device commands without validating each response; use the explicit installation sequence when you need to inspect results before disabling stock HOME.

Windows can discover Android Studio's bundled JDK and default SDK location. Bash defaults target macOS; on Linux or Git Bash, set `JAVA_HOME` and `ANDROID_HOME` explicitly. An existing `launcher/local.properties` is not overwritten. Invoke PowerShell in a separate process as shown because the installer changes its working directory.

</details>

## Debloating and restoration

Copy the closest [example](examples/README.md) to `backup/debloat-reviewed.txt` and edit that copy. Comment out packages you use, packages already disabled before your changes, and anything you have not identified. Some lists disable casting, voice, updates, accessibility services or streaming apps by personal choice.

Preserve TV/input services, remote and Bluetooth services, keyboard, Play Store and Play Services. Several lists include stock HOME: **install and verify Claude TV first**. Keep the exact reviewed list for restoration.

```powershell
# Windows: apply
powershell -ExecutionPolicy Bypass -File tools/disable.ps1 -TvHost 192.168.1.50 -List backup/debloat-reviewed.txt

# Restore: substitute your recorded stock package
powershell -ExecutionPolicy Bypass -File tools/restore.ps1 -TvHost 192.168.1.50 -List backup/debloat-reviewed.txt -Stock com.google.android.tvlauncher
```

```bash
# macOS / Linux: TV_HOST must still be exported
bash tools/disable.sh backup/debloat-reviewed.txt

# Restore: substitute your recorded stock package
bash tools/restore.sh backup/debloat-reviewed.txt com.google.android.tvlauncher
```

Both restore scripts re-enable listed packages and stock HOME, attempt to select stock HOME, and set all three global animation scales to `1.0`. They leave Claude TV installed. They do **not** restore previous settings, screensaver selection, permissions or disabled states. Inspect printed results: the batch client does not turn every Android command error into a failing exit code.

Bash restore resolves the full HOME component. PowerShell passes the package alone, which may fail on firmware requiring a full component, including some Android 9 devices. Use the exact component saved in `backup/home-before.txt` in that case:

```powershell
# Replace the placeholder with the recorded package/activity component.
./tools/venv/Scripts/python.exe -I tools/tv.py 'cmd package set-home-activity --user 0 STOCK_PACKAGE/STOCK_ACTIVITY'

# Re-enable a single app: replace PACKAGE_NAME with its actual identifier.
./tools/venv/Scripts/python.exe -I tools/tv.py 'pm enable PACKAGE_NAME'
```

In Bash, substitute `tools/venv/bin/python` for the Windows Python path. Nothing is flashed or uninstalled, but disabling the wrong package can interrupt TV features. Keep your baseline available.

## Customization

Edit [launcher/app/src/main/res/values/config.xml](launcher/app/src/main/res/values/config.xml), then rebuild and reinstall with the same signing key.

| Setting | Behaviour |
|---|---|
| `dock_apps` | Package identifiers in priority order. Defaults: Netflix, Prime Video, YouTube, Hotstar. Missing apps are skipped; other visible apps can fill remaining dock slots. |
| `hidden_apps` | Hides tiles only; does not disable or uninstall packages. |
| `live_tv_package` | TV/tuner app for Inputs. Default: `com.mediatek.wwtv.tvcenter`. Empty omits the Live TV entry. |
| `columns` | Grid columns and dock slots; default `4`. Test layout changes at your TV's resolution. |

The grid discovers activities with `CATEGORY_LEANBACK_LAUNCHER`; phone-only apps may not appear. Find identifiers using `tv.py 'pm list packages'` and verify TV launch activities before adding them.

English/default and German strings live in [values/strings.xml](launcher/app/src/main/res/values/strings.xml) and [values-de/strings.xml](launcher/app/src/main/res/values-de/strings.xml). There is no on-TV dock editor or wallpaper picker. Visual customization requires resource/code changes; the existing design uses black and generated ambient colour.

<details>
<summary><strong>Enable the optional Claude TV Clock screensaver</strong></summary>

Choose **Claude TV Clock** (German: **Claude TV Uhr**) in the TV's screensaver settings if exposed, or use your configured Python client. First set `$appId` in your current shell using the matching command in [Install and select Home](#4-install-and-select-home).

```powershell
./tools/venv/Scripts/python.exe -I tools/tv.py "settings put secure screensaver_components $appId/.ClockDream"
./tools/venv/Scripts/python.exe -I tools/tv.py 'settings put secure screensaver_enabled 1'
```

Use `tools/venv/bin/python` in Bash. The TV controls idle timing. The clock moves once a minute; this is not a guarantee against display burn-in. Undo through Settings or restore original secure-settings values from your baseline. Package restore scripts do not reset screensaver selection.

</details>

## Architecture

```text
Claude-TV/
|-- README.md                  Project guide
|-- PROMPT.md                  Assistant-guided setup
|-- CONTRIBUTING.md            Contribution and validation guide
|-- LICENSE                    Original MIT notice and font attribution
|-- docs/                      Original screenshots and comparisons
|-- examples/                  Six model-specific package lists
|-- tools/
|   |-- tv.py                  ADB TCP shell, batch, push and pull
|   |-- b64push.py             Base64 upload with MD5 comparison
|   |-- disable.{sh,ps1}       Disable listed packages for user 0
|   |-- restore.{sh,ps1}       Re-enable packages and select stock HOME
|   `-- requirements.txt       Python dependencies
`-- launcher/
    |-- build-and-install.*    Device installers
    |-- gradlew / gradlew.bat  Gradle wrapper
    |-- FONT-LICENSE-OFL.txt    Outfit license
    `-- app/src/main/
        |-- AndroidManifest.xml
        |-- java/              Existing application package directory
        |   |-- MainActivity.java    Home, navigation, status and inputs
        |   |-- AppAdapter.java      Discovery, ordering and app tiles
        |   |-- GlassView.java       Focus highlight drawing
        |   |-- DockDecoration.java  Dock band and spacing
        |   `-- ClockDream.java      Optional screensaver
        `-- res/               Layouts, strings, config and artwork
```

The launcher uses Java 17, RecyclerView 1.3.2 and DynamicAnimation 1.0.0. SDK settings remain `minSdk 26`, `compileSdk 34`, `targetSdk 29`. The application ID and namespace in [app/build.gradle](launcher/app/build.gradle) are unchanged, and the internal Gradle name remains `tvhome`, preserving installation commands and component paths. The displayed application name is **Claude TV**.

<details>
<summary><strong>Tool reference</strong></summary>

With `TV_HOST` set, use the appropriate virtual-environment Python executable:

| Invocation after `python -I` | Purpose |
|---|---|
| `tools/tv.py --probe` | Model, device, Android release/API, firmware and manufacturer. |
| `tools/tv.py 'pm list packages'` | One remote shell command. |
| `tools/tv.py --batch` | One stdin command per line, one connection, separate shell calls. |
| `tools/tv.py --push LOCAL REMOTE` | Transfer a small file to the TV. |
| `tools/tv.py --pull REMOTE LOCAL` | Retrieve a file. |
| `tools/b64push.py LOCAL REMOTE` | Upload larger files in chunks and compare MD5 checksums. |

Use simple remote paths without spaces or shell metacharacters: the upload helper interpolates them into shell commands. Run package-manager operations separately rather than chaining multiple `pm` commands.

</details>

## Troubleshooting

| Symptom | Check or action |
|---|---|
| Connection refused or timeout | Check `TV_HOST`, port, debugging, reachability and authorization. USB debugging does not expose network ADB on every firmware. |
| Pairing-code Wireless debugging only | The bundled client has no pairing implementation; it requires a compatible authenticated TCP endpoint. |
| Stock adb fails on macOS | Try the bundled Python client; check local network permissions and reachability if it also fails. |
| Java or SDK not found | Set `JAVA_HOME` and `ANDROID_HOME`; check for stale SDK paths in `launcher/local.properties`. Use JDK 17 and the required SDK components. |
| Gradle cannot resolve dependencies | First builds need repository access. Offline builds need a cached distribution and dependencies. |
| APK signature mismatch | The installed app must use the same signing key. Keeping the package ID means a differently signed build cannot update it in place. |
| Home opens stock or a factory app | Verify successful installation and repeat the HOME selection command in [Install and select Home](#4-install-and-select-home). Do not disable stock HOME before verifying. |
| Windows restore cannot select HOME | Use the full recorded component as described in [restoration](#debloating-and-restoration). |
| Wi-Fi name missing | Check location permission and device location settings; the UI falls back to connection status. |
| Missing app tile | Check `hidden_apps`, enabled state and whether the app exposes a Leanback launcher activity. |
| Missing HDMI or Live TV | Retain manufacturer input services and configure `live_tv_package`; inputs come from the TV framework. |
| Large upload hangs | Use `b64push.py` and require its checksum to match. |
| Empty TCL screenshot on HDMI/Live TV | Return to Home before capture; this is recorded in the TCL example. |
| Mi TV 4A keyboard missing | Inspect the current IME. If Gboard is installed, the example's fix is `ime set com.google.android.inputmethod.latin/com.android.inputmethod.latin.LatinIME` through `tv.py`. |
| Git Bash rewrites Android paths | Supplied scripts set `MSYS_NO_PATHCONV=1`; set it for manual commands too. |

## FAQ

<details>
<summary><strong>Is this an Anthropic product or an AI assistant on my TV?</strong></summary>

No. Claude TV is an independent derivative of the included open-source launcher and tools. There is no Claude API integration or AI chat feature. A coding assistant can optionally help install it.

</details>

<details>
<summary><strong>Does it remove all ads or telemetry?</strong></summary>

The replacement home has no advertising feed. Reviewed lists can disable particular services, but this is not a network-wide ad blocker or a guarantee that all telemetry is removed. Streaming apps retain their content, subscriptions and ads.

</details>

<details>
<summary><strong>Can I install only the launcher?</strong></summary>

Yes. Debloating is optional. The convenience installers also disable the supplied stock launcher; use the explicit installation sequence to control that separately.

</details>

<details>
<summary><strong>Is disabling packages reversible?</strong></summary>

Packages remain installed and can be re-enabled. Recovery depends on retaining access and knowing what changed. The supplied restore scripts do not reconstruct all previous settings or disabled states.

</details>

<details>
<summary><strong>What happens to casting, voice and updates?</strong></summary>

That depends on your reviewed list. Some examples disable those services; retain what you use and re-enable update services when needed. Firmware changes may alter package names or HOME behaviour and require a fresh review.

</details>

<details>
<summary><strong>Must debugging stay enabled?</strong></summary>

No. The installed launcher runs without ADB. Re-enable debugging when you need maintenance or restoration.

</details>

<details>
<summary><strong>Where is the downloadable APK?</strong></summary>

This guide builds the included source and does not depend on a hosted APK or release page. Keep your signing key consistent when rebuilding for an existing installation.

</details>

## Contributing

Device notes, careful bug reports, localization and documentation improvements are welcome. Read [CONTRIBUTING.md](CONTRIBUTING.md) for scope, validation and the example format. Never publish debugging keys or unredacted backups.

## Credits and license

Claude TV is an independent derivative of an open-source project. See [upstream attribution](PROMPT.md) and the [original copyright notice](LICENSE). Credit for the original launcher, tools, workflow and included screenshots belongs to the upstream project and its contributors. Model-specific observations remain in [examples/](examples/README.md).

The project uses the [MIT License](LICENSE), with the original copyright notice preserved in that file. The bundled Outfit font is credited to The Outfit Project Authors under the [SIL Open Font License 1.1](launcher/FONT-LICENSE-OFL.txt). Gradle wrapper scripts retain their Apache-2.0 notices. Original license files and notices are preserved.

Claude TV is **not officially affiliated with Anthropic**. Product names and app artwork in screenshots belong to their respective owners.

---

<div align="center">

**Claude TV**<br>
Your TV. Your Rules. No Clutter.

[Get started](#quick-start) · [Make it yours](#customization) · [Help improve it](CONTRIBUTING.md)

</div>
