# Switch Pro Controller on the original ROG Ally (Bazzite vs Windows)

Ticket: issue #6, "Switch Pro Controller support on the Ally". Researched 2026-11-07.

## Answer

**The controller works well on both OSes when the game runs through Steam. Steam Input does the heavy lifting on both, and the Linux kernel driver (`hid-nintendo`) gives a solid base on Bazzite.** Confidence: high for the controller itself, medium for Bluetooth on the Ally specifically.

- **Pairing:** Bluetooth and USB-C both work on both OSes. USB is the most reliable option, and it also charges the controller. Over Bluetooth, the Pro Controller is no faster than over USB (measured below), so pick whichever is more convenient.
- **FC 26 button prompts:** the game will show **Xbox prompts**, because Steam Input presents the controller to the game as an Xbox-style pad. Leave Steam's "Use Nintendo Button Layout" **off** (map by position) so the on-screen "A" is the bottom face button, which is the Nintendo "B". Turn it on only if you would rather press the button that carries the matching letter.
- **Rumble works and gyro is available** through Steam Input. FC 26 has no gyro feature, so gyro does not matter here. A known Linux bug where rumble causes Bluetooth dropouts is old (2022); if you hit it, turning rumble off in Steam is the standard workaround.
- **Latency:** Gamepadla measured about 17 ms (Bluetooth) and 22 ms (USB) on Windows. That is a "D" grade, but fine for FC 26.
- **Couch range:** normal living-room distances (2-4 m, line of sight) should be fine, though nothing Ally-specific confirms it (inference).
- **Streaming to the TV over Wi-Fi:** this should work if the Ally is on **5 GHz** Wi-Fi. Wi-Fi and Bluetooth share a single combo chip on the Ally, so 2.4 GHz Wi-Fi is the setup that causes trouble. Better still, use the **USB-C dock + HDMI** (ideally with Ethernet) and avoid streaming altogether.
- **BLOCKER, out of scope but critical:** **EA Sports FC 26 does not run on Bazzite/Proton at all**, because EA Javelin anti-cheat blocks it (GamingOnLinux lists it as "Broken"). So the Bazzite half of this question is mostly theoretical for FC 26. On Windows the Switch Pro plus Steam Input route is the one that applies. This deserves its own ticket or decision.

## Details

### Hardware facts (verified)
- The original ROG Ally (Z1 Extreme) uses a **MediaTek MT7922 Wi-Fi 6E + Bluetooth** combo card, the same chip AMD sells as **RZ616** (Notebookcheck review, iFixit chip ID). On Linux, Wi-Fi uses the `mt7921e` driver and Bluetooth uses `btusb`/`btmtk`.
- Because Wi-Fi and Bluetooth share one radio module, they rely on on-chip coexistence logic.

