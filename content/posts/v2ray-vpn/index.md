---
title: How to setup x-ui panel for v2ray vpn
description: setup v2ray using Bash, Ansible, Docker
date: 2022-03-22
draft: false
slug: /pensieve/v2ray-vpn
tags:
  - Ansible
  - Bash
  - Docker
---

## မာတိကာ

1. [Getting Started](#getting-started)
2. [Setup using differnet methods](#setup)
   - [Bash](#bash)
   - [Docker](#docker)
   - [Ansible](#ansible)
3. [Getting SSL Certificate](#ssl)
4. [Configuration X-UI](#configuration-x-ui-panel)

<a name="getting-started"></a>

## 1. Getting Started

V2ray VPN ကို Setup လုပ်တာကို Bash, Ansible, Docker တွေကိုသုံးပြီး setup လုပ်မှဖြစ်တယ် <br>
လိုအပ်သည့်အရာများ

- Domain
- VPS
- Cloudflare Account

ပထမဆုံး Domain ကို Cloudflare ရဲ့ Nameserver ထည့်ပေးရမယ်။ ပြီးရင် VPS IP ကို Domain ဆီကို A Record နဲ့ Point ပေးရမယ်။ <br>
[How to Point Domain to VPS IP?](https://privacymelon.com/how-to-setup-v2ray-ws-tls-cdn/)

<a name="setup"></a>

## 2. Setup using differnet methods

<a name="bash"></a>

### Bash Script ကိုသုံးပြီး setup လုပ်ခြင်း

VPS ကို ssh နဲ့ login ဝင်ပြီးရင် အောက်က cmd ကို run လိုက်ပါ။
Cmd run ရင် Email , Domain & TimeZone ကို ထည့်ပေးရမယ်။

```shell
bash <(curl -s https://raw.githubusercontent.com/ypo777/x-ui-panel/main/Bash/v2ray_setup.sh )
```

<a name="docker"></a>

### Docker ကိုသုံးပြီး setup လုပ်ခြင်း

First - Pull Docker Container to Your VPS

```shell
docker pull ypo007/x-ui
```

Second - Creating Docker Container

```shell
docker run --restart=always --name x-ui -d -p 54321:54321 -p 8000-8010:8000-8010/tcp -p 8000-8010:8000-8010/udp --tmpfs /tmp --tmpfs /run --tmpfs /run/lock -v /sys/fs/cgroup:/sys/fs/cgroup:ro -v /etc/x-ui:/etc/x-ui ypo007/x-ui
```

Last - [Get SSL](#ssl)

<a name="ansible"></a>

### Ansible ကိုသုံးပြီး setup လုပ်ခြင်း

Ansible Inventory မှာ VPS ရဲ့ IP Address, SSH Username & Keys ကိုထည့်ပါ

```shell
ansible-playbook v2raysetup.yaml
```

<a name="ssl"></a>

## 3. Getting SSL Certificate

```shell
sudo certbot certonly --standalone --preferred-challenges http --agree-tos --email your-email-address -d test.example.com
```

<a name="configuration-x-ui-panel"></a>

## 4. Configuration X-UI Panel

1. X-UI Panel ရဲ့ default port ကို သုံးပြီး Panel ကို browser ကနေဝင်ပါ။ <br>Exampel - http://YOUR_VPS_IP:54321
2. Admin & Password : both "admin" (default - တရုတ်စာ)
3. အရင်ဆုံး Admin Password or Both ကိုချိန်းပါ။
4. New User ထည့်ခြင်း
   - Go to Inbound List
   - Click `+` button
   - Enter your desired name in `remark`
   - Select `protocol: vmess`
   - Listen IP `yourIP`
   - Port : `your desired port` ( TCP:443 - Recommended)
   - Transmission: `ws`
   - Open TLS Option
   - Enter your domain name
   - Add Certificated you got from [SSL Certificate](#4-getting-ssl-certificate)
   - If you don't need , close `sniffing` option
