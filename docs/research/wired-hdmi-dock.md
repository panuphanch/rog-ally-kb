# Wired HDMI via USB-C dock: Bazzite vs Windows (original ROG Ally)

Ticket: issue #4. Researched 2026-11-07. Device: original ROG Ally (RC71L, Z1 Extreme), USB-C dock with HDMI, LG webOS / Xiaomi Google TV.

## Answer

**Bottom line (medium-high confidence):** Plain wired HDMI through a dock works on both OSes. The dock, the cable and the TV set the limits more than the OS does. With a typical HDMI 2.0 dock like the official ROG Gaming Charger Dock, you get up to 4K60 or 1080p120 and 65 W charging, which is enough for 30 W Turbo. Windows has the more predictable desktop-style controls: per-display resolution and refresh, the HDR toggle, and the audio device picker. Bazzite Game Mode is more console-like. It takes over the TV, gives you a resolution and refresh override in Steam's Display settings, upscales with gamescope (FSR/NIS), and has an HDR toggle. But it has more rough edges: the wrong audio sink, resume or sleep glitches, and an Ally-specific TDP that sticks when you plug or unplug power during standby. **HDMI-CEC (TV remote control of the Ally) effectively doesn't work on either OS with ROG or generic docks.** Docked TDP is the same 30 W ceiling on both.

**Blocker outside this ticket's scope (high confidence):** EA Sports FC 26 uses EA Javelin anti-cheat. EA says it is not compatible with Linux, and it is listed as broken on Proton or Steam Deck. On Bazzite the target game will most likely not run at all, docked or not, apart from unofficial career-mode hacks. That outweighs every docking difference below. See Sources [14-16].

## Comparison table

| Topic | Windows 11 (Armoury Crate SE) | Bazzite Game Mode (gamescope) | Confidence |
|---|---|---|---|
| Resolution / refresh | Windows Settings > Display > Advanced display; any EDID mode the dock passes. ASUS recommends 1080p, at least 60 Hz [2] | Gamescope drives the TV and the internal panel is off in Game Mode. It defaults to the TV's preferred (often max) mode. Steam Settings > Display has an external resolution/refresh override. QAM refresh slider is disabled for external displays by default [7,8] | Verified (Valve/ASUS docs); Bazzite-specific UI inferred from SteamOS parity |
| 4K vs 1080p | Set the desktop to 1080p, or run the game at 1080p with RSR/FSR to 4K [2] | Per-game resolution plus gamescope upscaling (FSR/NIS/integer) to the output mode. Or force 1080p output and let the TV scale | Verified (Windows); inferred (Bazzite) |
| HDR | Windows HDR toggle. Works at 4K60 over HDMI 2.0 only with reduced chroma (inference: 4K60 10-bit RGB exceeds 18 Gbps) | "HDR and VRR available by default" [9]. Toggle in Steam Display settings. Known "rainbow" glitch is fixed by toggling HDR, or Force Composite [5] | Medium. Official ROG dock spec does **not** list HDR [1] |
| HDMI audio | Shows up as a normal Windows audio device | PipeWire HDMI sink. Known quirk: wrong device picked, so disable non-HDMI profiles in Desktop Mode [5]. Ally has two audio devices that share volume [4]. An Ally X report has HDMI audio dropping after sleep while docked [10] | Verified (docs); dropouts anecdotal |
| Charging passthrough | Ally input max 65 W (stock 20 V/3.25 A) [3]. 30 W Turbo needs a full 65 W contract. BIOS 323 (Jul 2023) fixed under-negotiation with third-party docks [6] | Same. Charging is negotiated by firmware/EC, not the OS (inference). Bazzite known issue: TDP gets stuck if the charger is removed in standby [4] | Verified |
| CEC | No practical support over DP-alt-mode docks | Bazzite ships libCEC [9], but Steam's CEC menu only appears with the official Steam Deck Dock. Generic docks: no CEC (2025-26 forum) [11] | Medium-high |
| Docked TDP | Silent 10 W / Performance 15 W / Turbo 25 W, 30 W on AC; Manual mode [12] | Steam QAM TDP slider, or OGUI/PowerStation, or a Decky plugin (SimpleDeckyTDP). Use only one at a time [13]. CPU boost off by default [4] | Verified |

## Details

### Hardware path (verified)
- The original Ally's USB-C port is "USB 3.2 Gen2, support DisplayPort 1.4" [3]. Every HDMI dock is therefore a DP-alt-mode-to-HDMI converter. The converter chip in the dock decides the HDMI version, max mode, HDR passthrough and CEC.
- **ROG Gaming Charger Dock (AC65-03):** HDMI 2.0, 4K60 or 1080p120, 65 W total. That is 65 W on USB-C alone, or 60 W USB-C + 5 W USB-A when both are used. The USB-A port is USB 2.0. HDR and CEC are **not** listed in the spec [1][17].
- **Generic docks:** published limits are usually 4K60 HDMI 2.0, and a few claim 4K120/144 [18]. Common failure modes:
  - The dock eats some of the input power. A 65 W charger feeding a pass-through dock gives about 25 W Turbo, so feed the dock with a 100 W PD charger (ASUS ROG forum, user reports) [19].
  - The dock negotiates a weak PD contract. BIOS 323 fixed this, so keep the BIOS current [6].
  - HDMI 2.1 claims don't hold with HDR + VRR + 4K120 together. Reported on an Ally X / Bazzite / LG C5 in April 2026 [20].
  - Some docks output 4K only and won't step down to 1080p (Reddit, anecdotal) [21].

