# Programming

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Digital Ocean
brew install doctl
doctl auth init

# NodeJS
curl -fsSL https://fnm.vercel.app/install | bash
fnm install 15.3.0

# Visual studio code
brew install --cask visual-studio-code

# Terminal
brew install --cask iterm2
touch .hushlogin # No timestamp on startup
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)" # Oh my ZSH!
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
(echo 'ZSH_DISABLE_COMPFIX=true' && cat ~/.zshrc) > ~/.zshrc-tmp && mv ~/.zshrc-tmp ~/.zshrc
echo 'ssh-add -q -K ~/.ssh/id_rsa' >> ~/.zshrc
echo 'plugins=(git zsh-autosuggestions)' >> ~/.zshrc
code ~/.zshrc # Manually remove "plugins=(git)" line

# Database
brew install --cask robo-3t
brew tap mongodb/brew
brew install mongodb-community
echo '# Replication' >> /usr/local/etc/mongod.conf
echo 'replication:' >> /usr/local/etc/mongod.conf
echo '  replSetName: rs01:' >> /usr/local/etc/mongod.conf
brew services start mongodb/brew/mongodb-community
mongo
rs.initiate()

# Git
brew install git-flow

```

### Extra steps in Visual Studio code

1. Press `CMD + P`
2. Run `ext install esbenp.prettier-vscode`

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
