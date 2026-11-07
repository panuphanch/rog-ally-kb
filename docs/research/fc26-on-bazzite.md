# EA Sports FC 26 on Bazzite (original ROG Ally)

Ticket: issue #2, "Can EA Sports FC 26 run on Bazzite?". Researched 2026-11-07.

## Answer

**No. The Steam version of FC 26 does not legitimately run on Bazzite, and that includes the offline modes (Career, Kick-Off, Tournaments).** Confidence: **high**.

- **Why:** EA Javelin Anticheat is a kernel-level, client-side anti-cheat with no Linux or Proton support. It loads whenever the game launches, whatever mode you then pick. EA says plainly that it protects single-player modes on purpose. The launch itself is blocked, so the offline modes never get a chance to run.
- **Valve's rating:** Steam's own compatibility API rates FC 26 (app 3405690) as **Unsupported** on Steam Deck, on SteamOS and on Steam Machine, with the reason "SteamOS does not support". GamingOnLinux lists its anti-cheat status as **Broken**.
- **Will it change?** Only a March 2026 EA job ad says EA will "chart a path" to Linux and Proton support. Nothing has been announced. Do not plan around it.
- **The only workaround:** the third-party **FC 26 Live Editor**, which bypasses Javelin for offline play. It is a Windows modding tool. Nobody has verified that it works under Proton, so on Bazzite it is untested and would have to be layered on a launch that EA already blocks. It also means bypassing anti-cheat while signed in to your linked EA account. The ban risk for offline-only use is unquantified but real. **Not recommended.**
- **Practical recommendation (inference):** if FC 26 is the main game, the Ally should run **Windows 11**, where FC 26 runs as a normal Steam/EA app game. Expect roughly 60 fps at 900p-1080p on Low/Medium settings, based on FC 25 data, since no FC 26 Ally benchmark exists.
- **Refund caveat:** don't buy FC 26 to "test it on Bazzite". It will not launch. And per the Steam store page, **Steam refunds for this game also take EA account playtime into account**.

## Details

### 1. EA Javelin Anticheat on Linux/Proton

**Verified facts:**
- The Steam store page for FC 26 lists **"Uses Kernel Level Anti-Cheat"** and **"EA Javelin Anticheat - Requires manual removal after game uninstall"**. It also lists Denuvo Anti-tamper.
- From EA's "Deep Dive on EA anticheat" (EA Security; written for the FIFA 23 launch, so about 2022, but still the official FAQ):
  - "EA anticheat only runs when a game with EA anticheat protection included is running."
  - "Protecting single player game modes is necessary to hamper cheat development." Cheat makers "use single player game modes to reverse-engineer a game".
  - If you uninstall EA anticheat, games that require it "will not be playable until EA anticheat is reinstalled".
- **So Javelin gates the game binary, not just the online modes.** No EA-supported route exists to launch FC 26 into Career or Kick-Off without it.
- A fan reproduction of EA's FC 26 FAQ (fifauteam.com) says "FC 26 on PC can be played offline with EAAC installed". In other words, offline play still needs the anti-cheat. It does not mean offline play works without it.
- GamingOnLinux (Apr 2025): EA renamed its anti-cheat to Javelin. It is used across EA SPORTS FC, Madden, Battlefield, F1, WRC and others, and it blocks Linux and Steam Deck **intentionally**.
- GamingOnLinux (4 Mar 2026): an EA job listing for a Senior Anti-Cheat Engineer includes the line "Chart a path for EA Javelin Anticheat to support additional OS and hardware in the future, such as Linux and Proton." The listing is mainly about ARM64 on Windows. This is a stated aspiration with no timeline or product attached.
- Community reports (EA forums, Steam forums) quote the error shown when the game is launched under Proton: *"Wine, Proton, and Steam Deck are not supported by this application!"* (EA forum thread about Nobara/Fedora; the page was 403 to my fetch, so I took the quote from search snippets). Search results also cite an EA community manager saying FC 26 "is not compatible with Linux like other games with EA Javelin anticheat". I could not fetch that page directly, so treat the wording as close to the original but unverified.

### 2. ProtonDB and Steam Deck verification