### Bazzite / Linux
- **Kernel driver (verified, primary source):** `drivers/hid/hid-nintendo.c` in mainline Linux supports Pro Controllers "over USB or Bluetooth", reads factory stick and IMU calibration, and implements rumble, the IMU (gyro and accelerometer), player LEDs and battery reporting. The 2026 header also lists Switch 2 Pro Controller support and a "Copyright (c) 2026 Valve Software" line, which shows Valve is actively maintaining Nintendo controller support in the kernel. The driver has been in mainline since 5.16, and Bazzite ships much newer kernels.
- **Steam Input** then picks the controller up and translates it for games. In Game Mode, pairing works through Steam's Bluetooth settings.
- **Known community issues (anecdotal, mostly 2024-2025):**
  - Switch Pro on Bazzite disconnecting or going unresponsive. Reported fixes are pairing through the OS Bluetooth settings, rebooting, then pressing Home before opening Steam (Dec 2024), and **disabling KDE Connect** on Bazzite-KDE (May 2025). (Universal Blue Discourse #5655)
  - The Pro Controller dropping over Bluetooth while rumble is active, from a kernel bug report and a Steam-for-Linux issue (2022, old). Workaround: disable rumble in Steam controller settings.
  - **Ally-specific:** on Bazzite GNOME, Bluetooth switches off when going from Game Mode to Desktop Mode (Jul 2025, no fix in the thread, called a bug). Original vs Ally X was not stated.
  - General Bazzite 41 Bluetooth instability (late 2024) was reported as resolved by updates in May 2025. A user in Oct 2026 says kernel 6.18 added many Bluetooth IDs. That thread is about USB dongles, not the Ally's internal card.
  - MT7922 Wi-Fi on the Ally under Bazzite has reports of dropping after a few minutes. Suggested workarounds include `mt7921e` module options (`disable_aspm=1`, and `disable_clc=1` for "coexistence legacy"). These are **unverified for this exact problem**, so treat them as leads only.
- **Inference:** the internal Bluetooth on the original Ally generally works on current Bazzite. The main risk is occasional flakiness (state after mode switch or suspend), not missing support.

### Windows
- Windows has no Nintendo driver of its own. The Pro Controller appears as a generic HID device. **Steam Input supports it natively** (since 2018), so for a Steam-bought game launched from Steam it works the same as on Linux: gyro, rumble and layout are all handled by Steam.
- FC 26 on PC natively supports Xbox and DualSense controllers. No source confirms native Switch Pro support, so **keep Steam Input on** for FC 26 (inference based on EA's and the guides' controller lists). Some FC 26 users report Steam Input quirks, such as prompts flipping to keyboard/mouse or overlay glitches. These are not Switch-specific.
- Ally Bluetooth disconnects on Windows also appear on ASUS ZenTalk. The usual advice is updated MediaTek drivers through MyASUS/Armoury Crate and power settings that keep Bluetooth on.

### Button layout in FC 26
- Steam's global **"Use Nintendo Button Layout"** setting (Settings > Controller) swaps A/B and X/Y in the loaded configuration. Valve's guidance is to use this setting rather than remapping face buttons by hand. Community reports (2023-2024) say it behaves inconsistently with overlay glyphs, and that it only affects controllers Steam recognises as Nintendo-type, which the genuine Pro Controller is.
- The game receives a virtual Xbox pad, so **prompts will be Xbox (A/B/X/Y in Xbox positions)**. No source shows FC 26 on PC displaying Nintendo glyphs (unverified; inference).
- Recommendation: keep the setting **off** (positional mapping), so the on-screen prompt location matches the physical button location. The only drawback is that the printed letters disagree.

### Gyro and rumble
- Both work through `hid-nintendo` (Linux) and Steam Input (both OSes). The kernel reports IMU data about every 15 ms by default (driver source).
- FC 26 does not use motion controls, so gyro is irrelevant unless you map it in Steam Input. Rumble passes through Steam Input. Disable it if Bluetooth drops appear (see above).

### Input latency (verified measurement, Windows 11, Gamepadla, Sep-Oct 2025)
| Mode | Button avg | Stick avg | Jitter (button) | Polling |
|---|---|---|---|---|
| Bluetooth | 16.8 ms | 21.1 ms | 6.0 ms | ~66 Hz |
| USB cable | 21.8 ms | 22.2 ms | 4.9 ms | ~74 Hz |

- Over USB the Pro Controller is **not** faster than over Bluetooth. Handheld Legend's explainer says the same (USB uses an 8 ms interval; Handheld Legend sells competing controllers, so treat it with caution). Either mode adds roughly one frame at 60 fps compared with a good Xbox pad. That is irrelevant for FC 26 offline play.
- These figures were measured on Windows. No Linux measurement was found. Expect similar results, since the controller's own report rate is the bottleneck (inference).

### Bluetooth range: couch to Ally
- No Ally-specific range test was found. A typical Class 2 Bluetooth link covers ~10 m in open space, so a couch-to-TV distance of 2-4 m with line of sight is well within range (inference). Range drops if the Ally sits behind the TV or inside a cabinet. If you see stutter, move the dock or Ally so it has line of sight.

### Streaming to the TV over Wi-Fi while using the Bluetooth controller
- Bluetooth uses 2.4 GHz. **If the Ally's Wi-Fi is also on 2.4 GHz, the two compete on the same combo chip.** An Ubuntu bug report on MT7921 Bluetooth latency got the maintainer advice to move Wi-Fi to 5 GHz. The owner already has 5 GHz, so with the Ally on 5 GHz the controller and the stream should coexist well (inference based on that guidance plus chip design).
- The streaming option with the least risk is not to stream from the Ally at all: **dock the Ally on the USB-C dock with HDMI to the TV**, preferably on Ethernet, and pair the controller to the Ally. The controller can also be plugged into the dock by USB.
- If streaming to a TV app instead (Steam Link on Google TV, for example), you can pair the controller **to the TV/streamer** rather than to the Ally. Reports for the Switch Pro paired to Steam Link clients are mixed (one older report says "Unknown Controller" with disconnects over Bluetooth, but it works wired). Steam Link on LG webOS: no official app found in the sources checked (2022-era threads). That belongs to the streaming ticket.

## Verified vs inferred
- **Verified (primary or measured):** MT7922/RZ616 card; `hid-nintendo` features (USB+BT, rumble, IMU, LEDs, battery); Gamepadla latency numbers; the Steam Nintendo layout setting exists and swaps A/B and X/Y; FC 26 is blocked on Proton by Javelin (GamingOnLinux anti-cheat DB).
- **Community or anecdotal:** Bazzite disconnect fixes (KDE Connect, pairing order); rumble-induced drops; Ally Game Mode to Desktop Bluetooth-off bug; Windows Ally Bluetooth disconnects; Steam layout glyph inconsistencies.
- **Inference:** couch range is fine; 5 GHz Wi-Fi plus Bluetooth coexist fine on the Ally; FC 26 shows Xbox glyphs only; Linux latency is similar to Windows.
- **Stale (pre-2024):** kernel rumble bug (2022), Steam Link Switch Pro reports, LG webOS Steam Link status (2022), Steam's 2018 layout announcement.

## Sources
- Linux `hid-nintendo` driver source (mainline): https://github.com/torvalds/linux/blob/master/drivers/hid/hid-nintendo.c
- Gamepadla, Nintendo Switch Pro Controller latency (tests 2025-09-28 / 2025-10-07): https://gamepadla.com/nintendo-switch-pro-controller.html
- Handheld Legend, Pro Controller latency explainer: https://docs.handheldlegend.com/s/progcc-3/doc/pro-controller-latency-explainer-do8eVTJBY4
- Notebookcheck, ROG Ally Z1 Extreme review (MT7922): https://www.notebookcheck.net/Asus-ROG-Ally-Z1-Extreme-Review-Gaming-handheld-with-120-Hz-display-and-AMD-Zen4.716680.0.html
- iFixit, ASUS ROG Ally chip ID: https://www.ifixit.com/Guide/ASUS+ROG+Ally+Chip+ID/162238
- Universal Blue Discourse, Pro Switch controller connection issues (Bazzite, 2024-2025): https://universal-blue.discourse.group/t/pro-switch-controller-connection-issues/5655
- Universal Blue Discourse, Ally desktop mode Bluetooth disabled (Jul 2025): https://universal-blue.discourse.group/t/ally-desktop-mode-bluetooth-disabled/9995
- Universal Blue Discourse, Major Bluetooth issues Bazzite 41 (2024 to Oct 2026): https://universal-blue.discourse.group/t/major-bluetooth-issues-bazzite-41/5124
- Universal Blue Discourse, Rog Ally Wi-Fi problem on Bazzite: https://universal-blue.discourse.group/t/rog-ally-wifi-problem-on-bazzite/8620
- Universal Blue Discourse, Fails to recognize connected Bluetooth controller: https://universal-blue.discourse.group/t/fails-to-recognize-or-display-the-connected-bluetooth-controller/11857
- Kernel bugzilla 216218 (Pro Controller BT drops with force feedback, 2022): https://bugzilla.kernel.org/show_bug.cgi?id=216218
- Steam for Linux issue 7776: https://github.com/ValveSoftware/steam-for-linux/issues/7776
- Ubuntu bug 1995798, MT7921K high latency with Bluetooth (5 GHz advice): https://bugs.launchpad.net/ubuntu/+source/linux/+bug/1995798
- Arch forum, MT7922 lag / ASPM: https://bbs.archlinux.org/viewtopic.php?id=309442
- ASUS ZenTalk, ROG Ally Extreme Bluetooth disconnects: https://zentalk.asus.com/t5/others/rog-ally-extreme-bluetooth-disconnects/m-p/447075
- Steam Client Beta group, Switch Pro support and Nintendo layout (2018): https://steamcommunity.com/groups/SteamClientBeta/discussions/1/1696046342854517917
- Perfectly Nintendo, Steam Switch Pro support / global layout swap: https://www.perfectly-nintendo.com/nintendo-switch-pro-controller-support-added-to-steam
- Steam Community, Why does Steam flip A/B and X/Y with Switch Pro: https://steamcommunity.com/discussions/forum/0/6733519464636814806
- Steam Client Beta group, Nintendo Button Layout inconsistent (2023): https://steamcommunity.com/groups/SteamClientBeta/discussions/0/3710433479218773768
- GamingOnLinux, Steam Deck beta Nintendo controller changes (Jan 2023): https://gamingonlinux.com/2023/01/steam-deck-beta-steam-input-nintendo-controllers
- GamingOnLinux item DB, EA Sports FC 26 anti-cheat status: https://www.gamingonlinux.com/itemdb/app/43028
- GamingOnLinux, EA Javelin job listing mentions future Linux support (Mar 2026): https://gamingonlinux.com/2026/03/ea-javelin-anticheat-job-listing-mentions-future-support-for-linux-and-proton
- Destructoid, FC 26 controller not working on PC fix: https://www.destructoid.com/fc-26-controller-not-working-on-pc-fix
- EA Forums, FC 26 DualShock 4 showing Xbox inputs: https://forums.ea.com/discussions/fc-26-technical-issues-en/fc26-dualshock-4-having-xbox-inputs/13143261
- Steam Link discussions (Switch Pro paired to Steam Link client): https://steamcommunity.com/app/353380/discussions/0/1745594817430994141
