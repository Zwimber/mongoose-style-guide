# Programming

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
brew install --cask iterm2
xcode-select --install

# iTerm2
touch .hushlogin # No timestamp on startup
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)" # Oh my ZSH!
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
(echo 'ZSH_DISABLE_COMPFIX=true' && cat ~/.zshrc) > ~/.zshrc-tmp && mv ~/.zshrc-tmp ~/.zshrc
echo 'ssh-add -q -K ~/.ssh/id_rsa' >> ~/.zshrc
sed -i '.bak' 's/(git)/(git zsh-autosuggestions)/g' ~/.zshrc

# NodeJS
curl -fsSL https://fnm.vercel.app/install | bash
fnm install 14.15.1

# Visual studio code
brew install --cask visual-studio-code

# Database
# (2020-12-12 - Rosetta 2)
brew install --cask robo-3t
brew tap mongodb/brew
brew install mongodb-community
echo '# Replication' >> /usr/local/etc/mongod.conf
echo 'replication:' >> /usr/local/etc/mongod.conf
echo '  replSetName: rs01' >> /usr/local/etc/mongod.conf
brew services start mongodb/brew/mongodb-community
mongo
db.disableFreeMonitoring()
rs.initiate()

# Git
# (2020-12-12 - Rosetta 2)
brew install git-flow

# Digital Ocean
brew install doctl
doctl auth init


```

### Extra steps in Visual Studio code

1. Press `CMD + p` and run `ext install esbenp.prettier-vscode`
1. Press `CMD + p` and run `ext install angular.ng-template`
1. Press `CMD + SHIFT + P` and run `Settings JSON`
1. Copy and paste following:

```json
{
  "telemetry.enableTelemetry": false,
  "workbench.statusBar.visible": false,
  "workbench.activityBar.visible": false,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnSave": true,
  "explorer.confirmDelete": false,
  "window.zoomLevel": 0,
  "files.exclude": {
    "node_modules/": true,
    "dist/": true,
    "e2e/": true
  }
}
```

# Brew applications must haves

```bash
brew install --cask spotify
brew install --cask private-internet-access
brew install --cask brave-browser
brew install --cask google-chrome
brew install --cask qbittorrent
brew install --cask vlc
brew install --cask whatsapp
brew install --cask telegram
brew install --cask caffeine
```

# Settings

- Drag with three fingers enable
- Spotlight don’t look for files or web
- Keyboard use as F1 keys
- Finder / Advanced / Keep folders on top
