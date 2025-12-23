# Quick Reference: Supervisor Sync Guide

**Status:** ⚠️ Ready for sync (backup created)  
**Date:** December 23, 2025  
**Current Version:** 2024.10.04 → **Target:** 2025.12.3

---

## 📋 Quick Status

```
Repository: supervisor (my-smart-homes fork)
Backup Branch: backup-pre-sync-20251223
Commits Behind: 823 commits
Custom Changes: 5 commits (infrastructure customizations)
```

---

## 🚀 Quick Start Sync Commands

```bash
# Verify you're on main
git checkout main

# Merge upstream (preserve custom changes)
git merge upstream/main

# If conflicts, resolve manually keeping:
# - my-smart-homes image URLs
# - Custom version endpoint
# - Custom registry paths

# Push backup first (safety)
git push origin backup-pre-sync-20251223

# Push merged changes
git push origin main
```

---

## ⚠️ Critical Custom Changes to Preserve

| File | Change | Why Critical |
|------|--------|--------------|
| `supervisor/const.py` | Version URL → `my-smart-homes.github.io/version-data/` | Update checks |
| `supervisor/homeassistant/module.py` | Image → `ghcr.io/my-smart-homes/*` | Core container |
| `supervisor/plugins/base.py` | Plugin images → `ghcr.io/my-smart-homes/*` | All plugins |
| `supervisor/updater.py` | Channel validation bypass | Allows custom versions |
| `.devcontainer/devcontainer.json` | Dev container → custom | Development |

---

## 📊 What Changed Upstream (823 commits)

### Major Features
- ✅ Migrated to aiodocker for container operations
- ✅ Removed overlay2 driver support
- ✅ Enhanced type annotations throughout
- ✅ Fixed add-on options validation
- ✅ Improved Core API timeout handling
- ✅ Git store error handling improvements

### Dependency Updates
- Python packages: time-machine, voluptuous, ruff, sentry-sdk, urllib3, etc.
- GitHub Actions: checkout, cache, upload/download-artifact, etc.
- Build tools: uv to v0.9.18

### Breaking Changes
- ⚠️ Removed Codenotary/cosign verification
- ⚠️ Removed overlay2 support
- ⚠️ Stricter type checking

---

## 🛠️ Conflict Resolution Quick Guide

### If you see conflicts in:

**`supervisor/const.py`**
```python
# KEEP THIS:
URL_HASSIO_VERSION = "https://my-smart-homes.github.io/version-data/data.json"
# Accept other upstream changes
```

**`supervisor/homeassistant/module.py`**
```python
# KEEP THIS:
return f"ghcr.io/my-smart-homes/{self.sys_machine}-my-smart-homes"
```

**`supervisor/plugins/base.py`**
```python
# KEEP THIS:
return f"ghcr.io/my-smart-homes/{self.sys_arch.supervisor}-hassio-{self.slug}"
```

**`supervisor/updater.py`**
```python
# KEEP RELAXED VALIDATION (for now):
if not data:
# But add TODO to fix properly later
```

---

## ✅ Post-Merge Verification

```bash
# Run tests
pytest tests/

# Check no unintended changes
git diff backup-pre-sync-20251223 -- supervisor/const.py
git diff backup-pre-sync-20251223 -- supervisor/homeassistant/module.py
git diff backup-pre-sync-20251223 -- supervisor/plugins/base.py

# Verify version endpoint
curl https://my-smart-homes.github.io/version-data/data.json

# Build and test locally if possible
```

---

## 🔄 Rollback if Needed

```bash
# Option 1: Reset to backup
git reset --hard backup-pre-sync-20251223

# Option 2: Create revert commit
git revert <merge-commit-sha>
```

---

## 📚 Detailed Documentation

- **Full sync details:** `SYNC_DOCUMENTATION.md`
- **Custom changes analysis:** `CUSTOM_CHANGES.md`
- **This quick guide:** `SYNC_QUICKREF.md`

---

## 🎯 TODO After Sync

1. [ ] Fix channel validation properly (remove workaround)
2. [ ] Test all custom infrastructure endpoints
3. [ ] Verify container image availability
4. [ ] Run integration tests
5. [ ] Update version tags appropriately
6. [ ] Document any new conflicts encountered

---

## 📞 Important URLs

- Fork: `https://github.com/my-smart-homes/supervisor`
- Upstream: `https://github.com/home-assistant/supervisor`
- Version Data: `https://my-smart-homes.github.io/version-data/data.json`
- Container Registry: `ghcr.io/my-smart-homes/*`

---

**Remember:** Always keep infrastructure URLs pointing to `my-smart-homes`!

---

_Last Updated: December 23, 2025 05:09 UTC_
