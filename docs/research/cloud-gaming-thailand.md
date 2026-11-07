# Cloud gaming options for EA Sports FC 26 in Thailand, and what they cost

Ticket: #3. Researched 2026-11-07.

## Answer

**No cloud service legitimately streams EA Sports FC 26 to Thailand today.** Confidence: high.

- **GeForce NOW (bro.game, the Thai operator):** the service runs from a data centre in Thailand and has clients for Linux, LG webOS and Android TV. But **FC 26 is not in the GeForce NOW catalogue**, so owning it on Steam does not help. I checked NVIDIA's live game-list API for both the US and TH catalogues on 2026-11-07, and no EA Sports FC or FIFA title appears.
- **Xbox Cloud Gaming:** FC 26 *is* cloud-playable through Game Pass Ultimate / EA Play (added 2026-06-18). But **Xbox Cloud Gaming is not offered in Thailand.** It runs in 29 countries, and the nearest are India, Japan and South Korea. Using it would mean a VPN, a foreign account and payment, and 50-100+ ms of latency. That breaks the terms of service and is not recommended.
- **Boosteroid, Shadow and other cloud PCs:** Boosteroid does not list FC 26 and has no servers in Asia. Cloud PCs (Shadow, airgpu and similar) are blocked by EA's kernel anti-cheat, which refuses to run in virtual machines. FC 25 players on Shadow hit "Cannot run in a virtual machine" errors.
- **PlayStation Plus:** Thailand gets the **Deluxe** tier (no cloud streaming), not Premium. TrueID Game Hub (Blacknut) is an Android-game catalogue and does not include FC 26.

