---
title: Telegram Bot만들기
subtitle: Telegram을 이용해 Bot만들기
description: Telegram을 이용해 Bot만들기
date: 2021-07-13 00:30:09
layout: post
category: telegram
tags:
  - js
  - telegram
  - bot
paginate: false
author: kingsae
image: https://telegram.org/file/811140934/1/tbDSLHSaijc/fdcc7b6d5fb3354adf
---

최근 들어 Bot을 구현함에 있어서 다양한 메신저에서 API를 제공하고 있는데, 그 중 Telegram을 이용하여 Bot을 구현해보았다. 다양한 메신저에서 Telegram을 선택한 이유에는 간편함이 가장 큰 이유가 될것 같다. Telegram을 선택하기 전 Slack, 잔디, 카카오톡 등등 다양한 메신저를 검토해보았지만, Telegram이 다른 메신저에 비해 비교적 쉬운 API를 제공하고 있었다.

구현하기 앞서 Telegram을 깔고 Telegram에서 BotFather를 호출해준다