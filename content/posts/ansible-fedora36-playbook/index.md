---
title: Automation Playbook For Setting Up Fedora 36
description: ansible-fedora36-playbook
date: 2022-04-16
draft: false
slug: /pensieve/fedora-playbook
tags:
  - Linux
  - Fedora
  - Ansible
---

## Fedora Ansible Playbook

This playbook -

**Install**

- Chrome
- Visual Studio Code
- Sublime Text
- Spotify (lpf-spotify)

**Configure**

- DNF Settings
  - dnf default 'NO' to 'Yes'
  - add fastestmirror
  - add parallel download
- Add Media Encoding REPO
- Add Third Party Free & Non-Free RPM REPO
- Change Default Shell to ZSH
- Change Starship command prompt
- Add my custom dotfiles
  - gitignore
  - gitconfig
  - custom zshrc
  - custom tmux

on Local Machine (Version: Fedora 36).

## Installation

1. Install Ansible on Your Machine ` sudo dnf install ansible`
2. Clone or Download this repository to your local machine.
3. Run `ansible-playbook main.yml -K` in repo directory. Enter your account password.

TODO

- Install Oh-my-zsh
