---
title: How to setup X-UI panel for v2ray vpn
description: setup v2ray using Bash, Ansible, Docker
date: 2022-03-22
draft: false
slug: /pensieve/x-ui
tags:
  - Ansible
  - Bash
  - Docker
---

## မာတိကာ

1. [Getting Started](#getting-started)
2. [Setup X-UI using differnet methods](#setup)
   - [Bash](#bash)
   - [Docker](#docker)
   - [Ansible](#ansible)
3. [Getting SSL Certificate](#ssl)
4. [Configuration X-UI](#configuration-x-ui-panel)

<a name="getting-started"></a>

## 1. Getting Started

X-UI Panel ကို Setup လုပ်တာကို Bash, Ansible, Docker တွေကိုသုံးပြီး setup လုပ်မှဖြစ်တယ် <br>
လိုအပ်သည့်အရာများ

- Domain Name
- Server
- Cloudflare Account

ပထမဆုံး Domain Name ကို Cloudflare ရဲ့ Name Server ထည့်ပေးရမယ်။ ပြီးရင် Server IP ကို Domain Name ဆီကို A Record နဲ့ Point ပေးရမယ်။ <br>

**If you don't know how to do this step** -> [How to Point Domain to VPS IP?](https://privacymelon.com/how-to-setup-v2ray-ws-tls-cdn/#setup)

<a name="setup"></a>

## 2. Setup X-UI Panel using differnet methods

<a name="bash"></a>

### Bash Script ကိုသုံးပြီး setup လုပ်ခြင်း

Server ကို ssh နဲ့ login ဝင်ပြီးရင် အောက်က command ကို run လိုက်ပါ။
Run ရင် Email , Domain & TimeZone ကို ထည့်ပေးရမယ်။
<br>
If You Don't Know Your TimeZone -> **[TimeZone List](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)**

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

Email & Domain Name ကို မိမိအသုံးပြုမိဟာနဲ့ ပြောင်းပါ

```shell
sudo certbot certonly --standalone --preferred-challenges http --agree-tos --email your-email-address -d test.example.com
```

SSL ကို ရက် ၉၀ တခါ Update လုပ်ချင်ရင်

```shell
certbot renew --force-renewal
```

<a name="ansible"></a>

### Ansible ကိုသုံးပြီး setup လုပ်ခြင်း

Ansible Inventory မှာ Server ရဲ့ IP Address, SSH Username & Key Path ကိုထည့်ပါ

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
