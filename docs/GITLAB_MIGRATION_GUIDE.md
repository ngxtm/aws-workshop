# GitLab Migration Guide

> Complete guide for migrating MapVibe from GitHub to GitLab with CI/CD setup

**Migration Date**: 2025-11-19
**Repository**: `gitlab.com/4n1d/mapvibe`

---

## Table of Contents

1. [Overview](#overview)
2. [Why GitLab](#why-gitlab)
3. [Migration Process](#migration-process)
4. [Protected Branches Setup](#protected-branches-setup)
5. [CI/CD Pipeline Configuration](#cicd-pipeline-configuration)
6. [Git Workflow](#git-workflow)
7. [Troubleshooting](#troubleshooting)

---

## Overview

This guide documents the complete migration from GitHub to GitLab, including:
- Repository migration with full history
- Protected branch configuration
- CI/CD pipeline setup for monorepo
- Local git configuration updates
- GitHub repository archival

---

## Why GitLab

### Decision Factors

**Advantages over GitHub**:
- ✅ **Better CI/CD**: 400 free minutes/month vs GitHub's limitations
- ✅ **Monorepo Support**: Better artifact management and caching
- ✅ **Unlimited Collaborators**: Free tier allows unlimited team members
- ✅ **Built-in DevOps**: Docker registry, K8s integration, security scanning
- ✅ **Cost Effective**: More features in free tier

**Mentor Recommendation**:
- GitLab better suited for learning DevOps
- Stronger AWS/Docker/Kubernetes integration
- Better for team collaboration

---

## Migration Process

### Step 1: Create GitLab Account

1. Visit `https://gitlab.com`
2. Click "Register now"
3. Fill in:
   - Username (becomes part of URL: `gitlab.com/username`)
   - Email
   - Password
4. Verify email address

### Step 2: Import Repository from GitHub

**Method**: GitLab Import Tool (recommended)

1. On GitLab, click "New project"
2. Select "Import project"
3. Choose "GitHub"
4. Authenticate with GitHub
5. Select repository to import
6. Configure:
   - **Project name**: `mapvibe`
   - **Visibility**: Private (recommended initially)
   - **Initialize with README**: ❌ Uncheck (already have code)

7. Click "Import project"
8. Wait for import to complete (~2-5 minutes)

**What Gets Imported**:
- ✅ All commits and history
- ✅ All branches
- ✅ All tags
- ✅ Issues (optional)
- ✅ Pull Requests → Merge Requests
- ✅ Milestones
- ✅ Labels

**What Doesn't Import**:
- ❌ GitHub Projects (spreadsheet views)
- ❌ GitHub Actions workflows (convert to GitLab CI)
- ❌ Stars/watchers count
- ❌ GitHub Discussions

### Step 3: Update Local Git Remote

```bash
# Check current remote
git remote -v

# Change origin to GitLab
git remote set-url origin https://gitlab.com/YOUR-USERNAME/mapvibe.git

# Verify change
git remote -v

# Test connection
git fetch origin
```

**Optional**: Keep GitHub as backup remote:
```bash
git remote add github https://github.com/YOUR-USERNAME/mapvibe.git
```

### Step 4: Verify Migration

**Check branches**:
```bash
git branch -a
```

Expected output:
```
  dev
* feature/add-homepage
  prod
  remotes/origin/dev
  remotes/origin/feature/add-homepage
  remotes/origin/prod
```

**Verify on GitLab Web UI**:
1. Go to `https://gitlab.com/YOUR-USERNAME/mapvibe`
2. Check "Code" → "Branches": Should see `prod`, `dev`, feature branches
3. Check "Code" → "Commits": Verify commit history
4. Check "Code" → "Repository": Verify all files present

### Step 5: Archive GitHub Repository

**Recommended approach**: Archive (not delete)

1. Go to GitHub repository
2. Navigate to "Settings"
3. Scroll to "Danger Zone"
4. Click "Archive this repository"
5. Confirm archival

**Benefits of archiving**:
- Repository becomes read-only
- Preserves history for reference
- Can unarchive later if needed
- No risk of accidental deletion

---

## Protected Branches Setup

### Overview

Protected branches prevent:
- ❌ Force pushes (history rewriting)
- ❌ Direct commits (bypassing code review)
- ❌ Accidental deletion

### Configuration

#### 1. Access Settings

1. On GitLab project page
2. Click "Settings" → "Repository"
3. Expand "Protected branches" section

#### 2. Protect `prod` Branch

**Branch**: `prod`
**Allowed to merge**: Maintainers
**Allowed to push and merge**: No one
**Allowed to force push**: ❌ Disabled

**Explanation**:
- `prod` is production branch
- Changes must go through Merge Request
- No direct pushes allowed
- Code review required

#### 3. Protect `dev` Branch

**Branch**: `dev`
**Allowed to merge**: Maintainers
**Allowed to push and merge**: Developers + Maintainers *(or "No one" for stricter control)*
**Allowed to force push**: ❌ Disabled

**Explanation**:
- `dev` is integration branch
- More flexible than `prod`
- Choose based on team size:
  - Solo/small team: "Developers + Maintainers"
  - Larger team: "No one" (require MR)

#### 4. Verify Protection

Terminal test:
```bash
git checkout prod
git commit --allow-empty -m "test: protected branch"
git push origin prod
```

Expected error:
```
remote: GitLab: You are not allowed to push code to protected branch prod
```

If you see this error: ✅ Protection working!

Rollback test commit:
```bash
git reset --hard HEAD~1
```

---

## CI/CD Pipeline Configuration

### Overview

Automated pipeline that runs on every push:
1. **Install stage**: Install dependencies with Bun
2. **Build stage**: Build all packages in correct order
3. **Test stage**: Run tests (future)

### Pipeline File: `.gitlab-ci.yml`

**Location**: Repository root

**Structure**:
```yaml
stages:
  - install
  - build
  - test

cache:
  key:
    files:
      - bun.lock
  paths:
    - node_modules/
    - .bun-cache/
    - apps/*/node_modules/
    - packages/*/node_modules/

default:
  image: oven/bun:latest

# Jobs...
```

### Jobs Explained

#### Install Job
```yaml
install:
  stage: install
  script:
    - bun install --frozen-lockfile
  artifacts:
    paths:
      - node_modules/
      - apps/*/node_modules/
      - packages/*/node_modules/
```

**Purpose**: Install all dependencies
**Caching**: Speeds up subsequent runs
**Artifacts**: Pass node_modules to next stages

#### Build Jobs

**Database Package**:
```yaml
build:database:
  stage: build
  script:
    - echo "✅ Skipping build - uses TypeScript directly"
```
Skipped because Bun runs TypeScript directly.

**UI Components Package**:
```yaml
build:ui-components:
  stage: build
  needs: [install]
  script:
    - cd packages/ui-components
    - bun run build
```
Compiles TypeScript components.

**Web App**:
```yaml
build:web:
  stage: build
  needs: [install, build:ui-components]
  script:
    - cd apps/web
    - bun run build
```
Builds production Vite bundle.

### Monitoring Pipeline

**View pipelines**:
1. Go to "Build" → "Pipelines"
2. See list of recent pipeline runs

**Pipeline states**:
- 🔵 **Running**: Pipeline in progress
- ✅ **Passed**: All jobs succeeded
- ❌ **Failed**: At least one job failed
- ⏸️ **Manual**: Awaiting manual trigger

**View job details**:
1. Click on pipeline
2. See job graph (visualizes dependencies)
3. Click individual job to see logs

### Common Pipeline Issues

**Issue 1: "vite: command not found"**
- **Cause**: Dependencies not installed in workspace
- **Fix**: Ensure `install` job artifacts include workspace node_modules

**Issue 2: TypeScript errors**
- **Cause**: Code has type errors
- **Fix**: Fix type errors locally before pushing

**Issue 3: "No runners available"**
- **Cause**: GitLab shared runners busy
- **Fix**: Wait a few minutes, pipeline will start automatically

**Issue 4: Out of CI/CD minutes**
- **Cause**: Exceeded 400 free minutes/month
- **Fix**: Wait for next month or upgrade plan

---

## Git Workflow

### Branch Strategy

```
prod (production)
  ↑
  └── Merge Request (after testing)
       ↑
dev (development)
  ↑
  └── Merge Request (after feature complete)
       ↑
feature/feature-name (feature branches)
```

### Creating Feature Branch

```bash
# 1. Start from dev
git checkout dev
git pull origin dev

# 2. Create feature branch
git checkout -b feature/add-new-feature

# 3. Make changes and commit
git add .
git commit -m "feat: implement new feature"

# 4. Push to GitLab
git push -u origin feature/add-new-feature
```

### Creating Merge Request

**On GitLab Web UI**:
1. Navigate to "Code" → "Merge requests"
2. Click "New merge request"
3. Select:
   - **Source branch**: `feature/add-new-feature`
   - **Target branch**: `dev`
4. Fill in:
   - **Title**: Descriptive title
   - **Description**: What changed and why
5. Check:
   - ✅ "Delete source branch when merge request is accepted"
   - ✅ "Squash commits when merge request is accepted" (optional)
6. Click "Create merge request"

**Review process**:
1. CI/CD pipeline runs automatically
2. Wait for pipeline to pass ✅
3. Request code review (if team)
4. Address feedback
5. Merge when approved

### Merging to Production

```bash
# 1. Ensure dev is tested
# 2. Create MR: dev → prod
# 3. Verify CI passes
# 4. Merge via GitLab UI
# 5. Tag release (optional)
git checkout prod
git pull origin prod
git tag -a v1.0.0 -m "Release v1.0.0"
git push origin v1.0.0
```

---

## Troubleshooting

### Authentication Issues

**Problem**: `git push` asks for password repeatedly

**Solution**: Use SSH keys or Personal Access Token

**SSH Setup**:
```bash
# Generate SSH key
ssh-keygen -t ed25519 -C "your_email@example.com"

# Copy public key
cat ~/.ssh/id_ed25519.pub

# Add to GitLab: Settings → SSH Keys
```

Change remote to SSH:
```bash
git remote set-url origin git@gitlab.com:USERNAME/mapvibe.git
```

**Personal Access Token**:
1. GitLab: Settings → Access Tokens
2. Create token with `read_repository` and `write_repository` scopes
3. Use token as password when prompted

### Pipeline Failures

**Check logs**:
1. Go to failed pipeline
2. Click failed job (red X)
3. Read error logs
4. Common issues:
   - Missing dependencies
   - TypeScript errors
   - Wrong Node/Bun version

**Retry pipeline**:
- Click "Retry" button on pipeline page

### Merge Conflicts

**When you see**:
```
Auto-merging failed. Please resolve conflicts manually.
```

**Resolution**:
```bash
git checkout dev
git pull origin dev
git checkout feature/your-feature
git merge dev

# Resolve conflicts in files
# Then:
git add .
git commit -m "fix: resolve merge conflicts"
git push origin feature/your-feature
```

---

## Best Practices

### Commit Messages

Follow conventional commits:
```
feat: add user authentication
fix: resolve login bug
chore: update dependencies
docs: update README
ci: fix pipeline configuration
```

### Merge Request Guidelines

- ✅ Descriptive title and description
- ✅ Link related issues
- ✅ Keep MRs small and focused
- ✅ Ensure CI passes before requesting review
- ✅ Respond to review feedback promptly

### Branch Naming

- `feature/description` - New features
- `bugfix/description` - Bug fixes
- `hotfix/description` - Urgent production fixes
- `chore/description` - Maintenance tasks

---

## Resources

- **GitLab Docs**: https://docs.gitlab.com
- **GitLab CI/CD**: https://docs.gitlab.com/ee/ci/
- **Protected Branches**: https://docs.gitlab.com/ee/user/project/protected_branches.html
- **MapVibe Repository**: https://gitlab.com/4n1d/mapvibe

---

*Last updated: 2025-11-19*
