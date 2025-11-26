# Git Workflow and Branch Strategy

This document describes the Git workflow, branch strategy, and repository management for the DeaPrint website project.

## Repository Information

- **Repository Name**: dea-web
- **Description**: DeaPrint Photography Website
- **Visibility**: Public (for free GitHub Pages and Actions)
- **Platform**: GitHub

## Branch Strategy

We use a simplified Git Flow with three main branches:

### Main Branches

1. **main** (Production)
   - Contains production-ready code
   - Deployed automatically to GitHub Pages
   - Protected branch - requires pull request reviews
   - Always stable and deployable

2. **preprod** (Staging)
   - Pre-production testing environment
   - Used for final testing before production
   - Mirrors production environment
   - Deployed to staging URL (if configured)

3. **develop** (Development)
   - Active development branch
   - Integration branch for features
   - May contain unstable code
   - Used for local testing

### Feature Branches

For new features or significant changes:

```bash
# Create feature branch from develop
git checkout develop
git pull origin develop
git checkout -b feature/feature-name

# Work on feature...
git add .
git commit -m "feat: description of feature"

# Push to remote
git push origin feature/feature-name

# Create pull request to develop
```

### Hotfix Branches

For urgent production fixes:

```bash
# Create hotfix branch from main
git checkout main
git pull origin main
git checkout -b hotfix/issue-description

# Fix the issue...
git add .
git commit -m "fix: description of fix"

# Push and create PR to main
git push origin hotfix/issue-description
```

## Commit Message Convention

Use conventional commits format:

```
<type>(<scope>): <description>

[optional body]

[optional footer]
```

### Types
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code style changes (formatting, etc.)
- `refactor`: Code refactoring
- `test`: Adding or updating tests
- `chore`: Maintenance tasks

### Examples
```bash
git commit -m "feat(services): add photography services page"
git commit -m "fix(contact): correct phone number format"
git commit -m "docs(readme): update installation instructions"
```

## Workflow Steps

### 1. Clone Repository

```bash
git clone https://github.com/YOUR_USERNAME/dea-web.git
cd dea-web
```

### 2. Set Up Remote

```bash
# Verify remote
git remote -v

# Add remote if needed
git remote add origin https://github.com/YOUR_USERNAME/dea-web.git
```

### 3. Daily Development

```bash
# Start of day - update develop branch
git checkout develop
git pull origin develop

# Create feature branch
git checkout -b feature/your-feature

# Make changes and commit
git add .
git commit -m "feat: your changes"

# Push to remote
git push origin feature/your-feature
```

### 4. Merge to Develop

```bash
# Update develop
git checkout develop
git pull origin develop

# Merge feature
git merge feature/your-feature

# Push to remote
git push origin develop

# Delete feature branch
git branch -d feature/your-feature
git push origin --delete feature/your-feature
```

### 5. Release to Production

```bash
# Merge develop to preprod for testing
git checkout preprod
git pull origin preprod
git merge develop
git push origin preprod

# After testing, merge to main
git checkout main
git pull origin main
git merge preprod
git push origin main

# Tag the release
git tag -a v1.0.0 -m "Release version 1.0.0"
git push origin v1.0.0
```

## .gitignore Configuration

The following files and directories are ignored:

```
# Hugo
/public/
/resources/_gen/
/assets/jsconfig.json
hugo_stats.json

# Node
node_modules/
package-lock.json

# OS
.DS_Store
Thumbs.db

# IDE
.vscode/
.idea/
*.swp
*.swo

# Temporary
*.log
.hugo_build.lock
```

## Authentication

### HTTPS with Personal Access Token (PAT)

```bash
# Use PAT instead of password
git remote set-url origin https://YOUR_TOKEN@github.com/YOUR_USERNAME/dea-web.git
```

### SSH Keys (Recommended)

```bash
# Generate SSH key
ssh-keygen -t ed25519 -C "your.email@example.com"

# Add to SSH agent
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

# Add public key to GitHub
cat ~/.ssh/id_ed25519.pub

# Update remote URL
git remote set-url origin git@github.com:YOUR_USERNAME/dea-web.git
```

## Best Practices

1. **Pull Before Push**: Always pull latest changes before pushing
2. **Small Commits**: Make small, focused commits with clear messages
3. **Branch Naming**: Use descriptive branch names (feature/add-gallery, fix/contact-form)
4. **Code Review**: Use pull requests for code review before merging
5. **Keep Branches Updated**: Regularly merge develop into feature branches
6. **Delete Merged Branches**: Clean up branches after merging
7. **Tag Releases**: Tag production releases with semantic versioning

## Troubleshooting

### Push Rejected
```bash
# Pull and rebase
git pull --rebase origin main
git push origin main
```

### Merge Conflicts
```bash
# Resolve conflicts in files
git add .
git commit -m "fix: resolve merge conflicts"
```

### Undo Last Commit
```bash
# Keep changes
git reset --soft HEAD~1

# Discard changes
git reset --hard HEAD~1
```

## GitHub Actions Integration

The repository uses GitHub Actions for automated deployment. Pushes to `main` branch trigger automatic deployment to GitHub Pages.

See `.github/workflows/deploy.yml` for deployment configuration.
