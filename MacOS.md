---
tags:
  - MacOS
---

# MacOS

---
- [Summary](#summary)
- [Install](#install)
  - [Homebrew](#homebrew)
  - [Initial Apps](#initial-apps)
- [Tips](#tips)
  - [Showing Hidden Files via Mac Finder](#showing-hidden-files-via-mac-finder)
- [Apps](#apps)
  - [Commander](#commander)
  - [Window Management](#window-management)
  - [System Stats](#system-stats)
- [Problems](#problems)
  - [Problem 5000 / 7000 blocked](#problem-5000--7000-blocked)
  - [Port mappings](#port-mappings)
---

## Summary

Notes for setting up MacOS: install Homebrew, core CLI tools, common desktop apps, a quick Finder tip, and references for troubleshooting port conflicts.

## Install

### Homebrew

[https://brew.sh](https://brew.sh)  
Install Homebrew first, then use it for CLI tools and casks.

#### CLI tools

```bash
brew install git bat lnav
```

Resources:
- [BAT - a cat clone with wings](https://github.com/sharkdp/bat)
- [The Logfile Navigator](https://lnav.org/)

#### Initial Apps

```bash
brew install --cask iterm2
brew install --cask visual-studio-code
brew install --cask postman
brew install --cask intellij-idea
brew install --cask google-chrome
brew install --cask drawio
# Optional:
# brew install --cask rectangle
# brew install plantuml
```

## Tips

### Showing hidden files via Mac Finder

Command + Shift + . (period)

## Apps

### Commander

[Commander One](https://mac.eltima.com/commander-one-purchase.html)

promo code: **CMNDRMT-30**

### Window Management

[Rectangle](https://rectangleapp.com/)

[Magnet](https://apps.apple.com/us/app/magnet/id441258766?mt=12)

### System Stats

```bash
brew install --cask stats
```

## Problems

### Problem 5000 / 7000 blocked

Why is Control Center on Monterey listening to port 5000 and port 7000?

https://developer.apple.com/forums/thread/682332

### Port mappings

```bash
lsof -nP | grep LISTEN
```

https://forums.docker.com/t/port-mappings-are-not-released/10565/33