### Resolution, refresh and scaling
- **Windows:** standard desktop display settings. ASUS's own guide recommends 1080p, at least 60 Hz, and RSR/FSR for 4K TVs [2].
- **Bazzite Game Mode:** follows SteamOS behaviour.
  - With an external display connected, the internal panel is disabled in Game Mode [7].
  - Steam > Settings > Display offers a resolution/refresh override for the external display [7]. This UI first landed in SteamOS 3.3.2 beta in 2022, which is older history [8].
  - Separate refresh and FPS-limit settings are kept for internal and external displays. The QAM refresh slider is off for external displays by default [8].
  - A July 2025 CachyOS report has an Ally in Game Mode stuck at 4K30 through an "HDMI 2.0" dock, unresolved [22]. The likely cause is dock or link bandwidth (inference).
  - **Practical recommendation (inference):** for FC 26-class games on a Z1 Extreme, force 1920x1080@60 in Steam Display settings, or render at 1080p and let gamescope or the TV upscale. Don't render at native 4K.
- Upstream gamescope now includes an Ally/Ally X display-config script. That covers the **internal** panel VRR range (48-120 Hz), not external output [23].

### HDR
- Bazzite advertises HDR and VRR in Game Mode [9]. The quirks page documents two things [5]:
  - A "rainbow display" fix: toggle HDR in the QAM, or enable Force Composite.
  - An optional "Enable HDMI 2.1 for AMD Graphics Cards" tweak, with a warning that it can cause flicker.
