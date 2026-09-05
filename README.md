# gentoo-unsupervised
Gentoo installer forged 100% by local AI — zero human code, zero cloud, zero API keys. Two open-weights models, one consumer PC.

# ROADMAP — Gentoo Installer Nexus v2.0
> **GR-20:** At-a-glance = full project status. · As of: 2026-09-04  
> Canonical source of truth: `MEMORY.md` `PROJ-10` · Details: `CHANGELOG.md` · Plan: `TASK_SPLIT.md`

## OVERALL PROGRESS: ~45% *(Estimate: T0–T9 = 10 units; T0–T4 = 5 done)*

| Task | Scope | Status | % |
|------|-------|--------|---|
| T0 | Scaffolding (18 modules, build system) | ✅ DONE — verified (0/0, `--help` ✓) | 100 |
| T1 | Pre-Flight/Hardware (4 modules, swap heuristics) | ✅ DONE — verified (functional test 11/11) | 100 |
| T2 | Partitioning (LVM/FS/partition manager) | ✅ DONE — verified (0/0, GPT ✓ + **MBR-FIX via sfdisk ✓**, 2026-09-01) | 100 |
| T3 | Stage3 (GPG→SHA256→Extraction, Guard, 3 pure helpers) | ✅ DONE — verified (0/0 build, harness **39/39** incl. live pipeline + negative cases, 2026-09-03) | 100 |
| T4 | Chroot (safe-setup, executeInChroot, unmount reverse) | ✅ DONE — verified (0/0 build, harness **28/28** incl. order/rollback/guard/fork-plumbing, 2026-09-04) | 100 |
| T5/T7 | Desktops MODULAR (DE/WM selection + installation) | ⬜ open | 0 |
| T6/T8 | Kernel/Bootloader/System Config | ⬜ open | 0 |
| T9 | **Tested automated installation = SUCCESS** | ⬜ open | 0 |

## NEXT STEP
Roxanne: **Draft T5 prompt** (`subagents/prompts/T5_*.md`, InstallationPipeline: Profile check → `emerge --sync` 3× retry + webrsync fallback → make.conf → Kernel (`gentoo-kernel-bin` + `installkernel`) → System Config; **Size check: T5a (Portage+Kernel) / T5b (System Config) split likely**) → Elias runs it on LWM → Roxanne: Report FIRST (GR-01) + clean rebuild + code review + harness → T6.

## BLOCKERS / OPEN ITEMS
- None hard-blocking. sgdisk/sfdisk/gpg/sha256sum/tar = runtime dependencies (sandbox = build + code review + live tests; gpg/sha256sum/tar/xz present).
- **T4 (2026-09-04):** `subagents/prompts/T4_CHROOT.md` is **missing** (LOG-26: "created"; likely crash/revert) — contract reconstructible (LOG-26 + T4 report + TASK_SPLIT); backfill during T5 prep (flagged as RECONSTRUCTED).
- **T4 DESIGN EDGE (monitor in live test):** `chmod 1777 /dev/shm /run/shm` — missing `/run/shm` on target host → setup error + clean rollback (fail-safe).
- LVM+MBR fstab contract: `/boot vfat` vs. xfs boot partition = inconsistency in T2 contract → sort out in LVM phase (T3+).
- T3 design (2026-09-01): SHA256 = parse SUMS line + `sha256sum <file>` + hex compare (**documented divergence** from Handbook literal `--check` → CWD-independent) · GPG key path = default arg (production = Handbook path, tests = test key) · Guard: extraction strictly guarded after GPG+SHA256 for the exact same file.



-> translated from german by AI
