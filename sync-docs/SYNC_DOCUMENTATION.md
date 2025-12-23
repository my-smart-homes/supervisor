# Supervisor Repository Sync Documentation

**Date:** December 23, 2025  
**Repository:** my-smart-homes/supervisor  
**Sync Type:** Upstream merge from home-assistant/supervisor

---

## 1. Repository Status Before Sync

### Current State
- **Current Version:** 2024.10.04
- **Upstream Version:** 2025.12.3
- **Branch:** main
- **Last Sync Commit:** 5a84f9ae5 "Merge branch 'home-assistant:main' into main"

### Backup Information
- **Backup Branch:** `backup-pre-sync-20251223`
- **Backup Point:** commit 84eb0fdba (2024.10.04)

### Fork-Specific Commits
The following custom commits exist in the fork (not in upstream):
1. `84eb0fdba` - update
2. `5a84f9ae5` - Merge branch 'home-assistant:main' into main
3. `c3f61260c` - update
4. `58ce96ea1` - update
5. `159684e7b` - update

### Upstream Remote
- **URL:** https://github.com/home-assistant/supervisor.git
- **Status:** Added and fetched successfully
- **Commits Behind:** 823 commits

---

## 2. Major Changes in Upstream (2024.10.04 → 2025.12.3)

### Version History
Major version releases between fork and upstream:
- 2024.10.x series (already in fork)
- 2024.11.x series (4 releases)
- 2024.12.x series (3 releases)
- 2025.01.x series (2 releases)
- 2025.02.x series (4 releases)
- 2025.03.x series (4 releases)
- 2025.04.x series (1 release)
- 2025.05.x series (5 releases)
- 2025.06.x series (2 releases)
- 2025.07.x series (3 releases)
- 2025.08.x series (3 releases)
- 2025.09.x series (3 releases)
- 2025.10.x series (1 release)
- 2025.11.x series (6 releases)
- 2025.12.x series (3 releases, latest: 2025.12.3)

### Statistical Overview
- **Total Commits:** 823 new commits
- **Files Changed:** 4,358 files
- **Lines Added:** 33,534 insertions
- **Lines Removed:** 510,310 deletions (major refactoring/cleanup)

### Key Feature Categories

#### 1. Docker/Container Management
- Migrated container operations to aiodocker library
- Removed support for `overlay2` driver (moved to `overlay` only)
- Disabled timeout for Docker image pull operations
- Improved progress tracking for containerd snapshotter
- Fixed missing metadata of stopped add-ons after aiodocker migration

#### 2. Core Integration
- Increased timeout waiting for Core API (workaround for 2025.12.x issues)
- Added option to Core settings to enable duplicated logs
- Better error handling for Core API communication

#### 3. Add-on System
- Fixed addon options reset to defaults issue
- Improved type annotations in addon options validation
- Removed unknown errors from addons and auth modules
- Better handling of addon metadata

#### 4. Git/Store Management
- Handle missing origin remote in git store pull operation
- Fixed issues with git directory handling

#### 5. Security & Dependencies
- Removed Codenotary/cosign verification from container
- Updated securetar library (2025.2.1 → 2025.12.0)
- Bumped base Docker image to 2025.12.2

#### 6. Type Safety & Code Quality
- Extensive type annotation improvements across the codebase
- Fixed type annotations in:
  - API modules
  - AddonModel
  - NetworkManager D-Bus integration
  - Middleware methods
- Removed tests/utils/test_codenotary.py (128 lines)

#### 7. Dependency Updates
Major library updates include:
- Python packages:
  - time-machine: 3.1.0 → 3.2.0
  - voluptuous: 0.15.2 → 0.16.0
  - ruff: 0.14.7 → 0.14.10
  - sentry-sdk: 2.46.0 → 2.48.0
  - mypy: 1.19.0 → 1.19.1
  - urllib3: 2.5.0 → 2.6.2
  - aiodns: 3.5.0 → 3.6.1
  - backports-zstd: 1.1.0 → 1.2.0
  - pytest: 9.0.1 → 9.0.2
  - orjson: 3.11.4 → 3.11.5
  - blockbuster: 1.5.25 → 1.5.26
  - coverage: 7.12.0 → 7.13.0
  - debugpy: 1.8.17 → 1.8.19
  - pre-commit: 4.5.0 → 4.5.1