- **Steam Deck / SteamOS verification (verified, Valve API, fetched 2026-11-07):** `ajaxgetdeckappcompatibilityreport?nAppID=3405690` returns category 1 = **Unsupported** for Deck, SteamOS and Steam Machine, with the reason `SteamOSDoesNotSupport`.
- **GamingOnLinux item DB:** Steam Deck "Unsupported"; anti-cheat "Broken", EA Javelin Anticheat (status last updated 2025-09-18).
- **ProtonDB (verified via its summary API):** tier **Silver**, trending Gold, best report Platinum, 17 reports, confidence "good". **Treat this with caution.** I could not read the individual reports. Given everything above, these reports most likely come from people who used the Live Editor anti-cheat bypass, or who only reached menus or the launcher (inference). A Silver rating does **not** mean the game works out of the box. Every other source, including Valve's own rating, says it does not boot.
- The vaporlens.app review aggregator also summarises Steam reviews as calling Linux/Deck "Broken / Unsupported" because Javelin blocks Proton/Wine.

### 3. EA app and account linking (verified, Steam store page)

- "Incorporates 3rd-party DRM: EA online activation and EA app software installation and background use required."
- You need an internet connection, an EA Account and the EA app. **Your EA and Steam accounts must be linked to play.** The game is limited to one EA and one Steam account and is non-transferable.
- "Steam refunds consider EA account playtime."
- Offline modes on Windows: per fifplay's FAQ, Career, Tournaments, Kick Off, Practice Arena and Skill Games don't need a connection. Some Steam reviews, as summarised by vaporlens, complain that Career needs a connection to start or advance. First activation and EA app sign-in definitely need internet. Expect to be online at least at launch; fully offline play after that is not guaranteed (inference).

### 4. Workarounds and ban risk

| Option | Works on Bazzite? | Risk |
|---|---|---|
| Plain Proton / Proton-GE / Experimental | **No.** Javelin refuses to launch (verified by Valve rating, GOL, user reports). | None, it just fails. |
| **FC 26 Live Editor** (xAranaktu, GitHub/Patreon; v26.3.1 is the latest seen) | **Unverified.** The README lists "EAAC/EA Javelin Anticheat Bypass – Runs the game without the anticheat. Offline use only." It is a PC/Windows tool, and I found no report of it working under Proton. One GamingOnLinux commenter (Mar 2026) says Career can be played that way, but did not say which OS. | Bypassing anti-cheat is against EA's terms. The README doesn't mention bans. No hard data on bans for offline-only use exists, but because the EA account is linked, a ban would hit the account itself (inference). There is also a supply-chain risk from running a third-party injector. |
| "Force-disable anti-cheat" tools from piracy sites (Steam forum, Jul 2025) | Claimed by one user to drop into offline mode under Proton. Unverified. | Piracy-site software = malware risk plus ToS violation. **Do not do this.** |
| Cloud streaming (GeForce NOW etc.) | FC 26 does **not** appear to be on GeForce NOW (clouddosage.com, undated). GFN availability in Thailand is also not covered here. | N/A |
| Dual-boot Windows | Excluded by the owner's one-OS constraint. | - |

**Bottom line:** no clean, low-risk workaround exists. On Bazzite, FC 26 is a no.

### 5. Fallback baseline: FC 26 on the Ally under Windows

- **Requirements (Steam page):** minimum Ryzen 5 1600 / RX 570 / 8 GB RAM; recommended Ryzen 7 2700X / RX 5600 XT / 12 GB RAM; 100 GB storage; Windows 10/11, DX12. The Z1 Extreme's 780M iGPU sits around or below the minimum GPU tier, but the game is light (inference).
- **No FC 26-specific Ally benchmark found.** The closest data:
  - **FC 25 on the ROG Ally** (The Backdash, Sep 2024): 900p fullscreen, **15 W** Performance preset (18 W optional), 60+ fps baseline with FPS cap off to use the 120 Hz VRR panel. Settings: V-Sync Off, Dynamic Resolution Off, Rendering Quality Medium, Grass Medium/Low, Crowd Low, Cloth Medium, AO Medium/Low, Strand Hair Off, Motion Blur Off, Cutscenes Full Frame Rate. AMD RIS On, RSR Off, AFMF off. VRAM 4-6 GB. About 2 h battery on the original Ally.
  - **FC 26 handheld guide** (deltiasgaming.com, covers the ROG Ally): Render Scale 100, Dynamic Resolution Scaling off, claims 60+ fps. I could not fetch the full settings table.
  - **FC 26 Steam Deck guide** (Sportskeeda): 60 fps cap, V-Sync off, render scale 100.
