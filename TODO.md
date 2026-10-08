# TODO — khan007/pdfcraft (fork of storytold/pdfcraft, released as "PrintCraft")

Personal work state for this fork. Upstream's own plan lives in `plan/STATUS.md` and `ROADMAP.md`.

## Done this session (2026-10-07 → 08)
- Cloned sibling forks into `~/git` with `origin`=khan007, `upstream`=storytold: photocraft, vectorcraft, filmcraft, lightcraft, effectcraft, designcraft (designcraft newly forked).
- Added `upstream` remote to this repo.
- Built macOS release locally: `packaging/macos/package.sh --arch aarch64` → `dist/release/pdfcraft-0.2.1-macos-aarch64.dmg` (19 MB, ad-hoc signed); ran `cargo clean` afterwards.
- Installed `/Applications/PdfCraft.app` (0.2.1, pre-sync code); launches OK.
- Added `.github/workflows/windows-build.yml` (commit bedd839): manual, unsigned Windows MSI + portable zip.
- Merged upstream/main (10 commits, i18n + fixes) into main (5307d75), pushed to fork.
- Ran Windows x64 build (run 37718568586, success, ~18 min); artifacts in `dist/windows/windows-x64/`.

## In progress
- Nothing half-done.

## Up next
1. Test the Windows `.msi` on a real Windows PC or VM (untested — built in CI only).
2. Rebuild/reinstall the macOS app from synced main (installed app predates the upstream merge).
3. Decide what to change in the fork (no own feature work yet) — read `plan/STATUS.md` + `ROADMAP.md` first.
4. Optionally build `arm64` / `x86` Windows via `gh workflow run windows-build.yml -f arch=all`.

## Known issues / gotchas
- **Disk is nearly full** (~8 GB free of 228 GB). Local release build needs ~3.2 GB (1.6 GB `target/` + cargo registry). Run `cargo clean` after packaging. A Windows VM (~40–50 GB) is not possible without freeing space.
- Homebrew Rust (no rustup): only `aarch64-apple-darwin` is installed, so no cross-compiling. `package.sh --arch universal` would fail; use `--arch aarch64`.
- Don't run upstream `release.yml` on the fork: its macOS job targets storytold's runner and will hang. Use `windows-build.yml`.
- WiX/MSI is Windows-only; Windows builds must go through GitHub Actions.
- Fork artifacts are unsigned (no storytold secrets): SmartScreen warns on Windows; the macOS app is ad-hoc signed (works only on this Mac).
- Copying the .app out of the DMG with `ditto` brought `com.apple.FinderInfo`/`provenance` xattrs, which broke `codesign --verify`; fix with `xattr -cr /Applications/PdfCraft.app`.
- Version still reads 0.2.1 after the sync (upstream hasn't bumped it).
- Sync with upstream: `git pull upstream main && git push` (merge, no force-push).
- The upstream repo's CLAUDE.md requires a quality gate before every commit (fmt, clippy -D warnings, tests) and one task id per commit for product code.
