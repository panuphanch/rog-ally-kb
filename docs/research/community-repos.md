# Community repos: mikeroyal/Asus-ROG-Ally-Guide and 0Chencc/MRAB

Ticket: issue #8, "What do the mikeroyal and MRAB repos offer?". Researched 2026-11-07.

## Answer

- **mikeroyal/Asus-ROG-Ally-Guide** is a big link list ("awesome list") for the ROG Ally, written mostly for **Windows 11**. Its last commit was **2024-01-04**, so it is stale. It says nothing about Bazzite, FC 26, the Switch Pro Controller, or getting a picture on a TV through a dock. Its battery section only covers Windows tools. **It has little value for this setup.** At most we could link it as a background reference for hardware specs and docks. **Confidence: high.**
- **0Chencc/MRAB** ("Make Rog Ally Better") **is about the ROG Ally.** It is a small MIT-licensed shell script. It changes how the Ally's buttons are mapped by writing InputPlumber config files: the Control Center and Armoury Crate buttons become Select and Start, and it adds the back paddles. It was **active recently**: v1.1.1 came out 2026-09-20. It is **written for SteamOS, not Bazzite**, and it does not help with FC 26, TV output or battery life. It only changes the Ally's built-in controller. **It is not needed for this setup**, and installing it is not recommended. **Confidence: high** on what MRAB does. **Medium** on how it would behave on Bazzite, because nobody has tested it there.
- **What to adopt:** no code or steps from either repo. Two facts are worth recording. First, Bazzite now runs **InputPlumber** on non-Valve handhelds such as the Ally (checked against Bazzite's own source). Second, button remaps belong in `/etc/inputplumber/...` if the owner ever wants one.

## mikeroyal/Asus-ROG-Ally-Guide

| Item | Finding (verified via GitHub API, 2026-11-07) |
|---|---|
| What it is | "A Community Guide to getting all your Accessories, Games, Emulators, and Gaming Tips for your Asus ROG Ally". It has these pages: README, Getting Started, Accessories, Glossary, and Games and Software (Windows Gaming, Linux Gaming, Operating Systems). |
| Last commit on `main` | **2024-01-04** ("Updated Maintenance."). `pushed_at` is 2024-05-11, but `main` has nothing newer than January 2024. The repo's `updated_at` (2026-11-05) only changes when someone stars it, so it does not mean the content is maintained. The README's own badge says "maintenance: yes 2024". |
| Stars / archived | 383 stars. Not archived. |
| License | No LICENSE file (the API reports `null`). The README says the guide is distributed under **CC BY 4.0**. |
| Target OS | Windows 11 and Armoury Crate (see the repo topics and the specs section). The Linux pages are from 2023: they mention ChimeraOS v43 and Nobara and never mention Bazzite. |

How it relates to our goals (based on a grep of README, Getting Started, Accessories, Linux Gaming and Operating Systems):

- **FC 26:** not mentioned. The only EA content is a 2023 Game Pass / EA Play list for Windows.
- **TV output / dock:** Accessories has a list of 2023-era docks (JSAUX, Ugreen, Anker 341, and others) and the ROG Gaming Charger Dock. It has no guidance on HDMI, resolution, HDR, or TV setup, and nothing for Linux.
- **Switch Pro Controller:** not mentioned.
- **Battery life:** Getting Started has a "Battery Health" section, but it only covers Windows tools (MyASUS, `powercfg /batteryreport`, Microsys Smarter Battery). It also links ASUS's article "How to extend the battery life of your ROG Ally", which is about Windows and Armoury Crate. None of this applies on Bazzite.

Worth adopting: nothing directly. If we want a background link, we could add it under a "further reading (Windows-era, 2023)" note. We would need to say it is stale and Windows-only.

## 0Chencc/MRAB

| Item | Finding (verified via GitHub API, 2026-11-07) |
|---|---|
| What it is | "Make Rog Ally Better - Optimization Solutions for SteamOS". It is a one-shot `install.sh` (594 lines) with a matching `uninstall.sh` that writes InputPlumber capability maps and device profiles. |
| Supported hardware | ROG Ally (RC71L) and ROG Ally X (RC72LA). The original Ally is RC71L, so **it does target the owner's device**. |
| Activity | Created 2025-11-08. Releases: v1.0.0 (2025-11-08), v1.0.1 (2025-11-09), v1.1.0 (2026-09-18), v1.1.1 (2026-09-20). Last commit 2026-09-20: "fix: keep all Ally inputs in one controller across reconnects". 6 commits in total. 5 stars. It is a one-person project. |
| License | **MIT**. |
| Requirements | The script checks three things before doing anything: the `inputplumber` binary exists, `inputplumber.service` exists, and `/usr/share/inputplumber/schema/capability_map_v2.json` exists. It quits if any of them is missing. It does **not** check which OS is running. |

What it changes, according to its README:

- Control Center short press becomes Select, and Armoury Crate short press becomes Start.
- The original Select button becomes Guide, and the original Start button becomes Quick Access.
- Long presses become Keyboard and Quick Access 2.
- The rear paddles become Left/Right Paddle 1.
- The remap only applies to the built-in controller. The README says external controllers keep their own layout.
- Files go to `/etc/inputplumber/capability_maps.d/` and `/etc/inputplumber/devices.d/`, with backups in `/var/lib/mrab/backup`.
- It also adds `/etc/atomic-update.conf.d/mrab.conf` so that SteamOS updates do not wipe the files. That mechanism is specific to SteamOS 3.6 and later.
- It started from a Valve SteamOS issue: ValveSoftware/SteamOS#1937.

### Does it apply to Bazzite?

- **Verified:** current Bazzite installs `inputplumber` in its handheld (deck) image. Its `bazzite-hardware-setup` script runs `systemctl enable --now inputplumber.service` (along with `powerstation`) on "Other handheld" (non-Valve) hardware. A code search of the Bazzite repo finds no `hhd` references. So the ROG Ally on Bazzite runs the same input daemon that MRAB configures.
- **Inference (not tested):** MRAB's checks would probably pass on Bazzite. Bazzite's `/etc` is writable and persists across rpm-ostree updates, so the SteamOS-only `atomic-update.conf.d` file would be harmless but useless. We did not confirm whether Bazzite's InputPlumber version ships the `capability_map_v2.json` schema. Bazzite also ships its own InputPlumber device files (for example `60-cec.yaml`), and how those interact with MRAB is unknown.
- **Recommendation:** do not install it. It is unofficial, it is not tested on Bazzite, and it installs by piping a script from the internet into root (`curl … | sudo bash`). It only changes face-button ergonomics, which Steam Input / Bazzite already handle. If the owner later wants Start and Select on the top buttons, Steam Input's per-controller layout is the safer first step.

How it relates to our goals:

- **FC 26:** no. Button mapping has no effect on whether the game runs or how well.
- **TV output:** no.
- **Switch Pro Controller:** no direct effect. MRAB's external-controller guard targets controllers that report the Xbox 360 ID `045e:028e`, the same ID the original Ally uses. The Switch Pro Controller reports Nintendo's `057e:2009` (inference from the standard Nintendo USB ID), so the guard should not touch it either way.
- **Battery life:** no. The script contains no TDP, power or charge-limit logic.

Worth adopting: only the facts above, that Bazzite on the Ally uses InputPlumber and that user overrides live in `/etc/inputplumber/`. They are useful in any future controls or input troubleshooting page.

## Sources

- https://github.com/mikeroyal/Asus-ROG-Ally-Guide (GitHub API: repo metadata, commits on `main`, README, `Getting Started.md`, `Accessories.md`, `Games and Software/Linux Gaming.md`, `Games and Software/Operating Systems.md`)
- https://github.com/0Chencc/MRAB (GitHub API: repo metadata, commits, releases, README.md, install.sh read as text only and not run)
- https://github.com/0Chencc/MRAB/releases
- https://github.com/ValveSoftware/SteamOS/issues/1937 (cited by MRAB, not reviewed in depth)
- https://github.com/ublue-os/bazzite/blob/main/build_files/bazzite-deck/03-install-packages.sh (installs `inputplumber`, `steamos-manager-powerstation`)
- https://github.com/ublue-os/bazzite/blob/main/system_files/desktop/shared/usr/libexec/bazzite-hardware-setup (enables InputPlumber on non-Valve handhelds; latest Bazzite commit checked 2026-11-06)
- https://github.com/ShadowBlip/InputPlumber (the input daemon MRAB configures)
