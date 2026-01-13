# Development Environment Setup

Installation commands and versions for DeaPrint website project.

## Required Software

### Hugo Extended

**Version:** v0.152.2+extended or later

```bash
# Check version
hugo version

# Install (Linux)
wget https://github.com/gohugoio/hugo/releases/download/v0.121.2/hugo_extended_0.121.2_linux-amd64.deb
sudo dpkg -i hugo_extended_0.121.2_linux-amd64.deb

# Verify
hugo version
```

**Important:** Must be "Extended" version for Tailwind CSS/SCSS processing.

### Node.js and npm

**Version:** Node.js v18+, npm v9+

```bash
# Check version
node --version
npm --version

# Install via nvm (recommended)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
source ~/.bashrc
nvm install 18
nvm use 18
```

### Git

```bash
git --version

# Configure
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

## Project Setup

```bash
cd /home/devlin/src/github/dea-web
npm install
```

## Development Server

```bash
npm run dev
# Or: hugo server -D
```

Site available at `http://localhost:1313`

## Build Commands

```bash
npm run dev      # Development server
npm run build    # Production build
npm run clean    # Clean artifacts
```

## Troubleshooting

**Hugo Extended not found:** Ensure "Extended" version installed, not regular Hugo.

**Node version issues:** Use nvm to manage versions. Requires Node.js v18+.

**Permission errors:** Use nvm for user-level installation (recommended).
