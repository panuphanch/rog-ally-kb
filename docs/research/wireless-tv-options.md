# Wireless TV options: ROG Ally to LG webOS, Xiaomi Google TV, old Chromecast

Issue: #5. Researched 2026-11-07.

## Answer

**Use Moonlight on the TV and Sunshine on the Ally. It is the best choice on both OSes. The best TV for it is the Xiaomi Google TV, wired to the router by Ethernet.** Confidence: medium-high.

- **Bazzite:** Run `ujust setup-sunshine` → `enable-brew` on the Ally. Install **Moonlight** from the Play Store on the Xiaomi Google TV. **Steam Link** on the same TV is the zero-config fallback: it needs nothing installed on the Ally, because Steam is already there.
- **Windows:** Use the same TV apps. On the Ally, run **Sunshine**, or the **Apollo** fork if you want a virtual display. On Windows only, Apollo lets the stream run at the TV's resolution, so the Ally screen isn't the thing being streamed. For a quick no-install mirror to the **LG**, use Miracast (`Win+K`). That's fine for menus or video but not for competitive FC 26.
- **LG webOS:** There's **no official Steam Link app and no official Moonlight app**. The only way to stream games to it is a homebrew Moonlight build in Developer Mode, which has to be renewed about every 41 days. It's workable, but it's the second choice.
- **Old Chromecast (non-Google-TV dongle):** It can't run apps, so it can only receive casts. Casting a whole screen from Linux or Chrome adds roughly a second of lag. **Not usable for gaming.** If it's actually a *Chromecast with Google TV*, it counts as an Android TV box and Moonlight and Steam Link both work.
- **Wired fallback:** The USB-C dock with HDMI beats every wireless option on latency and reliability. Since the Ally stays near the TV anyway, keep it as the fallback for serious FC 26 sessions.

The Switch Pro Controller stays paired to the Ally in every option. The Ally is the **streaming host**, so controller input reaches the game locally with no added delay. Only the *picture* you see on the TV is late, by the stream's latency. The Ally just needs to be within Bluetooth range of the couch.

## Verified facts vs inference

Each claim below is marked **[V]** (verified against a cited source) or **[I]** (inference or community reports, not primary-sourced).

### 1. Moonlight (TV) + Sunshine (Ally)

- **Bazzite ships Sunshine tooling.** [V] The `ujust setup-sunshine` recipe offers `enable` (Flatpak), `enable-brew` (Homebrew build), `disable`, `update`, `portal` (web UI at `https://localhost:47990`) and `virtual-monitor`. On Deck images the recipe warns that **the Flatpak cannot use KMS capture**. `enable-brew` sets capture to KMS and adds a startup delay. The `virtual-monitor` option works **only on the KDE (Kinoite) image**, which is not the Ally's Game Mode image. [Source 1]
  - On the Ally (bazzite-deck image), use `enable-brew`. [I, follows from the recipe]
