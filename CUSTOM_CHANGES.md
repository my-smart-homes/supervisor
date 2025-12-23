# Custom Fork Changes Documentation

**Repository:** my-smart-homes/supervisor  
**Date:** December 23, 2025  
**Purpose:** Document custom modifications made to fork before upstream sync

---

## Overview

This document details the 5 custom commits in the fork that differ from upstream. These changes customize the Home Assistant Supervisor to use custom infrastructure (my-smart-homes organization) instead of the official Home Assistant infrastructure.

---

## Custom Commits Summary

### 1. Commit 159684e7b (October 8, 2024)
**Message:** "update"  
**Author:** Muhtasim Fuad  
**Changes:** Builder workflow modifications

**Files Modified:**
- `.github/workflows/builder.yml` (35 lines changed)
- `build.yaml` (2 lines changed)

**Purpose:** Configure GitHub Actions builder workflow for custom organization

---

### 2. Commit 58ce96ea1 (October 8, 2024)
**Message:** "update"  
**Author:** fuadnafiz98  
**Changes:** Additional builder workflow tweaks

**Files Modified:**
- `.github/workflows/builder.yml` (8 lines changed)

**Purpose:** Further adjustments to builder configuration

---

### 3. Commit c3f61260c (October 8, 2024)
**Message:** "update"  
**Author:** fuadnafiz98  
**Changes:** Minor adjustments to addon build and testing

**Files Modified:**
- `supervisor/addons/build.py` (1 line)
- `supervisor/homeassistant/module.py` (1 line)
- `tests/conftest.py` (1 line)

**Purpose:** Small refinements to addon building and test configuration

---

### 4. Commit 5a84f9ae5 (October 8, 2024)
**Message:** "Merge branch 'home-assistant:main' into main"  
**Author:** Md. Muhtasim Fuad  
**Type:** Merge commit

**Files Modified:**
- `.github/workflows/builder.yml`
- `.github/workflows/ci.yaml`
- `.github/workflows/release-drafter.yml`
- `.github/workflows/sentry.yaml`

**Purpose:** Previous merge from upstream (GitHub Actions version bumps)

---

### 5. Commit 84eb0fdba (October 16, 2024) - **MOST IMPORTANT**
**Message:** "update"  
**Author:** fuadnafiz98  
**Tag:** 2024.10.04  
**Current:** HEAD of main branch

**Files Modified:**
- `.devcontainer/devcontainer.json`
- `supervisor/const.py`
- `supervisor/homeassistant/module.py`
- `supervisor/plugins/base.py`
- `supervisor/updater.py`
- `tests/test_validate.py`

---

## Detailed Analysis of Key Changes (Commit 84eb0fdba)

### 1. DevContainer Configuration
**File:** `.devcontainer/devcontainer.json`

```json
- "image": "ghcr.io/home-assistant/devcontainer:supervisor",
+ "image": "ghcr.io/my-smart-homes/devcontainer:supervisor",
```

**Impact:** Development environment now uses custom container image  
**Preservation:** ✅ REQUIRED - Custom infrastructure dependency

---

### 2. Version Update URL
**File:** `supervisor/const.py`

```python
- URL_HASSIO_VERSION = "https://version.home-assistant.io/{channel}.json"
+ # URL_HASSIO_VERSION = "https://version.home-assistant.io/{channel}.json"
+ URL_HASSIO_VERSION = "https://my-smart-homes.github.io/version-data/data.json"
```

**Impact:** 
- Supervisor now checks for updates from custom version data endpoint
- Critical for maintaining independent version control
- Original URL commented out for reference

**Preservation:** ✅ REQUIRED - Core customization for version management

---

### 3. Home Assistant Docker Image
**File:** `supervisor/homeassistant/module.py`

```python
- return f"ghcr.io/my-smart-homes/{self.sys_machine}-homeassistant"
+ return f"ghcr.io/my-smart-homes/{self.sys_machine}-my-smart-homes"
```

**Impact:** 
- Changes default Home Assistant Core container image name
- From: `{machine}-homeassistant`
- To: `{machine}-my-smart-homes`

**Preservation:** ✅ REQUIRED - Points to custom Core builds

---

### 4. Plugin Container Images
**File:** `supervisor/plugins/base.py`

```python
- return f"ghcr.io/home-assistant/{self.sys_arch.supervisor}-hassio-{self.slug}"
+ # return f"ghcr.io/home-assistant/{self.sys_arch.supervisor}-hassio-{self.slug}"
+ return f"ghcr.io/my-smart-homes/{self.sys_arch.supervisor}-hassio-{self.slug}"
```

**Impact:**
- All plugins (DNS, Audio, CLI, Observer, Multicast) now pull from my-smart-homes
- Original line commented for reference

**Preservation:** ✅ REQUIRED - Custom plugin infrastructure

---

### 5. Channel Validation Bypass
**File:** `supervisor/updater.py`

```python
- if not data or data.get(ATTR_CHANNEL) != self.channel:
+ # TODO: have to fix later with different channel
+ # if not data or data.get(ATTR_CHANNEL) != self.channel:
+ if not data:
```

