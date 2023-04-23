<img src="https://raw.githubusercontent.com/geerlingguy/mac-dev-playbook/master/files/Mac-Dev-Playbook-Logo.png" width="250" height="156" alt="Mac Dev Playbook Logo" />

# Mac Setup Ansible Playbook

[![CI][badge-gh-actions]][link-gh-actions]

This playbook installs and configures most of the software I use on my Mac for web and software development. Some things in macOS are slightly difficult to automate, so I still have a few manual installation steps, but at least it's all documented here.

## Installation

1. Ensure Apple's command line tools are installed (`xcode-select --install` to launch the installer).
2. [Install Ansible](https://docs.ansible.com/ansible/latest/installation_guide/index.html):

   1. Run the following command to add Python 3 to your $PATH: `export PATH="$HOME/Library/Python/3.9/bin:/opt/homebrew/bin:$PATH"`
   2. Upgrade Pip: `sudo pip3 install --upgrade pip`
   3. Install Ansible: `pip3 install ansible`

3. Clone or download this repository to your local drive.
4. Run `ansible-galaxy install -r requirements.yml` inside this directory to install required Ansible roles.
5. Run `ansible-playbook main.yml --ask-become-pass` inside this directory. Enter your macOS account password when prompted for the 'BECOME' password.

> Note: If some Homebrew commands fail, you might need to agree to Xcode's license or fix some other Brew issue. Run `brew doctor` to see if this is the case.

## Overriding Defaults

Not everyone's development environment and preferred software configuration is the same.

You can override any of the defaults configured in `default.config.yml` by creating a `config.yml` file and setting the overrides in that file. For example, you can customize the installed packages and apps with something like:

```yaml
homebrew_installed_packages:
  - cowsay
  - git
  - go

mas_installed_apps:
  - { id: 443987910, name: "1Password" }
  - { id: 498486288, name: "Quick Resizer" }
  - { id: 557168941, name: "Tweetbot" }
  - { id: 497799835, name: "Xcode" }

composer_packages:
  - name: hirak/prestissimo
  - name: drush/drush
    version: "^8.1"

gem_packages:
  - name: bundler
    state: latest

npm_packages:
  - name: webpack

pip_packages:
  - name: mkdocs

configure_dock: true
dockitems_remove:
  - Launchpad
  - TV
dockitems_persist:
  - name: "Sublime Text"
    path: "/Applications/Sublime Text.app/"
    pos: 5
```

Any variable can be overridden in `config.yml`; see the supporting roles' documentation for a complete list of available variables.

## Included Applications / Configuration (Default)

Applications (installed with Homebrew Cask):

- [ChromeDriver](https://sites.google.com/chromium.org/driver/)
- [Docker](https://www.docker.com/)
- [Dropbox](https://www.dropbox.com/)
- [Firefox](https://www.mozilla.org/en-US/firefox/new/)
- [Google Chrome](https://www.google.com/chrome/)
- [Handbrake](https://handbrake.fr/)
- [Homebrew](http://brew.sh/)
- [LICEcap](http://www.cockos.com/licecap/)
- [nvALT](http://brettterpstra.com/projects/nvalt/)
- [Sequel Ace](https://sequel-ace.com) (MySQL client)
- [Slack](https://slack.com/)
- [Sublime Text](https://www.sublimetext.com/)
- [Transmit](https://panic.com/transmit/) (S/FTP client)

Packages (installed with Homebrew):

- autoconf
- bash-completion
- doxygen
- gettext
- gifsicle
- git
- github/gh/gh
- go
- gpg
- httpie
- iperf
- libevent
- sqlite
- mcrypt
- nmap
- node
- nvm
- php
- ssh-copy-id
- cowsay
- readline
- openssl
- pv
- wget
- wrk
- zsh-history-substring-search

My [dotfiles](https://github.com/geerlingguy/dotfiles) are also installed into the current user's home directory, including the `.osx` dotfile for configuring many aspects of macOS for better performance and ease of use. You can disable dotfiles management by setting `configure_dotfiles: no` in your configuration.

Finally, there are a few other preferences and settings added on for various apps and services.

## Testing the Playbook

Many people have asked me if I often wipe my entire workstation and start from scratch just to test changes to the playbook. Nope! This project is [continuously tested on GitHub Actions' macOS infrastructure](https://github.com/geerlingguy/mac-dev-playbook/actions?query=workflow%3ACI).

## Author

This project was created by [Simon Phumin Schweikert](https://simonphum.in/) (inspired by [geerlingguy/mac-dev-playbook](https://github.com/geerlingguy/mac-dev-playbook))

# Full Mac Setup Process (Simon Phumin Schweikert)

There are some things in life that just can't be automated... or aren't 100% worth the time :(

This document covers that, at least in terms of setting up a brand new Mac out of the box.

## Initial configuration of a brand new Mac

Before starting, I completed Apple's mandatory macOS setup wizard (creating a local user account, and optionally signing into my iCloud account). Once on the macOS desktop, I do the following (in order):

- Install Ansible (following the guide in [README.md](README.md))
- **Sign in in App Store** (since `mas` can't sign in automatically)
- Clone mac-dev-playbook to the Mac: `git clone git@github.com:geerlingguy/mac-dev-playbook.git`
- Drop `config.yml` from `~/Dropbox/Apps/Config` to the playbook (copy over the network or using a USB flash drive).
  - Run the playbook with `--skip-tags post`.
    - If there are errors, you may need to finish up other tasks like installing 'old-fashioned' apps first (since I try to place Photoshop in the Dock and it can't be installed automatically). Then, run the playbook again ;)
  - Start Synchronization tasks:
    - Open Photos and make sure iCloud sync options are correct
    - Open Music, make sure computer is authorized, and set Library sync options
    - Open Dropbox, sign in, and set up sync
  - Install old-fashioned apps:
    - Install [Creative Cloud](https://creativecloud.adobe.com/apps/download/creative-cloud)
      - Install Photoshop/Illustrator manually
    - (If required:)
      - Install [Elgato Stream Deck](https://www.elgato.com/en/downloads)
        - Open Livestream profile inside `~/Dropbox/Apps/Config/Stream Deck`
      - Install [Elgato Key Light Air (Control Center)](https://www.elgato.com/en/downloads)
      - Install [Autodesk Fusion 360](https://www.autodesk.com)
      - Install Microsoft Office Home & Student 2019 (https://account.microsoft.com/services/)
      - Install [Fritzing](https://fritzing.org/download/)
      - Install Meshmixer (but it looks like it's gone now!)
  - Configure FastMail account:
    - Log into Fastmail
    - Go to settings, go to the setup page for macOS Mail
    - Download the profile and double click to install
    - Head to the 'Profiles' System Preference pane and click install
  - Open Calendar and enable personal Google CalDAV account (you have to manually sign in).
  - Manually copy `~/Development` folder from another Mac (to save time).
  - Manual settings to automate someday:
    - System Preferences:
      - Accessibility > Display > Reduce transparency
      - Keyboard > Modifier Keys... > Caps Lock to Esc
    - Safari:
      - View > Show Status Bar
      - Preferences > Advanced > "Show full website address"
      - Preferences > Advanced > "Show Develop menu in menu bar"
    - Dock:
      - Add jgeerling, Downloads, Applications, and Video Projects folders
    - Terminal:
      - Preferences > Profiles > Set JJG-Term as the default theme
  - _After Dropbox Sync completes_: Run the playbook with `--tags post` to complete setup.
  - Symlink the synchronized `config.yml` into the playbook dir: `ln -s /Users/jgeerling/Dropbox/Apps/Config/mac-dev-playbook/config.yml /Users/jgeerling/Development/mac-dev-playbook/config.yml`
  - These things might be automatable, but I do them manually right now:
    - Configure Time Machine backup drive and [Time Machine Editor](https://tclementdev.com/timemachineeditor/) (if needed)
    - Install Wireguard from App Store and add configuration (if needed)

## To Wrap in Post-provision automation

The following tasks have to wait for the initial Dropbox sync to complete before they'll succeed. So ideally I'll stick this all in a post-provision script but somehow flag it not to run on first provision.

```
# ZSH Aliases.
ln -s /Users/jgeerling/Dropbox/Apps/Config/.aliases /Users/jgeerling/.aliases

# Electrum BTC Wallet.
ln -s /Users/jgeerling/Dropbox/Apps/Electrum/default_wallet /Users/jgeerling/.electrum/wallets/default_wallet

# SSH setup.
ssh-keygen  # and create a default key to set up .ssh folder
sudo ln -s /Users/jgeerling/Dropbox/Apps/Config/ssh/config ~/.ssh/config
# TODO - Manually copy any shared SSH keys that are needed.

# Ansible setup.
sudo mkdir -p /etc/ansible
sudo ln -s /Users/jgeerling/Dropbox/Apps/Config/ansible/ansible.cfg /etc/ansible/ansible.cfg
sudo ln -s /Users/jgeerling/Dropbox/Apps/Config/ansible/hosts /etc/ansible/hosts
sudo ln -s /Users/jgeerling/Dropbox/VMs/roles /etc/ansible/roles
mkdir -p /Users/jgeerling/.ansible
ln -s /Users/jgeerling/Dropbox/Apps/Config/ansible/galaxy_token /Users/jgeerling/.ansible/galaxy_token
ln -s /Users/jgeerling/Dropbox/Apps/Config/ansible/mm-vault-password.txt /Users/jgeerling/.ansible/mm-vault-password.txt
ln -s /Users/jgeerling/Dropbox/VMs/ /Users/jgeerling/.ansible/collections

# Final Cut Pro setup. (Open Motion first)
cp -r /Users/jgeerling/Dropbox/Apps/Config/Motion/Motion\ Templates.localized/ /Users/jgeerling/Movies/Motion\ Templates.localized/
cp -r /Users/jgeerling/Dropbox/Apps/Config/Motion/Text\ Styles/ /Users/jgeerling/Library/Application\ Support/Motion/Library/Text\ Styles.localized/

# Sequel Ace favorites. (Open Sequel Ace first)
cp /Users/jgeerling/Dropbox/Apps/Config/Sequel\ Ace/Favorites.plist /Users/jgeerling/Library/Containers/com.sequel-ace.sequel-ace/Data/Library/Application\ Support/Sequel\ Ace/Data/Favorites.plist

# Font setup.
cp ~/Dropbox/Apps/Config/Fonts/* ~/Library/Fonts/

# Vim setup.
mkdir -p ~/.vim/autoload
mkdir -p ~/.vim/bundle
cd ~/.vim/autoload
curl https://raw.githubusercontent.com/tpope/vim-pathogen/master/autoload/pathogen.vim > pathogen.vim
cd ~/.vim/bundle
git clone git://github.com/scrooloose/nerdtree.git
```

## When formatting old Mac

- Sign out of Adobe Creative Cloud
- Sign out of Panic Sync in Transmit
- Deauthorize Apple Music in iTunes/Music App
- Make sure anything new merged into `~/Dropbox/Apps/Config`:
  - Fonts from ~/Library/Fonts
  - Motion Plugins from ~/Movies/Motion
  - Final Cut Pro Text Styles in ~/Library/Application Support/Motion/Library/Text Styles
  - Sequel Ace shortcuts from ~/Library/Containers/com.sequel-ace.sequel-ace/Data/Library/Application\ Support/Sequel\ Ace/Data/Favorites.plist
- Follow Apple's guide [here](https://support.apple.com/en-au/HT212749)
