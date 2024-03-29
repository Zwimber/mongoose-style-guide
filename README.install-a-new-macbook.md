# Programming

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
brew install --cask iterm2
xcode-select --install

# Terminal should be silent
touch .hushlogin # No timestamp on startup

# Oh My Zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" # Oh my ZSH!
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
sed -i '.bak' 's/(git)/(git zsh-autosuggestions)/g' ~/.zshrc
# (echo 'ZSH_DISABLE_COMPFIX=true' && cat ~/.zshrc) > ~/.zshrc-tmp && mv ~/.zshrc-tmp ~/.zshrc
# echo 'ssh-add -q -K ~/.ssh/id_rsa' >> ~/.zshrc

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
echo '# Replication' >> /opt/homebrew/etc/mongod.conf
echo 'replication:' >> /opt/homebrew/etc/mongod.conf
echo '  replSetName: rs01' >> /opt/homebrew/etc/mongod.conf
brew services stop mongodb/brew/mongodb-community
brew services start mongodb/brew/mongodb-community
mongosh
rs.initiate()
db.disableFreeMonitoring()

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
    "dist-new/": true,
    "e2e/": true
  },
  "explorer.confirmDragAndDrop": false
}
```

# Brew applications must haves

```bash
brew install --cask google-chrome # If Cypress user, install Chrome Dev (different icon)
brew install --cask spotify
brew install --cask private-internet-access
brew install --cask brave-browser
brew install --cask qbittorrent
brew install --cask vlc
brew install --cask whatsapp
brew install --cask telegram
brew install --cask caffeine
brew install --cask basecamp
brew install nmap
brew install thefuck # Correct small mistakes
```

# Settings

- Drag with three fingers enable
- Spotlight don’t look for files or web
- Keyboard use as F1 keys
- Finder / Advanced / Keep folders on top
- Auto-hide top bar