- **TV clients:**
  - Moonlight officially lists Android clients. [V] [Source 3] The Android client supports Android TV, which covers the Xiaomi Google TV. [I, widely reported; it's in the Play Store for TV]
  - **LG webOS is a community homebrew port that needs Developer Mode** (Moonlight's own site). [V] [Source 3]
  - The port (`mariotaku/moonlight-tv`) installs with webOS Dev Manager or as an IPK from GitHub releases, and supports up to 4 controllers. [V] [Source 4]
  - Developer Mode expires after about 999 h (~41 days) and must be renewed, either by hand or with a scheduled "extend" call. [V, single 2024 blog, Source 5]
  - Community quality reports for moonlight-tv vary (some report audio stutter or high latency). [I]
- **Network:** Moonlight's setup guide says to put the **host** on Ethernet and use **5 GHz Wi-Fi 5/6 on the client**. [V] [Source 6] In this setup the roles are reversed. The host (the Ally) is the device stuck on Wi-Fi, so the best setup is: **TV on Ethernet** and the **Ally on 5 GHz**, close to the router. That leaves one wireless hop. [I]
- **Latency/quality:**
  - Typical on a good LAN: about 1 frame of encode, plus network, plus TV decode. That's roughly 10–30 ms on a good Android TV SoC, more on cheap SoCs. A 2023 user test measured client decode around 2 ms on Shield and around 10 ms on a Fire TV 4K Max (GameStream era, not Sunshine). [I/anecdotal, Source 9]
  - Image quality is good at 1080p60 at 20–40 Mbps (HEVC on the Z1 Extreme's AMD encoder). [I]
- **Ally screen off:**
  - **Bazzite:** KMS capture records what is on the panel, so the screen has to stay on. Set brightness to minimum. [I] The virtual-monitor helper is KDE-image-only. [V, Source 1]
  - **Windows:** **Apollo** (a Sunshine fork) creates a SudoVDA virtual display per stream. Virtual-display support is Windows-only. [V] [Source 7] That lets the stream use the TV's resolution. Whether the physical panel can be fully off isn't documented; expect to turn it off or dim it manually. [I]
- **Setup effort:** About 15 minutes on the Google TV: install the app and pair with a PIN. On LG it's about an hour (developer account, Dev Manager, IPK), plus renewal every ~41 days. [I]

### 2. Steam Link (TV app) + Steam Remote Play (Ally)

- **Platforms:** Steam Link officially runs on Android phones, tablets and **TVs**, plus Fire TV, Apple TV, Raspberry Pi and desktops. [V] [Source 2]
  - **LG webOS isn't listed**, and every 2022–2025 report says there's no Steam Link app in the LG Content Store. [V for Valve's list; I for "never released", from forum and blog reports, Sources 5 and 8]
  - IHSplay (an unofficial Steam-streaming client for webOS) looks early-stage. Its GitHub repo shows no releases. [V] [Source 10]
- **Host side:** Steam itself is the host. No install is needed on Bazzite or Windows. [V for Steam Remote Play generally; Source 2] On Linux hosts, community reports range from "works fine" to black screens tied to the GPU or capture method. [I]
- **Network:** Valve's page gives specific advice only for VR streaming: 5 GHz on Wi-Fi 5 and a wired PC. [V] [Source 2]
- **Latency/quality:** Comparable to Moonlight on a good network. Users generally rate it slightly worse, with fewer knobs. [I]
- **Screen off:** No virtual display, so the Ally screen stays on (dim it). [I]
- **Setup effort:** The lowest of any option: install the app on the Google TV, enter a PIN, play. [I]

### 3. Miracast / Wi-Fi Display

- **Windows → LG:**
  - LG documents `Win+K` → pick the TV, via Screen Share. [V] [Source 11] Reports of failed connections exist, and Intel's community still recommends HDMI as the most reliable route. [V that such reports exist; Source 11 search set]
  - Latency is typically about 100–250 ms, and the encoder/bitrate isn't user-controlled. It's OK for menus and video, noticeably laggy for FC 26. [I]
- **Windows → Xiaomi Google TV / Chromecast:** Google TV has **no native Miracast receiver**. It relies on Google Cast and third-party receiver apps. [I, consistent across reports; check the specific Xiaomi model]
- **Bazzite:**
  - **KDE Plasma has no built-in Miracast.** [V, KDE Discuss thread, Source 12]
  - The workaround is **GNOME Network Displays** (Flatpak), which supports Miracast and Chromecast. [V for the protocols, Source 13] One user reports it working on KDE with quirks. [I, Source 12]
  - It needs Desktop Mode, not Game Mode. [I] Wi-Fi Direct (P2P) support in the Ally's MediaTek Wi-Fi driver is unverified. [I]
- **Screen off:** No. It mirrors the panel. [I]

### 4. Chromecast / Google Cast casting

- Old Chromecast dongles (pre-Google TV) only accept casts. There's no app store, so Steam Link and Moonlight can't run on them. [I, well established]
- Screen casting works from Chrome/Edge on Windows ("Cast desktop") or from GNOME Network Displays on Linux. It's built for video and slides, and lag is commonly about 0.5–1+ s. [I]
- **Unsuitable for gaming.**

### 5. Others considered

- **Cloud gaming on the TV** (GeForce NOW and similar on LG/Google TV): bypasses the Ally entirely. EA FC 26 support depends on the service catalogue, and it's online-only. Out of scope. [I]
- **Steam Link on Samsung Tizen / Apple TV:** not owned. [V that those apps exist, Source 2]

## Comparison table

| Option | Bazzite | Windows | LG webOS | Xiaomi Google TV | Old Chromecast | Setup | Latency (est.) | Quality | Reliability | Ally screen off? |
|---|---|---|---|---|---|---|---|---|---|---|
| **Moonlight + Sunshine** | Yes (`ujust setup-sunshine`, brew) | Yes (Sunshine/Apollo) | Homebrew, Dev Mode, renew ~41 d | Yes (Play Store) | No | Medium (LG: high) | Low (~10–30 ms on LAN) | Best, tunable bitrate/HEVC | Good | Bazzite: dim only. Windows: Apollo virtual display |
| **Steam Link** | Yes (Steam is host) | Yes | No official app | Yes (Play Store) | No | Lowest | Low–medium | Good | Good; Linux-host bugs reported | Dim only |
| **Miracast** | Not native on KDE; GNOME Network Displays, Desktop Mode only | Native `Win+K` | Yes (Screen Share) | Not native | No | Low (Windows) / high (Linux) | Medium–high (~100–250 ms) | Fair, fixed | Flaky | No (mirror) |
| **Chromecast cast** | GNOME Network Displays | Chrome "Cast desktop" | Some LGs accept cast | Yes | Yes | Low | Very high (~0.5–1 s+) | Fair | OK for video | No |
| **USB-C dock HDMI (wired)** | Yes | Yes | Yes | Yes | n/a | Lowest | None | Native | Best | Yes (external only) |

**Network for all streaming options:** The Ally on **5 GHz** (Wi-Fi 6 if the router has it), close to the router. The **TV on Ethernet**. Avoid 2.4 GHz and Wi-Fi client isolation or guest networks, because discovery needs the same LAN.

## Recommendation per OS

- **Bazzite:**
  1. **Steam Link on the Xiaomi Google TV** (no Ally-side setup).
  2. Then try **Moonlight on the Xiaomi Google TV + Sunshine** (`ujust setup-sunshine` → `enable-brew`). It's better latency control and the better long-term option.
  - Skip Miracast and Chromecast.
  - LG only via Moonlight homebrew, if you prefer that screen.
- **Windows:** **Moonlight on the Xiaomi Google TV + Apollo (or Sunshine)**. Apollo's virtual display lets the stream run at the TV's resolution independently of the Ally panel. Keep `Win+K` Miracast to the LG for casual, non-gaming use.
- **Either OS, for serious FC 26 play:** The USB-C dock with HDMI. Wireless streaming always adds latency.

## Sources

1. Bazzite `setup-sunshine` ujust recipe (source code): https://github.com/ublue-os/bazzite/blob/main/system_files/desktop/shared/usr/share/ublue-os/just/82-bazzite-sunshine.just
2. Valve, Steam Remote Play / Steam Link supported devices: https://store.steampowered.com/remoteplay
3. Moonlight official site (client platforms; LG webOS = homebrew + Developer Mode): https://moonlight-stream.org/
4. moonlight-tv (LG webOS port): https://github.com/mariotaku/moonlight-tv
5. "Streaming PC to LG TV" (2024, Dev Mode expiry, no Steam Link on webOS): https://kcode.de/blog/2024/2024-06-23-streaming-pc-to-lg-tv/
6. Moonlight Setup Guide (network recommendations): https://github.com/moonlight-stream/moonlight-docs/wiki/Setup-Guide
7. Apollo (Sunshine fork, SudoVDA virtual display, Windows-only): https://github.com/ClassicOldSong/Apollo
8. Steam Community, no Steam Link on LG webOS (2022–2023, stale but uncontradicted): https://steamcommunity.com/discussions/forum/10/3758849893339776873
9. r/cloudygamer latency comparison (2023, anecdotal): https://www.reddit.com/r/cloudygamer/comments/104wu95/quick_latency_comparison_for_inhouse_moonlight/
10. IHSplay (unofficial Steam streaming client for webOS): https://github.com/mariotaku/ihsplay
11. LG, connect a wireless display on Windows 11: https://www.lg.com/us/support/help-library/how-to-connect-a-wireless-display-on-windows-11--20152891013904
12. KDE Discuss, built-in Miracast support request: https://discuss.kde.org/t/built-in-miracast-support-wireless-display-specification/5570
13. GNOME Network Displays (Miracast + Chromecast): https://gitlab.gnome.org/GNOME/gnome-network-displays

**Stale / unverified flags:**
- The "no Steam Link on LG" evidence dates from 2022–2024. Check the LG Content Store on the actual TV.
- Xiaomi Miracast support is model-specific.
- Wi-Fi Direct support for the Ally's Wi-Fi chip under Linux wasn't checked.
- Latency figures are estimates, not measured on this hardware.
