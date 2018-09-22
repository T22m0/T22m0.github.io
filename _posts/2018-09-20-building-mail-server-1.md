---
layout: post
title: Building mail server - 1 (setting)
description: postfix, dovecot, letsencypt를 이용한 메일서버 구축 첫번째 단계입니다. 
category: MailServer
date: 2018-09-20

---

안녕하세요 여러분~
이번에는 라즈베리파이와 도메인을 이용해서 나만의 메일버서를 구축해보려고 합니다. 
구축전에 준비물은, `라즈베리파이`,`자신이 등록한 도메인`,`DDNS와 포트포워딩을 지원하는 공유기`,입니다. (저는 RPI3에 Raspbian, 도메인은 mast3r.com, 공유기는 IPtime A3004NS로 진행합니다) 
생각보다 준비물이 많네요...;;;

포스팅은 총 4단계로 나누어서 진행하구요, 이 글애는 메일서버구축을 위한 사전준비를 포스팅 하려고 합니다.
최대한 쉽게 따라오실 수 있게 노력했으니 도움이 됐으면 좋겠습니다.!

자 이제, 시작해보겠습니다. 


첫번째로 세팅할 곳은 공유기입니다. 
![1](assets/images/2018-09-20-building-mail-server-1/1.png)