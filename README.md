# gentoo-unsupervised
Gentoo installer forged 100% by local AI — zero human code, zero cloud, zero API keys. Two open-weights models, one consumer PC.


# ROADMAP — Gentoo Installer Nexus v2.0
> **GR-20:** 1 Blick = kompletter Projektstatus. · Stand: 2026-09-04
> Fakten kanonisch: MEMORY.md `PROJ-10` · Details: `CHANGELOG.md` · Plan: `TASK_SPLIT.md`

## GESAMTFORTSCHRITT: ~45 %  *(Schätzung: T0–T9 = 10 Einheiten; T0–T4 = 5 erledigt)*

| Task | Inhalt | Status | % |
|------|--------|--------|----|
| T0 | Scaffolding (18 Module, Build-System) | ✅ DONE — verifiziert (0/0, `--help` ✓) | 100 |
| T1 | Pre-Flight/Hardware (4 Module, Swap-Heuristik) | ✅ DONE — verifiziert (Funktionstest 11/11) | 100 |
| T2 | Partitionierung (LVM/FS/Partition-Manager) | ✅ DONE — verifiziert (0/0, GPT ✓ + **MBR-FIX via sfdisk ✓**, 2026-09-01) | 100 |
| T3 | Stage3 (GPG→SHA256→Extraction, Guard, 3 pure Helpers) | ✅ DONE — verifiziert (0/0-Build, Harness **39/39** inkl. Live-Pipeline + Negative-Cases, 2026-09-03) | 100 |
| T4 | Chroot (safe-setup, executeInChroot, unmount-Reverse) | ✅ DONE — verifiziert (0/0-Build, Harness **28/28** inkl. Order/Rollback/Guard/Fork-Plumbing, 2026-09-04) | 100 |
| T5/T7 | Desktops MODULAR (DE/WM-Wahl + Installation) | ⬜ offen | 0 |
| T6/T8 | Kernel/Bootloader/System-Config | ⬜ offen | 0 |
| T9 | **Getestete automatische Installation = ERFOLG** | ⬜ offen | 0 |

## NÄCHSTER SCHRITT
Roxanne: **T5-Prompt schreiben** (`subagents/prompts/T5_*.md`, InstallationPipeline: Profile-Check → `emerge --sync` 3× Retry + webrsync-Fallback → make.conf → Kernel (`gentoo-kernel-bin` + `installkernel`) → System-Config; **Größe-Check: T5a (Portage+Kernel) / T5b (System-Config) Split wahrscheinlich**) → Elias läuft ihn auf LWM → Roxanne: Report ZUERST (GR-01) + Clean-Rebuild + Code-Review + Harness → T6.

## BLOCKER / OFFEN
- Keiner hart. sgdisk/sfdisk/gpg/sha256sum/tar = Runtime-Abhängigkeiten (Sandbox = Build + Code-Review + Live-Tests, gpg/sha256sum/tar/xz vorhanden).
- **T4 (2026-09-04):** `subagents/prompts/T4_CHROOT.md` **fehlt** (LOG-26: \"angelegt\"; vermutlich Crash/Revert) — Vertrag rekonstruierbar (LOG-26 + T4-Report + TASK_SPLIT); bei T5-Prep nachlegen (als REKONSTRUIERT markiert).
- **T4-DESIGN-EDGE (Live-Test beobachten):** `chmod 1777 /dev/shm /run/shm` — fehlt `/run/shm` auf Ziel-Host → Setup-Fehler + sauberer Rollback (fail-safe).
- LVM+MBR-Fstab-Vertrag: `/boot vfat` vs. xfs-Boot-Partition = Inkonsistenz im T2-Vertrag → klären in LVM-Phase (T3+).
- T3-Design (2026-09-01): SHA256 = SUMS-Zeile parsen + `sha256sum <datei>` + Hex-Vergleich (**dokumentierte Abweichung** vom Handbook-Literal `--check` → CWD-unabhängig) · GPG-Key-Path = Default-Argument (Produktion = Handbook-Pfad, Tests = Test-Key) · Guard: Extraction nur nach GPG+SHA256 für dieselbe Datei.
