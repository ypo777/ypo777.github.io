---
title: How to setup v2ray vpn
description: setup v2ray using Bash, Ansible, Docker
date: 2022-03-22
draft: false
slug: /pensieve/v2ray-vpn
tags:
  - Ansible
  - Bash
  - Docker
---

V2ray VPN ကို Setup လုပ်တာကို Bash, Ansible, Docker တွေကိုသုံးပြီး setup လုပ်မှဖြစ်တယ် <br>
လိုအပ်သည့်အရာများ

- Domain
- VPS
- Cloudflare Account

ပထမဆုံး VPS IP ကို Domain ဆီကို A Record နဲ့ Point ပေးရမယ် <br>
[How to Point Domain to VPS IP?](https://privacymelon.com/how-to-setup-v2ray-ws-tls-cdn/)

## Bash Script ကို အသုံးပြုခြင်း

```shell
bash <(curl -s https://raw.githubusercontent.com/ypo777/x-ui-panel/main/Bash/v2ray_setup.sh )
```

## Docker ကိုအသုံးပြုခြင်း

Pulling Docker Container

```shell
docker pull ypo007/x-ui
```

Creating Docker Container

```shell
docker run --restart=always --name x-ui -d -p 54321:54321 -p 8000-8010:8000-8010/tcp -p 8000-8010:8000-8010/udp --tmpfs /tmp --tmpfs /run --tmpfs /run/lock -v /sys/fs/cgroup:/sys/fs/cgroup:ro -v /etc/x-ui:/etc/x-ui ypo007/x-ui
```