**Bottom line on cost:** the cheapest legitimate way to play FC 26 is local, not cloud. **PC Game Pass in Thailand costs ฿129/month and includes EA Play, which now includes FC 26.** That runs on Windows, not in the cloud. (Verified: Thai price, EA Play included, FC 26 added to EA Play. Inferred: whether it runs on Bazzite, which is outside this ticket. EA's anti-cheat blocks most EA online titles on Linux, so check the Bazzite/Proton research note.) If you just want a GeForce NOW subscription for other games, bro.game currently lists Performance at **฿599/month** and Ultimate at **฿999/month**.

## Details

### 1. GeForce NOW in Thailand (bro.game / Brothers Pictures)

**Operator and server location (verified).** GeForce NOW in Thailand is run by **bro.game** (Brothers Pictures), NVIDIA's Thai Alliance partner. It launched in June 2025 and is hosted at **Telehouse Thailand**, inside the country ("ให้บริการบนระบบที่ตั้งในประเทศไทยของ Telehouse Thailand", The Standard, 2025-06-30). Nobody publishes a latency figure. Because the servers are in-country, a good Thai connection should see low single- or double-digit milliseconds. That figure is my inference, not a published one. bro.game also bills through AIS and AIS PLAYBOX / 3BB GIGATV, and True and dtac have run bundles.

**Is FC 26 supported? No (verified, primary source).**
- I enumerated NVIDIA's public game-list API (`https://games.geforce.com/graphql`, the backend of nvidia.com/geforce-now/games) for country `US` (2,272 titles) and `TH` (2,265 titles) on 2026-11-07. Neither list has a title containing "FC", "FIFA" or "EA SPORTS".
- Other EA titles *are* on the service: Battlefield 6, Battlefield 2042/V/1/4 and Apex Legends, with Steam and EA App variants.
- The third-party tracker cloudbase.gg (updated 2026-09-24) also marks FC 26 "Unavailable" on GeForce NOW. EA's FC 26 platform list names PC, PlayStation, Xbox, Switch and Amazon Luna, but not GeForce NOW.
- **Steam ownership does not matter.** GeForce NOW only streams titles the publisher has onboarded. Linking a Steam library does not add FC 26.

**Does EA's anti-cheat block it? Inference.** Battlefield 6 uses EA's Javelin kernel anti-cheat and *is* on GeForce NOW. So the anti-cheat is not a technical barrier on GFN's managed servers. FC 26's absence looks like an EA publishing/licensing decision, not an anti-cheat block. No EA or NVIDIA statement explains it, and NVIDIA's forum has community request threads for EA FC titles. Any home-brewed workaround on a general cloud PC *is* blocked by the anti-cheat (see section 3).

**Price (verified from bro.game/en/plans on 2026-11-07).**

| bro.game plan | Price | Notes |
|---|---|---|
| Performance (monthly) | ฿599 | 1440p/60 fps, standard queue, 100 h/month, 6 h sessions |
| Ultimate (monthly) | ฿999 | 4K/240 fps, DLSS + Reflex, highest queue, 100 h/month, 8 h sessions |
| Day pass (24 h) | ฿99 Performance / ฿199 Ultimate | Per sheapgamer, ~Oct 2026. The plans page has a Day Pass tab but I could not read its prices |
| Extra hours | ฿20-99 per 5/10/15 h depending on tier | Up to 15 unused hours roll over |

Caveat: launch prices in June 2025 were Lite ฿219, Performance ฿399 and Ultimate ฿649. The page now shows ฿599/฿999. Its layout is a tabbed Framer page (Day Pass / 1-Month / 6-Month / 1-Year), and I could not tell for certain which tab those default prices belong to. Confirm at checkout.

**Clients (verified from bro.game/en/download).**

| Device | GeForce NOW (bro.game) |
|---|---|
| Bazzite / Linux | Yes. Official Linux app (beta) and the Steam Deck installer; the browser (play.geforcenow.com) also works |
| LG webOS | Yes. The LG Content Store app is linked from bro.game |
| Xiaomi Google TV | Yes. The Android TV app is on Google Play |

None of these clients matter for FC 26, because the game is not in the catalogue.

### 2. Xbox Cloud Gaming / Game Pass Ultimate / EA Play

- **FC 26 on Game Pass (verified):** FC 26 joined EA Play on **2026-06-18**, for **Game Pass Ultimate and PC Game Pass** but not Premium. It is cloud-playable for Ultimate members in supported countries (Thurrott, 2026-06-16).
- **Thailand not supported (verified):** Xbox Cloud Gaming runs in 29 countries: Argentina, Australia, Austria, Belgium, Brazil, Canada, Czech Republic, Denmark, Finland, France, Germany, Hungary, India, Ireland, Italy, Japan, Mexico, Netherlands, New Zealand, Norway, Poland, Portugal, Slovakia, South Korea, Spain, Sweden, Switzerland, UK and US (Wikipedia, as of 2026-09). A Thai guide from May 2026 (Gamingdose) confirms "Cloud Gaming ไม่รองรับในโซนไทย". India was added in Nov 2025 with Azure servers in Pune and Chennai. I found no Southeast Asia launch announcement.
- **What Thailand can buy (verified):** **PC Game Pass at ฿129/month** including VAT (Gamingdose 2026-05-20; the Xbox TH store snippet shows a ฿35 14-day trial, then ฿129). It includes EA Play, so FC 26 is included, but **it has no cloud gaming**: you download and run the game on a Windows PC through the EA app. Game Pass Ultimate is not sold in Thailand.
- **Workaround (not recommended):** a foreign-region account plus a VPN. Ultimate is about US$29.99/month, roughly ฿1,000 at ~฿33/USD (my conversion). Since 2026-09 there are also monthly cloud-hour caps (Ultimate 15 h, then paid hours at about US$0.24-0.40/h). Nearest servers would be India, Japan or Korea, and I estimate **50-100+ ms** from Bangkok (inference, not measured). That is noticeable for a reaction-heavy football game, and it breaks the ToS.
- **Clients (inference from the platform list):** Linux works through a Chromium/Edge browser at xbox.com/play. LG webOS has an official Xbox app on supported 2022+ models. Xiaomi Google TV has no official Android TV app (Fire TV only), so it would need a sideloaded browser, if it works at all.

### 3. Boosteroid, cloud PCs and others

| Service | FC 26? | Available in Thailand / nearest server | Notes |
|---|---|---|---|
| **Boosteroid** | No (cloudbase.gg tracker; not in the library) | No Asian servers. Its Jan 2026 press release lists 28 server sites in Europe and the Americas | From €7.49/month on annual billing (~฿280, my conversion); Ultra Pro €11.22/month. Apps for Linux, LG webOS and Android TV, but it is moot here |
| **Shadow / airgpu / CloudDeck** (Windows cloud PCs, bring your own game) | No. EA anti-cheat refuses to run in VMs | Shadow has no Asian data centres (inference) | Shadow's support says games using EA Anticheat "cannot run on Shadow PC". FC 25 users on Shadow reported error 137 "Cannot run in a virtual machine" (EA forums) |
| **PlayStation Plus** | Not by cloud in Thailand | Thailand has Deluxe (฿370/month at the 2022 launch), not Premium. "For markets without cloud streaming, PlayStation Plus Deluxe will be offered" | Cloud streaming only works on PS5/Portal anyway |
| **Amazon Luna** | FC 26 was announced for Luna | Not offered in Thailand | n/a |
| **TrueID Game Hub** (Blacknut + Radian Arc, launched May 2026) | No. Android/casual catalogue | Thai edge servers | Packages are bundled with TrueID/True mobile |

## Cost comparison

Prices are monthly in THB. Rows marked * are not usable from Thailand without breaking the ToS, or do not run FC 26 at all.

| Option | Monthly cost (THB) | Must also buy FC 26? | FC 26 playable? | Nearest server / latency | Bazzite/Linux | LG webOS | Xiaomi Google TV |
|---|---|---|---|---|---|---|---|
| GeForce NOW (bro.game) Performance | ฿599 (day pass ฿99) | n/a | **No**: not in catalogue | Telehouse Thailand; low ms (inferred) | Yes (app + browser) | Yes | Yes |
| GeForce NOW (bro.game) Ultimate | ฿999 (day pass ฿199) | n/a | **No** | Same as above | Yes | Yes | Yes |
| Xbox Cloud Gaming via Game Pass Ultimate* | ~฿1,000 (US$29.99, foreign account) | No, FC 26 is in EA Play since 2026-06-18 | Yes, but **not offered in Thailand** | India/Japan/Korea, ~50-100+ ms (inferred) | Browser | Yes (Xbox app) | No official app |
| PC Game Pass (local download, not cloud) | **฿129** | No, included via EA Play | Yes, on a local Windows PC | n/a (local) | Not via cloud; Linux viability depends on EA anti-cheat (separate note) | n/a | n/a |
| Boosteroid* | ~฿280-420 (€7.49-11.22) | Would need to own it | **No** | Europe/Americas only | Yes | Yes | Yes |
| Shadow / cloud Windows PC* | varies (~฿1,000+) | Yes (Steam copy) | **No**: EA anti-cheat blocks VMs | No Asian sites | Yes | Partial | Yes |
| PS Plus Deluxe (Thailand) | ~฿370 (2022 launch price) | n/a | No cloud streaming in Thailand | n/a | No | No | No |

For comparison, buying FC 26 on Steam and playing it locally has no monthly cost. The real choice is local play (ROG Ally/TV dock, or a Windows PC with PC Game Pass at ฿129/month), not cloud.

## Sources

Primary / first-party:
- NVIDIA GeForce NOW game-list API, queried 2026-11-07 for US and TH: https://games.geforce.com/graphql (backend of https://www.nvidia.com/en-us/geforce-now/games/)
- bro.game plans (current prices, hours, add-ons): https://www.bro.game/en/plans
- bro.game downloads (Linux, LG webOS, Android TV clients): https://www.bro.game/en/download
- bro.game system requirements: https://www.bro.game/user-guide/system-requirements
- EA FC 26 release dates and platforms: https://www.ea.com/games/ea-sports-fc/fc-26/news/fc-26-release-dates
- Xbox Store TH, PC Game Pass: https://www.xbox.com/th-TH/games/store/pc-game-pass/cfq7ttc0kgq8
- PlayStation Blog, PS Plus tiers for Southeast Asia (Deluxe, no cloud streaming): https://blog.playstation.com/2022/03/29/20220329-psplus/
- Boosteroid home page (pricing, clients): https://boosteroid.com/
- Boosteroid infrastructure blog (Feb 2025): https://boosteroid.com/blog/2025/02/10/boosteroid-infrastructure-expansion-plans-to-meet-the-growing-demand/
- Shadow, games incompatible with Shadow PC (EA Anticheat): https://support.shadow.tech/hc/en-us/articles/32731823908625-Games-Incompatible-with-Shadow-PC

Secondary (dated):
- The Standard, bro.game GFN launch and Telehouse Thailand hosting (2025-06-30): https://thestandard.co/geforce-now-powered-by-bro-game/
- Sheapgamer, bro.game day pass ฿99/฿199 (~Oct 2026): https://sheapgamer.com/geforce-now-bro-game-day-pass/
- Brand Inside, AIS x GeForce NOW by bro.game: https://brandinside.asia/ais-x-geforce-now-by-brogame/
- Thurrott, FC 26 joining Game Pass Ultimate/PC Game Pass via EA Play on 2026-06-18: https://www.thurrott.com/games/337453/ea-sports-fc-26-and-call-of-duty-vanguard-are-coming-to-game-pass
- Wikipedia, Xbox Cloud Gaming (29 countries; Sep 2026 hour caps): https://en.wikipedia.org/wiki/Xbox_Cloud_Gaming
- Windows Central, Xbox Cloud Gaming in India: https://www.windowscentral.com/gaming/xbox/xbox-cloud-gaming-could-be-gearing-up-for-an-official-launch-in-india
- Gamingdose, PC Game Pass in Thailand at ฿129, no cloud in Thailand (2026-05-20): https://www.gamingdose.com/feature/pc-game-pass-2026/
- cloudbase.gg, FC 26 cloud availability tracker (updated 2026-09-24): https://cloudbase.gg/g/fc-26/
- EA Forums, FC 25 anti-cheat blocking Shadow PC: https://forums.ea.com/discussions/fc-25-technical-issues-en/anti-cheat-issues-fc25---shadow-pc-vm/7845385 and https://forums.ea.com/discussions/fc-25-technical-issues-en/1-week-and-not-solved-error-anti-cheat-from-cloud-gaming-ea-fc-25--shadow-pc-/7869059
- EinPresswire, Boosteroid on Whale TV (Jan 2026; 28 server sites in EU and the Americas): https://www.einpresswire.com/article/880541702/boosteroid-cloud-gaming-service-launches-on-whale-tv
- TrueID Game Hub launch (May 2026): https://ecommercenews.asia/story/trueid-launches-android-cloud-gaming-hub-in-thailand

Stale or unverified flags: the June 2025 bro.game prices (฿219/399/649) have been superseded. The ฿370 PS Plus Deluxe price is from 2022. Latency figures are my estimates, not measurements. THB conversions use ~฿33/USD and ~฿37/EUR.