- GitHub Actions:
  - actions/checkout: 6.0.0 → 6.0.1
  - actions/stale: 10.1.0 → 10.1.1
  - actions/cache: 4.3.0 → 5.0.1
  - actions/upload-artifact: 5.0.0 → 6.0.0
  - actions/download-artifact: 6.0.0 → 7.0.0
  - peter-evans/create-pull-request: 7.0.9 → 8.0.0
  - dessant/lock-threads: 5.0.1 → 6.0.0
  - codecov/codecov-action: 5.5.1 → 5.5.2

- Build tools:
  - uv: bumped to v0.9.18

#### 8. CI/CD Improvements
- Moved wheels build to the build job
- Use ARM runner for aarch64 build
- Avoid getting changed files for releases

#### 9. D-Bus/NetworkManager
- Fixed typing issues in NetworkManager D-Bus integration

---

## 3. Potential Conflicts & Concerns

### Custom Changes Review Required
The 5 custom "update" commits in the fork need to be reviewed to determine:
1. What changes they contain
2. Whether they conflict with upstream changes
3. If they need to be preserved or can be dropped

### Breaking Changes to Watch
1. **Overlay2 removal** - If custom code depends on overlay2, it will break
2. **Codenotary removal** - Security verification process changed
3. **aiodocker migration** - Docker API calls may have changed signatures
4. **Type annotation strictness** - Mypy checks are now stricter

### Merge Strategy Considerations
Given 823 commits and massive refactoring:
- **Option 1:** Clean merge - May require resolving conflicts in custom commits
- **Option 2:** Rebase custom changes - Cleaner history but higher risk
- **Option 3:** Cherry-pick custom changes onto upstream - Most control

---

## 4. Recommended Sync Steps

### Pre-Sync Checklist
- [x] Create backup branch (`backup-pre-sync-20251223`)
- [x] Add upstream remote
- [x] Fetch upstream tags and branches
- [ ] Review custom commits for important changes
- [ ] Check for custom configuration files
- [ ] Document any fork-specific features

### Sync Process
1. Checkout main branch
2. Review and document custom commits
3. Choose merge strategy based on custom commit review
4. Execute merge/rebase
5. Resolve conflicts (if any)
6. Test critical functionality
7. Update version tags
8. Push changes to origin

### Post-Sync Verification
- [ ] Verify all custom features still work
- [ ] Run test suite
- [ ] Check dependency compatibility
- [ ] Verify Docker operations
- [ ] Test add-on management
- [ ] Validate API endpoints

---

## 5. Rollback Plan

If issues arise after sync:

```bash
# Return to backup branch
git checkout backup-pre-sync-20251223

# Force main to backup state (DANGEROUS - use with caution)
git branch -D main
git checkout -b main
git push origin main --force

# Or create a revert
git checkout main
git revert --no-commit <bad-merge-commit>..HEAD
git commit -m "Revert sync to 2025.12.3"
```

---

## 6. Related Repositories

This sync is part of a larger smart home project update:
- **core-updated** - Already updated
- **frontend-updated** - Already updated  
- **operating-system** - Pending (should be done first)
- **supervisor** - Current repository

### Recommended Order
1. ✅ Core
2. ✅ Frontend
3. ⏳ Operating System (do first before supervisor)
4. 📍 **Supervisor** (current - do after OS)

---

## 7. Contact & References

- **Upstream Repository:** https://github.com/home-assistant/supervisor
- **Fork Repository:** https://github.com/my-smart-homes/supervisor
- **Home Assistant Documentation:** https://www.home-assistant.io/
- **Supervisor Documentation:** https://github.com/home-assistant/supervisor/tree/main/docs

---

## 8. Notes

- The massive line deletion (510k lines) is likely due to:
  - Code refactoring and cleanup
  - Removal of deprecated features
  - Test file restructuring
  - Dependency file reorganization

- Key security change: Removed Codenotary verification - verify if this affects your deployment security requirements

- The wheels/.gitkeep addition suggests build process changes for Python wheels

---

**Document Version:** 1.0  
**Last Updated:** December 23, 2025 05:09 UTC
