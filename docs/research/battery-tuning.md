# Battery life tuning on Bazzite vs Windows (original ROG Ally)

Ticket: #7. Researched 2026-11-07. Device: original ROG Ally (RC71L, Z1 Extreme, 40 Wh battery).

## Answer

**Bottom line:** The settings that save the most battery are the same on both OSes: a lower power limit (TDP), a frame cap, a lower refresh rate and lower brightness. Choosing Bazzite over Windows changes much less than those settings do. No one has published a controlled Bazzite-vs-Windows battery test on the **original** Ally, and for **FC 26 no such comparison can exist**, because the game's anti-cheat blocks Linux and Proton.

1. **FC 26 is the deciding fact, not battery life (high confidence).** EA Javelin anti-cheat blocks Proton. GamingOnLinux lists FC 26 as "Broken", AreWeAntiCheatYet lists it as "Denied", and Steam Deck marks it "Unsupported". On Bazzite you cannot play FC 26 normally, so Windows is the only OS that can run it. ProtonDB does show a "silver" summary from 17 reports, which conflicts with this; see the FC 26 section. The game-compatibility ticket should settle it.
2. **Battery parity, with a small edge to Linux in some games (medium confidence).** The only same-device, same-power-limit observations are from The Verge, on newer Allys. The ROG Ally X sometimes drained "one, two, even three watts slower" on Bazzite, though not in every game. On the 2025 Xbox Ally there was "no battery life advantage for Bazzite", and occasionally Bazzite drew about 1 W more. My inference: expect roughly equal runtime at the same TDP and frame cap. Any gain is per game and small next to what a lower TDP buys you.
3. **Sleep drain is where Bazzite clearly wins (medium-high confidence).** On Windows, The Verge's two Xbox Ally units woke by themselves and lost 10–23% in 12 hours of sleep. On Bazzite the reporter said it "sleeps like a dream". On Windows, ASUS's own fix is Extreme Standby Mode in Armoury Crate.
4. **Bazzite changed in 2026, which affects this ticket (high confidence, verified in source).** Bazzite **removed Handheld Daemon (HHD)** on 2026-03-16 (commit `ce953e43`), first shipped in stable `44.20260429`. Handhelds now use InputPlumber, PowerStation, OpenGamepadUI and a PowerStation-aware `steamos-manager`. On a current Bazzite build, TDP and the charge limit come from **Steam's own Quick Access Menu (QAM) and Settings**, not the HHD overlay. Guides that say "set it in HHD" are out of date.

**What to do (all low-risk, no hardware mods):**
- **TDP 10–15 W.** On the Ally: Silent = 10 W, Performance = 15 W, Turbo = 25 W on battery / 30 W plugged in.
- **Cap the frame rate** to 30, 40 or 60 fps.
- **Lower the refresh rate** to match the frame cap.
- **Lower the brightness.**
- **Turn off the RGB lights** and the radios you don't need.
- **Set an 80% charge limit** for battery health.
- **On Windows only:** turn CPU Boost off and turn Extreme Standby on.

## Details

### 1. Power limit (TDP): the biggest lever

**Verified, from HHD source code. These are the Ally's firmware presets, which HHD copies from Armoury Crate** (`hhd-dev/hhd`, `src/adjustor/core/const.py`, `ALLY_DATA`, and `drivers/unified/const.py`, `ROG_ALLY`):

| Mode (Armoury Crate name) | On battery | Plugged in |
|---|---|---|
| Silent | 10 W | 10 W |
| Performance | 15 W | 15 W |
| Turbo | 25 W | 30 W |
| Custom / Manual range | 5–25 W | 5–30 W |

- **Measured (Windows):** Tom's Guide got **1 h 43 min at 15 W** on the original Ally. A 40 Wh battery at about 20–23 W total system draw lands at the same figure, so the numbers are consistent.
- **ASUS also notes** that the real draw from the battery can be higher than the mode's label. One review measured 11.5–15 W total draw in Silent mode.
- **Inference:** For a sports game like FC 26 on the 1080p panel, 10–15 W with a 30–60 fps cap is the efficient range. Above about 15 W, each extra watt buys very little fps on a Z1 Extreme, while runtime drops roughly in proportion to total draw.

**Bazzite (current stack, verified in source):**
- **Steam QAM handles TDP now.** The upstream `steamos-manager` device file for the Ally (`data/devices/rog-ally-series.toml`, board `RC71L`) defines:
  - `tdp_limit` through the `asus-armoury` firmware attribute,
  - `performance_profile` through `asus-wmi`,
  - `battery_charge_limit` (method `acpi_sb`).

  This means the Steam QAM performance panel can set the Ally's TDP, and Steam's settings can expose a battery charge limit.
