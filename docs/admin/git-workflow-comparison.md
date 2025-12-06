# Git Workflow Comparison

This document compares three Git workflows for the DeaPrint project.

---

## 1. Current Documented Workflow (Simplified Git Flow)

### Branch Structure
- **main** - Production (auto-deploy)
- **preprod** - Staging (testing)
- **develop** - Development (integration)
- **feature/** - Feature branches
- **hotfix/** - Emergency fixes

### Workflow
```
feature → develop → preprod → main
         (test)    (staging)  (production)
```

### Pros
- ✅ Simple and clear
- ✅ Three-tier testing (dev, staging, prod)
- ✅ Suitable for small teams
- ✅ Easy to understand

### Cons
- ❌ Extra preprod branch adds complexity
- ❌ More manual merges required
- ❌ Slower release cycle

---

## 2. Requirements Workflow (Advanced with CI/CD)

### Branch Structure
- **main** - Production (no processing, just deploy)
- **preprod** - Staging (full build: translations, image processing, minification)
- **develop** - Development (local testing, can deploy to dev)
- **feature/** - Feature branches

### Key Differences from Current
1. **preprod does heavy processing**:
   - AI translations via OpenRouter
   - Image optimization
   - CSS minification
   - Asset preparation

2. **main is deployment-only**:
   - No build steps
   - Uses artifacts from preprod
   - Fast deployment

3. **develop can deploy to dev environment**:
   - Optional dev URL for testing
   - Can sync with prod (rebase)

### Workflow
```
feature → develop → preprod (BUILD) → main (DEPLOY)
         (local)   (staging+build)    (production)
```

### Pros
- ✅ Separation of build and deploy
- ✅ Automated translations
- ✅ Optimized assets in production
- ✅ Fast production deployments
- ✅ Dev environment for testing

### Cons
- ❌ More complex CI/CD setup
- ❌ Requires OpenRouter API
- ❌ Longer preprod build times
- ❌ Need to manage build artifacts

---

## 3. GitHub Flow (Industry Standard)

### Branch Structure
- **main** - Production (always deployable)
- **feature/** - Feature branches

### Workflow
```
feature → main (via PR)
         (review + CI)
```

### Process
1. Create feature branch from main
2. Make changes and commit
3. Open Pull Request
4. Code review + automated tests
5. Merge to main
6. Auto-deploy to production

### Pros
- ✅ Simplest workflow
- ✅ Industry standard
- ✅ Fast releases
- ✅ Continuous deployment
- ✅ Built-in GitHub features

### Cons
- ❌ No staging environment
- ❌ All testing in PR
- ❌ Risky for complex projects
- ❌ No separate build step

---

## Comparison Table

| Feature             | Current                             | Requirements                        | GitHub Flow       |
|---------------------|-------------------------------------|-------------------------------------|-------------------|
| **Branches**        | 4 (main, preprod, develop, feature) | 4 (main, preprod, develop, feature) | 2 (main, feature) |
| **Complexity**      | Medium                              | High                                | Low               |
| **Staging**         | Yes (preprod)                       | Yes (preprod + build)               | No                |
| **Dev Environment** | No                                  | Yes (optional)                      | No                |
| **Build Process**   | On every branch                     | Only on preprod                     | On main           |
| **Translations**    | Manual                              | Automated (preprod)                 | Manual or on main |
| **Deploy Speed**    | Medium                              | Fast (main), Slow (preprod)         | Fast              |
| **Best For**        | Small teams                         | Content-heavy sites                 | Simple projects   |
| **Learning Curve**  | Easy                                | Hard                                | Very Easy         |

---

## Recommendation for DeaPrint

### Phase 1 (MVP): Use GitHub Flow
**Why**: Simplest, fastest to production

```
feature/homepage → main (PR + review) → deploy
```

**Setup**:
- Cloudflare Pages auto-deploys from main
- No preprod/develop needed yet
- Focus on shipping MVP

### Phase 2+ (After MVP): Migrate to Requirements Workflow
**Why**: Automated translations and asset processing needed

```
feature → develop → preprod (build) → main (deploy)
```

**Setup**:
1. Add preprod branch
2. Configure GitHub Actions for preprod:
   - OpenRouter translations
   - Image optimization
   - CSS minification
3. Main deploys pre-built artifacts
4. Optional: Add develop environment

---

## Implementation Roadmap

### Now (Phase 1)
```bash
# Use GitHub Flow
git checkout -b feature/homepage
# ... make changes ...
git push origin feature/homepage
# Create PR to main
# Merge → auto-deploy
```

### Later (Phase 2+)
```bash
# Migrate to Requirements Workflow
git checkout -b develop
git push origin develop

git checkout -b preprod
git push origin preprod

# Update GitHub Actions
# Add OpenRouter integration
# Configure staging URL
```

---

## Detailed Workflow: Requirements Approach

### For Content Creators

**Adding new content**:
```bash
# 1. Create feature branch
git checkout develop
git pull origin develop
git checkout -b content/new-blog-post

# 2. Edit markdown file
# Edit content/blog/my-post.md

# 3. Commit and push
git add content/blog/my-post.md
git commit -m "content: add blog post about photobooks"
git push origin content/new-blog-post

# 4. Create PR to develop
# Review locally

# 5. Merge to develop
git checkout develop
git merge content/new-blog-post

# 6. Merge to preprod (triggers build)
git checkout preprod
git merge develop
git push origin preprod
# → CI/CD runs: translations, image processing, minification

# 7. Test on staging URL
# Visit https://preprod.deaprint.pl

# 8. Merge to main (fast deploy)
git checkout main
git merge preprod
git push origin main
# → Deploys to https://deaprint.pl
```

### For Developers

**Adding new feature**:
```bash
# 1. Create feature branch from develop
git checkout develop
git pull origin develop
git checkout -b feature/gallery-lightbox

# 2. Develop feature
# ... code changes ...

# 3. Test locally
hugo server -D

# 4. Commit and push
git add .
git commit -m "feat(gallery): add lightbox functionality"
git push origin feature/gallery-lightbox

# 5. Optional: Deploy to dev environment
# Push to develop for dev URL testing

# 6. Create PR to develop
# Code review

# 7. Follow content creator workflow (steps 5-8)
```

### CI/CD Configuration (preprod)

**GitHub Actions** (`.github/workflows/preprod-build.yml`):
```yaml
name: Preprod Build
on:
  push:
    branches: [preprod]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      # Translate content
      - name: AI Translation
        run: |
          npm run translate
        env:
          OPENROUTER_API_KEY: ${{ secrets.OPENROUTER_API_KEY }}
      
      # Optimize images
      - name: Process Images
        run: npm run optimize-images
      
      # Build Hugo
      - name: Build
        run: hugo --minify
      
      # Deploy to staging
      - name: Deploy to Cloudflare (staging)
        run: wrangler pages deploy public
        env:
          CLOUDFLARE_API_TOKEN: ${{ secrets.CLOUDFLARE_API_TOKEN }}
```

**GitHub Actions** (`.github/workflows/main-deploy.yml`):
```yaml
name: Production Deploy
on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      # Simple build (content already processed)
      - name: Build
        run: hugo --minify
      
      # Deploy to production
      - name: Deploy to Cloudflare
        run: wrangler pages deploy public
        env:
          CLOUDFLARE_API_TOKEN: ${{ secrets.CLOUDFLARE_API_TOKEN }}
```

---

## Key Differences Summary

### Current Workflow
- **Purpose**: Simple, manual workflow
- **Use case**: Learning, small projects
- **Deployment**: Manual or basic CI/CD

### Requirements Workflow
- **Purpose**: Automated content processing
- **Use case**: Multilingual, content-heavy sites
- **Deployment**: Advanced CI/CD with build pipeline

### GitHub Flow
- **Purpose**: Fast iteration
- **Use case**: Simple sites, continuous deployment
- **Deployment**: Direct to production

---

## Migration Path

### Step 1: Start with GitHub Flow (Now)
- Single main branch
- Feature branches
- Direct deployment

### Step 2: Add develop (After MVP)
- Create develop branch
- Feature branches merge to develop
- Test before production

### Step 3: Add preprod + CI/CD (Phase 2)
- Create preprod branch
- Add GitHub Actions
- Implement translations
- Add image processing

### Step 4: Optimize (Phase 3+)
- Add dev environment
- Implement artifact caching
- Optimize build times
- Add automated testing

---

## Cloudflare Pages Integration

### How Cloudflare Knows About Commits

Cloudflare uses **GitHub webhooks** to detect new commits:

1. You connect GitHub repo to Cloudflare Pages
2. Cloudflare registers a webhook with GitHub
3. When you push, GitHub sends HTTP POST to Cloudflare
4. Cloudflare receives notification and triggers build
5. Site deploys automatically

**You can see webhooks**: GitHub → Settings → Webhooks

### Cloudflare Auto-Deploy Options

**Option 1: Cloudflare Builds Everything (Simple)**
```
main → Cloudflare builds → Cloudflare deploys
preprod → Cloudflare builds → Cloudflare deploys
```
- Same build process for all branches
- No custom processing per branch
- Perfect for Phase 1 (MVP)

**Option 2: Hybrid (GitHub Actions + Cloudflare)**
```
preprod → GitHub Actions (translate, process) → Cloudflare deploys
main → Cloudflare builds → Cloudflare deploys
```
- Different processing per branch
- Disable Cloudflare auto-build for preprod
- Use GitHub Actions to process and deploy with wrangler
- Keep Cloudflare auto-build for main

### Recommended Cloudflare Setup

**Phase 1 (MVP)**:
- **Production branch**: `main`
- **Build command**: `hugo --minify`
- **Build output**: `/public`
- **Preview branches**: All branches
- Let Cloudflare handle everything

**Phase 2+ (Multilingual)**:
- **Production branch**: `main` (Cloudflare auto-build)
- **Preprod branch**: Disable auto-build, use GitHub Actions
- **Develop branch**: Optional Cloudflare preview

### Cloudflare Settings Configuration

**To disable auto-build for specific branch**:
1. Cloudflare Pages → Settings → Builds & deployments
2. Production branch: `main`
3. Preview branches: Select specific branches or "None"
4. This prevents Cloudflare from building preprod

**GitHub Actions then handles preprod**:
```yaml
# .github/workflows/preprod.yml
on:
  push:
    branches: [preprod]
jobs:
  build:
    steps:
      - Translate content
      - Optimize images
      - Build Hugo
      - Deploy with wrangler pages deploy
```

### Branch URLs

**With Cloudflare auto-deploy**:
- `main` → https://deaprint.pl
- `preprod` → https://preprod.deaprint.pages.dev
- `develop` → https://develop.deaprint.pages.dev
- `feature/x` → https://feature-x.deaprint.pages.dev

**With hybrid approach**:
- `main` → https://deaprint.pl (Cloudflare auto)
- `preprod` → https://preprod.deaprint.pl (GitHub Actions + wrangler)
- `develop` → https://develop.deaprint.pages.dev (Cloudflare auto)

---

## Conclusion

### Key Insights from Discussion

1. **GitHub Webhooks**: Cloudflare receives instant notifications via webhooks when you push. You cannot intercept this webhook to process before Cloudflare gets notified.

2. **Processing Before Deploy**: To process content (translations, image optimization) before deployment:
   - Disable Cloudflare auto-build for branches needing processing
   - Use GitHub Actions to process and deploy with wrangler
   - Keep Cloudflare auto-build for production (main)

3. **Wrangler**: Cloudflare's CLI tool for manual deployment. Only needed when you build outside Cloudflare (GitHub Actions). Not needed if Cloudflare auto-deploys from GitHub.

4. **Hybrid Approach**: Best of both worlds:
   - Cloudflare auto-deploys main (simple, fast)
   - GitHub Actions processes preprod (translations, optimization)
   - Separate concerns: build vs deploy

### Final Recommendation for DeaPrint

**Phase 1 (MVP)**: Use **GitHub Flow + Cloudflare Auto-Deploy**
- Single `main` branch
- Cloudflare auto-deploys on push
- Zero configuration
- Focus on building features

**Why**: Simplest possible setup. No GitHub Actions, no wrangler, no complexity. Just push and deploy.

**Phase 2+ (Multilingual)**: Migrate to **Hybrid Workflow**
- `main`: Cloudflare auto-deploy (production)
- `preprod`: GitHub Actions + wrangler (staging with processing)
- `develop`: Optional Cloudflare preview (development)

**Why**: Automated translations and asset processing only where needed. Production stays simple and fast.

### Implementation Strategy

1. **Start Simple** (Phase 1)
   ```
   feature → main → Cloudflare auto-deploys
   ```

2. **Add Staging** (Phase 2)
   ```
   feature → preprod (GitHub Actions) → main (Cloudflare)
   ```

3. **Add Development** (Phase 3)
   ```
   feature → develop → preprod → main
   ```

### Don't Overthink It

- **MVP doesn't need translations** → Use simple Cloudflare auto-deploy
- **Add complexity only when needed** → Migrate to hybrid when adding multilingual
- **Keep production simple** → Always let Cloudflare auto-deploy main

**Bottom line**: Start with GitHub Flow + Cloudflare auto-deploy. Add GitHub Actions + wrangler only when you need custom processing (Phase 2+).