- Through an HDMI 2.0 dock, 4K60 HDR needs 4:2:2 or 4:2:0 chroma (inference from HDMI 2.0's 18 Gbps limit). Whether that works depends on the dock chip. The ASUS dock spec doesn't promise HDR [1].
- Treat HDR as "maybe, test it" on both OSes. Windows is likely the more predictable of the two (inference).

### Audio over HDMI
- **Windows:** HDMI audio shows up as a normal output device. No special notes found.
- **Bazzite:**
  - HDMI TV audio sometimes goes to the wrong sink. The fix is to disable unused devices or profiles in Desktop Mode [5].
  - The Ally exposes "Family 17h/19h/1ah HD Audio Controller" and "ROG Ally". Their volumes affect each other [4].
  - An Ally X user on Bazzite reported HDMI audio dropouts after sleep while docked. The display settings showed the 7" panel until they rebooted docked, and a TV pass-through box was suspected [10]. This is anecdotal and dates from 2024.

### Charging passthrough
- The Ally accepts up to 65 W (20 V / 3.25 A stock charger) [3].
- Turbo is 30 W only on AC with a capable source. Chargers below 65 W may drop it back to 25 W Turbo [12].
- BIOS 323 (2023-07-21) lets the Ally draw whatever a dock offers. If the source is too weak, the battery drains slowly while in 30 W mode [6].
- The official ROG dock delivers the full 65 W only when nothing is drawing from its USB-A port. With USB-A in use it gives 60 W [1][17].
- OS-independent (inference): PD negotiation lives in firmware.
- Bazzite-specific: plugging or unplugging the charger during standby can leave the TDP stuck. Workaround: plug or unplug while the Ally is awake [4].

### CEC
- DP alt mode does not carry CEC natively. Docks need their own CEC bridge [24].
- Bazzite lists HDMI CEC support (libCEC) [9]. In practice, Settings > Display > CEC ("Enable HDMI CEC Support", "Wake TV on resume") appears only with the official Steam Deck Dock. A JSAUX dock that the vendor says supports CEC did not expose it (thread from 2025-06 to 2026-05) [11].
- No source found showing CEC with the ROG Gaming Charger Dock on either OS.
- **Expect no TV-remote control of the Ally. Use the Switch Pro Controller instead.**

### Docked TDP and performance
- **Windows Armoury Crate SE:** Silent / Performance / Turbo at 10 / 15 / 25 W, with 30 W on AC [12].
- **Bazzite:** TDP comes from the Steam QAM ("TDP managed by Steam"), OGUI via PowerStation, or Decky plugins (SimpleDeckyTDP, PowerControl). Use only one at a time [13]. CPU boost is disabled by default to limit power draw [4].
- Docking doesn't unlock more than 30 W on either OS.
- Bazzite is often reported equal or faster on Ally hardware (2025 Ally X comparisons) [25]. That doesn't help with FC 26, given the anti-cheat blocker.

## Verified vs inferred
- **Verified (primary or first-party):**
  - Dock specs [1][17] and Ally port and charger specs [3]
  - ASUS docking guide [2] and BIOS 323 behaviour [6]
  - Bazzite docs and README claims [4][5][9][13]
  - Steam external display behaviour [7][8]
  - FC 26 unsupported on Linux, per an EA staff forum post [15]
- **Inferred or anecdotal:**
  - HDR chroma limits through HDMI 2.0
  - That Bazzite's Game Mode display UI matches SteamOS exactly
  - That charging is OS-independent
  - The audio dropout, 4K30 and generic-dock CEC reports (forum or Reddit)
- **Stale flags:** Steam Deck dock UI notes (2022) [8]; HDMI audio dropout thread (2024) [10]; BIOS 323 article (2023) [6].

## Sources
1. ASUS, ROG Gaming Charger Dock spec: https://rog.asus.com/docks-chargers-and-cables/chargers/rog-gaming-charger-dock/spec/
2. ASUS, How to connect the ROG Ally to a TV or monitor: https://rog.asus.com/us/articles/guides/how-to-connect-the-rog-ally-to-a-tv-or-monitor-for-big-screen-gaming/
3. ASUS, ROG Ally (2023) RC71L spec: https://rog.asus.com/gaming-handhelds/rog-ally/rog-ally-2023/spec
4. Bazzite docs, ASUS handhelds: https://docs.bazzite.gg/Handheld_and_HTPC_edition/Handheld_Wiki/ASUS_ROG_Ally/
5. Bazzite docs, Steam Gaming Mode quirks: https://docs.bazzite.gg/Handheld_and_HTPC_edition/quirks/
6. SteamDeckHQ, ROG Ally BIOS 323 adapter improvements (2023-07-21): https://steamdeckhq.com/news/asus-rog-ally-bios-323-adapter-improvements/
7. Steam Support, Docking the Steam Deck: https://help.steampowered.com/faqs/view/4C18-08B5-DEC9-3AF4 (body not machine-readable; content per search summary)
8. Overkill, SteamOS beta improves docking support (2022): https://archive.overkill.wtf/new-steam-deck-beta-update-improves-docking-support/
9. Bazzite README: https://github.com/ublue-os/bazzite
10. Bazzite Discord (Answer Overflow), Audio dropouts over HDMI (Ally X): https://answeroverflow.com/m/1271775592135655545
11. Universal Blue forum, CEC with 3rd Party Docks: https://universal-blue.discourse.group/t/cec-with-3rd-party-docks/9353
12. ASUS, How to extend the battery life of your ROG Ally (operating modes / 30 W on AC): https://rog.asus.com/id/articles/news/56a682d8-6627-4e80-b931-f9473d435812/
13. Bazzite docs, Handheld Wiki: https://docs.bazzite.gg/Handheld_and_HTPC_edition/Handheld_Wiki/
14. PCGamesN, Can you play EA Sports FC 26 on Steam Deck?: https://www.pcgamesn.com/ea-sports-fc-26/steam-deck
15. EA Forums, FC 26 won't start on Linux (EA staff reply): https://forums.ea.com/discussions/fc-26-technical-issues-es/ea-fc-26-no-inicia-en-linux-nobara-43--fedora-desde-epic-games/13554267
16. GamingOnLinux, EA Javelin job listing mentions Linux/Proton (2026-03): https://gamingonlinux.com/2026/03/ea-javelin-anticheat-job-listing-mentions-future-support-for-linux-and-proton
17. ChargerLAB, ROG 65W Gaming Charger Dock review: https://chargerlab.com/charging-review-of-rog-65w-gaming-charger-dock
18. Best docks for ROG Ally 2026 (third-party roundup): https://thunderboltlaptop.com/best-docks-for-rog-ally/
19. ASUS ROG forum, Dock issues not allowing 30W Turbo: https://rog-forum.asus.com/t5/handheld-gaming/rog-ally-dock-issues-not-allowing-30w-turbo-mode/td-p/936655/page/8
20. Universal Blue forum, Cannot get 4K 120Hz, HDR and VRR at the same time (2026-04): https://universal-blue.discourse.group/t/cannot-get-4k-120hz-hdr-and-vrr-at-the-same-time/12054
21. Reddit r/ROGAlly, Ally X dock help: https://reddit.birdcat.cafe/r/ROGAlly/comments/1kfd95i/rog_ally_x_dock_help
22. CachyOS forum, ROG Ally external monitor can't 4K 60fps (2025-07): https://discuss.cachyos.org/t/rog-ally-external-monitor-can-not-4k-60fps/12237
23. Tom's Hardware, SteamOS gets display configuration and VRR support for ROG Ally: https://www.tomshardware.com/video-games/handheld-gaming/steamos-gets-display-configuration-and-vrr-support-for-asus-rog-ally-and-rog-ally-x
24. GamingOnLinux, JSAUX Steam Deck dock review (DP-alt has no CEC; 2022): https://www.gamingonlinux.com/2022/08/no-need-to-wait-on-valve-the-steam-deck-docking-station-from-jsaux-is-great
25. Galaxus, SteamOS makes the ROG Ally X faster: https://www.galaxus.ch/en/page/linux-statt-windows-steamos-macht-den-asus-rog-ally-x-schneller-und-besser-37027