- **Bazzite's version:** Bazzite ships `steamos-manager-powerstation`, and the `powerstation` service is enabled on all non-Valve handhelds (commit `b7e1045d`, 2026-09-05). OpenGamepadUI runs as an overlay (`--overlay-mode --steamos-manager`).
- **Power profiles:** On handheld hardware, the deck image *masks* `tuned` and `tuned-ppd` (`bazzite-hardware-setup`). The desktop-style "Power Profile" (powersave / balanced / performance) therefore does not apply in Gaming Mode. TDP is the control to use.
- **Not verified on a device:** the exact QAM menu labels in today's build. Check on the Ally itself.

**Older Bazzite builds with HHD:**
- HHD offered Silent / Performance / Turbo / Custom (5–30 W). Values sent from Steam's TDP slider switch HHD to Custom mode (`drivers/asus/__init__.py`).
- HHD also offered a charge limit of 70 / 80 / 85 / 90 / 95% through `charge_control_end_threshold` (`drivers/battery/battery.yml`), and "Extreme Standby Mode".
- This still applies if the owner's install is older than April 2026. It no longer applies to current Bazzite.

**Windows:**
- **Armoury Crate SE:** Operating Mode (Silent, Performance or Turbo), or Manual mode with your own wattage.
- **ASUS's official battery guide (2025-10-15) also recommends:**
  - turning CPU Boost off (Settings > Performance > Eco Assist),
  - capping fps with the game's limiter or the Command Center FPS Limit,
  - lowering brightness and turning RGB off,
  - turning on Modern Standby Assistant and Extreme Standby Mode.

### 2. Frame cap and refresh rate

- **Verified (ASUS):** A frame cap is one of ASUS's main battery tips. ASUS gives 60 fps as an example that "still feels excellent in many games".
- **Inference:** A cap saves power only when the GPU would otherwise run above it. Matching the panel's refresh rate to the cap (for example 40 fps at 40 Hz, or 60 fps at 60 Hz) avoids uneven frame pacing and cuts display and compositor work.
  - On Bazzite, gamescope's QAM has frame-limit and refresh-rate controls, which work like a Steam Deck's.
  - On Windows, use Command Center or the game's own limiter.
  - Neither OS has a clear edge here.

### 3. Brightness, RGB, radios, background services

- **Verified (ASUS):** The panel reaches 500 nits. Turning brightness down indoors and turning RGB off are both official ASUS tips.
- **Inference:** FC 26 career mode and Kick-Off work offline, so Bluetooth can go off. The owner uses a Switch Pro Controller, so keep Bluetooth on if it connects wirelessly. Wi-Fi can go off for offline play.
- **Inference, Windows background load:** Windows runs more in the background (updates, Defender scans, Armoury Crate services, launchers). This is the usual explanation for small watt differences in Linux's favour. Microsoft's Xbox full-screen experience frees about 2 GB of RAM and lowers idle draw on the Xbox Ally (Microsoft figures, mid-2025). I could not confirm whether it has officially reached the original Ally by 2026-11. It was available there early, through 25H2 previews and workarounds.

### 4. Charge limit for battery health

- **Windows (verified, ASUS):** Armoury Crate's Battery Care Mode stops charging at 80%. Instant Full-Charge mode gives 100% for 24 hours.
- **Bazzite current (verified in source):** `steamos-manager` exposes `BatteryChargeLimit1.MaxChargeLevel` for the Ally. Expect the setting in Steam's Power or Battery settings.
- **Not verified on a device:** the exact menu path.
- **Bazzite with HHD (old):** the limit choices were 70–95%.
- **Community report (anecdotal):** a limit set inside Bazzite held even with the device fully shut down. The same user said values below 70% set from KDE were ignored.
- **Recommendation:** 80% for a device that sits docked to the TV a lot. This is generic Li-ion practice, and it matches ASUS's default Battery Care value.

### 5. Measured comparisons found (and what's missing)

| Source | Device | Finding |
|---|---|---|
| The Verge, Dec 2024 | ROG Ally X | Bazzite sometimes 1–3 W lower draw at the same power level. "Not in every game." |
| The Verge, 2025-10-29 | Xbox Ally (2025) | "No battery life advantage for Bazzite." Occasionally about 1 W more. Windows sleep lost 10–23% in 12 h. |
| ETA Prime, via Windows Central | ROG Xbox Ally X | Battery "similar". Bazzite slightly better in efficiency per watt. |
| Tom's Guide | Original Ally, Windows | 1 h 43 min at 15 W. No Bazzite run. |
| justin.how blog (early 2025) | Original Ally, Bazzite only | Yakuza at Turbo: 85% to 5% in 44 min. Sleep about 1–2%/h. No Windows run. |

- **What's missing:** a controlled, same-game test of the **original Ally** on both OSes. None was found.
- **Weak source:** a windowsnews.ai claim that Bazzite doubles battery life (or gives +25–30%) has no named method. Disregard it.

