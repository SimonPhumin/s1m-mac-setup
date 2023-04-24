<img src="https://raw.githubusercontent.com/geerlingguy/mac-dev-playbook/master/files/Mac-Dev-Playbook-Logo.png" width="250" height="156" alt="Mac Dev Playbook Logo" />

# Mac Setup Ansible Playbook

[![CI Tests](https://github.com/SimonPhumin/s1m-mac-setup/actions/workflows/ci.yml/badge.svg)](https://github.com/SimonPhumin/s1m-mac-setup/actions/workflows/ci.yml)

This playbook installs and configures most of the software I use on my Mac for web and software development. Some things in macOS are slightly difficult to automate, so I still have a few manual installation steps, but at least it's all documented here.

## Installation & Prerequesites

In order to use t in this repository you probably need to create a new ssh key and add it to GitHub to clone this repository.

### SSH setup

ssh-keygen # and create a default key to set up .ssh folder

```bash
ssh-keygen -t ed25519 -C "your_email@domain.cool"
```

## Ansible Setup

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

You can override any of the defaults configured in `config.yml`. For example, you can customize the installed packages and apps with something like:

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

npm_yarn_packages:
  - name: webpack

pip_packages:
  - name: mkdocs
```

## Included Applications / Configuration (Default)

### Applications (installed with Homebrew Cask)

```yaml
- podman
- dropbox
- firefox
- google-chrome
- visual-studio-code
- postman
- adobe-creative-cloud
- figma
- sketch
- keepassxc
- slack
- webex
- warp
- iterm2
- imageoptim
- vlc
- keka
- karabiner-elements
- fontforge
- rectangle-pro
- onyx
- miro
- qlmarkdown
- packages
```

### Applications installed via AppStore (mas)

- [Podman](https://podman.io/)
- [Dropbox](https://www.dropbox.com/)
- [Firefox](https://www.mozilla.org/en-US/firefox/new/)
- [Google Chrome](https://www.google.com/chrome/)
- [Homebrew](http://brew.sh/)

### Packages (installed with Homebrew)

```yaml
- aom
- cloudflared
- composer
- ctop
- curl
- cyberduck
- exiftool
- flyctl
- fontconfig
- hcloud
- htop
- httpd
- macos-term-size
- mkcert
- mono
- n
- python@3.11
- git
- libevent
- sqlite
- node
- php
- screenresolution
- snappy
- syncthing
- openssl
- yarn
- wget
```

## Applications (installed via AppStore and mas)

```yaml
- { id: 1206020918, name: "Battery Indicator" }
- { id: 497799835, name: "Xcode" }
- { id: 784801555, name: "Microsoft OneNote" }
- { id: 462058435, name: "Microsoft Excel" }
- { id: 985367838, name: "Microsoft Outlook" }
- { id: 462054704, name: "Microsoft Word" }
- { id: 462062816, name: "Microsoft PowerPoint" }
```

## Testing

This project is [continuously tested on GitHub Actions' macOS infrastructure](https://github.com/SimonPhumin/s1m-mac-setup/actions/workflows/ci.yml). So you don't need to reset your new Mac if something does not work as intended ;-).

## Author

This project was created by [Simon Phumin Schweikert](https://simonphum.in/) (inspired by [geerlingguy/mac-dev-playbook](https://github.com/geerlingguy/mac-dev-playbook))
