# Decisions — khan007/pdfcraft fork

## 2026-10-07 — Windows builds via a fork-only GitHub Actions workflow
**What:** `.github/workflows/windows-build.yml`, run on demand (`workflow_dispatch`, arch input x64/x86/arm64/all). It reuses `packaging/windows/package.ps1`, without the `release` environment or signing secrets, and uploads an unsigned MSI + portable zip as artifacts.
**Why:** WiX/MSI only runs on Windows; the Mac has only ~8 GB free and Homebrew Rust without rustup; GitHub Actions is free for public repos and uses no local disk.
**Rejected:**
- Running upstream `release.yml` on the fork: its macOS job needs storytold's self-hosted runner and would hang.
- Cross-compiling on the Mac (rustup + cargo-xwin): about 3–5 GB extra, produces a bare `.exe` with no MSI, and the project has never been cross-built.
- A local Windows VM: needs ~40–50 GB of disk.

## 2026-10-08 — Sync upstream by merge, not rebase
**What:** Upstream is merged into the fork's `main` (`git pull upstream main && git push`).
**Why:** The fork's `main` already carries its own commit (the workflow), so rebasing would require force-pushing a published branch.
**Rejected:** Rebase + `--force-with-lease`, because it rewrites published history.
