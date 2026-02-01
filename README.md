# Personal Website (Hugo + LoveIt)

This repository contains my personal website built with [**Hugo**](https://gohugo.io/) using the [**LoveIt**](https://hugoloveit.com/) theme and deployed automatically to **GitHub Pages** via **GitHub Actions**.

The project is intentionally pinned to a specific Hugo version to ensure long-term stability and compatibility with the LoveIt theme.

---

## 🚀 Tech Stack

- **Hugo (extended)** – Static site generator
- **LoveIt theme** – Modern Hugo theme
- **GitHub Pages** – Hosting
- **GitHub Actions** – Continuous Deployment
- **Dart Sass** – SCSS compilation

---

## 📦 Prerequisites

### Required
- Git
- Hugo **extended** `0.145.0`

### Optional
- Node.js (only required if `package.json` exists)
- Linux or macOS
- Bash-compatible shell

---

## 🔧 Installing Hugo (Extended)

This project uses **Hugo 0.145.0 (extended)**.

### Ubuntu / Debian
```bash
cd /tmp && \
wget https://github.com/gohugoio/hugo/releases/download/v0.145.0/hugo_extended_0.145.0_linux-amd64.deb && \
sudo dpkg -i hugo_extended_0.145.0_linux-amd64.deb
```

### Verify the installation
```bash
hugo version
```

### Expected output
```bash
hugo v0.145.0+extended linux/amd64
```

---

## ▶️ Running the Site Locally

### Start the development server
```bash
hugo serve
```

### Open the site in your browser:
```bash
http://localhost:1313
```

The site will automatically reload when files are changed.

---

## 🔄 Updating an Existing Hugo Project

When upgrading an older Hugo project, follow these steps carefully.

### 1️⃣   Update the LoveIt Theme
If LoveIt is included directly:
```bash
cd themes/LoveIt
git pull
cd ../..
```

If LoveIt is a Git submodule:
```bash
git submodule update --init --recursive
git submodule update --remote --merge
```

### 2️⃣   Ignore Generated Files

Recent Hugo versions removed several deprecated keys.

Generated files must never be committed.

Ensure .gitignore contains:

```bash
/public/
/resources/_gen/
.hugo_build.lock
node_modules/
```

---

## 🌍 Deployment with GitHub Pages

Deployment is fully automated using GitHub Actions.

### Deployment Flow

- Push changes to the main branch
- GitHub Actions:
  - installs Hugo
  - builds the site
  - uploads the build artifact
  - deploys to GitHub Pages

---

## 📁 Project Structure
```text
.
├── content/        # Markdown content
├── layouts/        # Custom layout overrides
├── static/         # Static assets
├── assets/         # SCSS, JS, images
├── themes/LoveIt/  # Hugo theme
├── hugo.toml       # Hugo configuration
└── .github/
    └── workflows/
        └── hugo.yaml
```