### 6. FC 26 specifically

- **Verified status:** GamingOnLinux lists FC 26 (released 2025-09-26) as anti-cheat "Broken" (EA Javelin), with Steam Deck "Unsupported". That listing was last updated 2025-09-18. AreWeAntiCheatYet lists FC 24, FC 25 and FC 26 as "Denied". An EA community reply says FC 26 is not compatible with Linux.
- **Conflicting data:** ProtonDB's API shows FC 26 (app 3405690) as tier "silver", trending "gold", from 17 reports.
- **Inference:** Those reports probably rely on workarounds. One example is mods such as FC Live Editor that disable Javelin for offline modes. Those are unofficial, could carry account risk, and break with game updates.
- **EA's plans:** A March 2026 EA job listing mentions possible future Linux and Proton support, with no commitment.
- **Consequence:** A Bazzite-vs-Windows battery comparison for FC 26 is meaningless until FC 26 runs on Bazzite. If FC 26 is the target, Windows is the platform, and the Windows tuning above applies.

## Sources

Primary (code and vendor):
- HHD source, ROG Ally TDP and charge-limit settings (clone at commit 4184f03, 2026-11-01): https://github.com/hhd-dev/hhd — files `src/adjustor/core/const.py`, `src/adjustor/drivers/unified/const.py`, `src/adjustor/drivers/asus/settings.yml`, `src/adjustor/drivers/asus/__init__.py`, `src/adjustor/drivers/battery/battery.yml`
- Bazzite source (clone 2026-11-06): https://github.com/ublue-os/bazzite — commits `ce953e43` ("drop HHD", 2026-03-16), `2c689877` (InputPlumber), `426bb8ac` (PowerStation-aware SteamOS-Manager), `b7e1045d` (powerstation on all non-Valve handhelds, 2026-09-05); files `system_files/desktop/shared/usr/libexec/bazzite-hardware-setup`, `build_files/bazzite-deck/03-install-packages.sh`, `build_files/bazzite-deck/05-finalize.sh`
- steamos-manager, ROG Ally device definition and the BatteryChargeLimit1 interface: https://gitlab.steamos.cloud/holo/steamos-manager — `data/devices/rog-ally-series.toml`, `data/interfaces/com.steampowered.SteamOSManager1.xml`
- ASUS, "How to extend the battery life of your ROG Ally or ROG Xbox Ally" (2025-10-15): https://rog.asus.com/my/articles/guides/how-to-extend-the-battery-life-of-your-rog-ally/
- ProtonDB summary API, FC 26: https://www.protondb.com/api/v1/reports/summaries/3405690.json
- AreWeAntiCheatYet data: https://github.com/AreWeAntiCheatYet/AreWeAntiCheatYet

Secondary (measurements and reports):
- The Verge, "Say it with me: Windows is the problem with Windows handhelds" (2025-10-29), read via a mirror: https://www.theverge.com/games/807711/xbox-ally-sleep-fail-bazzite-fix-performance
- The Verge, Bazzite on ROG Ally X (Dec 2024), quoted in search results; not read directly: https://www.theverge.com/24329005/bazzite-asus-rog-ally-x-steam-os-editorial
- Windows Central (ETA Prime testing): https://www.windowscentral.com/gaming/linux-supercharges-the-rog-xbox-ally-x-higher-fps-smoother-performance-and-a-big-win-for-handheld-gaming
- Tom's Guide, original Ally battery at 15 W: https://www.tomsguide.com/gaming/handheld-gaming/i-love-my-rog-ally-and-these-5-simple-tweaks-help-me-game-longer
- justin.how, "How Bazzite transformed my ROG Ally": https://justin.how/blog/linux-on-rog-ally
- GamingOnLinux, FC 26 entry: https://www.gamingonlinux.com/itemdb/app/43028
- GamingOnLinux, EA Javelin job listing (2026-03): https://gamingonlinux.com/2026/03/ea-javelin-anticheat-job-listing-mentions-future-support-for-linux-and-proton
- Reddit r/SteamOS, charge limit on the Ally (anecdotal): https://www.reddit.com/r/SteamOS/comments/1sh6r0v/battery_limit_with_steam_os_on_rog_ally_only
- Notebookcheck, Xbox full-screen experience on the original Ally: https://www.notebookcheck.net/Xbox-full-screen-mode-on-original-Asus-ROG-Ally-boosts-gaming-handheld-performance-but-with-caveats.1117352.0.html

Stale or weak: the Universal Blue ROG Ally wiki (HHD-era, superseded): https://universal-blue.discourse.group/t/asus-rog-ally-handheld-compatibility/2414. The windowsnews.ai "doubles battery" article has no stated method.