**Impact:**
- Removes channel validation (stable/beta/dev) from update checks
- Allows custom version endpoint without strict channel matching
- Has TODO note indicating this is a workaround

**Preservation:** ⚠️ REVIEW NEEDED - Temporary workaround that should be fixed properly

**Risk:** May allow incompatible version updates if not handled correctly

---

### 6. Test Updates
**File:** `tests/test_validate.py`

```python
# Multiple test assertions updated
- "ghcr.io/home-assistant/{machine}-homeassistant"
+ "ghcr.io/my-smart-homes/{machine}-homeassistant"
```

**Impact:** Updates test cases to match new image naming conventions

**Preservation:** ✅ REQUIRED - Tests must match implementation

---

## Infrastructure Dependencies

The custom fork relies on these external resources from my-smart-homes organization:

### Container Images Required
1. `ghcr.io/my-smart-homes/devcontainer:supervisor` - Development environment
2. `ghcr.io/my-smart-homes/{machine}-my-smart-homes` - Core containers
   - Where {machine} = amd64, aarch64, armhf, armv7, i386
3. `ghcr.io/my-smart-homes/{arch}-hassio-{plugin}` - Plugin containers
   - dns, audio, cli, observer, multicast
   - Where {arch} = amd64, aarch64, armhf, armv7, i386

### API Endpoints Required
1. `https://my-smart-homes.github.io/version-data/data.json` - Version update information

---

## Merge Strategy Recommendations

### Option 1: Preserve All Custom Changes (RECOMMENDED)
**Approach:**
```bash
git checkout main
git merge upstream/main
# Resolve conflicts preserving all custom infrastructure changes
```

**Pros:**
- Maintains custom infrastructure
- All version checks point to my-smart-homes
- Tests remain aligned

**Cons:**
- May require conflict resolution
- Need to ensure custom infrastructure is ready

---

### Option 2: Rebase Custom Changes
**Approach:**
```bash
git checkout main
git rebase -i upstream/main
# Squash the 5 custom commits into 1 meaningful commit
```

**Pros:**
- Cleaner history
- Easier to maintain going forward
- Can add better commit message

**Cons:**
- More complex process
- Requires interactive rebase expertise

---

## Pre-Merge Checklist

Before merging upstream changes, verify:

### Infrastructure Readiness
- [ ] `ghcr.io/my-smart-homes/devcontainer:supervisor` image exists and is up to date
- [ ] Core images for all architectures are built and published
- [ ] Plugin images for all architectures are built and published
- [ ] Version data endpoint returns valid JSON

### Version Data Endpoint
- [ ] `https://my-smart-homes.github.io/version-data/data.json` is accessible
- [ ] Returns valid JSON structure
- [ ] Contains appropriate version information
- [ ] Can handle requests without channel validation

### Testing
- [ ] Development container builds successfully
- [ ] Supervisor can pull core images
- [ ] Supervisor can pull plugin images  
- [ ] Version update checks work correctly
- [ ] Tests pass with custom image names

---

## Conflict Resolution Guidelines

### Expected Conflicts

1. **supervisor/const.py**
   - Upstream may have modified URL_HASSIO_VERSION area
   - Resolution: Keep custom URL, merge other const changes

2. **supervisor/homeassistant/module.py**
   - Upstream may have refactored image handling
   - Resolution: Ensure default_image() returns my-smart-homes images

3. **supervisor/plugins/base.py**
   - Upstream may have modified plugin image logic
   - Resolution: Preserve my-smart-homes registry path

4. **supervisor/updater.py**
   - Channel validation may have been modified
   - Resolution: Keep relaxed validation or implement proper channel support

### Conflict Resolution Priority
1. 🔴 **HIGH:** Image URLs - Must point to my-smart-homes
2. 🔴 **HIGH:** Version endpoint - Must use custom URL
3. 🟡 **MEDIUM:** Channel validation - Review and fix properly
4. 🟢 **LOW:** Test assertions - Update to match implementation

---

## Post-Merge Action Items

### Immediate
1. Fix channel validation properly (remove TODO workaround)
2. Test update mechanism end-to-end
3. Verify all container images pull successfully
4. Run full test suite

### Future Improvements
1. Implement proper channel support in custom version endpoint
2. Add monitoring for version endpoint availability
3. Consider making registry configurable via environment variable
4. Document the custom infrastructure setup

---

## Technical Debt

### Current Issues
1. **Channel validation bypass** - Line in updater.py with TODO comment
   - Needs proper implementation to support channels
   - Current workaround may mask version incompatibilities

2. **Hardcoded registry paths** - Not easily configurable
   - Consider environment variable: `REGISTRY_URL=ghcr.io/my-smart-homes`
   - Would make testing and deployment more flexible

3. **Image naming inconsistency**
   - Core: `{machine}-my-smart-homes` 
   - Plugins: `{arch}-hassio-{slug}`
   - Consider standardizing naming scheme

---

## Related Documentation

- Main sync documentation: `SYNC_DOCUMENTATION.md`
- Upstream repository: https://github.com/home-assistant/supervisor
- Custom infrastructure: https://github.com/my-smart-homes

---

**Document Version:** 1.0  
**Last Updated:** December 23, 2025 05:09 UTC  
**Status:** Pre-merge analysis complete
