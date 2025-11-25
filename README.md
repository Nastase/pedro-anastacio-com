# Pedro Anastácio - Personal Website

Personal blog and portfolio website covering Software Engineering, Productivity, and more.

Built with [Hugo](https://gohugo.io/) static site generator using the [Gokarna](https://github.com/526avijitgupta/gokarna) theme.

**Live site:** [pedroanastacio.com](https://pedroanastacio.com)

## Prerequisites

### macOS
```bash
brew install hugo
```

### Linux/Debian
```bash
sudo apt update
sudo apt install hugo
```

For the latest version:
```bash
sudo snap install hugo
```

### Windows

Using Chocolatey:
```powershell
choco install hugo
```

Using Scoop:
```powershell
scoop install hugo
```

## Getting Started

Clone the repository with submodules:
```bash
git clone --recurse-submodules https://github.com/nastase/pedro-anastacio-com.git
cd pedro-anastacio-com
```

If already cloned, initialize the theme submodule:
```bash
git submodule update --init --recursive
```

Start the development server:
```bash
hugo server -D
```

Build for production:
```bash
hugo
```

## Project Structure

```
├── content/        # Blog posts and pages
│   ├── posts/      # Blog posts (page bundles)
│   └── work/       # Resume and work-related pages
├── static/         # Images, favicons, and static assets
├── themes/         # Gokarna theme (git submodule)
├── public/         # Generated site output
└── hugo.toml       # Site configuration
```
