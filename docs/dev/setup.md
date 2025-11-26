# Development Environment Setup

This document contains the installation commands and versions used for the DeaPrint website project.

## System Information

- **Operating System**: Linux
- **Project Path**: `/home/devlin/src/github/dea-web`

## Installed Software Versions

### Hugo Extended
- **Version**: v0.152.2+extended
- **Build**: 6abdacad3f3fe944ea42177844469139e81feda6
- **Platform**: linux/amd64
- **Build Date**: 2025-10-24T15:31:49Z
- **Vendor**: snap:0.152.2

**Installation Command**:
```bash
# Check current version
hugo version

# If not installed or version < 0.121.0, install latest
wget https://github.com/gohugoio/hugo/releases/download/v0.121.2/hugo_extended_0.121.2_linux-amd64.deb
sudo dpkg -i hugo_extended_0.121.2_linux-amd64.deb

# Verify installation
hugo version
```

### Node.js and npm
- **Node.js**: v18+ (recommended)
- **npm**: v9+ (recommended)

**Installation Command**:
```bash
# Check current version
node --version
npm --version

# Install via nvm (recommended)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
source ~/.bashrc
nvm install 18
nvm use 18

# Verify
node --version
npm --version
```

### Git
- **Version**: 2.x.x

**Verification Command**:
```bash
git --version

# Configure Git if not already done
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

## Project Dependencies

Install project dependencies after cloning:

```bash
cd /home/devlin/src/github/dea-web
npm install
```

## Development Server

Start the Hugo development server:

```bash
# Using npm script
npm run dev

# Or directly with Hugo
hugo server -D
```

The site will be available at `http://localhost:1313`

## Build Commands

```bash
# Development build
npm run dev

# Production build
npm run build

# Clean build artifacts
npm run clean

# Preview production build
npm run preview
```

## Troubleshooting

### Hugo Extended Not Found
- Ensure you installed the "Extended" version, not regular Hugo
- The extended version is required for Tailwind CSS/SCSS processing

### Node Version Issues
- Use nvm to manage Node.js versions
- Project requires Node.js v18 or later

### Permission Errors
- Use sudo for system-wide installation
- Or use nvm for user-level installation (recommended)

## Reproducibility

This document ensures all team members use compatible versions of required software. Always verify versions match before starting development.