- FC 26 runs on the same engine generation as FC 25, so similar results are a reasonable expectation (inference). **Starting point:** 1080p or 900p, Medium/Low, 15-25 W, 60 fps cap. Docked to a TV at 1080p, use 25-30 W.
- **Windows caveats seen in 2025-26 EA forum threads:** some Javelin launch crashes, including a reported incompatibility with Windows 11 25H2, plus FPS-drop threads, and one user calling it "super slow" on the ROG Xbox Ally X. All are anecdotal.

### 6. Steam's 2-hour refund policy

- Steam refunds games bought within **14 days** and played for **under 2 hours** (Steam refund page).
- For FC 26 specifically, the store page says **"Steam refunds consider EA account playtime"**, so playtime tracked by EA may count as well.
- **Relevance:** on Bazzite a test launch is pointless, because the outcome is already known (Javelin block). If the owner switches to Windows, buying and testing within 2 hours / 14 days is a reasonable way to check performance on the Ally, keeping in mind that EA-side playtime counts.

## Sources

- Steam store page, FC 26 (app 3405690): https://store.steampowered.com/app/3405690/
- Valve Deck/SteamOS compatibility API: https://store.steampowered.com/saleaction/ajaxgetdeckappcompatibilityreport?nAppID=3405690
- ProtonDB summary API: https://www.protondb.com/api/v1/reports/summaries/3405690.json (page: https://www.protondb.com/app/3405690)
- GamingOnLinux item DB, FC 26: https://www.gamingonlinux.com/itemdb/app/43028
- GamingOnLinux, EA Javelin job listing (2026-03-04): https://www.gamingonlinux.com/2026/03/ea-javelin-anticheat-job-listing-mentions-future-support-for-linux-and-proton
- GamingOnLinux, Javelin rebrand still blocks Linux (2025-04-30): https://gamingonlinux.com/2025/04/ea-rebrand-and-refresh-their-anti-cheat-into-ea-javelin-anticheat-still-blocks-linux-steam-deck
- EA Security, "A Deep Dive on EA anticheat for PC": https://www.ea.com/security/news/eaac-deep-dive
- fifauteam.com, FC 26 anticheat FAQ reproduction: https://fifauteam.com/?p=321126
- Steam Community, "Steam Deck / Linux" thread (Jul-Sep 2025): https://steamcommunity.com/app/3405690/discussions/0/595154681972445025/
- EA forums, FC 26 won't launch on Linux (Nobara/Fedora) (seen in search results only, 403): https://forums.ea.com/discussions/fc-26-technical-issues-es/ea-fc-26-no-inicia-en-linux-nobara-43--fedora-desde-epic-games/13554267
- EA forums, Steam Deck user (seen in search results only, 403): https://forums.ea.com/discussions/fc-26-technical-issues-en/steam-deck-user/13771624
- PCGamesN, FC 26 on Steam Deck (2025-09-25): https://www.pcgamesn.com/ea-sports-fc-26/steam-deck
- vaporlens.app review summary: https://vaporlens.app/app/3405690/ea_sports_fc™_26.md
- FC 26 Live Editor (GitHub): https://github.com/xAranaktu/FC-26-Live-Editor
- FC 26 Live Editor v26.3.1 (Patreon): https://www.patreon.com/posts/155852881
- fifplay FC 26 FAQ (offline modes): https://www.fifplay.com/fc-26-faq/
- The Backdash, best EA FC 25 ROG Ally settings (2024-09-22): https://thebackdash.com/tech/best-ea-fc-25-rog-ally-settings/
- Deltias Gaming, FC 26 handheld/ROG Ally settings: https://deltiasgaming.com/?p=351914
- Sportskeeda, best FC 26 Steam Deck settings: https://widgets.sportskeeda.com/gaming-tech/best-ea-fc-26-settings-steam-deck
- EA forums, FC 26 on ROG Xbox Ally X (search snippet only): https://forums.ea.com/discussions/fc-26-feedback-en/fc-26-on-rog-xbox-ally-x/13022953
- clouddosage.com, FC 26 cloud availability: https://clouddosage.com/games/ea-sports-fc-26/
- Steam refund policy: https://store.steampowered.com/steam_refunds/
