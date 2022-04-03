# MacOS

## Install

### Homebrew

[https://brew.sh](https://brew.sh)

#### apps

```
brew install git bat lnav
brew install --cask iterm2
brew install --cask rectangle
brew install --cask visual-studio-code
brew install --cask postman
brew install --cask intellij-idea
brew install --cask google-chrome
```

[BAT - a cat clone with wings](https://github.com/sharkdp/bat)

```
brew install bat
```

[The Logfile Navigator](https://lnav.org/)

```
brew install lnav
```

## Tips

### Showing hidden files via Mac Finder

Command + Shift + . (period) 

## Apps

### Commander

[Commander One](https://mac.eltima.com/commander-one-purchase.html)

promo code: **CMNDRMT-30**

### Windows Manager

[Rectangle](https://rectangleapp.com/)

[Magnet](https://apps.apple.com/us/app/magnet/id441258766?mt=12)

### stats


```
brew install --cask stats
```



## Problems

### Problem 5000 / 7000 blocked

Why is Control Center on Monterey listening to port 5000 and port 7000?

https://developer.apple.com/forums/thread/682332

### Port mappings

```
lsof -nP | grep LISTEN
```

https://forums.docker.com/t/port-mappings-are-not-released/10565/33

